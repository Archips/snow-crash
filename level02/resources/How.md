

# **Level02**

### **Walk through**

This time a simple `ls -la` gives us a result. We find a file **level02.pcap**.

Great, let's find out what's a .pcap file and how to analyze it.

A PCAP (Packet Capture) file is a binary data file that stores network traffic captured during packet sniffing or network monitoring. It contains the raw data of packets, including headers and payloads, allowing for analysis and troubleshooting of network issues. 

What we want to do now is to analyze that file and to look for our flag02 through those packets. To do so we gonna use the tool [**Wireshark**](https://www.wireshark.org/docs/wsug_html_chunked/ChapterIntroduction.html) which is a network protocol analyzer. 

We can see that the protocol of our network traffic is TCP. 

![wireshark](https://github.com/Cristinamois/snowcrash/blob/main/level02/resources/wireshark.png)

Knowing that, we will follow the TCP stream.

`Analyze > Follow > TCP stream`

![tcp_stream](https://github.com/Cristinamois/snowcrash/blob/main/level02/resources/tcp_stream.png)

We can see that the password seems to be **`ft_wandr...NDRel.L0L`** . Looking closer, we see that we can display the stream in ascii but also in hexadecimal. By analyzing the hex version, we see that the `.` corresponds to the ascii code `7f`. That code is actually the code of `DEL`.
Then, the password should be **`ft_waNDReL0L`**.

Using it we can `su flag02`. `getflag` gives us:

**`kooda2puivaav1idi4f57q8iq`**

We will use it to log as level03

`su level03`

