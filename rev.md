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

### Description/Sources

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

### Key observations/steps:
1. This is some sort of substitution cipher (or one-to-one mapping) where each character in the string is replaced by another character
2. The characters are independent of each other, i.e. changing 1 character in the original string does not affect the remaining characters in the resulting string.
3. Run the algorithm above with `s` as the string `0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`. The encoded string becomes `[\]^_ !"#$abcdefghijklmnopqrstuvwxyz%&'()*+,-./0123456789:;<=>`
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
      if(s[j] == flag[i]) {
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
