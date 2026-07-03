---
relatedlinks: https://github.com/canonical/canonical-sphinx-extensions, https://github.com/canonical/lxd-sphinx-extensions, [reStructuredText&#32;Primer](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html), [Canonical&#32;Documentation&#32;Style&#32;Guide](https://docs.ubuntu.com/styleguide/en)
myst:
  html_meta:
    description: Reference for the reST and MyST syntax conventions used by Canonical.
  substitutions:
    advanced_reuse_key: "This is a substitution that includes a code block:
                       ```
                       code block
                       ```"
---

# Syntax guide

The Sphinx Stack supports [reStructuredText](https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html) (reST), [Markdown](https://commonmark.org/) and
[MyST](https://myst-parser.readthedocs.io/).

See the following sections for syntax help and conventions.

```{note}
This guide assumes that you are using the [Sphinx
Stack](https://github.com/canonical/sphinx-stack). Some of the mentioned syntax requires
Sphinx extensions (which are enabled in the Sphinx Stack).
```

For general style conventions, see the [Canonical Documentation Style
Guide](https://docs.ubuntu.com/styleguide/en).

## Headings

```{list-table}
:header-rows: 1

* - MyST
  - reST
  - Description
* - `# Title`
  - ```
    Title
    =====
    ```
  - Page title and H1 heading
* - `## Heading`
  - ```
    Heading
    -------
    ```
  - H2 heading
* - `### Heading`
  - ```
    Heading
    ~~~~~~~
    ```
  - H3 heading
* - `#### Heading`
  - ```
    Heading
    ^^^^^^^
    ```
  - H4 heading
* - `##### Heading`
  - ```
    Heading
    .......
    ```
  - H5 heading
```

In reST, underlines must be at least as long as the title or heading.

## Inline formatting

```{list-table}
:header-rows: 1

* - MyST
  - reST
  - Output
* - `` {guilabel}`UI element` ``
  - `` :guilabel:`UI element` ``
  - {guilabel}`UI element`
* - `` `code` ``
  - ```` ``code`` ````
  - `code`
* - `` {file}`file path` ``
  - `` :file:`file path` ``
  - {file}`file path`
* - `` {command}`command` ``
  - `` :command:`command` ``
  - {command}`command`
* - `` {kbd}`Key` ``
  - `` :kbd:`Key` ``
  - {kbd}`Key`
* - `*Italic*`
  - `*Italic*`
  - *Italic*
* - `**Bold**`
  - `**Bold**`
  - **Bold**
```

## Code blocks

### Basic code block

```
# Demonstrate a code block
code:
- example: true
```

``````{tab-set}

`````{tab-item} reST
To start a code block, either end the introductory paragraph with two colons (``::``)
and indent the following code block, or explicitly start a code block with ``..
code::``. In both cases, the code block must be surrounded by empty lines.

````
Demonstrate a code block::

  code:
  - example: true
````

Or use the explicit directive:

````
.. code::

   # Demonstrate a code block
   code:
   - example: true
````
`````

`````{tab-item} MyST
Start an````d end a code block with three back ticks:

````
```
# Demonstrate a code block
code:
- example: true
```
````

``````

### Code block with language

```yaml
# Demonstrate a code block
code:
- example: true
```

``````{tab-set}

`````{tab-item} reST
When explicitly starting a code block, you can specify the code language to enforce a
specific lexer, but in many cases, the default lexer works just fine.

````
.. code:: yaml

   # Demonstrate a code block
   code:
   - example: true
````
For a list of supported languages and their respective lexers, see the official
`Pygments documentation <https://pygments.org/languages/>`__.

`````

`````{tab-item} MyST
You can specify the code language after the back ticks to enforce a specific lexer, but
in many cases, the default lexer works just fine.

````
```yaml
# Demonstrate a code block
code:
- example: true
```
````
`````

``````

### Terminal output

A terminal view can be useful to show the output of a specific command, where it is
important to see the difference between input and output. In addition, including a
terminal view can help break up a long text and make it easier to consume, which is
especially useful when documenting command-line-only products.

#### Basic

```{terminal}

input line 1
input line 2

output line 1
output line 2

output line 3
```

By default, everything before the fireST blank line in the directive's content is
rendered as input, while any content that follows is rendered as output. The terminal
directive can only display one input command, but that command can span multiple lines,
as in the previous example.

``````{tab-set}

`````{tab-item} reST
To include a terminal view, use the following directive:

````
.. terminal::
    
    input line 1
    input line 2

    output line 1
    output line 2

    output line 3
````
`````

`````{tab-item} MyST
To show a terminal view, use the following directive:

````text
```{terminal}

input line 1
input line 2

output line 1
output line 2

output line 3
```
````
`````

``````

#### Output only

To render only the output of a command, include the `:output-only:` flag in the
directive's options:

```{terminal}
:output-only:

This is rendered as output.
```

``````{tab-set}

`````{tab-item} reST
````
.. terminal::
    :output-only:

    This is rendered as output.
````
`````

`````{tab-item} MyST
````text
```{terminal}
:output-only:

This is rendered as output.
```
````
`````

``````

#### Custom prompt

To customize the prompt (`user@host:~$` by default), specify any of the following options:

* `:user:`
* `:host:`
* `:dir:`

```{terminal}
:user: author
:host: canonical
:dir: ~/path

input

output
```

``````{tab-set}

`````{tab-item} reST
````
.. terminal::
    :user: author
    :host: canonical
    :dir: ~/path
    
    input

    output
````
`````

`````{tab-item} MyST
````text
```{terminal}
:user: author
:host: canonical
:dir: ~/path

input

output
```
````
`````

``````

The copy button for input commands is **opt-in**. You must include the `:copy:` flag
in the directive's options for the button to be displayed.

To make the terminal scroll horizontally instead of wrapping long lines, include the
`:scroll:` option.

For more details, refer to the [`sphinx-terminal`
README](https://github.com/canonical/sphinx-terminal/blob/main/README.md).

## Links

How to link depends on if you are linking to an external URL or to another page in the
documentation.
Link markup depends on whether you need an external URL
or a page in the same documentation set.

### External links

For external links, use one of the following methods.

Link inline:
  Define occasional links directly within the surrounding text.
  To make the link text show up in code-style (which excludes it from the spelling check), use the ``:literalref:`` role.

For external links, use Markdown syntax. You can also use just the URL, but this will
usually cause issues with the spelling check, so you should specify the link text as
code in this case.

``````{tab-set}

`````{tab-item} reST
````{list-table}
:header-rows: 1

* - Input
  - Output
* - `` `Canonical website <https://canonical.com/>`_` ``
  - [Canonical website](https://canonical.com)
* - `` :literalref:`ubuntu.com` ``
  - {literalref}`ubuntu.com`
* - `` :literalref:`xyzcommand <https://example.com>` ``
  - {literalref}`xyzcommand <https://example.com>`
````

You can also use a URL as is (``https://example.com``),
but that might cause spellchecker errors.

````{tip}
To prevent a URL from appearing as a link,
add an escaped space character (``https:\ //``).
The space won't be rendered:

```{list-table}
:header-rows: 1

* - Input
  - Output
* - ``https:\ //canonical.com/``
  - {spellexception}`https://canonical.com/`
```
````

**Define the links at the bottom of the page:**
To keep the text readable, group the link definitions below.

````{list-table}
:header-rows: 1

* - Input
  - Output
  - Description
* - `` `Canonical website`_` ``
  - [Canonical website](https://canonical.com)
  - Using the below defined link
* - ```
    .. LINKS
    .. _Canonical website: https://canonical.com/
    ```
  - *n/a*
  - Defining links at the bottom
````

**Define the links in a shared file:**
To keep the text readable and links maintainable,
put all link definitions in a file named {file}`reuse/links.txt`
to include it in a custom ``reST_epilog`` directive
(see the [reST_epilog documentation](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-rst_epilog)).

```python
custom_reST_epilog = """
    .. include:: reuse/links.txt
    """
```

````{list-table}
:header-rows: 1

* - Input
  - Output
* - `` `Canonical website`_` ``
  - [Canonical website](https://canonical.com)
````
`````

`````{tab-item} MyST
````{list-table}
:header-rows: 1

* - Input
  - Output
* - `[Canonical website](https://canonical.com)`
  - [Canonical website](https://canonical.com)
* - `https://canonical.com`
  - [{spellexception}`https://canonical.com`](https://canonical.com)
* - ``[`https://canonical.com`](https://canonical.com)``
  - [`https://canonical.com`](https://canonical.com)
````

To display a URL as text and prevent it from being linked, add a `<span></span>`:

````{list-table}
:header-rows: 1

* - Input
  - Output
* - `https:/<span></span>/canonical.com`
  - {spellexception}`https:/<span></span>/canonical.com`
````
`````

``````

### Internal links

You can add links to related websites or Discourse topics to the sidebar.

To add a link to a related website, add the following field at the top of the page:

```{list-table}
:header-rows: 1

* - Format
  - Syntax
* - reST
  - `` :relatedlinks: https://github.com/canonical/lxd-sphinx-extensions, [RTFM](https://www.google.com) ``
* - MyST
  - `relatedlinks: https://github.com/canonical/canonical-sphinx-extensions, [RTFM](https://www.google.com)`
```

To override the title, use Markdown syntax. Note that spaces are ignored; if you need spaces in the title, replace them with ``&#32;``, and include the value in quotes if Sphinx complains about the metadata value because it starts with ``[``.

To add a link to a Discourse topic, configure the Discourse instance in the `conf.py` file.
Then add the following field at the top of the page (where ``12345`` is the ID of the Discourse topic):

```
  :discourse: 12345
```

### Manual-page links

When mentioning command line utilities, you may wish to link to the
corresponding manual page for the command. Ensure that the ``manpages_url``
setting in your `conf.py` is set appropriately and use the ``:manpage:``
inline role within your text to create a link.

For example, to link to man pages from the 24.04 LTS (Noble Numbat) release,
include the following in your `conf.py`:

```
    manpages_url = "https://manpages.ubuntu.com/manpages/noble/en/man{section}/{page}.{section}.html"
```

Then within your documentation, use the following:

``````{tab-set}

`````{tab-item} reST

You can use the `:manpage:dd(1)` utility to write the disk image to your
SD card. If the image is compressed, use `:manpage:aunpack(1)` to extract
it fireST.

`````

`````{tab-item} MyST

Not applicable.

``````

### YouTube links

To add a link to a YouTube video, use the following directive:

```````{tab-set}

``````{tab-item} reST
````{list-table}
:header-rows: 1

* - Input
  - Output
* - ```
    .. youtube:: https://www.youtube.com/watch?v=iMLiK1fX4I0
       :title: Demo
    ```
  - ```{youtube} https://www.youtube.com/watch?v=iMLiK1fX4I0
    :title: Demo
    ```
````
``````

``````{tab-item} MyST
`````{list-table}
:header-rows: 1

* - Input
  - Output
* - ````

    ```{youtube} https://www.youtube.com/watch?v=iMLiK1fX4I0
    :title: Demo
    ```

    ````

  - ```{youtube} https://www.youtube.com/watch?v=iMLiK1fX4I0
    :title: Demo
    ```

`````
``````

```````

The video title is extracted automatically and displayed when hovering over the link.
To override the title, add the ``:title:`` option.

### Internal references

You can reference pages and targets in this documentation set.
For referencing pages from other documentation sets, you can use
{ref}`Intersphinx <how-to-link-docs-intersphinx>``.

(a_section_target_myst)=

#### Referencing a section

To reference a section within the documentation (either on the same page or on another
page), add a target to that section and reference that target.

You can add targets at any place in the documentation. However, if there is no heading
or title for the targeted element, you must specify a link text.

To reference a section within the documentation (either on the same page or on another page), add a target to that section and reference that target.

```{list-table}
:header-rows: 1

* - Format
  - Syntax
* - reST
  - `` :ref:`a_section_target` ``
* - MyST
  - `` {ref}`a_section_target_myst` ``
```

You can add targets at any place in the documentation. However, if there is no heading or title for the targeted element, you must specify a link text.

To define a target, use the following syntax:

```{list-table}
:header-rows: 1

* - Format
  - Syntax
* - reST
  - `` .. _target_ID: ``
* - MyST
  - `(target_ID)=`
```

```{note}
In reST, when defining the target, you must prefix it with an underscore. Do not use the starting underscore when referencing the target.
```

To reference a target and specify a custom link text:

```{list-table}
:header-rows: 1

* - Format
  - Syntax
* - reST
  - `` :ref:`Provided link text <a_random_target>` ``
* - MyST
  - `` {ref}`link text <a_random_target_myst>` ``
```

In MyST, you can also use Markdown syntax if you need markup on the link text: `` [`xyz`](a_random_target_myst) ``

Adhere to the following conventions:

- Never use external links to reference a section in the same doc set or a doc set that
  is linked with Intersphinx. It would likely cause a broken link in the future.
- Override the link text only when it is necessary. If you can use the section title as
  link text, do so, because the text will then update automatically if the title
  changes.
- Never "override" the link text with the same text that would be generated
  automatically.

#### Referencing a page

If a documentation page does not have a target, you can still reference it. 

``````{tab-set}

`````{tab-item} reST
Use the ``:doc:`` role with the file name and path.

````{list-table}
:header-rows: 1

* - Input
  - Output
* - `` :doc:`index` ``
  - {doc}`index`
* - `` :doc:`Provided link text <index>` ``
  - {doc}`Provided link text <index>`
````
`````

`````{tab-item} MyST
Use the `{doc}` role with the file name and path.
Use MyST syntax to automatically extract the
link text. When overriding the link text, use Markdown syntax.

````{list-table}
:header-rows: 1

* - Input
  - Output
  - Status
* - `` {doc}`index` ``
  - {doc}`index`
  - Preferred.
* - `[](index)`
  - [](index)
  - Do not use.
* - `[Index page](index)`
  - [Index page](index)
  - Preferred when overriding the link text.
* - `` {doc}`Index page <index>` ``
  - {doc}`Index page <index>`
  - Alternative when overriding the link text.
````
`````

``````

Adhere to the following conventions:

- Only use the `doc` role when you cannot use the `ref` role, thus only if there is
  no target at the top of the file and you cannot add it. When using the `doc` role,
  your reference will break when a file is renamed or moved.
- Override the link text only when it is necessary. If you can use the document title as
  link text, do so, because the text will then update automatically if the title
  changes.

## Navigation

Every documentation page must be included as a sub-page to another page in the
navigation.

This is achieved with the
[`toctree`](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-toctree)
directive in the parent page:

````
```{toctree}
:hidden:

sub-page1
sub-page2
```
````

If a page should not be included in the navigation, you can mark it as orphan at the top of the page
using the following:

```{list-table}
:header-rows: 1

* - Format
  - Syntax
* - reST
  - `` :orphan: ``
* - MyST
  - ```
    ---
    orphan: true
    ---
    ```
```

Instead of hiding pages that you do not want to include in the documentation from the
navigation, you can exclude them from being built. This method will also prevent them
from being found through the search.

To exclude pages from the build, add them to the `custom_excludes` variable in the
`conf.py` file.

## Lists

``````{tab-set}

`````{tab-item} Output
- Item 1
- Item 2
- Item 3

1. Step 1
1. Step 2
1. Step 3

a. Step 1
b. Step 2
c. Step 3
`````

`````{tab-item} reST
Unordered list:

````
- Item 1
- Item 2
- Item 3
````

Numbered list:

````
1. Step 1
#. Step 2
#. Step 3
````

Alphabetical list:

````
a. Step 1
#. Step 2
#. Step 3
````
`````

`````{tab-item} MyST
Unordered list:

````
- Item 1
- Item 2
- Item 3
````

Numbered list (use `1.` for all items):

````
1. Step 1
1. Step 2
1. Step 3
````
`````

``````

You can also nest lists:

``````{tab-set}

`````{tab-item} Output
1. Step 1
   - Item 1
     * Sub-item
   - Item 2
1. Step 2
   1. Sub-step 1
   1. Sub-step 2
`````

`````{tab-item} reST
````
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
````
`````

`````{tab-item} MyST
````
1. Step 1
   - Item 1
     * Sub-item
   - Item 2
1. Step 2
   1. Sub-step 1
   1. Sub-step 2
````
`````

``````

### Definition lists

``````{tab-set}

`````{tab-item} Output
Term 1
: Definition

Term 2
: Definition
`````

`````{tab-item} reST
````
Term 1:
  Definition
Term 2:
  Definition
````
`````

`````{tab-item} MyST
````
Term 1
: Definition

Term 2
: Definition
````
`````

``````

## Tables

reST supports different markup for tables. Grid tables are most similar to tables in
Markdown, but list tables are usually much easier to use. See the `Sphinx documentation
<tables_>`_ for all table syntax alternatives.

When working with MyST, you can use standard Markdown tables. However, using the reST [list
table](https://docutils.sourceforge.io/docs/ref/rst/directives.html#list-table) syntax
is usually much easier. See the [Sphinx
documentation](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#table-directives)
for all table syntax alternatives.

Both markups result in the following output:

```{list-table}
   :header-rows: 1

* - Header 1
  - Header 2
* - Cell 1

    Second paragraph cell 1
  - Cell 2
* - Cell 3
  - Cell 4
```

### Markdown and grid tables

``````{tab-set}

`````{tab-item} Grid tables (reST)
See [grid tables](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#grid-tables) for reference.

````
+----------------------+------------+
| Header 1             | Header 2   |
+======================+============+
| Cell 1               | Cell 2     |
|                      |            |
| 2nd paragraph cell 1 |            |
+----------------------+------------+
| Cell 3               | Cell 4     |
+----------------------+------------+
````
`````

`````{tab-item} Markdown tables (MyST)
````
| Header 1                           | Header 2 |
|------------------------------------|----------|
| Cell 1<br><br>2nd paragraph cell 1 | Cell 2   |
| Cell 3                             | Cell 4   |
````
`````

``````

#### List tables

See `list tables
<https://docutils.sourceforge.io/docs/ref/rst/directives.html#list-table>`__ for
reference.

``````{tab-set}

`````{tab-item} reST
````
.. list-table::
    :header-rows: 1

    * - Header 1
      - Header 2
    * - Cell 1

        2nd paragraph cell 1
      - Cell 2
    * - Cell 3
      - Cell 4
````
`````

`````{tab-item} MyST
````
```{list-table}
   :header-rows: 1

* - Header 1
  - Header 2
* - Cell 1

    2nd paragraph cell 1
  - Cell 2
* - Cell 3
  - Cell 4
```
````
`````

``````

#### Data tables

Data can be either included in the doc source or from a file.
Both markups result in the following output:

```{csv-table}
:header-rows: 1

"Animal", "Number of legs", "Size"

"Worm", 0, "Small"
"Penguin", 2, "Medium"
"Horse", 4, "Large"
"Ant", 6, "Small"
"Octopus", 8, "Medium"
```

If you have a small amount of CSV data, you can include the data in the doc source. 

For example:

``````{tab-set}

`````{tab-item} reST
````
.. csv-table::
    :header: "Animal", "Number of legs", "Size"

    "Worm", 0, "Small"
    "Penguin", 2, "Medium"
    "Horse", 4, "Large"
    "Ant", 6, "Small"
    "Octopus", 8, "Medium"
````
`````

`````{tab-item} MyST
````
```{csv-table}
:header-rows: 1

"Animal", "Number of legs", "Size"

"Worm", 0, "Small"
"Penguin", 2, "Medium"
"Horse", 4, "Large"
"Ant", 6, "Small"
"Octopus", 8, "Medium"
```
````
`````

``````

If you have a large amount of CSV data, or the data is generated by an automated
process, you can include the data from a file. 

For example:

``````{tab-set}

`````{tab-item} reST
````
.. csv-table::
    :file: /assets/animals.csv
    :header-rows: 1
````
`````

`````{tab-item} MyST
````
```{csv-table}
:file: /reuse/animals.csv
:header-rows: 1
```
````
`````

``````

Customize the column widths, character encoding, and so on, as described in the [`csv-table` reST reference](https://docutils.sourceforge.io/docs/ref/rst/directives.html#csv-table) and
[`csv-table` MyST reference](https://mystmd.org/guide/directives#directive-csv-table).

The Sphinx Stack can also render interactive tables. See: {ref}`interactive-tables`.

## Notes

```````{tab-set}

``````{tab-item} reST
````{list-table}
:header-rows: 1

* - Input
  - Output
* - ```
    .. note::
      A note.
    ```
  - ```{note}
    A note.
    ```
* - ```
    .. warning::
      This might damage your hardware!
    ```
  - ```{caution}
    This might damage your hardware!
    ```
````
``````

``````{tab-item} MyST
`````{list-table}
:header-rows: 1

* - Input
  - Output
* - ````
    ```{note}
    A note.
    ```
    ````
  - ```{note}
    A note.
    ```
* - ````
    ```{tip}
    A tip.
    ```
    ````
  - ```{tip}
    A tip.
    ```
* - ````
    ```{important}
    Important information
    ```
    ````
  - ```{important}
    Important information.
    ```
* - ````
    ```{caution}
    This might damage your hardware!
    ```
    ````
  - ```{caution}
    This might damage your hardware!
    ```
`````
``````

```````

## Images

``````{tab-set}

`````{tab-item} reST
````{list-table}
:header-rows: 1

* - Input
  - Output
* - ```
    .. image:: https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png
    ```
  - ![Canonical logo](https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png)
* - ```
    .. figure:: https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png
      :width: 100px
      :alt: Alt text

      Figure caption
    ```
  - ```{figure} https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png
       :width: 100px
       :alt: Alt text

       Figure caption
    ```
````
`````

`````{tab-item} MyST
````{list-table}
:header-rows: 1

* - Input
  - Output
* - ```
    ![Alt text](https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png)
    ```
  - ![Alt text](https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png)
* - ````
    ```{figure} https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png
       :width: 100px
       :alt: Alt text

       Figure caption
    ```
    ````
  - ```{figure} https://assets.ubuntu.com/v1/b3b72cb2-canonical-logo-166.png
       :width: 100px
       :alt: Alt text

       Figure caption
    ```
````
`````

``````

Adhere to the following conventions:

- For local pictures, start the path with `/` (for example, `/images/image.png`).
- Use `PNG` format for screenshots and `SVG` format for graphics.
- See [Five golden rules for compliant alt
  text](https://abilitynet.org.uk/resources/digital-accessibility/five-golden-rules-compliant-alt-text)
  for information about how to word the alt text.

## Reuse

A big advantage of reST and MyST in comparison to plain Markdown is that they allow to reuse content.

### Substitution

To reuse sentences or paragraphs that have little markup and special formatting, use
[substitutions](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#substitutions).

Substitutions can be defined in the following locations:

``````{tab-set}

`````{tab-item} reST
**Globally**, in a file named ``reuse/substitutions.txt`` that is included in a
custom ``rst_epilog`` directive (see the [rst_epilog documentation](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-rst_epilog)):

```{code-block} python
:caption: "{spellexception}`conf.py`"

rst_epilog = """
    .. include:: reuse/substitutions.txt
    """
```

```{code-block} rest
:caption: "{spellexception}`reuse/substitutions.txt`"

.. |version_number| replace:: 0.1.0

.. |rest_text| replace:: *Multi-line* text
                          that uses basic **markup**.

.. |site_link| replace:: Website link
.. _site_link: https://example.com
```

**Locally**, putting the same directives in any reST file:

```{code-block} rest
:caption: "{spellexception}`index.rst`"

.. |version_number| replace:: 0.1.0

.. |rest_text| replace:: *Multi-line* text
                          that uses basic **markup**.

.. And so on
```

````{note}
Mind that substitutions can't be redefined; for instance, accidentally including a
definition twice causes an error:

```none
ERROR: Duplicate substitution definition name: "rest_text".
```
````

The definitions from the above examples are rendered as follows:

```{list-table}
:header-rows: 1

* - Input
  - Output
* - `` |version_number| ``
  - 0.1.0
* - `` |rest_text| ``
  - *Multi-line* text that uses basic **markup**.
* - `` |site_link|_ ``
  - [Website link](https://example.com)
```

```{tip}
Use substitution names that hint at the included content (for example,
``note_not_supported`` instead of ``note_substitution``).
```
`````

`````{tab-item} MyST
**Globally**, in a file named {file}`reuse/substitutions.yaml` that is loaded into the
[`myst_substitutions`](https://myst-parser.readthedocs.io/en/v0.13.5/using/syntax-optional.html#substitutions-with-jinja2)
variable in `conf.py`. Or if you have a limited amount of substitutions, enter them
directly into the `myst_substitutions` variable in `conf.py`:

```{code-block} python
:caption: "{spellexception}`conf.py`"

import os
import yaml

if os.path.exists('./reuse/substitutions.yaml'):
    with open('./reuse/substitutions.yaml', 'r') as fd:
        myst_substitutions = yaml.safe_load(fd.read())
else:
    myst_substitutions = {
        "version_number": "0.1.0",
        "formatted_text": "*Multi-line* text\n that uses basic **markup**.",
        "site_link": "[Website link](https://example.com)"
  }
```

```{code-block} yaml
:caption: "{spellexception}`reuse/substitutions.yaml`"

# Key/value substitutions to use within the Sphinx doc.
{version_number: "0.1.0",
  formatted_text: "*Multi-line* text\n that uses basic **markup**.",
  site_link: "[Website link](https://example.com)"}

```

**Locally**, putting the definitions at the top of a single file in the following
format:


````
---
myst:
  substitutions:
    version_number: "0.1.0"
    formatted_text: "*Multi-line* text
                      that uses basic **markup**."
    advanced_reuse_key: "This is a substitution that includes a code block:
                        ```
                        code block
                        ```"
---
````

You can combine both options by defining a default substitution in
`reuse/substitutions.py` and overriding it at the top of a file.

The definitions from the above examples are rendered as follows:

```{list-table}
   :header-rows: 1

* - Input
  - Output
* - `{{version_number}}`
  - {{version_number}}
* - `{{formatted_text}}`
  - {{formatted_text}}
* - `{{site_link}}`
  - {{site_link}}
* - `{{advanced_reuse_key}}`
  - {{advanced_reuse_key}}
```
`````

``````

Please note:

- Substitutions do not work on GitHub. Therefore, use substitution names that indicate
  the included content (for example, `note_not_supported` instead of `reuse_note`).

### File inclusion

To reuse longer sections or text with more advanced markup, you can put the content in a
separate file and include the file or parts of the file in several locations.

To select parts of the text in a file, use `:start-after:` and `:end-before:` if
possible. You can combine those with `:start-line:` and `:end-line:` if required (if the
same text occurs more than once). Using only `:start-line:` and `:end-line:` is
error-prone though.

You cannot put any targets into the content that is being reused (because references to
this target would be ambiguous then). You can, however, put a target right before
including the file.

By combining file inclusion and substitutions, you can even replace parts of the
included text.

`````{list-table}
:header-rows: 1

* - Format
  - Syntax
* - reST
  - ```
    .. include:: index.rst
       :start-after: Also see the following information:
       :end-before: Contents
    ```
* - MyST
  - ````
    % Include parts of the content from
    % file rst-syntax.rst
    ```{include} rst-syntax.rst
        :start-after: "Adhere to the following conventions:"
        :end-before: "  Use the ones specified above."
    ```
    ````
`````

Please note:

- File inclusion does not work on GitHub. Therefore, always add a comment linking to the
  included file.
- Files that only contain text that is reused somewhere else should be placed in the
  `reuse` directory and end with the extension ``.txt`` to distinguish them from
  normal content files.
- To make sure inclusions don't break, consider adding HTML comments (`<!-- some comment
  -->`) to the source file as markers for starting and ending.

## Tabs

The recommended way of creating tabs is to use the tabs that the [Sphinx
design](https://sphinx-design.readthedocs.io/en/latest/) extension provides.

```````{tab-set}

``````{tab-item} reST
````
.. tab-set::

  .. tab-item:: Tab 1
      :sync: key1

      Content Tab 1

  .. tab-item:: Tab 2
      :sync: key2

      Content Tab 2
````
``````

``````{tab-item} MyST
`````
````{tab-set}

```{tab-item} Tab 1
:sync: key1

Content Tab 1
```

```{tab-item} Tab 2
:sync: key2

Content Tab 2
```

````
`````
``````

```````

## Glossary

You can define glossary terms in any file. Ideally, all terms should be collected in one
glossary file though, and they can then be referenced from any file.

```````{tab-set}

``````{tab-item} reST
````{list-table}
:header-rows: 1

* - Input
  - Output
* - ```
    .. glossary::

      an example term
        Definition of an example term.
    ```
  - ```{glossary}

    an example term
      Definition of an example term.
    ```
* - `` :term:`an example term` ``
  - {term}`an example term`
````
``````

``````{tab-item} MyST
`````{list-table}
:header-rows: 1

* - Input
  - Output
* - ````

    ```{glossary}

    MyST example term
      Definition of the example term.
    ```

    ````

  - ```{glossary}

    MyST example term
      Definition of the example term.
    ```

* - ``{term}`MyST example term` ``
  - {term}`MyST example term`
`````
``````

```````

## versionadded

This directive can be used to distinguish between different versions.

`````{list-table}
:header-rows: 1

* - Output
  - reST
  - MyST
* - ```{versionadded} X.Y
    ```
  - `` .. versionadded:: X.Y ``
  - ````
    ```{versionadded} X.Y
    ```
    ````
`````

## Line breaks

This feature includes line breaks that are not paragraphs. Use this sparingly.

```{list-table}
:header-rows: 1

* - Output
  - reST
  - MyST
* - Line 1<br>Line 2<br>Line 3
  - ```
    | Line 1
    | Line 2
    | Line 3
    ```
  - N/A
```

## Horizontal line

They visually divide sections on a page.

```{list-table}
:header-rows: 1

* - reST
  - MyST
* - `` ---- ``
  - `` --- ``
```

## Comments

They are not visible in the output.

```{list-table}
:header-rows: 1

* - reST
  - MyST
* - `` .. This is a comment ``
  - `` <!-- This is a comment --> ``
```

## Abbreviation

Hover to display the full term.

```{list-table}
:header-rows: 1

* - Output
  - reST
  - MyST
* - {abbr}`API (Application Programming Interface)`
  - `` :abbr:`API (Application Programming Interface)` ``
  - `` {abbr}`API (Application Programming Interface)` ``
```

## Spell exception

Explicitly exempt a term from the spelling check.

```{list-table}
:header-rows: 1

* - Output
  - reST
  - MyST
* - {spellexception}`PurposelyWrong`
  - `` :spellexception:`PurposelyWrong` ``
  - `` {spellexception}`PurposelyWrong` ``
```

