## Add nameserver

```
# in /etc/resolvconf/resolv.conf.d/head
nameserver <nameserver-ip>
```

then, `sudo resolvconf -u`