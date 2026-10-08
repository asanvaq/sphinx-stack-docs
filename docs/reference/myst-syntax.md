---
relatedlinks: https://github.com/canonical/canonical-sphinx-extensions, [reStructuredText&#32;Primer](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html), [Canonical&#32;Documentation&#32;Style&#32;Guide](https://docs.ubuntu.com/styleguide/en)
myst:
  html_meta:
    description: Reference for the MyST syntax conventions used by Canonical.
  substitutions:
    advanced_reuse_key: "This is a substitution that includes a code block:
                       ```
                       code block
                       ```"
---

(myst-syntax)=

# MyST syntax

The Sphinx Stack supports [MyST Markdown](https://commonmark.org/) and
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

* - Input
  - Description
* - `# Title`
  - Page title and H1 heading
* - `## Heading`
  - H2 heading
* - `### Heading`
  - H3 heading
* - `#### Heading`
  - H4 heading
* - ...
  - Further headings
```

## Inline formatting

```{list-table}
   :header-rows: 1

* - Input
  - Output
* - `` {guilabel}`UI element` ``
  - {guilabel}`UI element`
* - `` `code` ``
  - `code`
* - `` {file}`file path` ``
  - {file}`file path`
* - `` {command}`command` ``
  - {command}`command`
* - `` {kbd}`Key` ``
  - {kbd}`Key`
* - `*Italic*`
  - *Italic*
* - `**Bold**`
  - **Bold**

```

## Code blocks

Start and end a code block with three back ticks:

    ```

`````{list-table}
   :header-rows: 1

* - Input
  - Output
* - ````

    ```
    # Demonstrate a code block
    code:
    - example: true
    ```

    ````

  - ```
    # Demonstrate a code block
    code:
    - example: true
    ```
* - ````

    ```yaml
    # Demonstrate a code block
    code:
    - example: true
    ```

    ````

  - ```yaml
    # Demonstrate a code block
    code:
    - example: true

    ```

`````

To include back ticks in a code block, increase the number of surrounding back ticks:

`````{list-table}
   :header-rows: 1

* - Input
  - Output
* -
    `````

    ````
    ```
    ````

    `````

  -
    ````

    ```

    ````

`````

### Terminal output

A terminal view emulates the command line experience more accurately than a code block.
This is particularly useful in tutorials or guides that are terminal-heavy, or where
it's helpful to customize the user/directory each command is run from.

To show a terminal view, use the following directive:

`````{list-table}
   :header-rows: 1

* - Input
  - Output
* - ````text
    ```{terminal}

    input line 1
    input line 2

    output line 1
    output line 2

    output line 3
    ```
    ````
  - ```{terminal}

    input line 1
    input line 2

    output line 1
    output line 2
    
    output line 3
    ```
`````

By default, everything before the first blank line in the directive's content is
rendered as input, and any content that follows is rendered as output. The terminal
directive can only display one input command, but that command can span multiple lines,
as in the previous example.

To render only the output of a command, include the `:output-only:` flag as a directive
option:

`````{list-table}
   :header-rows: 1

* - Input
  - Output
* - ````text
    ```{terminal}
    :output-only:

    This is rendered as output.
    ```
    ````
  - ```{terminal}
    :output-only:

    This is rendered as output.
    ```
`````

To customize the prompt (`user@host:~$` by default), specify any of the following options:

* `:user:`
* `:host:`
* `:dir:`

`````{list-table}
   :header-rows: 1

* - Input
  - Output
* - ````text
    ```{terminal}
    :user: author
    :host: canonical
    :dir: ~/path

    input

    output
    ```
    ````
  - ```{terminal}
    :user: author
    :host: canonical
    :dir: ~/path

    input

    output
    ```
`````

The copy button for input commands is **opt-in**. You must include the `:copy:` flag
in the directive's options for the button to be displayed.

To make the terminal scroll horizontally instead of wrapping long lines, include the
`:scroll:` option.

For more details, refer to the [`sphinx-terminal`
README](https://github.com/canonical/sphinx-terminal/blob/main/README.md).

## Links

Link markup depends on whether you need an external URL or a page in the same
documentation set.


### External links

To link to documents in other Sphinx projects, use {ref}`Intersphinx <how-to-link-docs-intersphinx>` with the `{ref}` or `{doc}` role:

```{list-table}
:header-rows: 1

* - Input
  - Output
* - `` {external+ubuntu-desktop:doc}`index` ``
  - {external+ubuntu-desktop:doc}`index`
* - `` {external+ubuntu-desktop:ref}`install-ubuntu-desktop` ``
  - {external+ubuntu-desktop:ref}`install-ubuntu-desktop`
```

For external links, use Markdown syntax. You can also use just the URL, but this will usually cause issues with the spelling check, so you should specify the link text as code in this case.

```{list-table}
:header-rows: 1

* - Input
  - Output
* - `[Canonical website](https://canonical.com)`
  - [Canonical website](https://canonical.com)
* - `https://canonical.com`
  - [{spellexception}`https://canonical.com`](https://canonical.com)
* - ``[`https://canonical.com`](https://canonical.com)``
  - [`https://canonical.com`](https://canonical.com)
```


#### Plain text

If you need a link rendered as a plain text, escape the colon in the protocol:

```{list-table}
:header-rows: 1

* - Input
  - Output
* - https\\://canonical.com/
  - {spellexception}`https://canonical.com/`
```

#### Related

You can add links to related websites or Discourse topics to the sidebar.

To add a link to a related website, add the following to the page's Markdown front
matter:

```
---
relatedlinks: https://github.com/canonical/lxd-sphinx-extensions, [RTFM](https://www.google.com)
---
```

If you override the title, note that spaces are ignored; if you need spaces in the title, replace them with `&#32;`, and include the value in quotes if Sphinx complains about the metadata value because it starts with `[`.
For example: `[My&#32;Title](https://...)`.

To add a link to a Discourse topic, configure the Discourse instance in the
:file:`conf.py` file. Then add the following field to the page's Markdown front matter:

```
---
discourse: <topic-id>
---
```


#### Manual pages

When mentioning command line utilities, you may wish to link to the corresponding manual
page for the command. Ensure that the `manpages_url` setting in your {file}`conf.py` is
set appropriately and use the `{manpage}` role within your text to create a link.

For example, to link to man pages from the 24.04 LTS (Noble Numbat) release, include the
following in your {file}`conf.py`:

```python
    manpages_url = "https://manpages.ubuntu.com/manpages/noble/en/man{section}/{page}.{section}.html"
```

Then within the document:

```md
You can use the {manpage}`dd(1)` utility to write the disk image to your
SD card. If the image is compressed, use {manpage}`aunpack(1)` to extract
it first.
```

#### YouTube

To add a link to a YouTube video, use the following directive:

`````{list-table}
:header-rows: 1

* - Input
  - Output
* - ````markdown
      ```{youtube} https://www.youtube.com/watch?v=iMLiK1fX4I0
          :title: Demo
      ```
    ````
  - ````{youtube} https://www.youtube.com/watch?v=iMLiK1fX4I0
        :title: Demo
    ````
`````

The video title is extracted automatically and displayed when hovering over the link. To
override the title, add the `{title}` option.


### Internal references


(a_section_label_myst)=
#### Sections

To reference a section within the documentation (either on the same page or on another
page), add a label to that section and reference that label.

You can add a label anywhere in any document. When referencing a label that isn't
attached to a heading, you must add link text. If you don't, the reference won't work.

(a_random_label_myst)=

```{list-table}
:header-rows: 1
:widths: 7 3 3

* - Input
  - Output
  - Description
* - `(label_ID)=`
  -
  - Adds the label ``label_ID``.
* - `` {ref}`a_section_label_myst` ``
  - {ref}`a_section_label_myst`
  - References a label that has a title.
* - `` {ref}`link text <a_random_label_myst>` ``
  - {ref}`link text <a_random_label_myst>`
  - References a label and specifies a title.
* - ``[`xyz`](a_random_label_myst)``
  - [`xyz`](a_random_label_myst)
  - Use Markdown syntax if you need markup on the link text.
```


#### Pages

If a documentation page does not have a label, you can still reference it by using the
`{doc}` role with the file name and path.

```{list-table}
:header-rows: 1
:widths: 8 2

* - Input
  - Output
* - `` {doc}`index` ``
  - {doc}`index`
* - `` {doc}`Provided link text <index>` ``
  - {doc}`Provided link text <index>`
```

Only use the `{doc}` role when you cannot use the `{ref}` role, thus only if there
is no label at the top of the file and you cannot add it. When using the `{doc}`
role, your reference will break when a file is renamed or moved.



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

If a page should not be included in the navigation, you can suppress the resulting build
warning by putting the following instruction at the top of the file:

```
---
orphan: true
---
```

Use orphan pages sparingly and only if there is a clear reason for it.

```{tip}
Instead of hiding pages that you do not want to include in the documentation from the
navigation, you can exclude them from being built. This method will also prevent them
from being found through the search.

To exclude pages from the build, add them to the `exclude_patterns` variable in the
`conf.py` file.
```

## Lists

````{list-table}
   :header-rows: 1

* - Input
  - Output
* - ```
    - Item 1
    - Item 2
    - Item 3
    ```
  - - Item 1
    - Item 2
    - Item 3
* - ```
    1. Step 1
    1. Step 2
    1. Step 3
    ```
  - 1. Step 1
    1. Step 2
    1. Step 3
* - ```
    1. Step 1
       - Item 1
         * Sub-item
       - Item 2
    1. Step 2
       1. Sub-step 1
       1. Sub-step 2
    ```
  - 1. Step 1
       - Item 1
         * Sub-item
       - Item 2
    1. Step 2
       1. Sub-step 1
       1. Sub-step 2
````

In numbered lists, use `1.` for all items to generate the step numbers automatically.
You can also use a higher number for the first item to start with that number.

Use `-` for unordered lists. When using nested lists, you can use `*` for the nested level.

### Definition lists

````{list-table}
   :header-rows: 1

* - Input
  - Output
* - ```
    Term 1
    : Definition

    Term 2
    : Definition
    ```
  - Term 1
    : Definition

    Term 2
    : Definition
````

(myst_style_guide_tables)=

## Tables

You can use standard Markdown tables. However, using the reST [list
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

### Markdown tables

```
| Header 1                           | Header 2 |
|------------------------------------|----------|
| Cell 1<br><br>2nd paragraph cell 1 | Cell 2   |
| Cell 3                             | Cell 4   |
```

### List tables

See [list
tables](https://docutils.sourceforge.io/docs/ref/rst/directives.html#list-table) for
reference.

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

### Data tables

If you have a small amount of CSV data, you can include the data in the doc source. 

For example:

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

If you have a large amount of CSV data, or the data is generated by an automated
process, you can include the data from a file. 

For example:

````
```{csv-table}
:file: /reuse/animals.csv
:header-rows: 1
```
````

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

Customize the column widths, character encoding, and so on, as described in the 
[`csv-table` reference](https://mystmd.org/guide/directives#directive-csv-table).

The Sphinx Stack can also render interactive tables. See: {ref}`interactive-tables`.

## Notes

````{code-block} markdown
```{admonition} <title>
:class: <class>
```
````

## Images

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

For local pictures, start the path with `/` (for example, `/images/image.png`).

Use `PNG` format for screenshots and `SVG` format for graphics.

See [Five golden rules for compliant alt
  text](https://abilitynet.org.uk/resources/digital-accessibility/five-golden-rules-compliant-alt-text)
  for information about how to word the alt text.

## Reuse

A big advantage of MyST in comparison to plain Markdown is that it allows to reuse content.

### Substitution

To reuse sentences or paragraphs that have little markup and special formatting, use
[substitutions](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#substitutions).

Substitutions can be defined in the following locations:

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

Adhere to the following convention:

- Substitutions do not work on GitHub. Therefore, use substitution names that indicate
  the included content (for example, `note_not_supported` instead of `reuse_note`).

### File inclusion

To reuse longer sections or text with more advanced markup, you can put the content in a
separate file and include the file or parts of the file in several locations.

To select parts of the text in a file, use `:start-after:` and `:end-before:` if
possible. You can combine those with `:start-line:` and `:end-line:` if required (if the
same text occurs more than once). Using only `:start-line:` and `:end-line:` is
error-prone though.

You cannot put any labels into the content that is being reused (because references to
this label would be ambiguous then). You can, however, put a label right before
including the file.

By combining file inclusion and substitutions, you can even replace parts of the
included text.

`````{list-table}
     :header-rows: 1

* - Input
  - Output
* - ````
    ```{include} /how-to/index.rst
        :start-after: .. _how-to-guides:
        :end-before: =============
    ```
    ````

  -
    ```{include} /how-to/index.rst
        :start-after: .. _how-to-guides:
        :end-before: =============
    ```

`````

File inclusion does not work on GitHub. Therefore, always add a comment linking to the
  included file.

Files that only contain text that is reused somewhere else should be placed in the
  `reuse` directory and end with the extension ``.txt`` to distinguish them from
  normal content files.

To make sure inclusions don't break, consider adding HTML comments (`<!-- some comment
  -->`) to the source file as markers for starting and ending.

## Tabs

The recommended way of creating tabs is to use the tabs that the [Sphinx
design](https://sphinx-design.readthedocs.io/en/latest/) extension provides.

``````{list-table}
   :header-rows: 1

* - Input
  - Output
* - `````

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

  - ````{tab-set}

    ```{tab-item} Tab 1
    :sync: key1

    Content Tab 1
    ```

    ```{tab-item} Tab 2
    :sync: key2

    Content Tab 2
    ```
    ````
``````

Alternatively, you can use the [Sphinx
tabs](https://sphinx-tabs.readthedocs.io/en/latest/) extension, which is also enabled by
default. This was previously recommended due to limitations in Sphinx Design that are
now fixed.

``````{list-table}
   :header-rows: 1

* - Input
  - Output
* - `````

    ````{tabs}

    ```{group-tab} Tab 1

    Content Tab 1
    ```

    ```{group-tab} Tab 2

    Content Tab 2
    ```

    ````

    `````

  - ````{tabs}

    ```{group-tab} Tab 1

    Content Tab 1
    ```

    ```{group-tab} Tab 2

    Content Tab 2
    ```
    ````
``````

## Collapsible sections

There is no support for details sections in MyST, but you can insert HTML to create
them.

````{list-table}
   :header-rows: 1

* - Input
  - Output
* - ```
    <details>
    <summary>Details</summary>

    Content
    </details>
    ```

  - <details>
    <summary>Details</summary>

    Content
    </details>

````

## Glossary

You can define glossary terms in any file. Ideally, all terms should be collected in one
glossary file though, and they can then be referenced from any file.

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

## More useful markup

`````{list-table}
   :header-rows: 1

* - Input
  - Output
  - Description
* - ````

    ```{versionadded} X.Y
    ```

    ````
  - ```{versionadded} X.Y
    ```
  - Can be used to distinguish between different versions.
* - ```
    ---
    ```
  - A horizontal line
  - Can be used to visually divide sections on a page.
* - ```
    <!-- This is a comment -->
    ```
  - <!-- This is a comment -->
  - Not visible in the output.
* - ```
    {abbr}`API (Application Programming Interface)`
    ```
  - {abbr}`API (Application Programming Interface)`
  - Hover to display the full term.
* - ```
    {spellexception}`PurposelyWrong`
    ```
  - {spellexception}`PurposelyWrong`
  - Explicitly exempt a term from the spelling check.

`````
