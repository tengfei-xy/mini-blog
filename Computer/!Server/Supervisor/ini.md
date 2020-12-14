# ini

## 完整配置（Nbzhiwei）

```ini
[program: logstash-6.1.1]
command=/usr/local/elk6/logstash-6.1.1/bin/kibana
process_name=%(program_name)s 	; process_name expr (default %(program_name)s)
numprocs=1 			; number of processes copies to start (def 1)
directory=/usr/local/elk6/logstash-6.1.1/
umask=022 			; umask for process (default None)
priority=999 			; the relative start priority (default 999)
startsecs=1 			; # of secs prog must stay up to be running (def. 1)
startretries=3 			; max # of serial start failures when starting (default 3)
autorestart=true 		; when to restart if exited after running (def: unexpected)
exitcodes=0,2 			; 'expected' exit codes used with autorestart (default 0,2)
stopsignal=kill 		; signal used to kill process (default TERM)
stopwaitsecs=10 		; max num secs to wait b4 SIGKILL (default 10)
stopasgroup=false 		; send stop signal to the UNIX process group (default false)
killasgroup=false 		; SIGKILL the UNIX process group (def false)
user=nbzhiwei 			; setuid to this UNIX account to run the program
redirect_stderr=true 		; redirect proc stderr to stdout (default false)
stdout_logfile=/usr/local/logs/logstash-6.1.1/logstash-6.1.1-info.log ; stdout log path, NONE for none; default AUTO
stdout_logfile_maxbytes=50MB 	; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=10 	; # of stdout logfile backups (0 means none, default 10)
stdout_capture_maxbytes=1MB 	; number of bytes in 'capturemode' (default 0)
stdout_events_enabled=false 	; emit events on stdout writes (default false)
stderr_logfile=/usr/local/logs/logstash-6.1.1/logstash-6.1.1-error.log ; stderr log path, NONE for none; default AUTO
stderr_logfile_maxbytes=10MB 	; max # logfile bytes b4 rotation (default 50MB)
stderr_logfile_backups=10 	; # of stderr logfile backups (0 means none, default 10)
stderr_capture_maxbytes=1MB 	; number of bytes in 'capturemode' (default 0)
stderr_events_enabled=false 	; emit events on stderr writes (default false)
autostart=no
```

