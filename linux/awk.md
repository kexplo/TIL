## Print the line of last match

```bash
$ cat test.txt
hello1
hello2
hello3
bye1
bye2

$ awk '/^hello/ {a=$0} END{print a}' test.txt
hello3
```


## Print the line number of last match

```bash
$ akw '/^hello/ {a=NR} END{print a}' test.txt
```