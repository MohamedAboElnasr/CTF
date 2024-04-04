# Empire
in this write up i will share how i solved Empire machine from Icex64 on vulnhub, which can be found [here]('https://www.vulnhub.com/entry/empire-lupinone,750/')

1. Information Gathering
   * at first i started by doing host host discovery on my network by using nmap
     ```sh 
        nmap -sn 192.168.1.1/24
     ```
  and i came p with this result 
  <div>
    <img src="./Empire/nmap.png" highet="500" width="500">
  </div>