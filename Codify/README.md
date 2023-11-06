# Codify

![Codify(1)](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/fa4a2641-5830-46ca-9fb5-fe77a316ed41)

## 1. Scanning
First, I enumerate the machine. I recommend to put the IP on /etc/hosts with the name server *"codify.htb"*.

![nmap](https://github.com/JulianEspadaRodriguez/HackTheBox/blob/main/Codify/nmap.png)

## 2. Enumeration
I haven't any ssh access so I go for the web server.
I try connect to the web through both ports and I see the same page:

![codify](https://github.com/JulianEspadaRodriguez/HackTheBox/blob/dcb54eb3513fc1e90654a321b48ea5be0cc1420c/Codify/Screenshot%202023-11-06%20at%2011-44-27%20Codify.png)

## 3. Vulnerability assesment
I see that in the button *"Try it now"* get me into a nodejs injection:

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/0958eeba-ba9e-41c9-8e88-1c44474b0e22)

I search more information on the web server, the dir enumeration didn't solve anything. But in the *about* section, I found this nodejs web server is using a library which is called vm2 (v3.9.16) that has the following vulnerability: [Sandbox escape (CVE-2023-29199)](https://gist.github.com/leesh3288/f693061e6523c97274ad5298eb2c74e9)

## 4. Exploitation
I turn on a listener in our local machine: `nc -lvnp 4444`
I copy the payload in the link before, and I modify it like the code below:

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/07cf2b17-5ddf-41bd-98cd-2b154ff10fc8)

And...voilà! I have access with the user svc:

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/8c8b3cae-3dc5-4c09-97d1-003f61a0484e)

I need to change the user to the user joshua to get the user flag:

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/cd005395-639c-471c-83d3-f9ea3f17e0b1)

After being time looking for files or something that I could use for get to the user joshua, I found a db file in the file system of the web server: `/var/www/contact/tickets.db`

Looking the file I found a hash: `joshua$2a$12$SOn8Pf6z8fO/nVsNbAAequ/P6vLRJJl7gCUEiYBU2iLHn4G/p/Zw2`

Using john the ripper for cracking the hash: `john --wordlist=/usr/share/wordlist/rockyou.txt hash.txt`

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/e76520d2-3b53-4813-a83e-9f0935b2a582)

I ssh with the user:password that I found and get the user flag:

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/484e9843-e64f-4263-b226-98e003dbaa7c)

## 5. Privilege escalation
I execute `sudo -l` to see what bins or commands can I execute as a sudo  with joshua user.

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/d1034bb2-da86-49e4-bd5a-16521845314f)

Analyzing that script I see a vulnerability in the code. This vulnerability is the order of putting the double quote in the comparison `if [[ $DB_PASS == $USER_PASS ]]; then`. What it happens here, is that the bash language in this case, without double quotes, do a matching pattern comparison, not a real comparison. To mitigate this, We could do this `"$USER_PASS"` in the *if*. Well, to vuln this I used what is known as *wildcard* which is a special character for bypassing this entry of password (in this case) and getting the password little by little, by bruteforcing this vuln. The exploit I used was this:

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/40b15a31-d998-4c94-b582-9c3790f41436)

And I got the password: 

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/ab6f2a17-7501-4485-90bd-57a09f732e66)

I ssh with root user and get the root flag:

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/41e7151e-ac08-4a35-9056-883ef0e6663b)









