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

> When not performing substring expansion, using the form described below (e.g., ‘:-’), Bash tests for a parameter that is unset or null. Omitting the colon results in a test only for a parameter that is unset. Put another way, if the colon is included, the operator tests for both parameter’s existence and that its value is not null; if the colon is omitted, the operator tests only for existence.

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

### divided by newlines

need to use the `-d` switch to `read`:

> `-d` *delim*
>
> The first character of delim is used to terminate the input line, rather than newline.

```bash
$ string=$'hello\nworld'
$ IFS=$'\n' read -r -d '' -a arr < <(printf '%s\0' "$string")
$ declare -p arr
declare -a arr='([0]="hello" [1]="world")'
```

Or

```bash
IFS=$'\n' read -r -d '' -a arr <<< "$var"
```

In this case, the content of `arr` is the same; the only difference is that the return code of `read` is 1 (failure).

reference: https://stackoverflow.com/a/28417633/1545387

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

## Empty array expansion with 'set -u'

ref: https://stackoverflow.com/a/7577209

```bash
$ set -u
$ arr=()
$ echo "foo: '${arr[@]}'"
bash: arr[@]: unbound variable
```

Use `${arr[@]+"${arr[@]}"}` instead of `"${arr[@]}"`.

```bash
$ function args { perl -E'say 0+@ARGV; say "$_: $ARGV[$_]" for 0..$#ARGV' -- "$@" ; }

$ set -u

$ arr=()

$ args "${arr[@]}"
-bash: arr[@]: unbound variable

$ args ${arr[@]+"${arr[@]}"}
0

$ arr=("")

$ args ${arr[@]+"${arr[@]}"}
1
0:

$ arr=(a b c)

$ args ${arr[@]+"${arr[@]}"}
3
0: a
1: b
2: c
```

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

### Extract tar.gz with wget

```bash
$ wget -qO- http://tar.gz.link | tar xvz -C /target/directory
```

### Check sudo requires password

ref: https://askubuntu.com/a/357222

```bash
if sudo -n true 2>/dev/null; then
  echo "I got sudo"
else
  echo "I don't have sudo"
fi
```

```bash
# explain
sudo -n true 2>/dev/null;
#     │ └─ If the password is not required, then this expression is true.
#     └─ (non-interactive) option. Avoid prompting.
#        if a password required, sudo will display an error and exit.
```

### Increase counter variable

```bash
counter=1
while cond; do
  # do something
  counter=$((counter+1))
done
```

### Read a file line by line

```bash
while IFS= read -r line; do
  echo "$line"
done < 'file.txt'
```

### Strip string

Trim leading and trailing spaces

```bash
awk '{$1=$1};1'
```

reference: https://unix.stackexchange.com/a/205854
