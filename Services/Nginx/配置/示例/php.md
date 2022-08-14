```nginx
location ~ \.php$ {
         root /usr/local/php/www/html;
         fastcgi_pass 192.168.0.11:9000;
         fastcgi_param PATH_INFO $fastcgi_path_info;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         fastcgi_split_path_info ^(.+\.php)(/.+)$;
         include fastcgi_params;
         fastcgi_index /usr/local/php/www/html/index.php;
       }
```

