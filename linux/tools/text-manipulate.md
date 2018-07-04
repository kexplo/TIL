## cut

```bash
$ cat test.csv
1,2,3,4
a,b,c,d
q,w,e,r

$ cut -d , -f 2- test.csv
#       │    └─ only select 2 or more fields
#       └─ use ',' as field delimiter (default: Tab)
2,3,4
b,c,d
w,e,r

$ cut -d , -f 3 test.csv
#       │    └─ only select 3 field
#       └─ use ',' as field delimiter (default: Tab)
3
c
e
```

## paste

```bash
$ cat test1.csv
1,2
3,4

$ cat test2.csv
5,6
7,8

$ paste -d , test1.csv test2.csv
#         └─ use ',' as field delimiter
1,2,5,6
3,4,7,8
```

### merge two CSV file

```bash
$ cat test1.csv
1,2
3,4

$ cat test2.csv
5,6,7
8,9,0

$ cut -d , -f 2 test2.csv | paste -d , test1.csv -
1,2,6
3,4,9
```

## truncate

```bash
# reference: manpage

truncate -s<SIZE>
#         └─ set or adjust the file size by SIZE bytes
#
# SIZE may also be prefixed by one of the following modifying characters:
# '+' extend by, '-' reduce by, '<' at most, '>' at least,
# '/' round down to multiple of, '%' round up to multiple of.
```

```bash
# remove 1 character from file
$ truncate -s-1 <file>
# remove 2 character from file
$ truncate -s-2 <file>
```