# Codify

## 1. Enumeration
First, We enumerate the machine. I recommend to put the IP on /etc/hosts with the name server "codify.htb".

![nmap](https://github.com/JulianEspadaRodriguez/HackTheBox/blob/main/Codify/nmap.png)


We haven't any ssh access so We go for web.
We try connect to the web through both ports and I see the same page:


![codify](https://github.com/JulianEspadaRodriguez/HackTheBox/blob/dcb54eb3513fc1e90654a321b48ea5be0cc1420c/Codify/Screenshot%202023-11-06%20at%2011-44-27%20Codify.png)

We see that in the button *"Try it now"* get us into a nodejs injection:

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/0958eeba-ba9e-41c9-8e88-1c44474b0e22)




