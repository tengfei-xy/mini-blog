# files

简单配置

```ini
[program: proxy]
command=java -Xmx1g -jar proxy-server-0.0.1-SNAPSHOT.jar
process_name=%(program_name)s
numprocs=1
directory=/home/monster/proxy
umask=022
priority=999
autostart=true
startsecs=3
startretries=3
autorestart=true
exitcodes=0,2
stopsignal=kill
stopwaitsecs=10
stopasgroup=true
killasgroup=true
user=monster
redirect_stderr=true
stdout_logfile=/usr/local/services/logs/proxy/proxy-info.log
stdout_logfile_maxbytes=50MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stdout_events_enabled=false
stderr_logfile=/usr/local/services/logs/proxy/proxy-error.log
stderr_logfile_maxbytes=10MB
stderr_logfile_backups=10
stderr_capture_maxbytes=1MB
stderr_events_enabled=false
```

配置说明

```ini
[program: proxy]
command=java -Xmx1g -jar proxy-server-0.0.1-SNAPSHOT.jar
process_name=%(program_name)s   ; process_name expr (default %(program_name)s)
numprocs=1                      ; number of processes copies to start (def 1)
directory=/home/monster/proxy
umask=022                       ; umask for process (default None)
priority=999                    ;
autostart=true                  ; the relative start priority (default 999)
startsecs=3                     ; # of secs prog must stay up to be running (def. 1)
startretries=3                  ; max # of serial start failures when starting (default 3)
autorestart=true                ; when to restart if exited after running (def: unexpected)
exitcodes=0,2                   ; 'expected' exit codes used with autorestart (default 0,2)
stopsignal=kill                 ; signal used to kill process (default TERM)
stopwaitsecs=10                 ; max num secs to wait b4 SIGKILL (default 10)
stopasgroup=true                ; send stop signal to the UNIX process group (default false)
killasgroup=true                ; SIGKILL the UNIX process group (def false)
user=monster                   ; setuid to this UNIX account to run the program
redirect_stderr=true            ; redirect proc stderr to stdout (default false)
stdout_logfile=/usr/local/services/logs/proxy/proxy-info.log ; stdout log path, NONE for none; default AUTO
stdout_logfile_maxbytes=50MB    ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=10       ; # of stdout logfile backups (0 means none, default 10)
stdout_capture_maxbytes=1MB     ; number of bytes in 'capturemode' (default 0)
stdout_events_enabled=false     ; emit events on stdout writes (default false)
stderr_logfile=/usr/local/services/logs/proxy/proxy-error.log ; stderr log path, NONE for none; default AUTO
stderr_logfile_maxbytes=10MB    ; max # logfile bytes b4 rotation (default 50MB)
stderr_logfile_backups=10       ; # of stderr logfile backups (0 means none, default 10)
stderr_capture_maxbytes=1MB     ; number of bytes in 'capturemode' (default 0)
stderr_events_enabled=false     ; emit events on stderr writes (default false)
```
