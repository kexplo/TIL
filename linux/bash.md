# bash script

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
