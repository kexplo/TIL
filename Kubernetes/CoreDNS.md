# Add Support DNS-over-HTTPS

```Corefile
forward . tls://1.1.1.1 {
    tls_servername tls.cloudflare-dns.com
}
```

reference: https://github.com/coredns/coredns/issues/1650#issuecomment-377790487
