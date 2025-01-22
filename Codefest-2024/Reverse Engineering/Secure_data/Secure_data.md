For this I used GDB debugger tool to analyse what the binary is doing.
If we look at the functions with `info functions` there is an interesting function.

```
(gdb) info functions
All defined functions:

Non-debugging symbols:
0x0000000000001000  _init
0x0000000000001030  puts@plt
0x0000000000001040  printf@plt
0x0000000000001050  snprintf@plt
0x0000000000001060  __cxa_finalize@plt
0x0000000000001070  _start
0x00000000000010a0  deregister_tm_clones
0x00000000000010d0  register_tm_clones
0x0000000000001110  __do_global_dtors_aux
0x0000000000001150  frame_dummy
0x0000000000001159  secret_function
0x00000000000011df  main
0x0000000000001208  _fini

```

We can set a  break at main function then and then look what happens inside. I will use layout asm as well to get a better visual.

```
(gdb) break main
Breakpoint 1 at 0x11e3
(gdb) layout asm

```

You will something like this.
![secure_data-1.png](https://github.com/dr4kali/Writeups/blob/c4af821c5d6dfa1ed394d05ae570a7f1c38bc08f/Codefest-2024/Reverse%20Engineering/Secure_data/secure_data-1.png)

As you can see the secret_function is not called in the main or from anywhere. May be if we manually call it we could get the flag.
the secret_function starts at 0x555555555159 so lets try calling it.

```
call ((void (*)())0x555555555159)()
```

![secure_data-2.png](https://github.com/dr4kali/Writeups/blob/c4af821c5d6dfa1ed394d05ae570a7f1c38bc08f/Codefest-2024/Reverse%20Engineering/Secure_data/secure_data-2.png)

And Boom we got another flag.
