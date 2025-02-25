[코드업 파이썬 기초 100제](https://codeup.kr/problemsetsol.php?psid=33)

# 코드업 파이썬 기초 100제 메모

6자리의 연월일(YYMMDD)을 입력받아 나누어 출력해보자.

참고
s = input()
print(s[0:2])

를 실행하면 0번째 문자부터 1번째 문자까지 잘라 출력한다.
s[a:b] 라고 하면, s라는 단어에서 a번째 문자부터 b-1번째 문자까지 잘라낸 부분을 의미한다.
다른 자르기 방법도 있다.

------

a, b = input().split()
c = int(a) + int(b)
print(c)

참고
입력되는 값은 기본적으로 문자열로 인식된다.

----------------  

~~~
a = input()
n = int(a)            #입력된 a를 10진수 값으로 변환해 변수 n에 저장
print('%x'% n)  #n에 저장되어있는 값을 16진수(hexadecimal) 소문자 형태 문자열로 출력
          ~~ 주의?

~~~

--------
목표 A -> 65

예시
n = ord(input())
print(n)

참고
n = ord(input())  #입력받은 문자를 10진수 유니코드 값으로 변환한 후, n에 저장한다.

-----------

목표 65 -> A

예시
c = int(input())
print(chr(c))  #c에 저장되어 있는 정수 값을 유니코드 문자(chracter)로 바꿔 출력한다. 

-------------
목표 a -> b

a=ord(input())
print(chr(a+1))

---------

6037번 https://codeup.kr/problem.php?id=6037&rid=0
`print(int(n)*s)` --> 한 줄로 여러 번 출력

-----

거듭제곱
2 10
1024
`int(a)**int(b)`
0.5하면 제곱근..

---------
6042번
3.141592 -> 3.14

print(format(수, ".2f"))를 사용하면 원하는 자리까지의 정확도로 **반올림 된** 실수 값을 만들어 준다. <br>
여기서 만들어진 값은 소수점 아래 3번째 자리에서 반올림한 값이다.

----------

```
c,d=input().split()
~~~~
a = int(c)
b = int(d) ?????? 꼭 이렇게 다른 이름 써야하나? 아닌 거 같던데 예외가 있나?

print(a+b)
print(a-b)
print(a*b)
print(a//b)
print(a%b)
print(format(a/b,".2f"))
      ~~~~~~~~~~~~~~~~~~
```

-------------

6046

정수 1개를 입력받아 2배 곱해 출력해보자.

참고
*2 를 계산한 값을 출력해도 되지만,
정수를 2배로 곱하거나 나누어 계산해 주는 비트단위시프트연산자 <<, >>를 이용할 수 있다.
컴퓨터 내부에는 2진수 형태로 값들이 저장되기 때문에,
2진수 형태로 저장되어 있는 값들을 왼쪽(<<)이나 오른쪽(>>)으로
지정한 비트 수만큼 밀어주면 2배씩 늘어나거나 1/2로 줄어드는데,

왼쪽 비트시프트(<<)가 될 때에는 오른쪽에 0이 주어진 개수만큼 추가되고,
오른쪽 비트시프트(>>)가 될 때에는 왼쪽에 0(0 또는 양의 정수인 경우)이나 1(음의 정수인 경우)이 개수만큼 추가되고,
가장 오른쪽에 있는 1비트는 사라진다.

~~~
예시
n = 10
print(n<<1)  #10을 2배 한 값인 20 이 출력된다.
print(n>>1)  #10을 반으로 나눈 값인 5 가 출력된다.
print(n<<2)  #10을 4배 한 값인 40 이 출력된다.
print(n>>2)  #10을 반으로 나눈 후 다시 반으로 나눈 값인 2 가 출력된다.

정수 10의 2진수 표현은 ... 1010 이다.
10 << 1 을 계산하면 ... 10100 이 된다 이 값은 10진수로 20이다.
10 >> 1 을 계산하면 ... 101 이 된다. 이 값은 10진수로 5이다.
~~~

n = 10 과 같이 키보드로 입력받지 않고 직접 작성해 넣은 코드에서, 숫자로 시작하는 단어(식별자, identifier)는 자동으로 수로 인식된다.  

n = 10 에서 10 은 10진수 정수 값으로 인식된다.
변수 n 에 문자열을 저장하고 싶다면, n = "10" 또는 n = '10'으로 작성해 넣으면 되고,

n = 10.0 으로 작성해 넣으면 자동으로 실수 값으로 저장된다.
n = 0o10 으로 작성해 넣으면 8진수(octal) 10으로 인식되어 10진수 8값이 저장되고,
n = 0xf 나 n = 0XF 으로 작성해 넣으면 16진수(hexadecimal) F로 인식되어 10진수 15값으로 저장된다.

** python에서 실수 값에 대한 비트시프트 연산은 허용되지 않고 오류가 발생한다.
(실수 값도 컴퓨터 내부적으로는 2진수 형태로 저장되고 비트시프트 처리가 될 수 있지만, python 에서는 허용하지 않는다.)

------------------

6047

~~~

2 * 2^10 = 2048 하는 법

  2 << 10
또는
 (2 ** 10) * 2


a를 2^b배 곱한 값을 출력하기.......

a << 1 ---> a * 2^1
a << 2 ---> a * 2^2
a << 3 ---> a * 2^3

~~~

-------

6052 
불리언 = 0 false, 0이 아닌 모든 숫자 true
not true == false
not false == true

------

~~~
6053

정수값이 입력될 때,
그 불 값을 반대로 출력하는 프로그램을 작성해보자.

예시
a = bool(int(input()))
print(not a)

참고
a = bool(int(input()))
와 같은 형태로 겹쳐 작성하면, 한 번에 한 단계씩 계산/처리/평가된다.
위와 같은 명령문의 경우 input( ), int( ), bool( ) 순서로 한 번에 한 단계씩 계산/처리/평가된다.

어떤 불 값이나 변수에 not True, not False, not a 와 같은 계산이 가능하다.

참 또는 거짓의 논리값을 역(반대)으로 바꾸기 위해서 not 예약어(reserved word, keyword)를 사용할 수 있다.

이러한 논리연산을 NOT 연산(boolean NOT)이라고도 부르고,
프라임 '(문자 오른쪽 위에 작은 따옴표), 바(기호 위에 가로 막대), 문자 오른쪽 위에 c(여집합, complement) 등으로 표시한다.
모두 같은 의미이다.

참, 거짓의 논리값 인 불(boolean) 값을 다루어주는 예약어는 not, and, or 이 있고,
불 값들 사이의 논리(not, and, or) 연산 결과도 마찬가지로 True 또는 False 의 불 값으로 계산 된다.

정수값 0은 False 이고, 나머지 정수 값들은 True 로 평가된다.
빈 문자열 "" 나 ''는 False 이고, 나머지 문자열들은 True 로 평가된다.

불 대수(boolean algebra)는 수학자 불이 만들어낸 것으로 True(참)/False(거짓) 값만 가지는 논리값과 그 값들 사이의 연산을 다룬다.
~~~

-------------

6056
xor 다를 때만 참
c = bool(int(a))
d = bool(int(b))
print((c and (not d)) or ((not c) and d))

--------

??? 6058
a,b=input().split()
c=int(a)
d=int(b)
print(c==0 and d==0)

--------

6059: [기초-비트단위논리연산] **비트단위로 NOT** 하여 출력하기(설명)(py)

입력 된 정수를 비트단위로 참/거짓을 바꾼 후 정수로 출력해보자.
비트단위(bitwise)연산자 **~** 를 붙이면 된다.(~ : tilde, 틸드라고 읽는다.)

~~~
비트단위(bitwise) 연산자는,
~(bitwise not), &(bitwise and), |(bitwise or), ^(bitwise xor),
<<(bitwise left shift), >>(bitwise right shift)
가 있다.
~~~

예를 들어 1이 입력되었을 때 저장되는 1을 32비트 2진수로 표현하면
        00000000 00000000 00000000 00000001 이고,
~1은 11111111 11111111 11111111 11111110 가 되는데 이는 -2를 의미한다.

예시
a = 1
print(~a) #-2가 출력된다.

참고
컴퓨터에 저장되는 모든 데이터들은 2진수 형태로 바뀌어 저장된다.
0과 1로만 구성되는 비트단위들로 변환되어 저장되는데,
양의 정수는 2진수 형태로 바뀌어 저장되고, 음의 정수는 "2의 보수 표현"방법으로 저장된다.

양의 정수 5를 32비트로 저장하면, 

5의 2진수 형태인 101이 32비트로 만들어져
00000000 00000000 00000000 00000101
로 저장된다.(공백은 보기 편하도록 임의로 분리)

32비트 형의 정수 0은
00000000 00000000 00000000 00000000

그리고 -1은 0에서 1을 더 빼고 32비트만 표시하는 형태로
11111111 11111111 11111111 11111111 로 저장된다.

-2는 -1에서 1을 더 빼면 된다.
11111111 11111111 11111111 11111110 로 저장된다.

이러한 내용을 간단히 표현하면, 정수 n이라고 할 때,

~n = -n - 1
-n = ~n + 1 과 같은 관계로 표현할 수 있다.

이 관계를 그림으로 그려보면 마치 원형으로 수들이 상대적으로 배치된 것과 같다.

![pimg6224_1](https://user-images.githubusercontent.com/53321189/117772499-37603580-b272-11eb-8dbb-c6f3d85e279c.png)

-------------------

6064

~~~
a,b,c=input().split()
d=int(a)
e=int(b)
f=int(c)
r1 = d if (d<e) else e
r2 = f if (f<r1) else r1
print (r2)


(a if a>b else b) if ((a if a>b else b)>c) else c
와 같은 계산식은 a, b, c 의 값 중 가장 큰 값으로 계산된다.
~~~


--------
6068

~~~
n=int(input())

if n>=90 :
    print('A')
elif n>=70 :
    print('B')
elif n>=40 :
    print('C')
else :
    print('D') 
~~~

else if는 안됨.. elif여야 ..

-----------
6075
~~~
영문 소문자(a ~ z) 1개가 입력되었을 때,
a부터 그 문자까지의 알파벳을 순서대로 출력해보자.

예시
c = ord(input())
t = ord('a')
while t<=c :
  print(chr(t), end=' ')
  t += 1

참고
알파벳 문자 a의 정수값은 ord('a')로 알아낼 수 있다.
chr(정수값)을 이용하면 유니코드 문자로 출력할 수 있다.
print(..., end=' ') 와 같이 작성하면 값 출력 후 공백문자 ' '를 출력한다. 즉, 마지막에 줄을 바꾸지 않고 빈칸만 띄운다.
(end='\n'로 작성하거나 생략하면, 값을 출력한 후 마지막(end)에 줄바꿈(newline)이 된다.)
~~~

-------------

6076

~~~
정수(0 ~ 100) 1개를 입력받아 0부터 그 수까지 순서대로 출력해보자.

예시
n = int(input())
for i in range(n+1) :
  print(i)

참고
range(n) 은 0, 1, 2, ... , n-2, n-1 까지의 수열을 의미한다.
예를 들어 range(3) 은 0, 1, 2 인 수열을 의미한다.

for i in range(n) :    #range(n)에 들어있는(in) 각각의 수에 대해서(for) 순서대로 i에 저장해 가면서...
이때의 for는 각각의 값에 대하여... 라는 for each 의 의미를 가진다고 생각할 수 있다.

range(끝)
range(시작, 끝)
range(시작, 끝, 증감)
형태로 수열을 표현할 수 있다. 시작 수는 포함이고, 끝 수는 포함되지 않는다. [시작, 끝)
증감할 수를 작성하지 않으면 +1이 된다.

반복 실행구조에 반복 횟수를 기록/저장하는 변수로 i를 자주 사용하는데,
i 는 반복자(iterator)를 나타내는 i라고 생각할 수 있다. i, j, k ... 알파벳 순으로 사용하기도 한다.
~~~

-------

6077 

 for에 있는 i는 안적으면 디폴트 0이야 증감도 디폴트가 +=1
~~~
n = int(input())
sum = 0
for i in range(1, n+1) :
  if i%2==0 :
    sum += i

print(sum)
~~~

--------

6078

~~~
a = 'a'
while(a!='q'): 
  a = input()
  print(a)

또는

while True:
  x = input()
  print(x)
  if x=='q':
          break
~~~

--------------


~~~
a=int(input())
sum=0    
c=0         # 초기화 까먹지 말고
while True:
    c+=1
    sum+=c
    if (a=<sum):
#        ~~~~ ................. 이런 에러는 좀 만들지 말기
        print(c)
        break
~~~    


------------


6080

~~~
a,b=input().split()
n=int(a)
m=int(b)

for i in range(1,n+1):
    for j in range(1,m+1):
        print(i,j)   ~~~~
              ~~~~
~~~

----------

6081 구구단

~~~
n=int(input(), 16)
             # ~~~~~ 주의
for i in range(1, 16):
                # ~~~~ 주의
    print('%X*%X=%X'%(n,i,n*i))

~~~


----------

~~~
r,g,b=map(int, input().split())
for i in range(r):
    for j in range(g):
        for k in range(b):
            print(i,j,k)
print(r*g*b)

~~~

----------

6092

출석 번호를 n번 무작위로 불렀을 때, 각 번호(1 ~ 23)가 불린 횟수를 각각 출력해보자.

~~~
예시
n = int(input())      #개수를 입력받아 n에 정수로 저장
a = input().split()  #공백을 기준으로 잘라 a에 순서대로 저장

for i in range(n) :  #0부터 n-1까지...
  a[i] = int(a[i])       #a에 순서대로 저장되어있는 각 값을 정수로 변환해 다시 저장

d = []                     #d라는 이름의 빈 리스트 [ ] 변수를 만듦. 대괄호 기호 [  ] 를 사용한다.
for i in range(24) :  #[0, 0, 0, ... , 0, 0, 0] 과 같이 24개의 정수 값 0을 추가해 넣음
  d.append(0)        #각 값은 d[0], d[1], d[2], ... , d[22], d[23] 으로 값을 읽고 저장할 수 있음.

for i in range(n) :    #번호를 부를 때마다, 그 번호에 대한 카운트 1씩 증가
  d[a[i]] += 1

for i in range(1, 24) :  #카운트한 값을 공백을 두고 출력
  print(d[i], end=' ')

참고
- d = []              #어떤 데이터 목록(list) 을 순서대로 저장하기 위해 아무것도 없는 리스트 변수 만들기
- d.append(값)  #d 리스트의 마지막에 원하는 값을 추가(append)해 넣음 
- d[a[i]] += 1     #2중 리스트 참조 : 만약 a[i]의 값이 1이었다면? d[1] += 1 이 실행되는 것이다. 1번 카운트 1개 증가..
~~~


--------

6095
~~~
2차원 배열 생성 및 초기화 표현식 5가지......

d = [[0] * 19 for i in range(19)]
d = [[0] * 19 for _ in range(19)]
d = [[0 for i in range(19)] for j in range(19)]
d = [[0 for _ in range(19)] for _ in range(19)]

d =[]                      
for i in range(20) :
  d.append([])       
  for j in range(20) : 
    d[i].append(0)   
~~~

-------------

6096

```py
game = [list(map(int, input().split()))for _ in range(19)] #2차원 배열 입력
num = int(input()) #몇 번 뒤집을지
for i in range(num):
  a, b = map(int, input().split())
  for j in range(19):
    game[a-1][j] = (1 if game[a-1][j] == 0 else 0)
    game[j][b-1] = (1 if game[j][b-1] == 0 else 0)
for i in game:
  print (" ".join(repr(j) for j in i))


d=[]
for i in range(20) :
    d.append([])
    for j in range(20) :
        d[i].append(0)
                    
for i in range(19) :
  a = input().split()
  for j in range(19) :
      d[i+1][j+1] = int(a[j])

n = int(input())
for i in range(n) :
  x,y=input().split()
  x=int(x)
  y=int(y)
  for j in range(1, 20) :
    if d[j][y]==0 :
      d[j][y]=1
    else : d[j][y]=0
    if d[x][j]==0 :
      d[x][j]=1 
    else : d[x][j]=0
                    
for i in range(1, 20) :
    for j in range(1, 20) :
        print(d[i][j], end=' ')
    print()


a = [[]*19 for _ in range(19)]
for i in range(19):
   a[i]=list(map(int,input().split()))

n = int(input())

for i in range(n):
    b,c=map(int,input().split())
    
    for j in range(19):
        if(a[b-1][j]==1):
            a[b-1][j]=0
        else: a[b-1][j]=1
    
    for j in range(19):
        if(a[j][c-1]==1):
            a[j][c-1]=0
        else: a[j][c-1]=1

for i in range(19):
    for j in range(19):
        print(a[i][j],end=' ')
    print()




```
