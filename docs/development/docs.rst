Building and Contributing to Documentation
==========================================

As one might expect,
the documentation for Read the Docs is built using Sphinx and hosted on Read the Docs.
The docs are kept in the ``docs/`` directory at the top of the source tree.

.. TODO: expand this section explaining there the PR is automatically built and
   the author can visualize changes without installing anything on their system.
   However, if there is going to be periodic/bigger contributions, it may be a
   good idea to install the Sphinx requirements to build our docs.

Please follow these guidelines when updating our docs.
Let us know if you have any questions or something isn't clear.

The brand
---------

We are called **Read the Docs**.
The *the* is not capitalized.

We do however use the acronym **RTD**.

Titles
------

For page titles, or Heading1 as they are sometimes called, we use title-case.

If the page includes multiple sub-headings (H2, H3),
we usually use sentence-case unless the titles include terminology that is supposed to be capitalized.

Content
-------

* Do not break the content across multiple lines at 80 characters,
  but rather break them on semantic meaning (e.g. periods or commas).
  Read more about this `here <https://rhodesmill.org/brandon/2012/one-sentence-per-line/>`_.
* If you are cross-referencing to a different page within our website,
  use the ``doc`` role and not a hyperlink.
* If you are cross-referencing to a section within our website,
  use the ``ref`` role with the label from the `autosectionlabel extension <http://www.sphinx-doc.org/en/master/usage/extensions/autosectionlabel.html>`__.
