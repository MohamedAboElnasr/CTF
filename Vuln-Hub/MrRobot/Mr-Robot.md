
Certainly! Here's the complete Markdown code with all the sections included:


### Mr-Robot Machine from VulnHub

I will provide a brief overview of the Mr-Robot machine, available on VulnHub. This machine is free and designed for beginners, offering an entry-level challenge.

#### Description
The Mr-Robot machine is an introductory level CTF (Capture The Flag) challenge available on VulnHub. As a beginner-friendly virtual machine, it aims to provide a learning platform for those new to penetration testing and ethical hacking.

`This VM has three keys hidden in different locations. Your goal is to find all three. Each key is progressively difficult to find.`

So, our goal is to find these 3 keys to solve the machine.

#### Reconnaissance Phase
To begin, I conducted reconnaissance on my network to discover the vulnerable machine. In this phase, I used tools such as Nmap and Naabu to scan for open ports, services, and potential vulnerabilities.

- **Checking Alive Hosts**
  
  ![nmap](https://github.com/MohamedAboElnasr/CTF/assets/114421344/aec6fedb-e758-43d0-80e9-980175c80bc3)
  
  I used the following command with Nmap:
  ```bash
  nmap -sn 192.168.1.1/24
 
  Here, the `-sn` option is used for host discovery.
  
  After the scan, we found the vulnerable machine with the IP '192.168.1.12', so let's proceed with our enumeration.

- **Fast Scan with Naabu**

  ![naabu](https://github.com/MohamedAboElnasr/CTF/assets/114421344/4e1df764-c848-4858-8fcc-cd3d5478103f)
  
  The open ports 80 and 443 suggest that the machine likely hosts a website using HTTP and HTTPS. Let's run Nmap again to check for other alive ports.
  
  ![nmapscan](https://github.com/MohamedAboElnasr/CTF/assets/114421344/9dfb1788-bcd8-4c47-94ad-c44af64a284f)

- **Nmap Options**

  I used the following Nmap command:
  ```bash
  sudo nmap -sS -sV -T4 192.168.1.12
  ```
  - `-sS`: Syn scan for port scanning.
  - `-sV`: Display the version of open ports and services.
  - `-T4`: Set the scan speed to make it faster.

  The Nmap output showed similar results to Naabu, indicating open ports. Our next step is to explore the website hosted on this machine.

  ![website](https://github.com/MohamedAboElnasr/CTF/assets/114421344/491ddf1c-4a8c-4718-8501-766edbb3dfa2)

  We should search for usernames, keys, passwords, or any hints that might assist us in gaining initial access. Unfortunately, we didn't find anything, so we proceeded with subdirectory enumeration using tools like ffuf, dirsearch, dirb, and burpsuite. In this challenge, I used dirsearch.
  and i found a directroy named "robots" which is used for making search enginges like google to not archive the end-points in that file, after accessing this end-point we find a 2 files 
<br>

  ![robots](https://github.com/MohamedAboElnasr/CTF/assets/114421344/f3ac0b0a-3c78-414f-a56a-2c60426e4137)
  
<br>

after accessing the file `key-1-of-3.txt` we get the first key 
<br>
  ![Key1](https://github.com/MohamedAboElnasr/CTF/assets/114421344/837bfb9d-d46b-4ad9-acbc-6f4b07cb4bea)
<br>
as with the second file `fsocity.dic` it's a txt file contains 858160 line in it,<br>![fslength](https://github.com/MohamedAboElnasr/CTF/assets/114421344/457fcba5-e07a-4a72-91b9-69f4981fab8d)<br> most of them are just repeated so we need to remove duplicated lines from it, for that i used a simple bash command
  ` sort  fsocity.dic|uniq >sorted ` now we broke it down to 11451 lines.<br>
![sorted](https://github.com/MohamedAboElnasr/CTF/assets/114421344/60d744a7-4d72-466e-96f0-d45d7c90bf7b)
  <br> let's get back to our dirsearch tool and see if there any other directories, and yea there is another directroy called `wp-login.php`, so the website is using a wordpress, the first thing that got to my mind is to use wpscan which is a great tool for extracting plugins and enumerates for the users usernames and passwords. we will get back to it soon, but first let's try to enter default credinitals like `username=admin&password=admin`, after doing so we got an error says that the username is Invalid.
<br>
![invalid](https://github.com/MohamedAboElnasr/CTF/assets/114421344/e751a152-9ecc-41bd-8ce9-f87a224bd708)
<br>
we will use tools for burpsuite for intercepting the request and the the parameters that begin sent, cuz we will hydra for enumerating our username and then the password.
this is our request
<br>
![req](https://github.com/MohamedAboElnasr/CTF/assets/114421344/6d760a58-c872-4498-96c6-cd273d2fc2ef)
<br>
![rep](https://github.com/MohamedAboElnasr/CTF/assets/114421344/39118a5d-41f4-45cb-97bf-21bf04a1b319)
<br>
and this is the response, Notice the invalid username in the response.
<br>
so to use hydra we are gonna give it the list of usernames and the paramters from the request and the invalid message from the server.
`hydra -L ./Mr.robot/sorted.txt -p 132 -t 10  192.168.1.12  http-post-form -I '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username'`
<br>
![hydra](https://github.com/MohamedAboElnasr/CTF/assets/114421344/1e418d2f-05d7-40aa-9073-0bf243e15581)
<br>
and here we go, we have a valid username which is elliot.<br> 
Note: don't copy the invalid message from the response just write it in the terminal cuz it didnt work for me i don't know why.
- now it's time for password remember WPscan, it's turn has come we will use it for enumerating the password for the user elliot with the same wordlist we have.
<br>
` wpscan --url http://192.168.1.12/wp-login.php -U elliot -P ./Mr.robot/sorted.txt `
<br>
![wpscan](https://github.com/MohamedAboElnasr/CTF/assets/114421344/29b3ce20-9be0-4395-8690-f32f3148bb6c)
<br>
![wpspassword](https://github.com/MohamedAboElnasr/CTF/assets/114421344/a20b6504-cbc0-42b5-94cb-2eb0676f8864)
<br>
### Initial Access
* now we have a username `elliot` and password `ER28-0652` we are now can signin into the wordpress, after signing in we will move to `Apperance --> editor `
<br>
![dashboard](https://github.com/MohamedAboElnasr/CTF/assets/114421344/30b2ee54-67ea-4a80-bd0f-60997263ee1b)
<br>
* we will shows 404.php and replace it's content with our reverse shell code so we can get access on the machine
* we are going to use php reverse shell from pentestmonkey which you can download it from [here](https://github.com/pentestmonkey/php-reverse-shell)
* replace the ip and port with your own and update the file
* <br>
![changeip_port](https://github.com/MohamedAboElnasr/CTF/assets/114421344/9d3a6578-f253-4b20-bea6-6e049040ee03)
  <br>
* start a nc listener on your machine
 ![nc](https://github.com/MohamedAboElnasr/CTF/assets/114421344/ff41bc37-dbb6-41f0-9489-fba93a8654e9)
  <br>
* access any invalid end-point on the website
  ![invalidend](https://github.com/MohamedAboElnasr/CTF/assets/114421344/55fde1cf-9014-4ba6-a542-aca7f267ba26)
  <br>
* now we are in
  ![we_in](https://github.com/MohamedAboElnasr/CTF/assets/114421344/7d7565bb-2cad-42d7-a77f-21d0fd56b05b)
  <br>
  ### shell initilization
  * after getting into any machine always upgrade your shell, this is going to save your life litteraly
    ```
    which python
    python -c 'import pty;pty.spawn("/bin/bash")'
    Control^Z.##^Z
    echo $TERM
    #save the output
    stty -a
    #save the output
     stty raw -echo;fg #cuz of zsh
     reset
     export TERM=xterm
     export SHELL=bash
     stty rows 56 columns 213 
    ```
  * now we have an interactive shell
    ![interactiveshell](https://github.com/MohamedAboElnasr/CTF/assets/114421344/d8ed4a77-e33a-4aff-961e-10ca527622ae)
    <br>
    after some enumeration inside the system i found a user called robot in the home directory along with a file called key2
    ![robot](https://github.com/MohamedAboElnasr/CTF/assets/114421344/dd31a1c0-75e1-4e13-ac79-715f24ccaeb7)
    <br>
    ###priviligaes Escalation
   *  now let's read this `password.raw-md5` file, it looks like a hashed password for the user robot, let's try to search on crackstation we may find the password value from that hash.
    and yes we now have the password for the robot user `abcdefghijklmnopqrstuvwxyz` from the hash `c3fcd3d76192e4007dfb496cca67e13b`
    ![password](https://github.com/MohamedAboElnasr/CTF/assets/114421344/5854fd3b-30fe-4da4-89ce-7622eac08656)
    <br>
    * now let's switch to robot user and read the second flag
      ![key2](https://github.com/MohamedAboElnasr/CTF/assets/114421344/8c4f6bf0-8e16-439c-803b-058ab9d9d4ae)
      <br>
    * now it's time for the final flag, but we have to be sudo first.
    * after some trial and error like seeing the `sudo -l` and tring to read `crontab` i found nothing but in my third trial for searching on a files that has the suid bit, i found that nmap has the suid bit enable on it.
 **note**
SUID Bit: is a file permssion that alows users on the system t excute the file as the owner of that file
* after i found nmap has the suid bit i go to [gtfobins](https://gtfobins.github.io/gtfobins/nmap/#suid) and tired the second method which is

``
sudo nmap --interactive
nmap> !sh
``
![root](https://github.com/MohamedAboElnasr/CTF/assets/114421344/dac25751-cb87-4cf6-baf4-f6687487da31)
<br>
* Now we are root, let's read the final flag
  ![key3](https://github.com/MohamedAboElnasr/CTF/assets/114421344/fe844a80-09da-46cd-8e23-7c686d80fad2)
  <br>
  any yaaaay we got all the 3 flags, thank you <3

    
    
    
    
  




