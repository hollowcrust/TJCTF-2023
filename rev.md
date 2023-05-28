# TJCTF 2023 - Rev Challenges

## #1: scramble <br/>

### Description/Sources
  <img width="693" alt="Screenshot 2023-05-26 at 18 29 25" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/9db00df7-cf2e-4afa-8ace-ed747bf3be26">

### chal.py
```python
#first 3 lines are given
import random
seed = 1000
random.seed(seed)

#unscramble the rest
def recur(lst):
l2[i] = (l[i]*5+(l2[i]+n)*l[i])%l[i]
l2[i] += inp[i]
flag = ""
flag+=chr((l4[i]^l3[i]))
return flag
l.append(random.randint(6, 420))
l3[0] = l2[0]%mod
for i in range(1, n):
def decrypt(inp):
for i in range(n):
assert(len(l)==n)
return lst[0]
l = []
main()
def main():
l4 = [70, 123, 100, 53, 123, 58, 105, 109, 2, 108, 116, 21, 67, 69, 238, 47, 102, 110, 114, 84, 83, 68, 113, 72, 112, 54, 121, 104, 103, 41, 124]
l3[i] = (l2[i]^((l[i]&l3[i-1]+(l3[i-1]*l[i])%mod)//2))%mod
if(len(lst)==1):
assert(lst[0]>0)
for i in range(1, n):
for i in range(n):
return recur(lst[::2])/recur(lst[1::2])
print("flag is:", decrypt(inp))
l2[0] +=int(recur(l2[1:])*50)
l2 = [0]*n
flag_length = 31
mod = 256
print(l2)
n = len(inp)
inp = [1]*flag_length
l3 =[0]*n
```

### Key observations/steps
1. Separate the `def` and match the `return` calls to the corresponding functions.
2. All lines containing `lst` belong to `recur(lst)` function.
3. `flag`, `l`, `l2`, `l3`, `l4` are related to each other --> They are in the same function `decrypt(inp)`.
4. Some lists assign the 0-th element separately. Use this to determine the correct `for` loop traversals (`range(n)` or `range(1, n)`).

### Unscrambled chal.py
```python
#first 3 lines are given
import random
seed = 1000
random.seed(seed)

#unscramble the rest

def recur(lst):
    if(len(lst)==1):
        assert(lst[0]>0)
        return lst[0]
    return recur(lst[::2])/recur(lst[1::2])

def decrypt(inp):
    mod = 256
    n = len(inp)
    l = []
    for i in range(n):
        l.append(random.randint(6, 420))
    assert(len(l)==n)

    l2 = [0]*n
    for i in range(1, n):
        l2[i] = (l[i]*5+(l2[i]+n)*l[i])%l[i]
        l2[i] += inp[i]
    l2[0] +=int(recur(l2[1:])*50)
    
    l3 =[0]*n
    l3[0] = l2[0]%mod
    for i in range(1, n):
        l3[i] = (l2[i]^((l[i]&l3[i-1]+(l3[i-1]*l[i])%mod)//2))%mod

    l4 = [70, 123, 100, 53, 123, 58, 105, 109, 2, 108, 116, 21, 67, 69, 238, 47, 102, 110, 114, 84, 83, 68, 113, 72, 112, 54, 121, 104, 103, 41, 124]

    flag = ""
    
    for i in range(n):
        flag+=chr((l4[i]^l3[i]))
        
    return flag

def main():
    flag_length = 31
    inp = [1]*flag_length
    print("flag is:", decrypt(inp))

main()
```

### Flag
```
tjctf{unshuffling_scripts_xdfj}
```

## #2: wtmoo

### Description/Sources<br/>

<img width="698" alt="Screenshot 2023-05-26 at 21 31 52" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/bd25bd72-dacb-4ce7-af08-e21aaafa1014"><br/>

