# Sau
![Sau](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/da2a89c3-dfe2-43ae-96b2-6d50b02fc655)

## 1. Scanning and enumeration

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/7a3d02b3-5805-4c5d-b6e4-3c9ef97115df)

I see the port 22, 80 (filtered) and 55555 as a http port. I visited the web and I saw a vulnerable version of request-baskets (v1.2.1) of the [CVE-2023–27163](https://medium.com/@li_allouche/request-baskets-1-2-1-server-side-request-forgery-cve-2023-27163-2bab94f201f7)

## 2. Exploitiation

Testing a little bit the web and knowing that the web is vulnerable to SSRF, I created a basket that has the forward URL the localhost but the port 80 that which is filtered. That's because SSRF allows connect to internals services without authorization.

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/7c02f03c-91bb-4e90-8d9e-fabb73551661)

I apply, and try to connect to that URL and... voilà! I got to web server of the port 80 and enumerate the software maltrail with the version 0.53 that has a vulnerability of unauthenticated OS RCE.

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/6955e765-497c-497b-8612-a6cf897617ed)

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/c52ba7a1-5d85-4a43-9ec4-6330bba4568f)

Setting a listener `nc -lvnp 4444` I throw the exploit

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/cd412717-6f83-4880-b30f-1729df72e2b4)

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/ed91a632-9b1c-4b51-b8a6-d4aa100e7f87)


## 3. Privilege escalation

With `sudo -l` I saw that I can have the privilege escalation with the systemctl command

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/ecb67431-3297-4000-9ae9-c12ff79dc81c)

I did like my Lord [GTFOBins](https://gtfobins.github.io/gtfobins/systemctl/) said.

![image](https://github.com/JulianEspadaRodriguez/HackTheBox/assets/111667186/3e845e6f-e429-467d-9e78-2aaeb30d31a5)



