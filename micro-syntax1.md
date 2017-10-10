# Language highlighting for beginner

For a quick overview read the documentation from micro  [colors](https://github.com/zyedidia/micro/blob/master/runtime/help/colors.md)

This text gives a simple introduction how to build a syntax highlighting file for micro.
The examples of this text cut together gives a template for your own definitions.

Before you start you might need a list of keywords for the language you want to define. One option is to find and copy a syntax file from an other editor.However it can also be helpful to just add a few keywords, edit the symbols, get the comments right and be ready.

You can  do many more clever things for your language syntax than is described in this text, but after reading this text you will be able to create a syntax file already.

Micro syntax files are defined following the [YAML](https://en.wikipedia.org/wiki/YAML) configuration syntax. To create a new syntax file create a new file with the ending `.yaml`.  
To add your own syntax definition you have to add this file in your home folder into  `~/.config/micro/syntax`.

## File header
To start some general information are needed. Most importantly the language you want to describe gets defined with `filetype:`

The filetype gets detected by the file ending described with `detect:`. For advanced users it is also possible to check the first line of the file to determine the filetype. Replace the  values in the example with the relevant name and file ending for your new language.

With the `rules:` keyword the actual language description gets started. Please mind that indentation is important. Everything after `rules:` has to be indented.

e.g.
``` Yaml
filetype: Test-lang

detect:
    filename: "\\.tst$"

rules:
```

## Language keywords
In this section you can describe the language keywords. The syntax looks a bit complex at first but you can mostly copy and paste it. There are three section:
- `-type :` defines the types of the language
- `statement:` defines the keywords of the language. YOu can cram all keywords in one statement or split them up on several statements.
- `special:` can be used to highlight certain keywords of a language in a different color to the othe rkeywords. In Ocaml the function definition is worht a special highlight. In Elixir the `do` and `end` could be useful to highlight. It is down to you if you want to use it at all and for which keywords.

For each of the above sections a string is defined with the relevant keywords: 
e.g. `"\\b(?i:(int|float|bool|char|string|unit))\\b"`. To define your own profile replace the part in the inner brackets () with your own keywords.

e.g.
``` Yaml
    - type: "\\b(?i:(int|float|bool|char|string|unit))\\b"
    
    - statement: "\\b(if|then|else)\\b"
    - statement: "\\b(for|and|or)\\b"

    - special: "\\b(let|rec|function|in)\\b"
```
## Operators, symbols, strings, comments


### Symbols and operators
Symbols are used in many languages for different thing. That said most languages use similar symbols, so from the highlighting perspactive there is not so much difference. Below list of symbols gives you some common symbols and operators. These can be extended for you r language. There is no need to remove unused symbols. They just do not get used.

e.g.
``` Yaml
    - symbol: "(\\||@|!|:|::|_|~|=|\\\\|;|\\(\\)|||\\[|\\]|\\{|\\})"

    - symbol.operator: "(==|/=|&&|\\|\\||<|>|<=|>=)"
```

### Language constants
Language constants means words like `true` that are no statements and cannot be changed. Depending on the language there can be other constants.

e.g.
``` Yaml
    - constant: "(Nothing|Just|LT|EQ|GT)"

    - constant.bool: "\\b(true|false)\\b"
```

### Strings

Strings in most languages are within within quotation marks " " and characters are within singel quote ' '. In this case you can just use the example below. The additional rule is to show additional controls in the string. e.g "Hallo /n" If your languages does not have that you can also write `rules: []`

e.g.
``` Yaml
    - constant.string:
        start: "\""
        end: "\""
        skip: "\\\\."
        rules:
            - constant.specialChar: "\\\\."

    - constant.string:
        start: "'"
        end: "'"
        skip: "\\\\."
        rules:
            - constant.specialChar: "\\\\."
```

### Comments
Most languages have different symbols for single line and multi liine comments.

e.g. for a multi line comment (*...*)
``` Yaml
    - comment:
        start: "\\(\\*"
        end: "\\*\\)"
        rules:
            - todo: "(TODO|XXX|FIXME):?"
```

e.g. for a single line comment after # ...
``` Yaml
    - comment:
        start: "#"
        end: "$"
        rules: []
```

To keep the overview in your syntax file you can and should add some comments. Single line comments in a `.yaml` file start with `#`. There are no multi line comments.

e.g. for a single line comment with a comment
``` Yaml
    # single line comment # ...
    - comment:
        start: "#"
        end: "$"
        rules: []
```


The full example with comments can be found [here](https://github.com/MarcusE1W/hello-world/blob/master/test-syntax.yaml)
