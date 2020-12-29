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
