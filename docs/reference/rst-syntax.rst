.. meta::
    :description: Reference for the reStructuredText syntax conventions used by Canonical.

:relatedlinks: [reStructuredText&#32;Primer](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html), [Canonical&#32;Documentation&#32;Style&#32;Guide](https://docs.ubuntu.com/styleguide/en)

.. _rst-syntax:

reStructuredText syntax
=======================

The Sphinx Stack supports `reStructuredText
<https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html>`__ (reST).

See the following sections for syntax help and conventions.

.. note::

    This guide assumes that you are using the `Sphinx Stack
    <https://github.com/canonical/sphinx-stack>`__. Some of the mentioned syntax
    requires the Sphinx extensions enabled in the Sphinx Stack.

For general style conventions, see the `Canonical Documentation Style Guide
<https://docs.ubuntu.com/styleguide/en>`__.


Headings
--------

.. list-table::
   :header-rows: 1

   * - Input
     - Description
   * - .. code::

          Title
          =====
     - Page title and H1 heading
   * - .. code::

          Heading
          -------
     - H2 heading
   * - .. code::

          Heading
          ~~~~~~~
     - H3 heading
   * - .. code::

          Heading
          ^^^^^^^
     - H4 heading
   * - .. code::

          Heading
          .......
     - H5 heading

Underlines must be at least as long as the title or heading.


Inline formatting
-----------------

.. list-table::
   :header-rows: 1

   * - Input
     - Output
   * - ``:guilabel:`UI element```
     - :guilabel:`UI element`
   * - ````code````
     - ``code``
   * - ``:file:`file path```
     - :file:`file path`
   * - ``:command:`command```
     - :command:`command`
   * - ``:kbd:`Key```
     - :kbd:`Key`
   * - ``*Italic*``
     - *Italic*
   * - ``**Bold**``
     - **Bold**

Code blocks
-----------

To start a code block, explicitly start a code block with ``..
code-block::``. The code block must be surrounded by empty lines.

.. list-table::
   :header-rows: 1

   * - Input
     - Output
   * - .. code::

          .. code-block::

             # Demonstrate a code block
             code:
             - example: true
     - .. code-block::

          # Demonstrate a code block
          code:
          - example: true
   * - .. code::

          .. code-block:: yaml

             # Demonstrate a code block
             code:
             - example: true
     - .. code-block:: yaml

          # Demonstrate a code block
          code:
          - example: true


Terminal output
~~~~~~~~~~~~~~~

A terminal view emulates the command line experience more accurately than a code block.
This is particularly useful in tutorials or guides that are terminal-heavy, or where
it's helpful to show the directory a command is run from.

To include a terminal view, use the following directive:

.. list-table::
    :header-rows: 1

    * - Input
      - Output
    * - .. code-block:: text

            .. terminal::
                
                input line 1
                input line 2

                output line 1
                output line 2

                output line 3
      - .. terminal::

            input line 1
            input line 2

            output line 1
            output line 2

            output line 3

By default, everything before the first blank line in the directive's content is
rendered as input, and any content that follows is rendered as output. The terminal
directive can only display one input command, but that command can span multiple lines,
as in the previous example.

To render only the output of a command, include the ``:output-only:`` flag in the
directive's options:

.. list-table::
    :header-rows: 1

    * - Input
      - Output
    * - .. code-block:: text

            .. terminal::
                :output-only:

                This is rendered as output.

      - .. terminal::
            :output-only:

            This is rendered as output.

To customize the prompt (``user@host:~$`` by default), specify any of the following options:

* ``:user:``
* ``:host:``
* ``:dir:``

.. list-table::
    :header-rows: 1

    * - Input
      - Output
    * - .. code-block:: text

            .. terminal::
                :user: author
                :host: canonical
                :dir: ~/path
                
                input

                output
      - .. terminal::
            :user: author
            :host: canonical
            :dir: ~/path

            input

            output

The copy button for input commands is **opt-in**. You must include the ``:copy:`` flag
in the directive's options for the button to be displayed.

To make the terminal scroll horizontally instead of wrapping long lines, include the ``:scroll:`` option.

For more details, refer to the `sphinx-terminal README <https://github.com/canonical/sphinx-terminal/blob/main/README.md>`__.


Links
-----

Link markup depends on whether you need an external URL or a page in the same
documentation set.


.. _reference-external-link-syntax:

External links
~~~~~~~~~~~~~~

To link to documents in other Sphinx projects, use :ref:`Intersphinx
<how-to-link-docs-intersphinx>` with the ``:ref:`` or ``:doc:`` role:

.. list-table::
  :header-rows: 1

  * - Input
    - Output

  * - .. code-block:: rst

        :external+ubuntu-desktop:doc:`index`

    - :external+ubuntu-desktop:doc:`index`

  * - .. code-block:: rst

        :external+ubuntu-desktop:ref:`install-ubuntu-desktop`

    - :external+ubuntu-desktop:ref:`install-ubuntu-desktop`

To link to other websites, use the hyperlink reference syntax:

.. list-table::
  :header-rows: 1

  * - Input
    - Output

  * - .. code-block:: rst

        `Canonical home <https://canonical.com>`__

    - `Canonical home <https://canonical.com>`__


If necessary, it's possible to write a standalone hyperlink, which won't contain any
link text:

.. list-table::
  :header-rows: 1

  * - Input
    - Output

  * - .. code-block:: rst

        https://canonical.com

    - https://canonical.com


The documentation checks will likely flag it as a spelling error.


Plain text
^^^^^^^^^^

Outside of directives, reST interprets every URL it finds as a hyperlink. If you need a link to be rendered as plain text, escape the colon in the protocol:

.. list-table::
  :header-rows: 1

  * - Input
    - Output
  * - https\\://canonical.com/
    - :spellexception:`https://canonical.com/`


Sidebar links
^^^^^^^^^^^^^

You can add links to related websites or Discourse topics to the sidebar.

To add a link to a related website, add the following field at the top of the page::

  :relatedlinks: https://github.com/canonical/lxd-sphinx-extensions, [RTFM](https://www.google.com)

To override the title, use Markdown syntax. Note that spaces are ignored; if you need spaces in the title, replace them with ``&#32;``, and include the value in quotes if Sphinx complains about the metadata value because it starts with ``[``.
For example: ``[My&#32;Title](https://...)``.

To add a link to a Discourse topic, configure the Discourse instance in the
`conf.py` file. Then add the following field at the top of the page:

.. code-block:: rst

  :discourse: <topic-id>


Manual pages
^^^^^^^^^^^^

When mentioning command-line utilities, you may wish to link to the
corresponding manual page for the command. Ensure that the ``manpages_url``
setting in your :file:`conf.py` is set appropriately and use the ``:manpage:``
role within your text to create a link.

For example, to link to man pages from the 24.04 LTS (Noble Numbat) release,
include the following in your :file:`conf.py`:

.. code-block:: python

    manpages_url = "https://manpages.ubuntu.com/manpages/noble/en/man{section}/{page}.{section}.html"

Then within the document, use the following reST:

.. code-block:: rst

    You can use the :manpage:`dd(1)` utility to write the disk image to your
    SD card. If the image is compressed, use :manpage:`aunpack(1)` to extract
    it first.


YouTube
^^^^^^^

To add a link to a YouTube video, use the following directive:

.. list-table::
   :header-rows: 1

   * - Input
     - Output
   * - .. code::

          .. youtube:: https://www.youtube.com/watch?v=iMLiK1fX4I0
             :title: Demo

     - .. youtube:: https://www.youtube.com/watch?v=iMLiK1fX4I0
          :title: Demo

The video title is extracted automatically and displayed when hovering over the link.
To override the title, add the ``:title:`` option.


Internal references
~~~~~~~~~~~~~~~~~~~

.. _a_section_label:

Sections
^^^^^^^^

To reference a section within the documentation (either on the same page or on another page), add a label to that section and reference that label.

.. _a_random_label:

You can add a label anywhere in any document.
When referencing a label that isn't attached to a heading, you must add link text.
If you don't, the reference won't work.

.. list-table::
   :header-rows: 1

   * - Input
     - Output
     - Description
   * - ``.. _label_ID:``
     -
     - Adds the label ``label_ID``.

       .. note::
          When defining the label, you must prefix it with an underscore. Do not use the starting underscore when referencing the label.
   * - ``:ref:`a_section_label```
     - :ref:`a_section_label`
     - References a label that has a title.
   * - ``:ref:`Provided link text <a_random_label>```
     - :ref:`Provided link text <a_random_label>`
     - References a label and specifies a title.


Pages
^^^^^

If a documentation page does not have a label, you can still reference it by using the ``:doc:`` role with the file name and path.

.. list-table::
   :header-rows: 1
   :widths: 8 2

   * - Input
     - Output
   * - ``:doc:`index```
     - :doc:`index`
   * - ``:doc:`Provided link text <index>```
     - :doc:`Provided link text <index>`

Only use the ``:doc:`` role when you cannot use the ``:ref:`` role, thus only if there
is no label at the top of the file and you cannot add it. When using the ``:doc:``
role, your reference will break when a file is renamed or moved.


Formatted link text
~~~~~~~~~~~~~~~~~~~

With the exception of inline code, reST doesn't support special formatting for link
text, such as *emphasized* and **strong** text.

Use the ``:literalref:`` role to format a reference's link text as inline code:

.. list-table::
   :header-rows: 1

   * - Input
     - Output

   * - ``:literalref:`example <https://example.com>```
     - :literalref:`example <https://example.com>`
   * - ``:literalref:`label <a_random_label>```
     - :literalref:`label <a_random_label>`

The link text is automatically excluded from the spelling check.


Navigation
----------

Every documentation page must be included as a sub-page to another page in the navigation.

This is achieved with the `toctree
<https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-toctree>`__
directive in the parent page::

  .. toctree::
     :hidden:

     sub-page1
     sub-page2

If a page should not be included in the navigation, you can suppress the resulting build
warning by putting ``:orphan:`` at the top of the file. Use orphan pages sparingly and
only if there is a clear reason for it.

.. tip::

    Instead of hiding pages that you do not want to include in the documentation from
    the navigation, you can exclude them from being built. This method will also prevent
    them from being found through the search.
  
    To exclude pages from the build, add them to the ``exclude_patterns`` variable in the
    ``conf.py`` file.


Lists
-----

.. list-table::
    :header-rows: 1

    * - Input
      - Output
    * - .. code::

            - Item 1
            - Item 2
            - Item 3
      - - Item 1
        - Item 2
        - Item 3
    * - .. code::

            1. Step 1
            #. Step 2
            #. Step 3
      - 1. Step 1
        #. Step 2
        #. Step 3
    * - .. code::

            a. Step 1
            #. Step 2
            #. Step 3
      - a. Step 1
        #. Step 2
        #. Step 3

You can also nest lists:

.. tab-set::

    .. tab-item:: Input

        .. code::

            1. Step 1

              - Item 1

                * Sub-item
              - Item 2

                i. Sub-step 1
                #. Sub-step 2

            #. Step 2

              a. Sub-step 1

                - Item

              #. Sub-step 2

    .. tab-item:: Output

        1. Step 1

          - Item 1

            * Sub-item
          - Item 2

            i. Sub-step 1
            #. Sub-step 2

        #. Step 2

          a. Sub-step 1

            - Item

          #. Sub-step 2

In numbered lists, number the first item and use ``#.`` for all subsequent items to
generate the step numbers automatically.

Use ``-`` for unordered lists. When using nested lists, you can use ``*`` for the
nested level.


Definition lists
~~~~~~~~~~~~~~~~

.. list-table::
    :header-rows: 1

    * - Input
      - Output
    * - .. code::

            Term 1:
              Definition
            Term 2:
              Definition
      - Term 1:
          Definition
        Term 2:
          Definition


.. _style-guide-tables:

Tables
------

reST supports different markup for tables. Grid tables are most similar to tables in
Markdown, but list tables are usually much easier to use. See the `Sphinx documentation
<tables_>`_ for all table syntax alternatives.

Both markups result in the following output:

.. list-table::
    :header-rows: 1

    * - Header 1
      - Header 2
    * - Cell 1

        Second paragraph cell 1
      - Cell 2
    * - Cell 3
      - Cell 4


Grid tables
~~~~~~~~~~~

See `grid tables
<https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#grid-tables>`__ for
reference.

.. code-block::

    +----------------------+------------+
    | Header 1             | Header 2   |
    +======================+============+
    | Cell 1               | Cell 2     |
    |                      |            |
    | 2nd paragraph cell 1 |            |
    +----------------------+------------+
    | Cell 3               | Cell 4     |
    +----------------------+------------+


List tables
~~~~~~~~~~~

See `list tables
<https://docutils.sourceforge.io/docs/ref/rst/directives.html#list-table>`__ for
reference.

.. code::

    .. list-table::
        :header-rows: 1

        * - Header 1
          - Header 2
        * - Cell 1

            2nd paragraph cell 1
          - Cell 2
        * - Cell 3
          - Cell 4


Data tables
~~~~~~~~~~~

If you have a small amount of CSV data, you can include the data in the doc source. 

For example:

.. code-block:: rst

    .. csv-table::
        :header: "Animal", "Number of legs", "Size"

        "Worm", 0, "Small"
        "Penguin", 2, "Medium"
        "Horse", 4, "Large"
        "Ant", 6, "Small"
        "Octopus", 8, "Medium"

If you have a large amount of CSV data, or the data is generated by an automated
process, you can include the data from a file. 

For example:

.. code-block:: rst

    .. csv-table::
        :file: /assets/animals.csv
        :header-rows: 1

Both markups result in the following output:

.. csv-table::
    :header: "Animal", "Number of legs", "Size"

    "Worm", 0, "Small"
    "Penguin", 2, "Medium"
    "Horse", 4, "Large"
    "Ant", 6, "Small"
    "Octopus", 8, "Medium"

Customize the column widths, character encoding, and so on, as described in the
`csv-table reference
<https://docutils.sourceforge.io/docs/ref/rst/directives.html#csv-table>`__.

The Sphinx Stack can also render interactive tables, which are described in
:ref:`interactive-tables`.


Notes
-----

.. code-block:: rst

    .. admonition:: <title>
        :class: <class>

Images
------

.. list-table::
    :header-rows: 1

    * - Input
      - Output
    * - ``.. image:: https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png``
      - .. image:: https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png
    * - .. code::

            .. figure:: https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png
              :width: 100px
              :alt: Alt text

              Figure caption
      - .. figure:: https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png
            :width: 100px
            :alt: Alt text

            Figure caption

For local pictures, start the path with ``/`` (for example, ``/images/image.png``).

Use ``PNG`` format for screenshots and ``SVG`` format for graphics.

If producing multiple output formats, use ``*`` as the file extension to have
Sphinx select the best image format for the output

See `Five golden rules for compliant alt text
<https://abilitynet.org.uk/resources/digital-accessibility/five-golden-rules-compliant-alt-text>`__
for information about how to word the alt text.


Reuse
-----

A big advantage of reST in comparison to plain Markdown is that it allows the reuse
of content.


.. _reference-substitution-syntax:

Substitution
~~~~~~~~~~~~

To reuse sentences and entire paragraphs that have little markup or special formatting,
define `substitutions
<https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#substitutions>`__
for them by putting the same directives in any reST file:

.. code-block:: rst
    :caption: :spellexception:`index.rst`

    .. |version_number| replace:: 0.1.0

    .. |rest_text| replace:: *Multi-line* text
                              that uses basic **markup**.

    .. And so on


.. note::

    Mind that substitutions can't be redefined; for instance, accidentally including a
    definition twice causes an error:

    .. code-block:: none

      ERROR: Duplicate substitution definition name: "rest_text".


The definitions from the above examples are rendered as follows:

.. list-table::
    :header-rows: 1

    * - Input
      - Output

    * - ``|version_number|``
      - |version_number|

    * - ``|rest_text|``
      - |rest_text|

    * - ``|site_link|_``
      - |site_link|_

File inclusion
~~~~~~~~~~~~~~

To reuse longer sections or text with more advanced markup, you can put the content in a
separate file and include the file or parts of the file in several locations.

To select parts of the text in a file, use ``:start-after:`` and ``:end-before:`` if
possible. You can combine those with ``:start-line:`` and ``:end-line:`` if
the same text occurs more than once. Using only ``:start-line:`` and ``:end-line:`` is
error-prone though.

You cannot put any labels into the content that is being reused (because references to
this label would be ambiguous then). You can, however, put a label right before
including the file.

By combining file inclusion and substitutions defined directly in a file, you can even
replace parts of the included text.

.. list-table::
   :header-rows: 1

   * - Input
     - Output
   * - .. code::

          .. include:: /how-to/index.rst
            :start-after: .. _how-to-guides:
            :end-before: =============
     - .. include:: /how-to/index.rst
         :start-after: .. _how-to-guides:
         :end-before: =============

Files that only contain text that is reused somewhere else should be placed in the
``reuse`` directory and end with the extension ``.txt`` to distinguish them from
normal content files.

To make sure inclusions don't break, consider adding comments (``.. some comment``) to
the source file as markers for starting and ending.


Tabs
----

The recommended way of creating tabs is with the `Sphinx design
<https://sphinx-design.readthedocs.io/en/latest/>`__ extension.

.. list-table::
    :header-rows: 1

    * - Input
      - Output
    * - .. code::

            .. tab-set::

              .. tab-item:: Tab 1
                  :sync: key1

                  Content Tab 1

              .. tab-item:: Tab 2
                  :sync: key2

                  Content Tab 2
      - .. tab-set::

          .. tab-item:: Tab 1
              :sync: key1

              Content Tab 1

          .. tab-item:: Tab 2
              :sync: key2

              Content Tab 2


Glossary
--------

You can define glossary terms in any file. Ideally, all terms should be collected in one
glossary so they can then be referenced from any file.

.. list-table::
    :header-rows: 1

    * - Input
      - Output
    * - .. code::

            .. glossary::

              an example term
                Definition of an example term.
      - .. glossary::

            an example term
              Definition of an example term.
    * - ``:term:`an example term```
      - :term:`an example term`


.. _section_more_useful_markup:

More useful markup
------------------

.. list-table::
    :header-rows: 1

    * - Input
      - Output
      - Description
    * - .. code::

            .. versionadded:: X.Y
      - .. versionadded:: X.Y
      - Can be used to distinguish between different versions.
    * - .. code::

            | Line 1
            | Line 2
            | Line 3
      - | Line 1
        | Line 2
        | Line 3
      - Line breaks that are not paragraphs. Use this sparingly.
    * - .. code::

            ----
      - A horizontal line
      - Can be used to visually divide sections on a page.
    * - ``.. This is a comment``
      - .. This is a comment
      - Not visible in the output.
    * - ``:abbr:`API (Application Programming Interface)```
      - :abbr:`API (Application Programming Interface)`
      - Hover to display the full term.
    * - ``:spellexception:`PurposelyWrong```
      - :spellexception:`PurposelyWrong`
      - Explicitly exempt a term from the spelling check.
