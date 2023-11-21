

# **Level14**

### **Walk through**

For this last level, any `find` or `ls -la` command seem to be useful. One option (and to us, the only one) is to find a way to exploit `getflag` itself.

#### **Option 1**

Using [**ghidra**](https://www.csoonline.com/article/567221/how-to-get-started-using-ghidra-the-free-reverse-engineering-tool.html), we disassemble the binary, both in C.

![ghidra](https://github.com/Cristinamois/snowcrash/blob/main/level14/resources/ghidra.png)

Based on those codes, we can disassemble `getflag` and try to exploit it.

`gdb getflag`  
`(gdb) break main`  
`(gdb) run`  
`(gdb) jump *0x08048de5`

```
Continuing at 0x8048de5.
7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
*** stack smashing detected ***: /bin/getflag terminated
```

The last flag is : **7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ**

We will use it to log as flag14 and finally finish snow-crash

`su flag 14`  
  
  #### **Option 2**

Following the same reasoning as the one of the level13, still using gdb we can proceed the following way.

`gdb getflag`  
`(gddb) disas main`  

```
[...]
   0x08048989 <+67>:    call   0x8048540 <ptrace@plt>
   0x0804898e <+72>:    test   %eax,%eax
   0x08048990 <+74>:    jns    0x80489a8 <main+98>
[..]
   0x08048af5 <+431>:   mov    %eax,(%esp)
   0x08048af8 <+434>:   call   0x80484c0 <fwrite@plt>
   0x08048afd <+439>:   call   0x80484b0 <getuid@plt>
   0x08048b02 <+444>:   mov    %eax,0x18(%esp)
[..]

```

We notice a ptrace syscall. `ptrace` is supervising the certain process, if the latter encounters an error `ptrace` stopped its execution. If the return of `ptrace == -1` the program is interrupted. We can bypass ptrace syscall using this method.

```
(gdb)catch syscall ptrace
Catchpoint 1 (syscall 'ptrace' [26])
(gdb) commands 1
Type commands for breakpoint(s) 1, one per line.
End with a line saying just "end".
> set $eax=0
> continue
> end
``` 
Now that's done, the next steps follow the same idea as the previous level. We'll break at the getuid function and set the right uid, the one of the flag14. To do so:

`cat /etc/passwd`  
`$> flag14:x:3014:3014::/home/flag/flag14:/bin/bash`

Back in gdb let's break at the getuid function and set $eax properly (3014). Indeed, we notice that $eax is set to 2014 which correspond to the uid of level14. 

```
(gdb) break getuid
Breakpoint 2 at 0x80484b0
(gdb) run
...
(gdb) step
Single stepping until exit from function getuid,
which has no line number information.
0x08048b02 in main ()
$1 = 2014
(gdb) set $eax=3014
(gdb) step
Single stepping until exit from function main,
which has no line number information.
Check flag.Here is your token : 7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
0xb7e454d3 in __libc_start_main () from /lib/i386-linux-gnu/libc.so.6
```

The last flag is : **7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ**

We will use it to log as flag14 and finally finish snow-crash

`su flag 14`  

***

### **Useful gdb commands**

1. **`break` command:**
   - **Usage:** `break [location]`
   - **Description:** Sets a breakpoint at a specified location in the code (e.g., line number, function name, or memory address). When the program reaches the breakpoint, it pauses execution, allowing you to inspect the program state.

2. **`commands` command:**
   - **Usage:** `commands [breakpoint number]`
   - **Description:** Defines a set of GDB commands to be executed when a specific breakpoint is hit. It allows you to automate actions at a breakpoint, like printing variables or running custom commands.

3. **`catch` command:**
   - **Usage:** `catch [event]`
   - **Description:** Sets a catchpoint for a specified event, such as a signal, system call, or exception. When the specified event occurs, the program stops, allowing you to examine the context.

4. **`continue` command:**
   - **Usage:** `continue`
   - **Description:** Resumes program execution until the next breakpoint is encountered or the program terminates. It's used to continue running the program after it has been stopped.

5. **`jump` command:**
   - **Usage:** `jump *address`
   - **Description:** Jumps to a specified address in the program's execution. This command allows you to change the flow of control, skipping over portions of code.

6. **`print` command:**
   - **Usage:** `print [expression]`
   - **Description:** Prints the value of the specified expression or variable. It's used to inspect the values of variables during debugging.

7. **`run` command:**
   - **Usage:** `run [args]`
   - **Description:** Starts the execution of the program from the beginning or continues from the last breakpoint. You can provide arguments to the program using the `run` command.

8. **`set` command:**
   - **Usage:** `set [variable] [value]`
   - **Description:** Modifies GDB settings or program variables. You can use it to change the value of a variable during debugging or configure various GDB options.

9. **`step` command:**
   - **Usage:** `step [count]`
   - **Description:** Executes the program one source line or machine instruction at a time. If a function call is encountered, it steps into the function, allowing you to trace the program's flow.
