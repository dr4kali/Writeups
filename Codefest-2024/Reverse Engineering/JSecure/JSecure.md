This challenge require some knowledge on javascript language which I didn't have but lets see what we can do. First we can take a loot it its code.
```
┌──(dr4kali㉿kali)-[~/CTF/Codefest/reversing/JSecure]
└─$ cat JSecure.js
function _0x2482(){const _0x2beee5=['fromCharCode','265488WdYeQx','table','sqrt','24BtYThv','542406ysXzox','35874ZHkLJS','length','push','round','Decoded\x20Flag:','328nWQpDy','233200vXRczB','16QUiqwi','706660eLqfGE','1545520JlWFsm','164213IXDlrr'];_0x2482=function(){return _0x2beee5;};return _0x2482();}const _0x4cfd38=_0x18fe;function _0x18fe(_0x5d78f5,_0x560707){const _0x2482ff=_0x2482();return _0x18fe=function(_0x18fe71,_0x4c4d71){_0x18fe71=_0x18fe71-0xda;let _0x3c9bda=_0x2482ff[_0x18fe71];return _0x3c9bda;},_0x18fe(_0x5d78f5,_0x560707);}(function(_0x2b49cb,_0xa700a8){const _0x2039e9=_0x18fe,_0x541567=_0x2b49cb();while(!![]){try{const _0x775573=parseInt(_0x2039e9(0xe1))/0x1+parseInt(_0x2039e9(0xdd))/0x2+-parseInt(_0x2039e9(0xe5))/0x3+-parseInt(_0x2039e9(0xdc))/0x4*(-parseInt(_0x2039e9(0xdb))/0x5)+-parseInt(_0x2039e9(0xe4))/0x6*(parseInt(_0x2039e9(0xdf))/0x7)+parseInt(_0x2039e9(0xda))/0x8*(-parseInt(_0x2039e9(0xe6))/0x9)+-parseInt(_0x2039e9(0xde))/0xa;if(_0x775573===_0xa700a8)break;else _0x541567['push'](_0x541567['shift']());}catch(_0xf84ac2){_0x541567['push'](_0x541567['shift']());}}}(_0x2482,0x33f1a));function complexMathOperation(_0x152704,_0xb5fc08){const _0x3ec09e=_0x18fe;return(_0x152704*_0x152704+_0xb5fc08*_0xb5fc08-_0x152704*_0xb5fc08)*Math[_0x3ec09e(0xe3)](_0x152704+_0xb5fc08+0x1);}function analyzeFlag(){const _0xfe3fbc=_0x18fe,_0x1ddd5f=[0x6,0x13,0x17,0x1,0x23,0x2,0xc,0x2b,0x5,0x12,0x1b,0x8],_0x3b5130=[0x56,0x45,0x43,0x54,0x52,0x41,0x7b,0x4a,0x40,0x56,0x61,0x53,0x63,0x72,0x69,0x70,0x74,0x5f,0x31,0x73,0x5f,0x65,0x41,0x73,0x79,0x7d];let _0x32fb47='',_0xadcdfb=[];for(let _0x5e5b0e=0x0;_0x5e5b0e<_0x3b5130[_0xfe3fbc(0xe7)];_0x5e5b0e++){let _0x51f074=_0x1ddd5f[_0x5e5b0e%_0x1ddd5f[_0xfe3fbc(0xe7)]],_0x2c2f67=_0x3b5130[_0x5e5b0e],_0x4a5845=Math[_0xfe3fbc(0xe9)](complexMathOperation(_0x51f074,_0x2c2f67)%0x100);_0xadcdfb[_0xfe3fbc(0xe8)]({'step':_0x5e5b0e+0x1,'x':_0x51f074,'y':_0x2c2f67,'operationResult':complexMathOperation(_0x51f074,_0x2c2f67),'charCode':_0x4a5845,'decodedChar':String[_0xfe3fbc(0xe0)](_0x4a5845)}),_0x32fb47+=String[_0xfe3fbc(0xe0)](_0x4a5845);}return console[_0xfe3fbc(0xe2)](_0xadcdfb),_0x32fb47;}console['log'](_0x4cfd38(0xea),analyzeFlag());                                                                                                        
```

Well you can see that its impossible to understand anything from this. Good thing was I knew something called javascript obfuscating exist. So I pasted the code on online deobfuscater.

![JSecure-1.png](https://github.com/dr4kali/Writeups/blob/3194502f62168de4b7d9311ed4db37d7f9178a20/Codefest-2024/Reverse%20Engineering/JSecure/JSecure-1.png)

Now this is much more readable. We can see that it logs something to the console. So i created index.php and include the js file as a javascript file. Then I open up inspection and looked at what it logs in the console.

![JSecure-2.png](https://github.com/dr4kali/Writeups/blob/3194502f62168de4b7d9311ed4db37d7f9178a20/Codefest-2024/Reverse%20Engineering/JSecure/JSecure-2.png)

It prints a flag but its not readable. But you can see in y column, the numbers are seems to  be ascii codes so lets try to decode them as ascii letters.

![JSecure-3.png](https://github.com/dr4kali/Writeups/blob/3194502f62168de4b7d9311ed4db37d7f9178a20/Codefest-2024/Reverse%20Engineering/JSecure/JSecure-3.png)

And here we go. We found another flag.
