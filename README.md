# logcheck-templates
Regex templating tool to simplify logcheck rule creation. Requires gpp.

Clone into `/etc/logcheck/templates`:

    git clone https://github.com/andrewgdotcom/logcheck-templates /etc/logcheck/templates

To use, execute `./build` inside the templates directory.
For each file `/etc/logcheck/templates/<dir>/<file>`, this will substitute
the macros and place the results in `/etc/logcheck/<dir>/<file>`.

Macros are Latex-style (strictly speaking, `gpp -T`), i.e. `\macro{argument}`.
This style was chosen so that most regexes will pass through the macro
expander unchanged - hovever there are a couple of caveats:

1. The quote character is `@`, so literal `@` signs must be doubled.
2. Normally, undefined macros evaluate to themselves, allowing `\w` etc to 
    pass through unscathed. However, if the subsequent text looks like an
    argument (e.g. `\w{3}`) it is silently eaten, producing `\w`.
    To explicitly suppress this, prefix the backslash with `@`: `@\w{3}`
3. Macro names continue to the next word boundary. To separate a macro name
    from any subsequent word characters, quote the next character with `@`.
    e.g. `\int@s` will match `20s`, but `\ints` will not.