### Decompiled code (Hex-Rays)
```C
//Unimportant lines omitted

char *flag = "8.'8*{;8m33[o[3[3[%\")#*\\}";

//Unimportant lines omitted

int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+8h] [rbp-58h]
  int v5; // [rsp+Ch] [rbp-54h]
  char s[32]; // [rsp+10h] [rbp-50h] BYREF
  char dest[40]; // [rsp+30h] [rbp-30h] BYREF
  unsigned __int64 v8; // [rsp+58h] [rbp-8h]

  v8 = __readfsqword(0x28u);
  printf("Enter text: ");
  fgets(s, 32, _bss_start);
  s[strlen(s) - 1] = 0;
  strcpy(dest, s);
  v5 = strlen(s);
  for ( i = 0; i < v5; ++i )
  {
    if ( s[i] <= 96 || s[i] > 122 )
    {
      if ( s[i] <= 64 || s[i] > 90 )
      {
        if ( s[i] <= 47 || s[i] > 52 )
        {
          if ( s[i] <= 52 || s[i] > 57 )
          {
            if ( s[i] != 123 && s[i] != 125 )
            {
              puts("wtmoo is this guess???");
              printf("%c\n", (unsigned int)s[i]);
              return 1;
            }
          }
          else
          {
            s[i] -= 21;
          }
        }
        else
        {
          s[i] += 43;
        }
      }
      else
      {
        s[i] += 32;
      }
    }
    else
    {
      s[i] -= 60;
    }
  }
  if ( !strcmp(s, flag) )
    printf(cow, dest);
  else
    printf(cow, s);
  return 0;
}
```

### Key observations/steps
1. This is some sort of substitution cipher (or one-to-one mapping) where each character in the string is replaced by another character.
2. The characters are independent of each other, i.e. changing 1 character in the original string does not affect the remaining characters in the resulting string.
3. Run the algorithm above with `s` as the string `0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`. The encoded string becomes `[\]^_ !"#$abcdefghijklmnopqrstuvwxyz%&'()*+,-./0123456789:;<=>`.
4. Map each character in `8.'8*{;8m33[o[3[3[%")#*\}` to the decoded string and obtain the flag.

### Solution code (in C++)
```Cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  int i;
  int v5;
  string flag = "8.'8*{;8m33[o[3[3[%\")#*\\}";
  string s = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
  string s2 = s;
  v5 = 62;
  for ( i = 0; i < v5; ++i )
  {
    if ( s[i] <= 96 || s[i] > 122 )
    {
      if ( s[i] <= 64 || s[i] > 90 )
      {
        if ( s[i] <= 47 || s[i] > 52 )
        {
          if ( s[i] <= 52 || s[i] > 57 )
          {
            if ( s[i] != 123 && s[i] != 125 )
            {
              puts("wtmoo is this guess???");
              printf("%c\n", (unsigned int)s[i]);
              return 1;
            }
          }
          else
          {
            s[i] -= 21;
          }
        }
        else
        {
          s[i] += 43;
        }
      }
      else
      {
        s[i] += 32;
      }
    }
    else
    {
      s[i] -= 60;
    }
  }
  
  //Decoding flag string
  for(i = 0; i < 26; i++) {
    for(int j = 0; j < 62; j++) {
      if(flag[i] == s[j]) {
        flag[i] = s2[j];
        break;
      }
    }
  }
  
  cout << flag;
  return 0;
}
```

### Flag
```
tjctf{wtMoo0O0o0o0a7e8f1}
```

## #3: div3rev

### Description/Sources<br/>

<img width="697" alt="Screenshot 2023-05-26 at 21 34 04" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/fa81407f-40bd-455e-96f5-aaef5673a609"><br/>

