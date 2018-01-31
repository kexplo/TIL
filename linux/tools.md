# iostat

```
iostat -xmdz 1
#       ││││ └─ repeat every 1 second
#       │││└─ omit output for any devices for which there was no activity during the sample period
#       ││└─ display the device utilization report
#       │└─ display statistics in megabytes per second
#       └─ display extended statistics
```

```
Device:  rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
xvdf       0.00  2934.00    0.00 2000.00     0.00    48.69    49.86     2.59    1.30    0.00    1.30   0.50  99.60
```

`rrqm/s, wrqm/s`

read/write requests merged per second

`r/s, w/s, rMB/s, wMB/s`

reads/writes (throughput) per second

`avgrq-sz`

Average request size in **sectors** (512 bytes) In general if this number is below 16 (16 * 512 bytes = 8KB). If this number is low (<50), you are going to be IOPS limited. If it’s high (>100), you are likely to be bandwidth limited.

`avgqu-sz`

Average queue size. Indicates how many requests are queued waiting to be serviced. If `avgqu-sz` gets big (>30), your application is submitting more requests per secondthan the volume can handle.

`await`

Average wait in **milliseconds.** The average amount of time the requests that were completed during this period waited from when they entered the queue to when they were serviced. This number is a combination of the queue length and the average service time. **This is one of the most important metrics.**

`svctm`

Service time in **milliseconds.** While `await` counts the whole wait time of requests, `svctm` counts only the time consumed by device. As Linux doesn’t measure the actual service time, so `svctm` is just approximation. **Consider await more importantly.**

`%util`

Percentage of CPU time during whchi I/O requests were issed to the device. High `%util` doesn’t always say that there is an overload. If the device serves requests in parallel, this value can constantly be high.