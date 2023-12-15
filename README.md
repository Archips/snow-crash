# snow-crash

This project is part of the cybersecurity post common core projects. It's an introduction to computer security involving 15 levels and thus 15 flags to progress from one level to another. Through those levels, we discover many sub security domains and become familiar with some languages (ASM, Perl, php, bash, ..). Moreover, this project offers a new logic to dissect programs we are unfamiliar with and an certain awareness of security risks linked to simple programming errors.

## Setup

Starting the project, all we have is an `.iso`. As suggest by the subject, it's really recommended to connect to that vm through ssh. To do so, we have to follow some certain steps, thanks to this [**tutorial** ](https://youtu.be/Y7KzV-Hl2bw )it's not really complicated. We have also a second vm running [**kali on virtual box**](https://www.kali.org/get-kali/#kali-virtual-machines).

## Useful commands
1. **ssh**  
		```ssh level<XX>@<IP> -p 4242```  
		For instance : ```ssh level12@192.168.56.101 -p 4242```
    
2. **scp**  
		```scp -P 4242 level<XX>@<IP>:<path/of/file/> <destination/path>```  
		For instance : ```scp -P 4242 level09@192.168.56.101:/home/user/level09/token .```  

## Helpful tools

1. [**Dogbolt**](https://dogbolt.org/)
2. [**GDB**](https://www.sourceware.org/gdb/documentation/)
3. [**Ghidra**](https://ghidra-sre.org/) 
4. [**John the Ripper**](https://www.openwall.com/john/doc/)
5. [**Wireshark**](https://www.wireshark.org/docs/)

## Author

[Archibald Thirion](https://github.com/Archips)  
[Cristina Mois](https://github.com/Cristinamois/)