### chal.py
```Python
def op1(b):
    for i in range(len(b)):
        b[i] += 8*(((b[i] % 10)*b[i]+75) & 1)
        cur = 1
        for j in range(420):
            cur *= (b[i]+j) % 420
    return b


def op2(b):
    for i in range(len(b)):
        for j in range(100):
            b[i] = b[i] ^ 69
        b[i] += 12
    return b


def op3(b):
    for i in range(len(b)):
        b[i] = ((b[i] % 2) << 7)+(b[i]//2)
    return b


def recur(b):
    if len(b) == 1:
        return b
    assert len(b) % 3 == 0
    a = len(b)
    return op1(recur(b[0:a//3]))+op2(recur(b[a//3:2*a//3]))+op3(recur(b[2*a//3:]))


flag = ""
b = bytearray()
b.extend(map(ord, flag))
res = recur(b)
if res == b'\x8c\x86\xb1\x90\x86\xc9=\xbe\x9b\x80\x87\xca\x86\x8dKJ\xc4e?\xbc\xdbC\xbe!Y \xaf':
    print("correct")
else:
    print("oopsies")
```

### Key observations/steps
1. For `op1(b)`:
    - `b[i] += 8*(((b[i] % 10)*b[i]+75) & 1)` can be intepreted as `b[i] += 8 if b[i] is even; else b[i] += 0`.
    - The `cur` is just a distraction since the function returns `b` not `cur`, and `cur` is not used elsewhere.
    - <strong>How to reverse</strong>: `b[i] -= 8 if b[i] is even; else continue`.
   
2. For `op2(b)`:
   - The `for` loop is redundant because `b[i] ^ 69 ^ 69 = b[i]`, so xor-ing `b[i]` 100 times with 69 does not change `b[i]` at all.
   - <strong>How to reverse</strong>: `b[i] -= 12`.

3. For `op3(b)`:
   - The 8th bit (counting from right to left) of the <strong>updated</strong> `b[i]` stores the parity of the <strong>original</strong> `b[i]`, i.e. (0 if even and 1 if odd).
   - <strong>How to reverse</strong>: `b[i] >> 7` gives the 8th bit of `b[i]`, and `b[i] & ((1 << k) - 1)` turns off the `(k+1)-th` bit of `b[i]`.

4. For `recur(b)`:
   - The 3 operations above are initially applied to `b` character-by-character, then groups of 3, groups of 9, etc. 
   - <strong>How to reverse</strong>: Apply the operations to the whole string, only then start recurring with smaller strings. So `recur(op1(b[0:a//3]))` will reverse the effect of `op1(recur(b[0:a//3]))` in the original code (`op2` and `op3` are also similar).

### Solution code (in Python)
```Python
def op1(b):
    for i in range(len(b)):
        b[i] -= 8 * (1 - (b[i] & 1))
    return b

def op2(b):
    for i in range(len(b)):
        b[i] -= 12
    return b


def op3(b):
    for i in range(len(b)):
        b[i] = (b[i] >> 7) + (b[i] & ((1 << 7)-1)) * 2
    return b


def recur(b):
    if len(b) == 1:
        return b
    assert len(b) % 3 == 0
    a = len(b)
    return recur(op1(b[0:a//3]))+recur(op2(b[a//3:2*a//3]))+recur(op3(b[2*a//3:]))

b = bytearray(b'\x8c\x86\xb1\x90\x86\xc9=\xbe\x9b\x80\x87\xca\x86\x8dKJ\xc4e?\xbc\xdbC\xbe!Y \xaf')
flag = recur(b).decode('utf-8')
print(flag)
```

### Flag
```
tjctf{randomfifteenmorelet}
```

## #4: maybe

### Description/Sources<br/>

<img width="697" alt="Screenshot 2023-05-27 at 11 04 14" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/105988d2-f800-4a7a-b591-d367c2c11c3e"><br/>

