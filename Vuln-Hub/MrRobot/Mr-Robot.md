  and
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

We began our search for usernames, keys, passwords, or any hints that could grant us initial access. Unfortunately, our search yielded no results. We then proceeded with subdirectory enumeration, utilizing tools like ffuf, dirsearch, dirb, and burpsuite. In this particular challenge, I opted for dirsearch.

While exploring, I discovered a directory named "robots." This directory is often used to instruct search engines, like Google, not to archive the endpoints within. Upon accessing this endpoint, we found two files:

![robots](https://github.com/MohamedAboElnasr/CTF/assets/114421344/f3ac0b0a-3c78-414f-a56a-2c60426e4137)

Within the file `key-1-of-3.txt`, we uncovered the first key:

![Key1](https://github.com/MohamedAboElnasr/CTF/assets/114421344/837bfb9d-d46b-4ad9-acbc-6f4b07cb4bea)

Next, we encountered `fsocity.dic`, a text file containing a whopping 858,160 lines. However, many of these lines were duplicates. To clean up the file, I employed a simple bash command: `sort fsocity.dic | uniq > sorted`, reducing it to 11,451 unique lines.

![sorted](https://github.com/MohamedAboElnasr/CTF/assets/114421344/60d744a7-4d72-466e-96f0-d45d7c90bf7b)

Returning to dirsearch tool, we uncovered another directory named `wp-login.php`. This revelation indicated that the website was built with WordPress. My immediate thought was to leverage wpscan, an excellent tool for extracting plugins and enumerating usernames and passwords. We'll revisit this shortly, but first, we attempted to log in with default credentials like `username=admin&password=admin`. The response informed us that the username was invalid.

![invalid](https://github.com/MohamedAboElnasr/CTF/assets/114421344/e751a152-9ecc-41bd-8ce9-f87a224bd708)

To further our investigation, we turned to Burp Suite for intercepting the request and examining the parameters being sent. Our request looked like this:

![req](https://github.com/MohamedAboElnasr/CTF/assets/114421344/6d760a58-c872-4498-96c6-cd273d2fc2ef)

And here's the corresponding response, where we noticed the mention of an invalid username:

![rep](https://github.com/MohamedAboElnasr/CTF/assets/114421344/39118a5d-41f4-45cb-97bf-21bf04a1b319)
<br>


To employ Hydra, we need to supply it with a list of usernames, the parameters from the request, and the invalid message received from the server:

```bash
hydra -L ./Mr.robot/sorted.txt -p 132 -t 10 192.168.1.12 http-post-form -I '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username'
```

![hydra](https://github.com/MohamedAboElnasr/CTF/assets/114421344/1e418d2f-05d7-40aa-9073-0bf243e15581)
<br>

And there we have it—a valid username: `elliot`. Note: Manually type the invalid message into the terminal, as copying it directly didn't work for me, for reasons unknown.

Now, it's time to focus on the password. Remember wpscan? Its turn has come. We will utilize it to enumerate the password for the user `elliot` using the same wordlist:

```bash
wpscan --url http://192.168.1.12/wp-login.php -U elliot -P ./Mr.robot/sorted.txt
```

![wpscan](https://github.com/MohamedAboElnasr/CTF/assets/114421344/29b3ce20-9be0-4395-8690-f32f3148bb6c)
<br>

![wpspassword](https://github.com/MohamedAboElnasr/CTF/assets/114421344/a20b6504-cbc0-42b5-94cb-2eb0676f8864)
<br>

### Initial Access

With the credentials `elliot:ER28-0652`, we can now sign in to the WordPress dashboard. Once logged in, we navigate to `Appearance --> Editor`.

![dashboard](https://github.com/MohamedAboElnasr/CTF/assets/114421344/30b2ee54-67ea-4a80-bd0f-60997263ee1b)

Here, we locate the `404.php` file and replace its content with our reverse shell code, allowing us to gain access to the machine. For this purpose, we use the PHP reverse shell from pentestmonkey, which can be downloaded [here](https://github.com/pentestmonkey/php-reverse-shell).

Remember to update the IP and port with your own:

![changeip_port](https://github.com/MohamedAboElnasr/CTF/assets/114421344/9d3a6578-f253-4b20-bea6-6e049040ee03)

Next, start a netcat (nc) listener on your machine:

![nc](https://github.com/MohamedAboElnasr/CTF/assets/114421344/ff41bc37-dbb6-41f0-9489-fba93a8654e9)

Finally, access any invalid endpoint on the website:

![invalidend](https://github.com/MohamedAboElnasr/CTF/assets/114421344/55fde1cf-9014-4ba6-a542-aca7f267ba26)



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
 Now, we have an interactive shell!

![interactiveshell](https://github.com/MohamedAboElnasr/CTF/assets/114421344/d8ed4a77-e33a-4aff-961e-10ca527622ae)

After some enumeration inside the system, I found a user called `robot` in the home directory along with a file called `key2`.

![robot](https://github.com/MohamedAboElnasr/CTF/assets/114421344/dd31a1c0-75e1-4e13-ac79-715f24ccaeb7)

### Privilege Escalation

Now, let's read the contents of the `password.raw-md5` file. It appears to be a hashed password for the user `robot`. Let's try to crack the hash using an online tool like CrackStation. We found the password for the `robot` user: `abcdefghijklmnopqrstuvwxyz` from the hash `c3fcd3d76192e4007dfb496cca67e13b`.

![password](https://github.com/MohamedAboElnasr/CTF/assets/114421344/5854fd3b-30fe-4da4-89ce-7622eac08656)

Now, let's switch to the `robot` user and read the second flag.

![key2](https://github.com/MohamedAboElnasr/CTF/assets/114421344/8c4f6bf0-8e16-439c-803b-058ab9d9d4ae)

Now, it's time for the final flag, but first, we need to escalate our privileges.
After some trial and error, such as checking `sudo -l` and attempting to read `crontab`, I found that `nmap` has the SUID bit enabled.

**Note:**
SUID Bit: This is a file permission that allows users to execute the file as the owner of that file.

After discovering that `nmap` has the SUID bit, I referred to [gtfobins](https://gtfobins.github.io/gtfobins/nmap/#suid) and tried the second method:

```bash
nmap --interactive
nmap> !sh
```

![root](https://github.com/MohamedAboElnasr/CTF/assets/114421344/dac25751-cb87-4cf6-baf4-f6687487da31)

Now, we are root! Let's read the final flag.

![key3](https://github.com/MohamedAboElnasr/CTF/assets/114421344/fe844a80-09da-46cd-8e23-7c686d80fad2)

And there we go! We have all three flags. Thank you! <3
