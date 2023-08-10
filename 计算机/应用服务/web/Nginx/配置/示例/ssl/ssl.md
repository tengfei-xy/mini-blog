# ssl

```nginx
ssl_certificate .pem;
ssl_certificate_key  .key;
ssl_session_timeout 5m;
ssl_protocols TLSv1.2 TLSv1.3;
ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
ssl_prefer_server_ciphers on;
```