### Decompiled code (Hex-Rays)
```C
//Unimportant lines ommitted

_BYTE flag[29] =
{
  18,
  17,
  0,
  21,
  11,
  72,
  60,
  18,
  12,
  68,
  0,
  16,
  81,
  25,
  46,
  22,
  3,
  28,
  66,
  17,
  10,
  74,
  114,
  86,
  13,
  122,
  116,
  79,
  0
};

//Unimportant lines ommitted

int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+Ch] [rbp-64h]
  char s[72]; // [rsp+10h] [rbp-60h] BYREF
  unsigned __int64 v6; // [rsp+58h] [rbp-18h]

  v6 = __readfsqword(0x28u);
  puts("Enter your flag");
  fgets(s, 64, _bss_start);
  if ( strlen(s) == 33 )
  {
    for ( i = 4; i < strlen(s) - 1; ++i )
    {
      if ( ((unsigned __int8)s[i - 4] ^ (unsigned __int8)s[i]) != flag[i - 4] )
      {
        puts("you're def wrong smh");
        return 1;
      }
    }
    puts("you might be right??? you might be wrong.... who knows?");
    return 0;
  }
  else
  {
    puts("bad");
    return 1;
  }
}
```

### Key observations/steps
1. The flag is 33 characters long (not so important but useful when writing script). 
2. The program is checking whether `s[i-4] ^ s[i] == flag[i-4]`, starting from the 4th character (0-indexed).
3. The flag always starts with `tjctf{`, which means we can construct the whole flag using the fact that `s[i] = flag[i-4] ^ s[i-4]`.

### Solution code (in Python)
```Python
a = [18,
  17,
  0,
  21,
  11,
  72,
  60,
  18,
  12,
  68,
  0,
  16,
  81,
  25,
  46,
  22,
  3,
  28,
  66,
  17,
  10,
  74,
  114,
  86,
  13,
  122,
  116,
  79,
  0]

flag = 'tjctf{'
for i in range(6, 33):
    flag += chr(ord(flag[i-4]) ^ a[i-4])
print(flag)
```

### Flag
```
tjctf{cam3_saw_c0nqu3r3d98A24B5}
```

## #5: dream

### Description/Sources<br/>
<img width="696" alt="Screenshot 2023-05-27 at 10 58 35" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/ca457311-f6e8-44e8-9994-3a576736a9a8">

### Decompiled code (Hex-Rays)
```C
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v4; // [rsp+0h] [rbp-230h]
  int i; // [rsp+4h] [rbp-22Ch]
  unsigned __int64 v6; // [rsp+8h] [rbp-228h]
  unsigned __int64 v7; // [rsp+10h] [rbp-220h]
  FILE *stream; // [rsp+18h] [rbp-218h]
  char s2[256]; // [rsp+20h] [rbp-210h] BYREF
  char s[264]; // [rsp+120h] [rbp-110h] BYREF
  unsigned __int64 v11; // [rsp+228h] [rbp-8h]

  v11 = __readfsqword(0x28u);
  setbuf(stdout, 0LL);
  type_text("last night, I had a dream...\ntaylor sw1ft, the dollar store version, appeared!\n");
  prompt("what should I do? ", s2, 256);
  if ( strcmp("sing", s2) )
  {
    puts("no, no, that's a bad idea.");
    exit(0);
  }
  prompt("that's a great idea!\nI started to sing the following lyrics: ", s2, 256);
  if ( strcmp("maybe I asked for too [many challenges to be written]", s2) )
  {
    puts("no, that's a dumb lyric.");
    exit(0);
  }
  type_text("ok... that's a weird lyric but whatever\n");
  prompt("that leads me to ask... how many challenges did you ask for??? ", s2, 256);
  v6 = atol(s2);
  if ( 35 * (((3 * v6) ^ 0xB6D8) % 0x521) % 0x5EB != 1370 )
  {
    type_text("that's a stupid number.\n");
    exit(0);
  }
  prompt("ok yeah you're asking too much of everyone; try to lower the number??? ", s2, 256);
  v7 = atol(s2);
  if ( (35 * ((5 * v7 % 0x1E61) | 0x457) - 5) % 0x3E8 != 80 )
  {
    type_text("yeah.");
    exit(0);
  }
  if ( v6 % v7 != 20202020 || v7 * v6 != 0x33D5D816326AADLL )
  {
    type_text("ok but they might think that's too much comparatively, duh.\n");
    exit(0);
  }
  type_text("that's a lot more reasonable - good on you!\n");
  usleep(0x124F8u);
  type_text("ok, now that we've got that out of the way, back to the story...\n");
  type_text("taylor was like, \"wow, you're so cool!\", and I said, \"no, you're so cool!\"\n");
  type_text("after that, we kinda just sat in silence for a little bit. I could kinda tell I was losing her attention, so ");
  v4 = 0;
  for ( i = 0; i <= 11; ++i )
  {
    prompt("what should I do next? ", s2, 256);
    if ( !strcmp("ask her about some flags", s2) )
    {
      ++v4;
    }
    else if ( !strcmp("ask her about her new album", s2) )
    {
      v4 *= v4;
    }
    else
    {
      if ( strcmp("ask her about her tour", s2) )
      {
        type_text("no, that's weird\n");
        exit(0);
      }
      v4 += 22;
    }
  }
  if ( v4 != 2351 )
  {
    type_text("taylor died of boredom\n");
    exit(0);
  }
  type_text("taylor got sick and tired of me asking her about various topics, so she finally responded: ");
  stream = fopen("flag.txt", "r");
  if ( !stream )
  {
    type_text("no flag </3\n");
    exit(0);
  }
  fgets(s, 256, stream);
  type_text(s);
  return 0;
}
```

