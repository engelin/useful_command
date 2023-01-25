### Based on Ubuntu

1. Check Root Login failure

```sh
# user name
perl -ne 'print "$1\n" if(/Failed password\D+((\d+\.){3}\d+)/)' /var/log/secure | sort | uniq -c | sort -rn |head -10

# ip
perl -ne 'print "$1\n" if(/Failed password for (\w.+) from/)' /var/log/auth.log | sort | uniq -c | sort -rn | head -10
```

2. Server uptime

```sh
# uptime
```

3. System error message
```sh
# OS level
dmesg | tail

# process level
cat /var/log/syslog | egrep -i "emerg|alert|crit|error|warn|fail"

# ssh connection failure
cat /var/log/auth.log | tail
```

4. Memory
```sh
free

# the latest kernel
cat /proc/meminfo | grep 'MemTotal\|MemFree\|Buffers\|Cached'
```

- [memory_usage_script](memory_usage_free.sh)

5. File system
```sh
df -Th

# Ordered by disk capacity
ionice -c 2 -n 7 nice -n 19 du -scm /* | sort -rn
```

6. Network
- [network_usage_script](netmon.sh)
- [port_connections_script](connection_ports.sh)

7. Load situation
```sh
top

#
ionice -c 2 -n 7 nice -n 19 top -c
```
- us(user): CPU rates for OS user(application level)
- sy(system): CPU rates for OS kernel
- wa(iowait): CPU rates for Disc I/O

```sh
# CPU, I/O, memory...
sar -u 3 10

# Process ready status
vmstat 1 10

# Process
ps aux --sort=-%cpu
```

```sh
# Storage
iostat -dx 5
```

---
- Ref: [linux server operations](https://www.mimul.com/blog/linux-server-operations/)
