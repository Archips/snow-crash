# **Level00**

### **Walk through**

 For this first level we can try a simple `ls -la`. Of course things aren't as simple as that.

As the aim of the whole project and thus of this level too is to be able to `su flag<level>`, we want to search for a file on which the user **flag00** would have some rights. To do so we can try the following command:

`find / -user flag00 -group flag00 2>/dev/null | grep -v proc`

This command gives us that output:

`/usr/sbin/john`  
`/rofs/usr/sbin/john`

`ls -la /usr/sbin/john` shows us this:

`----r--r-- 1 flag00 flag00 15 Mar  5  2016 /usr/sbin/john`

We have the permission to read the file, if we `cat /usr/sbin/john` we get:

**`cdiiddwpgswtgt`**

Unfortunately, if we give that as the password of `su flag00` we don't get anywhere. We can easily jump to the conclusion that **`cdiiddwpgswtgt`** is the encrypted password we're looking for. Let's decode it using [**dcode**](https://www.dcode.fr/cipher-identifier). This website offers many decoder tools as well as a cypher identifier. For this one we gonna try to decode it as a Caesar cypher. This tool gives us this solution which seems pretty good **`nottoohardhere`**. Indeed it is, as we able to log as `su flag00` with it. The last step, according to the subject is to `getflag`. This command gives us our first flag: 

**`x24ti5gi3x0ol2eh4esiuxias`** 

We will use it to log as level01

`su level01`
