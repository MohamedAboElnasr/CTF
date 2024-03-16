## emo challange from hackthebox
- at first i downloaded the file and the descryption was as follows
*WearRansom ransomware just got loose in our company. The SOC has traced the initial access to a phishing attack, a Word document with macros. Take a look at the document and see if you can find anything else about the malware and perhaps a flag.*

As indicated by the description, the file was a Word document containing a macro script or VBS. Since I didn't have Office or Microsoft Word installed on my Linux VM, I opted to open the file using Google Docs.
![alt text](https://github.com/MohamedAboElnasr/CTF/assets/114421344/4e40e025-4c62-44f1-9655-b1cad08dc382)

after i found an image inside the document the first thing that came into my mind is to try to extract the photo metadata maybe i could find the flag there but no :"(

after some researching i found that there are tools for analyzing documents which can be found inside oletools it's a python package,
you can install the package using the following command
`sudo -H pip3 install -U oletools`<br>
My initial step involved using oleid to check for the presence of VBS content within the file.
<br>
<br>
![alt text](https://github.com/MohamedAboElnasr/CTF/assets/114421344/252ee383-43ae-456c-a18b-707c2e35cf24)
<br>
<br>

After stumbling upon a VBS script, our task was to dump its contents. This was accomplished using olevdb.
<br>
![alt text](https://github.com/MohamedAboElnasr/CTF/assets/114421344/35a4d9ec-ca5e-4dcb-ab66-5751f6e5fff9)
<br>
Our analysis unveiled an obfuscated PowerShell script within the file, which is nonsense.<br>

Through the Any.Run website, a platform that monitors global traffic and detects malicious behavior, I identified our script among the captured data. <br>
![alt text](https://github.com/MohamedAboElnasr/CTF/assets/114421344/307239b3-5cc6-487e-a387-8c407bac0b7c)<br>
After copying the PowerShell script, I utilized CyberChef to decode the script and eliminate any null bytes. <br>
![5](https://github.com/MohamedAboElnasr/CTF/assets/114421344/a5174d10-3413-4aaf-86e9-93511d23de76)<br>
Even after initial attempts, the script persisted in its obfuscated form. To deobfuscate it, I replaced ';' with new lines<br>
After the final process, I extracted the script and focused on two specific lines: a 'foreach' loop that iterates through an array, XORing its elements with 0xdf. <br>
![6](https://github.com/MohamedAboElnasr/CTF/assets/114421344/ecc332e7-6bf0-4f9b-8248-459f58621905)

Afterwards, I requested our reliable ChatGPT to create a script for me, instructing it to XOR the elements and convert them to ASCII. The resulting script led me to the flag.<br>
![flag](https://github.com/MohamedAboElnasr/CTF/assets/114421344/b16b890f-dc6f-4b71-b8ae-10cc86b5baa5)
<br>
thanks for reading.
 






