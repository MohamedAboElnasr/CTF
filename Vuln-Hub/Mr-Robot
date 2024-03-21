
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

