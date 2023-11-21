

# **Level13**

### **Walk through**

In this level, we have a binary level13

`./level13`
		`>$ UID 2013 started us but we we expect 4242`

It seems that once launched the program checks the UserID (UID) of the user which using the binary. The output is quite clear, the UID should be 4242 while it's 2013.

Using gdb, we disassemble level13.

`gdb ./level13`
`disas main`

```
...
   0x08048592 <+6>:     sub    $0x10,%esp
   0x08048595 <+9>:     call   0x8048380 <getuid@plt>
   0x0804859a <+14>:    cmp    $0x1092,%eax
   0x0804859f <+19>:    je     0x80485cb <main+63>
   0x080485a1 <+21>:    call   0x8048380 <getuid@plt>
   0x080485a6 <+26>:    mov    $0x80486c8,%edx
...
```

In the line `0x08048595 <+9>:     call   0x8048380 <getuid@plt>` we see that `getuid` is called. In ASM, the return of a function is stored in the `$eax` register. In the line just after we see that the return value of `getuid` is compared with `0x1092`. If the values are identical, we get the flag.

Still in gdb, we  set a breakpoint at the moment of the comparison.

`break *0x0804859a`

Then we run the program using `run`, the program will pause at the breakpoint. We already know that `$eax` is set to the value 2013, which is not the value we want. Thanks to gdb we can reset on the fly the value of `$eax`

`set var $eax=4242`

Once it's done, we resume the program with the command `continue`. Finally, the execution of the program ends, and the flag appears **2A31L79asukciNyi8uppkEuSx**.

We will use it to log as level14

`su level14`

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
