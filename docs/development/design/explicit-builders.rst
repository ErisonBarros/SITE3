Explicit Builders
=================

Background
----------

In the past we have installed some dependencies on each build,
and tried to guess some options for users in order to make it *easy* for them to start using Read the Docs.
This has bring some unexpected problems and confusion to users, like:

- The Sphinx version (or from any other tool) isn't the one they expect.
- Unexpected dependencies are installed.
- The wrong docs directory is used.
- Their configuration file is changed on build time overriding the defaults or adding new things.
- Some files are auto-generated (like ``index.{rst,md}``).

Currently we are aiming to remove this *magic* behaviour from our build process,
and educating users to be more explicit about their dependencies and options
in order to make their builds reproducible.

We are using several feature flags to stop doing this for new projects,
but with so many flags to check our code starts to be hard to follow.
Instead we would manage a single feature flag and several classes of builds and environments.

Python Environments
-------------------

Currently we have two Python environments: Virtualenv and Conda,
they are in charge of installing requirements into an isolated environment.
We would need to refactor those classes into two types: the new build, and the old build.

The new Python environments would install only the latest versions of the following dependencies:

Virtualenv
   - Sphinx or MkDocs
   - readthedocs-sphinx-ext

Conda
   - readthedocs-sphinx-ext

Note that for conda we don't install Sphinx or MkDocs,
this is to avoid problems like #3829_

.. _#3829: https://github.com/readthedocs/readthedocs.org/issues/3829

Doc builders
------------

Currently we have two types of doc builders: Sphinx and MkDocs.
they are in charge of generating the HTML files (or pdf/epub) from the source files.
We would need to refactor those classes into two types: the new build, and the old build.

The new builders would do the minimal changes to the user's configuration in order to build their docs:

Sphinx
~~~~~~

- Read the configuration file from the user or default to one path.
  It errors if one doesn't exists.
- Don't override the values from the user's configuration (``conf.py``),
  specially from the ``html_context``.
- Pass the minimal information needed into the context.

Sphinx's ``html_context``
'''''''''''''''''''''''''

This is related to :doc:`/development/design/theme-context`.

With the :doc:`/api/v3` and environment variables (to store the secret token) should be possible to query several things from the project
without having to pass them at build time into the context.
Most the of basic information can be obtained from our environment variables (``READTHEDOCS_*``).

Some values from the context are used in our Sphinx extension,
we should define them as configuration options instead.
Others are used in our theme, should we stop setting them?

Extension
'''''''''

There are some things our extension does that are already supported by Sphinx or theme
(analytics, canonical URL, etc).

It also injects some custom js and css.
We could try another more general approach and inject these after the build is complete.

MkDocs
~~~~~~

- Read the configuration file from the user or default to one path.
  It errors if one doesn't exists.
- Don't change the values from the user's configuration (``mkdocs.yml``).
  Only the additional js/css files should be added.
  Additionally, we could try another more general approach and inject these after the build is complete.

Web settings
------------

Simple defaults without fallbacks.

Migration
---------

To start this would be under a feature flag,
where projects created after ``x`` date would use the new builders,
and past projects would use the old ones.

Doing this may bring some confusion to users that have a project created before the given date,
and other after that date.
We can opt-in users into the new builders by adding them into the feature flag.

Deprecation
-----------

In order to simplify our code and have all projects using the same options and dependencies
we want to fully migrate all projects to use the new builders.
We could just put a date and contact all users of old projects about this change (a blog post would also be great).
