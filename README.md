# courtdoc

Some experimental work on using Pandoc, LaTeX and Markdown to create court
documents from version-controllable plain text sources.

## Examples
* [Court order](order.pdf)
* [Outline of submissions](subs.pdf)

## Invocation

Build the Markdown document in `foo.md`:

    > ./build foo

You'll either get a nice `foo.pdf`, or an incomprehensible LaTeX error message.

## Dependencies

`build` is just a short shell script which calls `pandoc` and `xelatex`, so
you'll need those on your `PATH`.

You can modify `build` so that `pandoc` is responsible for choosing its own
LaTeX engine, but I wanted to use nice fonts and couldn't get pandoc to call
XeTeX.

## Writing your document (Markdown + YAML)

Use `order.md` or `subs.md` as a starting point. You can write anything that's
supported in Pandoc-flavoured Markdown, including any extensions enabled in the
`build` script.  I'm using `yaml_metadata_block`, `citations` and `footnotes`.

## Modifying metadata (YAML)

All documents are assumed to be court documents relating to the same action, defined
in `header.yaml`. The structure of this YAML file is pretty self-explanatory; Pandoc
feeds this YAML to the data you add in the block at the top of your Markdown file into
`courtdoc.template.tex`, so the allowable keywords are all variables in that template.

## Modifying the template (LaTeX)

The template is defined in `courtdoc.template.tex` and is based on the style
used in Western Australia. You can see how the keys in `header.yaml` and the
YAML metadata block at the start of each document are fed into variables like
`$foo$`. There are also a few special template keywords like `$if$`, `$for$`
and `$sep$`. This code isn't really LaTeX, it's a
[little-documented](http://pandoc.org/MANUAL.html#using-variables-in-templates)
Pandoc DSL which is compiled to an intermediate `.tex` file before `build`
invokes `xelatex`.

This templating language is fairly limited. A more sophisticated system might
involve Haskell scripts which parse custom Markdown features using
`Text.Pandoc.JSON`. See the [Pandoc scripting
documentation](http://pandoc.org/scripting.html).