### Key observations/steps
1.
```C
  type_text("last night, I had a dream...\ntaylor sw1ft, the dollar store version, appeared!\n");
  prompt("what should I do? ", s2, 256);
  if ( strcmp("sing", s2) )
  {
    puts("no, no, that's a bad idea.");
    exit(0);
  }
  prompt("that's a great idea!\nI started to sing the following lyrics: ", s2, 256);
  if ( strcmp("maybe I asked for too [many challenges to be written]", s2) )
  {
    puts("no, that's a dumb lyric.");
    exit(0);
  }
```
  - For the first part, just input `sing` and `maybe I asked for too [many challenges to be written]` when prompted.<br/>

2.
```C
  prompt("that leads me to ask... how many challenges did you ask for??? ", s2, 256);
  v6 = atol(s2);
  if ( 35 * (((3 * v6) ^ 0xB6D8) % 0x521) % 0x5EB != 1370 )
  {
    type_text("that's a stupid number.\n");
    exit(0);
  }
  prompt("ok yeah you're asking too much of everyone; try to lower the number??? ", s2, 256);
  v7 = atol(s2);
  if ( (35 * ((5 * v7 % 0x1E61) | 0x457) - 5) % 0x3E8 != 80 )
  {
    type_text("yeah.");
    exit(0);
  }
  if ( v6 % v7 != 20202020 || v7 * v6 != 0x33D5D816326AADLL )
  {
    type_text("ok but they might think that's too much comparatively, duh.\n");
    exit(0);
  }
```
  - Notice that we need to find suitable `v6` and `v7` such that eventually `v6 % v7 == 20202020 && v7 * v6 == 0x33D5D816326AAD` to pass, which means `v6` and `v7` are factors of `0x33D5D816326AAD = 14590347874298541 = 3 x 3 x 29 x 37 x 409 x 11071 x 333667`.
  - To find `v6` and `v7`, I used the following script which finds all possible values of `v6` and `v7` that pass the conditions `35 * (((3 * v6) ^ 0xB6D8) % 0x521) % 0x5EB == 1370` and `(35 * ((5 * v7 % 0x1E61) | 0x457) - 5) % 0x3E8 != 80` respectively.

#### Finding ~~Nemo~~ v6 and v7 (in Python)
```Python
import itertools
factors = []
prime = [3, 3, 29, 37, 409, 11071, 333667]
for i in range(1, 8):
    factors.append(list(map(list, itertools.combinations(prime, i))))
    
prod = []
for f in factors:
    for combi in f:
        tmp = 1
        for j in combi:
            tmp *= j
        if tmp not in prod:
            prod.append(tmp)
for i in prod:
    if (35 * (((3 * i) ^ 0xB6D8) % 0x521) % 0x5EB == 1370) and ((35 * ((5 * (0x33D5D816326AAD // i) % 0x1E61) | 0x457) - 5) % 0x3E8 == 80):
        print(i, 0x33D5D816326AAD//i)
```
  - Running the script gives only one pair of `(v6, v7)` of `(131313131, 111111111)`.<br/>

