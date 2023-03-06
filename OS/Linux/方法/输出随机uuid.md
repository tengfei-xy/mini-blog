```shell
head -1 /dev/urandom|od -x | awk '{print $2$3"-"$4$5"-"$6$7"-"$8$9}' | head -n1
```

