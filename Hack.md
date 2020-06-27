# Hack

Hackish tips and snippets.

## Single script to run in both Windows batch and Linux Bash

### Single line script

Use CMD label trick:

- The label character, a colon (`:`), is equivalent to `true` in most POSIXish shells
- CMD will ignore lines that start with `:` (label character)

```bash
:; echo "Hi, I’m ${SHELL}."; exit $?
@ECHO OFF
ECHO I'm %COMSPEC%
```

Don’t forget that any use of `$?` must be before your next colon `:` because `:` resets `$?` to 0.

### Multi line script

Use heredoc trick:

```bash
:; echo "I am ${SHELL}"
:<<"::CMDLITERAL"
ECHO I am %COMSPEC%
::CMDLITERAL
:; echo "And ${SHELL} is back!"
:; exit
ECHO And back to %COMSPEC%
```

Use heredoc with GOTO trick:

```bash
:<<"::CMDLITERAL"
@ECHO OFF
GOTO :CMDSCRIPT
::CMDLITERAL

echo "I can write free-form ${SHELL} now!"
if :; then
  echo "This makes conditional constructs so much easier because"
  echo "they can now span multiple lines."
fi
exit $?

:CMDSCRIPT
ECHO Welcome to %COMSPEC%
```

### Universal comment

Universal comments, of course, can be done with the character sequence `: #` or `:;#`. The space or semicolon are necessary because `sh` considers `#` to be part of a command name if it is not the first character of an identifier.

```bash
: # This is a special script which intermixes both sh
: # and cmd code. It is written this way because it is
: # used in system() shell-outs directly in otherwise
: # portable code. See https://stackoverflow.com/questions/17510688
: # for details.
:; echo "This is ${SHELL}"; exit
@ECHO OFF
ECHO This is %COMSPEC%
```

### reference

- https://stackoverflow.com/a/17623721