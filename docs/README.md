# mdBook preprocessor for simple text replacements

## Overview

[mdBook](https://github.com/rust-lang/mdBook) preprocessor that simply replaces text in all chapters.

Phrases to be replaced with specified content are defined in plain-text configuration file.

## Installation

Install using Cargo:

```shell
cargo install mdbook-yapp
```

Configure this preprocessor by adding to your `book.toml` the following line:

```toml
[preprocessor.yapp]
```

Build you book as usual:

```shell
mdbook build
```

There should be a warning message displayed when no configuration file is found.

```text
2023-11-11 12:01:02 [INFO] (mdbook::book): Book building has started
[WARNING][Yapp] configuration file not found, in current directory expected a file with the name starting with prefix: yapp
[WARNING][Yapp] configuration file not found, in current directory expected a file with the name starting with prefix: yapp
2023-11-11 12:01:03 [INFO] (mdbook::book): Running the html backend
```

Prepare the configuration file as described in the next section.

## Configuration

This preprocessor requires single configuration file in plain-text format.
This file's name should start with the prefix `yapp`. Letter case is not significant.
So names like `yapp`, `Yapp`, `Yappi`, `yapp.config` and similar will do.

Configuration file must contain pairs of lines of text.
First line is the phrase to be searched in the chapter and the second line is the replacement.
Having a configuration file named `yapp.config` with the following content:

```text
jd
John Doe
```

will inform this preprocessor to search for all `jd` instances in all chapters of the book
and to replace them with the text `John Doe`.

Configuration files may have empty lines, which are ignored.
Empty lines may make the configuration more readable when there are multiple replacements defined, like this:

```text
jd
John Doe

^note
**Note**:

@version
1.23.4
```

Replacements are done in the order defined in configuration file, so the substitutions may be chained, like this:

```text
a
ab

ab
abb

abb
abba

abba
ABBA
```

Every letter `a` in all chapters will be replaced with `ABBA`.

Note that each line in configuration file is trimmed before used. So configuration file with the content like this:

```text
a
    ab

         ab
 abb

   abb
                     abba

 abba
   ABBA
```

will have the same effect as the file in the previous example.

To preserve whitespaces in search pattern or replacement, enclose it in a single quotation mark:

```text
' a '
' b c '
```

or double quotation mark:

```text
" a "
" b c "
```

in this case, the result of replacement for input:

```text
│ a │
```

will be:

```text
│ b c │
```
