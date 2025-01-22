For this challenge I decided to use ltrace as it allows me to see the library callings in the binary. 

```
┌──(dr4kali㉿kali)-[~/…/Codefest/reversing/VaultKeeper/vault_keeper]
└─$ ltrace ./vault_keeper
<SNIP>

stringIS4_S5_T1_EE(0x5622742a6160, 0x7fff600d8490, 0x7f4777a5c310, 1024Enter password to unlock the safe: AAAAA
) = 0x5622742a6160
memcpy(0x7fff600d8450, "P0aa3kj1a123", 12)                                                                                                       = 0x7fff600d8450
isalpha(80, 0x316a6b3361613050, 12, 0x33323161316a6b33)                                                                                          = 1024
<SNIP>                                                           
```

As you can see there is a sus string in memcpy function. Lets  insert that as the password to see what happens.
```
┌──(dr4kali㉿kali)-[~/…/Codefest/reversing/VaultKeeper/vault_keeper]
└─$ ltrace ./vault_keeper
<SNIP>

memcmp("P0aa3kj1a123", "C0nn3xw1n123", 12)                                                                                                       = 13
<SNIP>          
```

Well now we another string which is kind of suspicious. Lets see what  happen if we enter that.

```
┌──(dr4kali㉿kali)-[~/…/Codefest/reversing/VaultKeeper/vault_keeper]
└─$ ./vault_keeper 
Enter password to unlock the safe: C0nn3xw1n123
Access granted! Flag: VECTRA{S3cure_Fl@g_F0und_1t}
                                                   
```

Boom! We got the flag.