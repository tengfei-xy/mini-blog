

```ini
[client]
port = 3606
socket = /tmp/mysql.sock

[mysqld_safe]
user = mysql
log-error=/usr/local/mysql-8.0.28/logs/mysql-safe-error.log
pid-file=/usr/local/mysql-8.0.28/mysql_safe.pid

[mysqld]
port = 3606
socket = /tmp/mysql.sock
basedir = /usr/local/mysql-8.0.28
datadir = /usr/local/mysql-8.0.28/data
pid-file = /usr/local/mysql-8.0.28/mysql.pid
user = mysql
bind-address = 0.0.0.0
server-id = 1 
skip-name-resolve
back_log = 600
max_connections = 1000
max_connect_errors = 6000
open_files_limit = 65535
table_open_cache = 128
max_allowed_packet = 4M
binlog_cache_size = 1M
max_heap_table_size = 8M
tmp_table_size = 16M
read_buffer_size = 2M
read_rnd_buffer_size = 8M
sort_buffer_size = 8M
join_buffer_size = 8M
thread_cache_size = 8
key_buffer_size = 4M
ft_min_word_len = 4
transaction_isolation = REPEATABLE-READ
log_bin = mysql-bin
binlog_format = mixed
log_error = /usr/local/mysql-8.0.28/logs/mysql-error.log 
slow_query_log = 1
long_query_time = 1 
slow_query_log_file = /usr/local/mysql-8.0.28/logs/mysql-slow.log
performance_schema = 0
explicit_defaults_for_timestamp
skip-external-locking 
default-storage-engine = InnoDB 
innodb_file_per_table = 1
innodb_open_files = 500
innodb_buffer_pool_size = 64M
innodb_write_io_threads = 4
innodb_read_io_threads = 4
innodb_thread_concurrency = 0
innodb_purge_threads = 1
innodb_flush_log_at_trx_commit = 2
innodb_log_buffer_size = 2M
innodb_log_file_size = 32M
innodb_log_files_in_group = 3
innodb_max_dirty_pages_pct = 90
innodb_lock_wait_timeout = 120 
bulk_insert_buffer_size = 8M
myisam_sort_buffer_size = 8M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
interactive_timeout = 28800
wait_timeout = 28800

[mysqldump]
quick
max_allowed_packet = 16M

[myisamchk]
key_buffer_size = 8M
sort_buffer_size = 8M
read_buffer = 4M
write_buffer = 4M
```

