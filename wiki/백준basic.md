# 백준 단계별 
https://www.acmicpc.net/step


## 2439 별찍기2

```py
import sys
input = sys.stdin.readline
n=int(input())

for i in range(1,n+1):
  print(("*"*i).rjust(n))
```
우측정렬



## 10951 eof

```
# 1번 방법 try except

import sys
input=sys.stdin.readline
l = []
while 1:
  try:
    a,b=map(int,input().split())
    l.append(a+b)
  except:
    break

for i in l:
  print(i)


# 2번 방법 sys.stdin

import sys
input=sys.stdin.readline
l = []
for i in sys.stdin:
    a,b=map(int, i.split())
    l.append(a+b)

for i in l:
  print(i)
```


import sys
input=sys.stdin.readline
n=int(input())
origin=n
count=0
while 1:
  n=(n%10)+((n//10) + (n%10))
  count+=1
  if (origin==n):
    break

print(count)
gksmswnd....
https://www.acmicpc.net/problem/1110
부터 다시

