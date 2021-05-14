# 나동빈 코테 

https://www.youtube.com/watch?v=ngHIVey3J6Y   ??
https://www.youtube.com/playlist?list=PLSK4WsJ8JS4dOszA7Zr8paqI81Mv27tNq

0. [백준 단계별](백준basic.md)
1. 프로그래밍 기본 문법 공부(파이썬)
2. [알고리즘 기본 100제(코드업:기초100제)](codeup100.md)
3. 백준 문제 풀기(그리디, 탐색, 기초 동적프로그래밍 50문제씩)
4. 기출 문제 풀기(프로그래머스:카카오)
코드포스 블루레벨 정도면 코딩테스트 합격 가능(또는 삼성 역량 테스트 B형)
추천 언어 : C++,파이썬


----------

![image](https://user-images.githubusercontent.com/53321189/117644830-e1818400-b1c4-11eb-9861-4219be774ffc.png)

![image](https://user-images.githubusercontent.com/53321189/117644378-5e602e00-b1c4-11eb-84d8-8d3100ebf0e5.png)

![image](https://user-images.githubusercontent.com/53321189/117644912-f4945400-b1c4-11eb-8a55-cbc0cdd22df3.png)



![image](https://user-images.githubusercontent.com/53321189/117646033-44bfe600-b1c6-11eb-9e2d-cf9f29aa20b1.png)


카카오 구현 위주 - 문자열 처리

![image](https://user-images.githubusercontent.com/53321189/117646542-da5b7580-b1c6-11eb-8af9-99dd750a5daa.png)
![image](https://user-images.githubusercontent.com/53321189/117646627-f0693600-b1c6-11eb-9bca-4b573def7952.png)
![image](https://user-images.githubusercontent.com/53321189/117646685-0119ac00-b1c7-11eb-8f75-267c99b35d37.png)
![image](https://user-images.githubusercontent.com/53321189/117646823-2dcdc380-b1c7-11eb-9bef-109c2e5c9043.png)
![image](https://user-images.githubusercontent.com/53321189/117647998-a719e600-b1c8-11eb-928b-0c11f09a0a1c.png)

자료형

![image](https://user-images.githubusercontent.com/53321189/117648313-08da5000-b1c9-11eb-8cee-74c156a65502.png)
![image](https://user-images.githubusercontent.com/53321189/117648628-6a022380-b1c9-11eb-80f8-a5d416d2063b.png)
![image](https://user-images.githubusercontent.com/53321189/117648706-856d2e80-b1c9-11eb-9f54-472152503c24.png)

https://youtu.be/GUwkMLtDQJE?list=PLVsNizTWUw7H9_of5YCB0FmsSc-K44y81&t=912 여기서부터 다시


## 리스트

- 맨 마지막 원소 접근 시 a[-1]
- 슬라이싱 a[1 : 4] (= 두번째 원소 ~ 4번째 원소) 사용시 끝 인덱스는 실제 인덱스보다 1 크게 설정 주의
- 리스트 컴프리헨션(? 리스트 초기화)
    - 예)
    - array = `[i for i in range(10) if i % 2 == 1]`
    - print(array) -> `[1, 3, 5, 7, 9]`
    - 예)
    - array = `[i * i for i in range(1, 6)]`
    - print(array) -> `[1, 4, 9, 16 ,25]`
    - 2차원 배열 예)
    - array = `[[0] * m for _ in range(n)]`
    - n번 동안 반복할 때마다 길이가 m인 리스트를 새롭게 초기화 한다..
- 언더바 : 변수 사용 없이 그냥 단순히 횟수 반복할 때 사용한다.
    - 예) for _ in range(5): print("Hello World")

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

![image](https://user-images.githubusercontent.com/53321189/118065595-2f220a80-b3d8-11eb-92ad-1d99137b1886.png)

`>>>>>>>>>>>>> remove는 한 개만 지움 주의!`

- 리스트에서 특정 값을 모두 지우기

```py
a = [1, 2, 3, 4, 5, 5, 5]
remove_set = {3, 5}

result = [i for i in a if i not in remove_Set]
print(result)

> [1, 2, 4]
```
## 문자열 자료형

- 인덱싱과 슬라이싱은 가능하지만, 특정 인덱스 값을 변경할 수는 없음 immutable
- 문자열 덧셈 = 공백 없이 더해짐
- 문자열 곱셈 = 곱한 수만큼 반복

### 튜플
- 한 번 선언된 값을 변경할 수 없음
- 소괄호 사용
- 리스트에 비해 공간 효율적

```py
a = (1, 2, 3, 4, 5)
print(a[3]) -> 4
print(a[1 : 4]) -> (2, 3, 4)
```
**튜플을 사용하면 좋은 경우**

- 서로 다른 성질의 데이터 묶어 사용할 때
    - 최단 경로 알고리즘 (비용-노드 번호) 묶어서 저장
- 데이터의 나열을 해싱의 키 값으로 사용할 때 (변경 불가한 점을 살려서)
- 리스트보다 메모리 사용을 줄여야 할 때

### 사전자료형

```
data = dict()
data['사과'] = 'Apple'
#키 데이터만 담은 리스트
k_list=data.keys()
#값 데이터만 담은 리스트
v_list=data.values()

```
