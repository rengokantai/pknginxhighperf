#### pknginxhighperf
#####4 Controlling buffers
######configuring buffers
request body
```
client_body_buffer_size 8k  (default 8k,32bit system, 16k, 64bitsystem)
```
If client body is too large, it will return 413
```
server{
  client_max_body_size 2m;
}
```

only save request body in temporary file
```
client_body_in_file_only on/off/clean;
```
store complete request body in single buffer
```
client_body_in_single_buffer on;
```
save files in temp path
```
client_body_temp_path filepath 1 2
```
allocate a buffer for request header
```
client_header_buffer_size 1k
```
If header does not fit into a specified buffer,use
```
large_client_header_buffers 4 8k
```
######configuring timeouts
(h s l) (may have a second param,end back to Keep-Alive header)
```
keepalive_timeout 20s (18s);  
```
max requests during a keep-alive (h s l)
```
keepalive_requests 20;
```
```
keepalive_disabled msie6 safari;
```
(h s l)If the timeout expires, nginx will close the connection.(does not apply to the entire transfer but only between two successive write operations)
```
send_timeout 30s
```
bodytimeout. 408error(request timed out)
```
client_body_timeout 30s;
```
sets a timeout to send a complete request header from the client
```
client_header_timeout 30s;
```
######compression
basic
```
gzip on;
```
level (1-3)
```
gzip_comp_level 3;
```
min length (in bytes)
```
gzip_min_length 1000;
```
limit gzip types
```
gzip_types text/xml text/css text/plain;
```
what kind of response should be compressed (for example,this enables compression if cache-control header field has the value no-cache)
```
gzip_proxied no-cache expired
```
```
gzip_http_version 1.1
```

```
gzip_vary on;
```
#####6 Using nginx Cache
######cache static content
```
open_file_cache max=1000 inactive=20s;
open_file_cache_valid 60s;
open_file_cache_min_uses 5;
open_file_cache_errors off;
```
######cache dynamic content
install php
```
tar -xvf php-5.tar.gz
cd php-5
./configure --enable-fpm -q
make --quiet
make install
```
Or install via apt
```
apt-get install php5 php5-fpm
```

