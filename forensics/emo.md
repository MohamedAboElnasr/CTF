## emo challange from hackthebox
- at first i downloaded the file and the descryption was as follows
*WearRansom ransomware just got loose in our company. The SOC has traced the initial access to a phishing attack, a Word document with macros. Take a look at the document and see if you can find anything else about the malware and perhaps a flag.*

so as the description says it's a word document conatins a macro script or VBS, i wanted to open the file with office or microsoft word on my vm but i don't have them installed on my linux vm, so i deided to open the file using google docs
![alt text](https://github.com/MohamedAboElnasr/CTF/assets/114421344/4e40e025-4c62-44f1-9655-b1cad08dc382)

after i found an image inside the document the first thing that came into my mind is to try to extract the photo metadata maybe i could find the flag there but no :"(

after some researching i found that there are tools for analyzing documents which can be found inside oletools python package
you can install the package using the following command
`sudo -H pip3 install -U oletools`
i started by using oleid to see if the file contains vbs or not just to make sure
![alt text](https://github.com/MohamedAboElnasr/CTF/assets/114421344/252ee383-43ae-456c-a18b-707c2e35cf24)



and i found that there is a VBS script, now we need to dump the script and for that i used olevdb 

![alt text](https://github.com/MohamedAboElnasr/CTF/assets/114421344/35a4d9ec-ca5e-4dcb-ab66-5751f6e5fff9)






