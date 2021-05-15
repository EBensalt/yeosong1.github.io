# 백준 단계별 
https://www.acmicpc.net/step


## 입출력 사칙연산

### 2439 별찍기2

```py
import sys
input = sys.stdin.readline
n=int(input())

for i in range(1,n+1):
  print(("*"*i).rjust(n))
```
우측정렬



## while

### 10951 eof

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

### 1110

```py
import sys
input=sys.stdin.readline
n=int(input())
origin=n
count=0
while 1:
  if (n<10):
    n = n%10*10 + (n//10 + n%10)
  else: 
    n = n%10*10 + (n//10 + n%10)%10
  count+=1
  if (origin==n):
    break

print(count)
```
## 1차원 배열

### 10818

```py
import sys
input=sys.stdin.readline
n=int(input())
a = list(map(int,input().split()))
print(min(a), end=' ')
print(max(a), end=' ')
```

### 2562

```py
import sys
input=sys.stdin.readline
a=[]
for i in range(9):
    a.append(int(input()))
print(max(a))
print(a.index(max(a))+1)
```

### 2577 0~9 사용 횟수

```py
import sys
input=sys.stdin.readline
n=[]
for _ in range(3):
  n.append(int(input()))
n =n[0] * n[1] * n[2]    # n = int 17037300  (150*266*427) 
s=repr(n)                # s = str 17037300  숫자(int)를 str로 바꾸기
ret=[0]*10
for i in range(len(s)):  # 해당 글자가 들어있는 수만큼을 해당 숫자가 대표하는 인덱스에 넣기
  ret[int(s[i])]=s.count(s[i])
for i in ret:
  print(i)
```

### 3052 10개의 나머지 종류

```py
import sys
input=sys.stdin.readline
a=[]
for i in range(10):
  a.append(int(input())%42)
a=set(a)
print(len(a))
```

### 평균 1546

```py
import sys
input=sys.stdin.readline

n = int(input())
a=list(map(int, input().split()))
M=max(a)

new=[] # 밑에서 i=i/M*100 이렇게 하니까 값이 할당은 되는데 for문 나가서 보면 안바뀌어있어서.. 새로 만듦.. 
for i in a:
  new.append(i/M*100)
print(sum(new)/n)
```

### OX 퀴즈!!!!! 8958
```py
import sys
input=sys.stdin.readline

n = int(input())
#풀이가 다 받아서 나눠서 출력하는게 아니라 -> 한 줄 받고 한 줄 출력해도 되네?...
for i in range(n):
  line=input()
  s=list(line)
  score=0
  total=0
  for j in s:
    if j == 'O':
      score+=1
    else :
      score=0
    total+=score
  print(total)

# 이건 다른 사람 풀이
# a = int(input())
# for i in range(a):
#     b = input()
#     s = list(b)
#     sum = 0
#     c = 1
#     for i in s:
#         if i == 'O':
#             sum += c
#             c += 1
#         else:
#             c = 1
#     print(sum)
  
```
