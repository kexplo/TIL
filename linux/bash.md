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
if ! command -v docker > /dev/null
then
    echo 'docker command not exist'
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


## Snippets

### Write file with cat

```bash
cat << 'EOF'| tee '/path/to/file'
blah blah
blah blah
EOF
```