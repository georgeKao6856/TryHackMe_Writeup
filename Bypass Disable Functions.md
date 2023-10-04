# Bypass Disable Functions

###### tags: `Tryhackme` `Hacking`
---

* **工具名稱:** Chankro

* **目的:** to evade disable_functions and open_basedir

* **安裝方法:** 
    > 1. git clone https://github.com/TarlogicSecurity/Chankro.git
    > 2. cd Chankro
    > 3. python2 chankro.py --help
    > 4. python chankro.py --arch 64 --input c.sh --output tryhackme.php --path /var/www/html

* 教學:

![](https://i.imgur.com/B4BDaf2.jpg)

![](https://i.imgur.com/ERTPNW5.jpg)

    * When executing the PHP script in the web server, the necessary files will be created to execute our payload.
![](https://i.imgur.com/CB4pUee.jpg)


---

* 邏輯:
1. 查看phpinfo資訊，找到DOCUMENT_ROOT和disable_functions(查看被禁用的指令)
2. 我先用png or jpeg圖檔上傳，是可以上傳的 (查看http://ip_address/uploads)
3. 我把用Chankro做出來的檔案(做之前會有一個shell檔)的副檔名改成.php.jpeg，然後試著上傳，結果是不行的
4. 所以把burp suite把request攔截一下，看一下內容
5. 在<?php 前面加GIF87a就可以bypass了
6. 上傳後，到uploads點選剛剛上傳的檔案，就會執行檔案了

![](https://i.imgur.com/2ujHj4v.jpg)

![](https://i.imgur.com/y3s61oM.jpg)

![](https://i.imgur.com/HRtxm5y.jpg)

![](https://i.imgur.com/NOh8IbV.jpg)

![](https://i.imgur.com/MgO1yFV.jpg)

![](https://i.imgur.com/jIJ4Okf.jpg)

* 所以同理，將c.sh的內容改成reverse_shell，就可以控制該主機了
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md
![](https://i.imgur.com/GJ9iVWX.jpg)

![](https://i.imgur.com/JGhJZEK.jpg)

* 監聽4444 port，Command: nc -lvnp 4444
![](https://i.imgur.com/EKqbZJd.jpg)

![](https://i.imgur.com/mw4SsEH.jpg)

* **Flag:** thm{bypass_d1sable_functions_1n_php}

OS Version: Linux ubuntu 4.4.0-210-generic #242-Ubuntu SMP Fri Apr 16 09:57:56 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
