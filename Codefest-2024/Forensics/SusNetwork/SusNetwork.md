This was another pcap file which means we gotta use wireshark again. After opening it and i went through it and found some ICMP packets going back and forth. The amount of ICMP messeges that were sent was kind of suspicious and there was a 1 byte of data sent with with each ICMP requests making it highly suspicious. 

![[susnetwork-1.png]]

So I used tshark to gather all that data sent through.
```
┌──(dr4kali㉿kali)-[~/CTF/Codefest/Forensics/SusNetwork]
└─$ tshark -r SusNetwok.pcap -Y icmp -T fields -e data | tr '\n' ' '
56 56 6b 6b 56 56 44 44 56 56 46 46 4a 4a 42 42 65 65 30 30 6c 6c 44 44 54 54 56 56 42 42 66 66 53 53 7a 7a 46 46 73 73 53 53 56 56 39 39 4e 4e 4d 4d 33 33 4d 4d 31 31 5a 5a 57 57 64 64 6c 6c 66 66 51 51 3d 3d 3d 3d                                                                                                         
```

Each byte is mentioned twice since both ICMP request and reply contains it. So I removed the duplicated and pasted them cyberchef to decode them from hex.

![[susnetwork-2.png]]

The decoded strings looks kind of looks like a base64 string. So i decided use base64 decoder on top of hex decoder.

![[susnetwork-3.png]]

And Voila! We solved another mystery.