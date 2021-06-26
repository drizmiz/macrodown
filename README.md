
# macrodown

Enhance your [pandoc](https://pandoc.org) markdown with macros enabled. 

Abbreviations are good. The pandoc extension `latex_macros` works good, but LaTeX definitions do not look good in markdown files. It does not support LaTeX arguments either. The already-exist pandoc filter [pandoc-abbreviations](https://github.com/scokobro/pandoc-abbreviations) needs a `+` that is unacceptable in math environments.

**macrodown** is in development and currently covers nothing but this documentation. The syntax and usage described below is draft-only.

## Syntax

All the `[identifier]` and `[content]` below can theoretically be any UTF-8 characters except the space `U+0020`. If you want them to include spaces, use `"` to wrap them.

### Define macros

#### Define simple macros

```
%#def [type](*) [identifier] [content]
```

`type` each corresponds to LaTeX:

`macro`: `\def`

`lmacro`: `\let`

`cmd`: `\newcommand`

`recmd`: `\renewcommand`

`pvdcmd`: `\providecommand`

`mathop`: `\DeclareMathOperator`

If there is no pairing, **macrodown** will ignore the `type` variable and take `\newcommand`. The corresponding relations can be personalized in `settings.yaml`.

If the output format is not LaTeX, `macrodown` will do the replacements itself.

#### Define macros with arguments

```
%#deff [identifier] [content]
```

**macrodown** parses the arguments the same as LaTeX does. Use `#1`, ..., `#9` to mark them in `[content]` and `\#` for the raw `#`.

### Choose the environment

```
%#if (not) [text-condition|output-condition]
%#else
%#endif
```

`text-condition` includes `math`, `text`, `code` and `all`.

`output-condition` includes `latex`, `html` and `all`. 

Bool algebra support is now not desired by **macrodown**.

### Undefine macros and Condition on definition

```
%#undef [identifier]
```

```
%#ifdef [identifier]
%#else
%#endif
```

The meanings are obvious.

### Exclude paragraphs manually

```
%#exclude (if (not) text-condition|output-condition)
%#endex
```

It is basically:

```
%#if (not) text-condition|output-condition
%#undef everything

%#endif
%#def everything back
```

(Of course, you cannot use `everything` this way yourself.)

## Usage

**macrodown** is not a pandoc filter. It does call `pandoc` for the [AST](https://pandoc.org/filters.html), but it has to do something before that.

### Help

```bash
python -m main.py [OPTIONS] [pandoc_args] [input_file]
python -m main.py --help
```

OPTIONS:

```bash
-f --format=FORMAT
```
FORMAT can be md, latex, html and others.

```bash
-r --replace
```
Set options `--output=input_file --format=md`. Please be careful to backup your file in case that **macrodown** or pandoc does something bad to it.

```bash
-R --raw
```

If this option is set, **macrodown** will try to parse your document itself in order to keep the original structure of it, but inevitably gets more bugs. In this way, pandoc will not be called and thus `[pandoc_args]` is not available. This option sets `--format=md`.

### Working with rmarkdown

**macrodown** just works as well in **rmarkdown**. See `macrodown.R` for more details.
