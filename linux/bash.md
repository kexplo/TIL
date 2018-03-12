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


# pass environment variable

```bash
export ENV=xxx
command blahblah
unset ENV
```

or

```bash
ENV=xxxx command blahblah
```


## Snippets

### Write file with cat

```bash
cat << 'EOF'| tee '/path/to/file'
blah blah
blah blah
EOF
```