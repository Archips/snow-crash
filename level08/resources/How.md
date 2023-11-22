

# **Level08**

### **Walk through**

This time, we have a binary `level08` and a file `token`

`./level08` gives us `./level08 [file to read]`

`./level08 token` gives us `You may not access 'token'`

`ls -la token` shows us that the permission of that file are `-rw-------`,

We don't have rights on **token**, so the idea is to find a way to use it even if we don't have any access. To do so, we could symlink **token** in `/tmp/token`. Indeed the permission of a symlink are typically always lrwxrwxrwx (read, write, execute for owner, group, and others), regardless of the permissions of the target file. The permissions of the target file are what determine access. If the target file has the permissions 600 (read and write for the owner only), the symlink allows anyone with read access to follow the link.

`ln -s /home/user/level08/token /tmp/flag08`

`./level08 /tmp/token` gives us the flag `quif5eloekouj29ke0vouxean`

Using that flag with `su flag08` and doing `getflag` gives us `25749xKZ8L7DkSCwJkT9dyv6f`

We will use it to log as level09

`su level09`
