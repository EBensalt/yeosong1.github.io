# 백준 단계별 
- https://www.acmicpc.net/step

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

```py
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

for i in sorted(self_number_set): #오름차순(1,2,3..)으로 정렬, 원소 하나씩 print 1번(=자동 줄바꿈)
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

## 문자열

### 10809 알파벳 찾기

주의) 어느 기준 집합에서 무엇을 찾는지 머릿속에 그룹을 시각적으로든 뭐든 확실히 분리하고 연관관계를 보고 변수 만들기
어느 배열의 순서를 따라 어느 쪽 배열이 따라오는지?

```py
str=input()
alpha=list(range(97,123))

for i in alpha:
    print(str.find(chr(i)), end=' ')
```

### 2675 문자열 반복

```py
n=int(input())

for i in range(n):
  cnt,word=input().split() # 이 부분 쪼개 받을 생각을 못했네? 한 배열로 받아서 c[0]*c[i] 하려고 했는데 타입이 다르면 좋으니까 쪼개 받는게 낫네 
  for j in word:
    print(j*int(cnt), end='')
  print()
```

### 1157 가장 많이 나온 알파벳 - upper로 필터링, count, max, index() 함수

```py
#타인 풀이 
import sys
input = sys.stdin.readline
word=input().strip().upper() #대문자로 통일
#           ~~~~~~~~ readline 쓰고 split 없을 때 주의
set_word=list(set(word)) #중복제거
tmp=[]
for i in set_word:
  tmp.append(word.count(i)) #글자순으로 총 개수 저장
if tmp.count(max(tmp)) > 1:
  print('?')
else:
  x=tmp.index(max(tmp))
  print(set_word[x])
  
#나의 1번 풀이
word=input() # ex) bBBba
alpha=list(range(65,92)) # ABCDEFGHIJK..
countlist=[0]*27 # ex) 나중에 14000000000..이 됨
j=0
for i in alpha:
  countlist[j]+=word.count(chr(i)) # word에 A가 있으면 cl에 총 개수 넣기
  countlist[j]+=word.count(chr(i+32)) # word에 a가 있으면 cl에 총 개수 넣기
  j+=1 # 다음 글자 B로

if countlist.count(max(countlist)) > 1: # 최대값 같은 게 여러개면
  print('?')
else: # 유일한 최대값이면
  print(chr(countlist.index(max(countlist))+65)) # 최대값이 있는 위치 + 65 = 문자
```

### 2908 거꾸로 숫자를 읽는 상근이 동생 - 받은 내용을 역순 정렬하는 표현 `a[::-1]`

```py
import sys
input=sys.stdin.readline
a,b=input().split()
a=a[::-1]
b=b[::-1]
if a > b:
  print(a)
else:
  print(b)
 
# print(max(input()[::-1].split())) 로 한줄 가능..
```

### 5622 옛날 전화기 다이얼 돌리기 - 규칙성 잡기

```py
alpabet_list = ['ABC','DEF','GHI','JKL','MNO','PQRS','TUV','WXYZ']
word = input()

time = 0
for unit in alpabet_list :  
    for i in unit:  # alpabet 리스트에서 각 요소를 꺼내서 한글자씩 분리
        for x in word :  # 입력받은 문자를 하나씩 분리
            if i == x :  # 두 알파벳이 같으면
                time += alpabet_list.index(unit) +3  # time = time + index +3
print(time)


# 약간 다르게
import sys
input=sys.stdin.readline
dial = ['ABC', 'DEF', 'GHI', 'JKL', 'MNO', 'PQRS', 'TUV', 'WXYZ']
str = input()
count = 0
for char in range(len(str)):
    for button in dial:
        if str[char] in button:
        #           ~~~~ 이 표현 처음 써봄.........
        #                정확하게 'ABC'가 있는지 묻는게 아니고 'B'있는지 묻는 건데도 되네...
            count += dial.index(button)+3
print(count)
```
(글자순 ---> 걸리는 시간 증가) 식으로 증가하는 규칙이 있으므로(무작위가 아니므로),
<br>굳이 걸리는 시간을 딕셔너리로 짝지어주지 않아도, 순서대로 넣으면
<br>인덱스 값을 활용해 count 계산을 마칠 수 있다.

### 2947 크로아티아 알파벳 - 문자열 검색 치환 replace()

```py
import sys
input=sys.stdin.readline

a = ['c=', 'c-', 'dz=', 'd-', 'lj', 'nj', 's=', 'z=']
str = input().strip()
for i in a:
  str=str.replace(i, 'T') # 실제 글자는 세지않고, 동일한 글자만 찾아서
  # 총 몇 글자인지만 세므로!! 임의의 문자 T로 변경후 길이만 잰다.
print(len(str))
```

### 1316 그룹단어 체커 - find()는 찾아낸 글자가 가장 처음 등장하는 위치를 리턴한다.

```py
result = int(input())
for _ in range(result):
    word = input()
    for i in range(1, len(word)):
        if word.find(word[i-1]) > word.find(word[i]): #abcabc에서 ca부분 같은 경우, c의find()는 2 > a의 find()는 0 
            result -= 1
            break
