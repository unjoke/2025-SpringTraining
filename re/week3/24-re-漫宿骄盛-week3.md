<h1 id="B2jxC">FishingKit</h1>
<font style="color:#262626;">一个钓鱼佬坐在河边准备钓鱼，从工具箱中掏出了钓鱼竿、钓鱼线，但是钓了半天一条鱼都没有。诶？似乎少了什么… …</font>

<h2 id="BkVxv"><font style="color:#262626;">错误WP：</font></h2>
![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1741077520835-3f8a8b48-4570-479f-a029-5435f2118958.png)  
注：函数名都是我的改的，原来的都是数字

先接受一个数据，然后送入getkey函数中。

![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1741077647120-e1b3d3fd-2ec9-4645-a40b-b03cf9723875.png)

一大串条件，经典的可以直接用Z3解决的问题：

```cpp
from z3 import *

a=[Int(f'a{i}')for i in range(10)]
s=Solver()
s.add(202*a[8] + 216*a[5] -4*a[4] -330*a[9] -13*a[4] -268*a[6] == -14982)
s.add(325*a[8] +195*a[0] +229*a[1] -121*a[6] -409*a[6] - (a[1] *128) ==22606)
s.add(489*a[1] +480*a[6] +105*a[2] +367*a[3] -135*a[4] -482*a[9] ==63236)
s.add(493*a[1] -80*a[4] -253*a[8] -121*a[2] -177*a[0] -243*a[9] ==-39664)
s.add(275*a[4] +271*a[6] +473*a[7] -72*a[5] -260*a[4] -367*a[4] ==14255)
s.add(286*a[0] +196*a[7] +483*a[2] +442*a[1] -495*a[8] -351*a[4] ==41171)
s.add(212*a[2] +283*a[7] -329*a[8] -429*a[9] -362*a[2] -261*a[6] ==-90284)
s.add(456*a[5] +244*a[7] +92*a[4] +348*a[7] -225*a[1] -31*a[2] ==88447)
s.add(238*a[9] +278*a[7] +216*a[6] +237*a[0] +8*a[2] -17*a[9] ==83838)
s.add(323*a[9] +121*a[1] +370*a[7] - (a[4] * 64) -196*a[9] -422*a[0] ==26467)
s.add(166*a[9] +90*a[1] +499*a[2] +301*a[8] -31*a[2] -206*a[2] ==88247)
s.add(355*a[0] +282*a[4] +44*a[9] +359*a[8] -167*a[5] -62*a[3] ==76658)
s.add(488*a[6] +379*a[9] +318*a[2] -85*a[1] -357*a[2] -277*a[5] ==35398)
s.add(40*a[0] +281*a[4] +217*a[5] -241*a[1] -407*a[7] -309*a[7] ==-35436)
s.add(429*a[3] +441*a[3] +115*a[1] +96*a[8] +464*a[1] -133*a[7] ==157448)
if s.check() == sat:
    m= s.model()
    key = bytes([m[i].as_long() for i in a])
    print(key.decode())
```

key:DeluxeBait

然后接着分析RC4函数：  
![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1741078166483-e0bd8891-8e59-43f4-8891-642dde582b7c.png)

ksa函数：  
![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1741078208192-75033746-c7d3-482d-8f79-5883cf647aab.png)

与标准的rc4加密没有什么区别，除了最后加了个异或

```cpp

import base64,urllib.parse
a = bytes([0xE9, 0x37, 0xF8, 0xE2, 0x0C, 0x0F, 0x3D, 0xB9, 0x5C, 0xA3, 
  0xDE, 0x2D, 0x55, 0x96, 0xDF, 0xA2, 0x35, 0xFE, 0xB3, 0xDD, 
  0x7F, 0x91, 0x3C])
key = "DeluxeBait"
s_box = list(range(256))
j = 0
for i in range(256):
    j = (j + s_box[i] + ord(key[i % len(key)])) % 256
    s_box[i], s_box[j] = s_box[j], s_box[i]
res = []
i = j = 0
for s in a:
    i = (i + 1) % 256
    j = (j + s_box[i]) % 256
    s_box[i], s_box[j] = s_box[j], s_box[i]
    t = (s_box[i] + s_box[j]) % 256
    k = s_box[t]
    res.append(s ^ k^0x14)
cipher = bytes(res)
print(cipher)
```

<h2 id="iKPHp">结果：NSSCTF{Fake!Fake!Fake!}</h2>
<h1 id="rOIcj">正确WP：</h1>
经过出题人提示，用动态调试：

于是采取了穷举法，将所有出现的函数的起点都加上了断点，然后调试

![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1741620378399-8dd4177b-202e-4770-8c15-bd47fc85b3c9.png)

然后发现在RC4加密后隐藏了一个函数：

![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1742826199374-35742f04-66d2-4e78-93ed-edfce93ef9c5.png?x-oss-process=image%2Fformat%2Cwebp)

一个典型的XTEA加密，然后将加密的结果与buf做比较

![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1742826465462-729f2e2a-8df4-456b-8500-0ced9889531d.png)

这里直接看buf的内容是错的，耗费了好多时间，需要对其引用才能看到正确的东西：

![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1742826558027-68f065ae-894c-48e3-a8c2-33fef209281e.png)

现在要知道密钥和密文

通过看栈，可以得知，将第一次输入的作为密钥，第二次输入的作为密文：

![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1742826324914-fea76387-fb5e-4456-811e-233ecc63e656.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49936689/1742826336148-d5a81179-1632-43d5-8c43-9df52d90b037.png)

```c
#include <iostream>
#include <cstdint>
void tea(uint32_t v[2], uint32_t key[4]){
	int i;
	uint32_t v0=v[0],v1=v[1],delta=1719109785,sum=delta*24;
	for (i = 0; i < 24; i++)
    {
        v1 -= (((v0 << 4) ^ (v0 >> 5)) + v0) ^ (sum + key[(sum >> 11) & 3]);
        sum -= delta;
        v0 -= (((v1 << 4) ^ (v1 >> 5)) + v1) ^ (sum + key[sum & 3]);
    }
    v[0] = v0;
    v[1] = v1;
}
int main(){
	uint8_t _V[24];
	memcpy(_V, "!V", 2);
  _V[2] = 0x97;
  _V[3] = 0xA6;
  _V[4] = 26;
  _V[5] = -43;
  _V[6] = -60;
  _V[7] = -34;
  _V[8] = -92;
  _V[9] = -100;
  _V[10] = -126;
  _V[11] = 77;
  _V[12] = -47;
  _V[13] = 69;
  _V[14] = 0xC8;
  _V[15] = 86;
  _V[16] = -89;
  _V[17] = -76;
  _V[18] = -106;
  _V[19] = 92;
  _V[20] = 77;
  _V[21] = 73;
  _V[22] = -121;
  _V[23] = 32;
  uint8_t key[]="DeluxeBait\0\0\0\0\0\0";
  tea((uint32_t*)(_V),(uint32_t*)key);
  tea((uint32_t*)(_V+8),(uint32_t*)key);
  tea((uint32_t*)(_V+16),(uint32_t*)key);
  printf("%s", _V);
}

```



