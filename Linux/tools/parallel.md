# GNU Parallel

GNU parallel is a command-line driven utility for Linux and other Unix-like operating systems which allows the user to execute shell scripts in parallel. (one or more computers)

By default, parallel runs as many jobs in parallel as there are CPU cores.

## Examples

`parallel ::: <inputs>`

```bash
$ parallel echo ::: A B C D
A
B
C
D
```

or

```bash
$ parallel echo {} ::: A B C D
A
B
C
D

$ (echo "A"; echo "B"; echo "C"; echo "D") | parallel echo
A
B
C
D
```

----

`parallel ::: -a <input file>`

```bash
$ cat input.txt
A
B
C
D

$ parallel -a input.txt echo
A
B
C
D
```

----

```bash
$!/usr/bin/env bash

set -euo pipefail

function job {
    print "$1"
    sleep "$1"
}

export -f job

parallel job ::: 1 2 3 4
```

## Options

- `--progress` : Show progress
- `-j N`, `--jobs N` : Specifies the number of jobs to be run. `0` means as many as possible
- `--joblog` : Write log file of executed jobs
- `--resume` : Resumes unfinished jobs by reading joblog (`--joblog`)
- `--resume-failed` : Retry and resumes all failed jobs by reading joblog (`--joblog`)

## Graceful shutdown

Send the signal **SIGTERM** to parallel process. **parallel** waiting for currently running jobs to complete and do not start new jobs.

```bash
killall -TERM parallel
```

## References

- https://www.gnu.org/software/parallel/
