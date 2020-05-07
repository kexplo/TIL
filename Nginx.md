# Nginx

## Return client's IP

Use `$remote_addr`

```conf
location /ip {
   # disable cache
   expires -1;
   add_header 'Cache-Control' 'no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0';

   default_type text/plain;
   return 200 "$remote_addr\n";
}
```

If you use the Cloudflare CDN, use `$http_cf_connection_ip`

```conf
location /ip {
   # disable cache
   expires -1;
   add_header 'Cache-Control' 'no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0';

   default_type text/plain;
   return 200 "$http_cf_connecting_ip\n";
}
```
