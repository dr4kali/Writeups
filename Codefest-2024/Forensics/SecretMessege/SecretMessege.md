In this challenge we are given pcap file. We know that we can open them in wireshark. So lets try doing so. 

![secret_messege-1.png](https://github.com/dr4kali/Writeups/blob/a01ca88547d1c7eecdbe666aee8c5fba1ae8194e/Codefest-2024/Forensics/SecretMessege/secret_messege-1.png)

Well its kind of embarassing because its my first time seeing USB protocol in wireshark and I had no idea how to proceed. But there is one thing i could try. Since I had already solved few challenges I knew that flags look like `VECTRA{Some_Jibberish}` . I did look up on packet bytes with `VECTRA`  as a strings.

![[secret_messege-2.png]]

We found another flag. I am not sure if this was the intended way. but anyway we got the flag.
