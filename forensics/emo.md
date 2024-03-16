## emo challange from hackthebox
- at first i downloaded the file and the descryption was as follows
*WearRansom ransomware just got loose in our company. The SOC has traced the initial access to a phishing attack, a Word document with macros. Take a look at the document and see if you can find anything else about the malware and perhaps a flag.*

As indicated by the description, the file was a Word document containing a macro script or VBS. Since I didn't have Office or Microsoft Word installed on my Linux VM, I opted to open the file using Google Docs.
![alt text](https://github.com/MohamedAboElnasr/CTF/assets/114421344/4e40e025-4c62-44f1-9655-b1cad08dc382)

after i found an image inside the document the first thing that came into my mind is to try to extract the photo metadata maybe i could find the flag there but no :"(

after some researching i found that there are tools for analyzing documents which can be found inside oletools python package
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

i used any-run website which is used for analyzing all the traffic in the world and captures malicious traffic and i found our script is pressent there <br>
![alt text](https://github.com/MohamedAboElnasr/CTF/assets/114421344/307239b3-5cc6-487e-a387-8c407bac0b7c)<br>
i copied the powershell script and i opend CyberChef to decode the script and remove the nullbytes from it <br>
![5](https://github.com/MohamedAboElnasr/CTF/assets/114421344/a5174d10-3413-4aaf-86e9-93511d23de76)<br>
so as we see it's stil obfuscated so i deofuscated the script by removing the ';' and replace it with a new line<br>
and as a final result i got the script and i'm only intersted in these two lines, it's an foreach that iterates throw an array and xor the elements of it with 0xdf <br>
![6](https://github.com/MohamedAboElnasr/CTF/assets/114421344/ecc332e7-6bf0-4f9b-8248-459f58621905)

so i asked our beloved chatgpt to make me a script to do this task for me just by instructor him to xor the elements and convert them to ascii, and as a final result i got this script
 