print(result)
```
다른 풀이 https://leedakyeong.tistory.com/entry/%EB%B0%B1%EC%A4%80-1316%EB%B2%88-%EA%B7%B8%EB%A3%B9-%EB%8B%A8%EC%96%B4-%EC%B2%B4%EC%BB%A4-in-python

## 기본수학1

### 1712 손익분기점

- 노트북을 x대 만들어 팔면 내 재정상황은 cx-(a+bx)이다.
  - (판매금액c * 판매대수) - (고정비용 + (가변비용 * 생산대수))
  1. 이 수익 cx-(a+bx)이 최초로 양수가 되게 하는 x를 찾기?
  2. 항을 정리하면 x=a/(c-b)가 된다. 0.01이라도 양수면 손익분기점이다
  3. 근데 0.1대를 팔 수는 없다. 손익분기점이되는 판매량을 구하는 거니까 x는 1 이상의 자연수여야 한다.
  4. 분모(c-b)가 0 이하면 수익 1 달성이 안된다 

```py
a,b,c=map(int,input().split())
if c <= b:  # 4번 부분에 대한 조건이다.
  print(-1)
else:
  print(int(a/(c-b))+1) #3번 부분을 충족시키기 위해 1대를 더해준다.
```

### 1193 분수찾기
https://leedakyeong.tistory.com/entry/%EB%B0%B1%EC%A4%80-1193%EB%B2%88-%EB%B6%84%EC%88%98%EC%B0%BE%EA%B8%B0-in-python-%EC%89%BD%EA%B2%8C%EC%84%A4%EB%AA%85%ED%95%98%EA%B8%B0

와 풀었다₩1ㅉ#다21ㅣjr2tq2l3i;rv;o1,c2ㅏ다1~!ㅣㅁㄷ주

공식 만드는 건 간단한데 규칙성 찾는 데에 오래 걸렸다.

```py
x a b
1  1 1
------
2  1 2
3  2 1
------
4  3 1
5  2 2 
6  1 3
------
7  1 4 
8  2 3 
9  3 2
10 4 1
------
11 5 1
12 4 2
13 3 3
14 2 4 
15 1 5
------
16 1 6
17 2 5
18 3 4
.
.
.
```

- 이렇게 입력에 따른 출력되어야하는 값을 쓰고 보면
- 그룹이 나눠지는데
- 그룹이 홀수번째 그룹이면 a 값이 (원소 개수)부터 1까지 1씩 작아지고
- 그룹이 짝수번째 그룹이면 a 값이 1부터 (원소 개수)까지 1씩 커진다.


```py
x=int(input()) #입력 받은 수
g_end=0 #그룹에서 가장 큰 수
g_count=0 #몇번째 그룹
while x > g_end: 
  g_end+=g_count 
  g_count+=1
g_count = g_count-1 if g_end>=x else g_count 
arr=[i for i in range(1, g_count+1)]
a=arr[g_end-x] if g_count % 2 else arr[::-1][g_end-x]
b=arr[::-1][g_end-x] if g_count % 2 else arr[g_end-x]
print(a,'/', b,sep='')
```

### 2869 달팽이

```py
a,b,v=map(int,(input().split()))
k = (v-b)/(a-b)
k = (v-b)//(a-b)+1 if k%1 else (v-b)//(a-b)
print (k)
```

0. a낮 b밤 v목표
1. 시간 제한이 있는 문제들은 1씩 더해가지고는 시간 안에 풀 수 없다
2. 낮과 밤이 돌아오는 주기를 생각해서 `V = ax-b(x-1)`이 된다. x는 날짜 수고, 밤은 1개 덜 돌아오니까 b에는 x-1을 곱한다.
3. 소숫점이 남으면 하루 하고 조금 더 걸리는 거니까, 소수점 부분을 떼고 1을 더해서 하루 단위로 만들어준다. 

### 10250 ACM 호텔

[https://nirsa.tistory.com/98](https://nirsa.tistory.com/98)

```py
for _ in range(int(input())):
  h,w,n=map(int,input().split())
  x=n%h if n%h else h
  y=n//h + 1 if (n/h)%1 else n//h
  print(x*100+y)
```
1. 처음에 나머지가 0일때 예외처리 안해서 틀렸고
2. 포맷 스트링 형식`print(x,'{0:02d}'.format(y),sep='')`으로 썼는데 숫자로 쓰는 것이 더 변수가 적고 확실함

### 2775 부녀회장

```py
import sys
input=sys.stdin.readline
for _ in range (int(input().strip())):
  k=int(input().strip())
  n=int(input().strip())
  base=[i for i in range(1,n+1)]
  for _ in range(k): #0층은 채웠고, 나머지 층수만큼 반복
    for m in range(1,n): #1호는 1이고, 2호부터 목표 호수 n까지 채우기
      base[m]+=base[m-1] #이 연산 전의 base값는 이제 이전 층의 사람 수가 된다
  print(base[n-1])
#최종값만 구하면 되기 때문에 모든 호수가 각각 메모리를 차지할 필요가 없음
```

### 2839 설탕 배달
```py
n=int(input())
sub=n
m=-1

while sub>=3:
  if sub%5==0:
    m=sub//5 #5키로 개수
    m+=(n-sub)//3 #3키로 개수
    break
  sub-=3
if sub==0: # 모두 3키로로만 가능할 때
  m=n//3
print(m)
```

## 정렬.......
https://yaboong.github.io/algorithms/2018/03/20/counting-sort/