3.
```C
  v4 = 0;
  for ( i = 0; i <= 11; ++i )
  {
    prompt("what should I do next? ", s2, 256);
    if ( !strcmp("ask her about some flags", s2) )
    {
      ++v4;
    }
    else if ( !strcmp("ask her about her new album", s2) )
    {
      v4 *= v4;
    }
    else
    {
      if ( strcmp("ask her about her tour", s2) )
      {
        type_text("no, that's weird\n");
        exit(0);
      }
      v4 += 22;
    }
  }
  if ( v4 != 2351 )
  {
    type_text("taylor died of boredom\n");
    exit(0);
  }
```
  - There are 3 operations applied to `v4`: `+1`, `+22`, and `squaring v4`, and we have to achieve `2351` from `0` in <strong>exactly</strong> 12 operations.
  - Using some "quick maths" `2351 = 48 ** 2 + 47 = (0 + 22 + 22 + 1 + 1 + 1 + 1) ** 2 + (22 + 22 + 1 + 1 + 1)` which uses exactly 12 operations. Hence one of the possible lists of input is
```
ask her about her tour
ask her about her tour
ask her about some flags
ask her about some flags
ask her about some flags
ask her about some flags
ask her about her new album
ask her about her tour
ask her about her tour
ask her about some flags
ask her about some flags
ask her about some flags
```
<br/>

4. Combine everything and we can obtain the flag. I recommend using `pwntools` to send the input.

### Solution code (in Python)

```Python
import itertools
from pwn import *

#Pre-calculate v6 and v7 before connecting to server.
factors = []
prime = [3, 3, 29, 37, 409, 11071, 333667]
for i in range(1, 8):
    factors.append(list(map(list, itertools.combinations(prime, i))))
    
prod = []
for f in factors:
    for combi in f:
        tmp = 1
        for j in combi:
            tmp *= j
        if tmp not in prod:
            prod.append(tmp)

v6, v7 = 0, 0

for i in prod:
    if (35 * (((3 * i) ^ 0xB6D8) % 0x521) % 0x5EB == 1370) and ((35 * ((5 * (0x33D5D816326AAD // i) % 0x1E61) | 0x457) - 5) % 0x3E8 == 80):
        v6 = i
        v7 = 0x33D5D816326AAD // i

#for part 3
op = ['ask her about some flags', 'ask her about her new album', 'ask her about her tour']
responses = [2, 2, 0, 0, 0, 0, 1, 2, 2, 0, 0, 0] 

#connecting to the server
r = remote('tjc.tf', 31500)

#part 1
r.recvuntil(b'what should I do? ')
r.sendline(b'sing')
r.recvuntil(b'I started to sing the following lyrics: ')
r.sendline(b'maybe I asked for too [many challenges to be written]')

#part 2
r.recvuntil(b'that leads me to ask... how many challenges did you ask for??? ')
r.sendline(str(v6).encode('utf-8'))
r.recvuntil(b'ok yeah you\'re asking too much of everyone; try to lower the number??? ')
r.sendline(str(v7).encode('utf-8'))

#part 3
for i in range(12):
    r.recvuntil(b'what should I do next? ')
    r.sendline(str(op[responses[i]]).encode('utf-8'))
    
#get flag
r.recvuntil(b'taylor got sick and tired of me asking her about various topics, so she finally responded: ')
flag = r.recvline()
print(flag)

r.close()
```
<img width="460" alt="Screenshot 2023-05-27 at 10 58 09" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/50229faa-8e15-47c5-b5bf-52fddc270c67">

### Flag
```
tjctf{5h3_ju5t_w4nt5_t0_st4y_1n_th4t_l4vend3r_h4z3_3a17362a}
```
