# Golang

## Memory leak with `time.After()`

Problematic code:

```go
select {
    case <- time.After(time.Second * 10):
        // do something after 10 seconds.
    case <- ctx.Done():
        // do something when the context is finished.
        // but, the underlying timer created by `time.After()` will not be garbage collected.
        // Memory leaked!
}
```

Correct code:

```go
timer = time.NewTimer(time.Second * 10)
// Call `Stop()` when the timer is no longer needed. It is important.
defer time.Stop()

select {
    case <-timer.C:
        // do something after 10 seconds.
    case <- ctx.Done():
        // do something when the context is finished.
}
```

from godoc: https://godoc.org/time#After

> After waits for the duration to elapse and then sends the current time on the returned channel. It is equivalent to NewTimer(d).C. The underlying Timer is not recovered by the garbage collector until the timer fires. If efficiency is a concern, use NewTimer instead and call Timer.Stop if the timer is no longer needed.

## Use `GOPRIVATE` environment variable with private module

```text
$ go help module-private
The go command defaults to downloading modules from the public Go module
mirror at proxy.golang.org. It also defaults to validating downloaded modules,
regardless of source, against the public Go checksum database at sum.golang.org.
These defaults work well for publicly available source code.

The GOPRIVATE environment variable controls which modules the go command
considers to be private (not available publicly) and should therefore not use the
proxy or checksum database. The variable is a comma-separated list of
glob patterns (in the syntax of Go's path.Match) of module path prefixes.
For example,

        GOPRIVATE=*.corp.example.com,rsc.io/private

causes the go command to treat as private any module with a path prefix
matching either pattern, including git.corp.example.com/xyzzy, rsc.io/private,
and rsc.io/private/quux.

The GOPRIVATE environment variable may be used by other tools as well to
identify non-public modules. For example, an editor could use GOPRIVATE
to decide whether to hyperlink a package import to a godoc.org page.

For fine-grained control over module download and validation, the GONOPROXY
and GONOSUMDB environment variables accept the same kind of glob list
and override GOPRIVATE for the specific decision of whether to use the proxy
and checksum database, respectively.

For example, if a company ran a module proxy serving private modules,
users would configure go using:

        GOPRIVATE=*.corp.example.com
        GOPROXY=proxy.example.com
        GONOPROXY=none

This would tell the go command and other tools that modules beginning with
a corp.example.com subdomain are private but that the company proxy should
be used for downloading both public and private modules, because
GONOPROXY has been set to a pattern that won't match any modules,
overriding GOPRIVATE.

The 'go env -w' command (see 'go help env') can be used to set these variables
for future go command invocations.
```
