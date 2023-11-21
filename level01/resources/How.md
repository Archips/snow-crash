

# **Level01**

### **Walk through**

As we'll do for almost all levels, let's try `ls -la`. Like the last level, it gives us nothing, and it will be the same for the `find` command, the following option `-name flag01` and `-user flag01 -group flag01` won't get us anywhere.

A good option would be to give a look at the file **/etc/passwd**. This file stores very useful users accounts information. This [**documentation**](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/) gives us some explanations to understand it and use it to our advantage. Let's see what this file contains.

`cat /etc/passwd` gives us this:

`...`  
`level13:x:2013:2013::/home/user/level13:/bin/bash`  
`level14:x:2014:2014::/home/user/level14:/bin/bash`  
`flag00:x:3000:3000::/home/flag/flag00:/bin/bash`  
**`flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash`**  
`flag02:x:3002:3002::/home/flag/flag02:/bin/bash`  
`flag03:x:3003:3003::/home/flag/flag03:/bin/bash`  
`...`

Reading the documentation given right above, we see that the **`x`** after the username means that the password of the user is hashed and stored into the file `/etc/shadow`. If we pay attention we'll see that it's not the case for the user we're looking for, we directly got the hash **`42hDRfypTqqnw`** .  The issue we're facing now is that a hash is not made to be decoded. Luckily the tool [**John the Ripper**](https://medium.com/@JAlblas/tryhackme-john-the-ripper-walkthrough-75331d14748c) is there to help us.

Following the tutorial we gonna use it the following way:

`echo 42hDRfypTqqnw > encrypt_flag01.txt`  
`john encrypt_flag01.txt --show`

John will quickly output the flag01's password: **`abcdefg`**.

All we have to do now is to log as `su flag 01` with that password. `getflag` gives us:

**`f2av5il02puano7naaf6adaaf`**

We will use it to log as level02

`su level02`
