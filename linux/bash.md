# bash script

## Unofficial Strict Mode

```bash
#!/bin/bash
set -euo pipefail
#    ││└─ 'set -o pipefail' will cause a pipeline to return a failure status 
#    ││   if any command fails.
#    │└ Treat unset variables and parameters other than the special parameters
#    │  ‘@’ or ‘*’ as an error when performing parameter expansion.
#    └ Exit immediately if a pipeline, which may consist of a single 
#      simple command, a list, or a compound command returns a non-zero status. 
#
# SEE: https://www.gnu.org/software/bash/manual/bashref.html#The-Set-Builtin
```

## Variable name

Uppercase only variable names are not recommanded.

ref: http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap08.html paragraph 4

> Environment variable names used by the utilities in the Shell and Utilities volume of POSIX.1-2017 consist solely of uppercase letters, digits, and the \<underscore\> ( '_' ) from the characters defined in Portable Character Set and do not begin with a digit.
> The name space of environment variable names containing lowercase letters is reserved for applications.

## Paremter expansion

ref: http://www.gnu.org/software/bash/manual/bash.html#Shell-Parameter-Expansion

```
${parameter:-word}

    If parameter is unset or null, the expansion of word is substituted. Otherwise, the value of parameter is substituted.

${parameter:=word}

    If parameter is unset or null, the expansion of word is assigned to parameter. The value of parameter is then substituted. Positional parameters and special parameters may not be assigned to in this way.

${parameter:?word}

    If parameter is null or unset, the expansion of word (or a message to that effect if word is not present) is written to the standard error and the shell, if it is not interactive, exits. Otherwise, the value of parameter is substituted.

${parameter:+word}

    If parameter is null or unset, nothing is substituted, otherwise the expansion of word is substituted.

...

```

## Check environment variable exists

```bash
if [ "${VARIABLE:-x}" == "x" ]; then
#               └─ if $VARIABLE is unset or null, set 'x' as default value.
  echo 'variable is unset or null'
fi
```

## check environment variable value

```bash
if [ "$variable" == "value" ]
then
    # blahblah
fi
```

## check directory exists

```bash
if [ ! -d "/path/to/check" ]
then
    echo 'path is not exist!'
fi
```

## check file exists

```bash
if [ ! -f /path/to/check ]
then
    echo 'file is not exist'
fi
```

## check command exists

```bash
if ! which docker > /dev/null
then
    echo 'docker command not exist'
fi
```

```bash
function has () {
    # Or type "$1" &> /dev/null
    type "$1" > /dev/null 2>&1
}

if has docker; then
    echo 'docker command exist'
fi
```

## check process is running

```bash
if pgrep ssh > /dev/null; then
    echo 'ssh is running'
fi
```

## pass environment variable

```bash
export ENV=xxx
command blahblah
unset ENV
```

or

```bash
ENV=xxxx command blahblah
```

## declare

```
SYNTAX
      declare [-afFrxi] [-p] [name[=value]]

OPTIONS

      -a  Each name is an array variable.

      -f   Use function names only.

      -F   Inhibit the display of function definitions; 
           only the function name and attributes are printed. 
           (implies -f)

      -i   The variable is to be treated as an integer; 
           arithmetic evaluation is performed when the 
           variable is assigned a value.

      -p   Display the attributes and values of each name. 
           When `-p' is used, additional options are ignored.

      -r   Make names readonly. These names cannot then
           be assigned values by subsequent assignment statements 
           or unset.

      -x   Mark each name for export to subsequent commands
           via the environment.
```

## What is difference in `declare -r` and `readonly` in bash?

reference: https://stackoverflow.com/a/30362832/1545387

So one difference is `readonly` will make the variable scope global.  `declare` makes variable scope local (which is expected).
Note: adding `-g` flag to the `declare` statement (e.g. `declare -rg a="a1"`) makes the variable scope global. 

## Getting the source directory of a Bash script from within

```bash
# from: https://stackoverflow.com/a/246128/1545387
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
```

## Print error line

ref: https://unix.stackexchange.com/a/39660

```bash
#! /bin/bash

err_report() {
    echo "Error on line $1"
}

trap 'err_report $LINENO' ERR
#    └─ 'single quotes to prevent `$LINENO` from being expanded when the trap line is first parsed.
```

## Tokenize a string

```bash
string="hello;world"
IFS=';' read -r -a tokens <<< "$string"
echo "${tokens[*]}"
```

## What is '<<<' ?

ref: https://stackoverflow.com/a/16045687

`<<<` is bash-specific redirection operator.

Bash:

```bash
if grep -q "^127.0.0." <<< "$RESULT"
then
    echo IF-THEN
fi
```

No Bash compatible:

```bash
if echo "$RESULT" | grep -q "^127.0.0."
then
    echo IF-THEN
fi
```

## Here Document

ref: http://www.gnu.org/software/bash/manual/bashref.html#Here-Documents

## Snippets

### Write file with cat

```bash
cat << 'EOF'| tee '/path/to/file'
blah blah
blah blah
EOF
```

### Confirm message

```bash
read -p "Are you sure? [y/N]: " -r
if [[ ! "$REPLY" =~ ^[Yy]$ ]]; then
  exit 1
fi
```