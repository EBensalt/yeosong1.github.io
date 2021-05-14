

**시작하기 전에**

```py
import sys
#input = sys.stdin.readline() 한 줄 + 개행 받음
input = sys.stdin.readline.rstrip() 개행문자 떼고 받음
```

# 1. 그리디

- 부분적인 최적해가 전체적인 최적해가 되는 마법!
- 언제나 통하지는 않지만, 이런 방법이 통하는 문제들을 만나보세요.
- 실생활에서 자주 쓰이지는 않지만, 답과 전략이 딱 떨어지게 문제를 만들어서 출제되긴 한다고.

## Greedy Algorithm의 조건

1. 최적 부분 구조 (Optimal Substructure)
동적 계획법(Dynamic Programming) 과 마찬가지로, 작은 부분의 문제에서 구한 해로 전체의 해를 구할 수 있어야 한다.

2. 탐욕스런 선택 조건 (Greedy Choice Property)
앞에서의 부분해가 **이후의 부분해 선택에 영향을 주면 안 된다.**

### 거스름돈

- 설명 https://www.youtube.com/watch?v=5OYlS2QQMPA&list=PLVsNizTWUw7H9_of5YCB0FmsSc-K44y81&index=12
- 코인의 종류 k 만큼의 시간 복잡도. O(k)
- 가장 큰 단위로 먼저 나누기(작은 단위의 배수여서)

```py
# 인풋 1260
# 아웃풋 6

n=1260
count=0

array=[500,100,50,10]

for coin in array:
  count += n // coin #(몫)
  n %= coin
print(count)

```

### 1이 될 때 까지 1 빼거나 나누기(나누어 떨어질 때만)

- 설명 https://www.youtube.com/watch?v=_TG0hVYJ6D8&list=PLVsNizTWUw7H9_of5YCB0FmsSc-K44y81&index=14
- 주어진 숫자 K가 2 이상이기만 하면 항상 나누기가 1 빼기 보다 크게 수를 줄여줌
- 어쨌거나 1이 될 거라는 것도 자명함
- 복잡도 : 로그시간
-> 가능하면 최대한 많이 나누기


```py
n = 25
k=3
count=0

while 1 :
  target = (n//k)*k  #(25//3)*3=24
  count += (n - target) # 나눌 수 있기 위해 1을 몇 번(1,3) 빼야한다
  n = target #24 , 3
  if n < k: #나눌 수 없으면 탈출
    break
  count += 1 #밑에줄에서 한 번 나누는 거 카운팅 
  n= n//k # 한 번 나눴네

count+=(n-1) # n이 1보다 크다면 남은 수에서 1 빼기가 카운팅 된다
print(count)
```


### 곱하기 혹은 더하기로 가장 큰 수 만들기

- 설명 https://www.youtube.com/watch?v=_TG0hVYJ6D8&list=PLVsNizTWUw7H9_of5YCB0FmsSc-K44y81&index=14
- 조건: 문자열 주고 사이사이 연산 넣기. 연산은 무조건 왼쪽부터로 가정
- 0,1일 경우를 제외하고 무조건 많인 곱해서 크게 만들기


```py
a=[0,2,9,8,4]
ret=a[0]
for i in range(len(a)):
  if (a[i] <= 1 or ret <= 1):
    ret+=a[i]
  else:
    ret*=a[i]
print(ret)
```

### 모험가 길드

- 설명 https://www.youtube.com/watch?v=_TG0hVYJ6D8&list=PLVsNizTWUw7H9_of5YCB0FmsSc-K44y81&index=14
- n명의 모험가
- 공포도x인 모험가는 x면 이상의 그룹과만 출발 가능
- 출발 가능 그룹 수의 최댓값을 구하라
-> 1회에 갈 수 있는 최대한 분할된(많은) 그룹의 수

https://youtu.be/_TG0hVYJ6D8?list=PLVsNizTWUw7H9_of5YCB0FmsSc-K44y81&t=796 부터다시

https://programmers.co.kr/learn/courses/30/lessons/42862.

```py

n=int(input())
data=list(map(int, input().split()))
data.sort()
groups=0
count=0

for i in data: #공포도가 낮은 순서대로 그룹을 짜서
  count += 1 # 현재 그룹에 해당 모험가를 포함시키기
  if count >= i: #현재 그룹에 포함된 모험가의 수보다 공포도보다 크다면 그룹 생성
    groups += 1
    count=0 #현재 그룹에 포함된 모험가의 수 초기화

print(groups)
  
```

### 체육복 

- 설명 https://programmers.co.kr/learn/courses/30/lessons/42862
- 여벌의 체육복을 도난피해자들에게 빌려준다.
- 번호는 체격순. 인접한 번호(바로 앞, 바로 뒤)끼리만 대여 가능
- 전체 학생수 2 <= n <= 30
- 도난피해자 번호가 담긴 배열 lost ( 1 <= 도난 수 <= n, 중복 없음)
- 여벌 체육복이 있는 학생 번호가 담긴 배열 reserve
  - 여벌이 있는 경우에만 빌려줄 수 있음
  - 여벌이 있고 1개 도난 당했을 수 있음. 이 경우 빌려줄 수 없음
- 체육 수업을 들을 수 있는 학생의 최댓값을 return하는 solution 함수를 만드시오

~~~
def solution(n, lost, reserve):
    set_res = set(reserve)-set(lost) # 여벌 있지만 도난피해자라서 자기가 써야하는 경우 미리 제거
    set_lost= set(lost)-set(reserve) # 도난 피해자지만 여벌이 있는 경우 lost에서 제거
    for i in set_res:   
        if i-1 in set_lost: 
            set_lost.remove(i-1)
        elif i+1 in set_lost:
            set_lost.remove(i+1)
    return n-len(set_lost) #최종적으로 다 돌고도 lost 상태인 애들만 전체 인원수에서 제거



입력 예)

 n lost reserve -> return
 5 [2, 4]  [1, 3, 5] -> 5
 5 [2, 4,] [3]       -> 4
 3 [3]     [1]       -> 2  
~~~

여기 풀이에 set을 사용하네 set을 보고 넘어가자

#### 집합 자료형 https://youtu.be/Mkk8WOCAlqQ?list=PLVsNizTWUw7H9_of5YCB0FmsSc-K44y81&t=297
- 문자열, 리스트에서 중복 원소 제거 되는게 특징
- 집합 연산 가능

### 조이스틱

```
>> 문제 설명
AAA -> JAZ

▲ - 다음 알파벳
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)
▶ - 커서를 오른쪽으로 이동
예를 들어 아래의 방법으로 "JAZ"를 만들 수 있습니다.

- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.
만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

>> 제한 사항
name은 알파벳 대문자로만 이루어져 있습니다.
name의 길이는 1 이상 20 이하입니다.

>> 입출력 예
name	    return
"JEROEN"	56
"JAN"	    23
```

1. 어느 글자로 갈지 (좌우)
2. a쪽으로 움직일지 z쪽으로 움직일지

```
def solution(name):
    answer = 0
    for letter in name:
  	    answer += min(ord(letter) - ord('A'), ord('Z') - ord(letter))
    
    return answer+len(name)-1
    ...........
    ...
    ..
```

음.. 문제가 별론 거 같다 다음 꺼 먼저 푼다

## 큰 수 만들기
설명 https://programmers.co.kr/learn/courses/30/lessons/42883




### 무지의 먹방 라이브
https://programmers.co.kr/learn/courses/30/lessons/42891
