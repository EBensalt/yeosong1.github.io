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

### 1546 평균

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

### 8958 OX 퀴즈!!!!! 
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

## 함수

### 15596 n 정수 합

```py
def solve(a):
  return sum(a)
```

### 4673 셀프 넘버

몰라서 풀이 찾아봤지여 https://onmyojiloves2.postype.com/post/7228327

```py
import sys
input=sys.stdin.readline

natural_number_set = set(range(1, 10001)) 
generated_number_set= set()#셀프 넘버가 아닌 수. 중복 제거를 위해 집합에 넣음

for i in range(1, 10001):
  for j in str(i): # str(i)하면 숫자 i를 스트링으로 봐서 각 자리수 계산에 편리. 예) 14면
    i+= int(j) # 14+1 , 15(방금 더해진 값)+4 이렇게 총 두 번 돌아서 19가 된다 
  generated_number_set.add(i) # 만든 수

self_number_set = natural_number_set - generated_number_set
# {범위 내에 존재하는 모든 수}에서 (만든 수) = (만들어낼 수 있었던 수) = (셀프넘버가 아닌 수)를 뺀다

for i in sorted(self_number_set): #오름차순으로 정렬, 원소 하나씩 print 1번(=자동 줄바꿈)
     print(i)
```

#### 풀이에 sorted를 쓰네.. sorted와 sort의 차이는?

- list.sort() - 리스트 멤버 함수. 해당 리스트 자체의 순서를 바꿈
- sorted() - 파이썬 내장 함수. 정렬한 새로운 객체를 반환

### 1065 한수

```py
import sys
input=sys.stdin.readline
count=0
n=int(input())
for i in range(1,n+1):
  if 1<=i<=99:
    count=i # 1부터 99는 모두 한수
  if 100<=i<=999:
    if (i//100 - (i%100)//10) == ((i%100)//10 - i%100%10) :
      count+=1 #(백의자리-십의자리 == 십의자리-일의자리)면 count+=1
  if i == 1000:
    count+=0
print(count)
```

### 10809 알파벳 찾기

주의) 어느 기준 집합에서 무엇을 찾는지 머릿속에 그룹을 시각적으로든 뭐든 확실히 분리하고 연관관계를 보고 변수 만들기
어느 배열의 순서를 따라 어느 쪽 배열이 따라오는지?

```py
str=input()
alpha=list(range(97,123))

for i in alpha:
    print(str.find(chr(i)), end=' ')
```

