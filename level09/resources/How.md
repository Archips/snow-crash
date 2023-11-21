

# **Level09**

### **Walk through**

Like in level08, we have a binary `level09` and a file `token`  

`./level09`     
		`$> You need to provied only one arg.`  
`./level09 token`   
		`$> tpmhr`  
`cat token`   
		`$> f4kmm6p|=�p�n��DB�Du{��`  

Let's make some more tests:

`./level09 1`   
		`>$ 1`  
`./level09 12`  
		`>$ 11`  
`./level09 123`  
		`>$ 111`  

We can observe that `./level09` output is arg but every char is the result of the initial char plus the number of chars before it.

`token`  gives us, **t** because `t - 0`, **p** because `o + 1`, and so on, so at the end we got **r** because `n - 4`.

Let's write a C program that will reverse this algorithm.

```C
#include <stdio.h>

int main(int ac, char **av) {

        if (ac == 2) {
                for (int i = 0; av[1][i]; i ++)
                        printf("%c", av[1][i] - i);
        }
        printf("\n");
        return (0);
}
``` 

`gcc -Wall -Wextra -Werror level09.c`
`./a.out $(cat token)`

This program will output **`f3iji1ju5yuevaus41q1afiuq`**

Using that flag with `su flag09` and doing `getflag` gives us **`s5cAJpM8ev6XHw998pRWG728z`**

We will use it to log as level10

`su level10`
