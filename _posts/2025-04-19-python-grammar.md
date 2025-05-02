---
title: 파이썬 문법
date: 2025-04-19
categories: [Programming, Python]
tags: [Programming, Python]
---

## 데이터 타입
### 숫자형(Numeric)
  ```python
  # 정수 (int)
  i = 10
  type(i)
  >>> int

  # 실수 (float)
  f = 3.14 
  type(f)
  >>> float
  ```  

### Boolean
  ```python
  # Bool
  flag = True
  type(flag)
  >>> bool
  ```  

### 시퀀스(Sequence)
- <span style="background-color: #fff5b1">메모리에 연속적으로 저장되어</span> 인덱싱과 슬라이싱이 가능!
  
- 문자열 (str)  
  ```python
  s = 'Hello'
  type(s)
  >>> str

  # 문자열에 작은 따옴표(')가 포함된 경우
  s = "Python's easy."
  print(s)
  >>> Python's easy.

  # 문자열에 큰 따옴표(")가 포함된 경우, 이스케이프 문자 이용
  s = "People said, \"Life is too short, You need Python.\""
  print(s)
  >>> People said, "Life is too short, You need Python."
  ```  

- 리스트 (list)  
\- 데이터 변경(수정/삭제) 가능 ✅
  ```python
  li = [1, 2, 3]
  type(li)
  >>> list

  # 인덱싱
  print(li[1])
  >>> 2
  ```  

- 튜플 (tuple)  
\- 데이터 변경(수정/삭제) ❌
  ```python
  t = (1, 2, 3)
  type(t)
  >>> tuple

  # 슬라이싱
  print(t[0:2])
  >>> (1, 2)
  ```  

### 매핑(Mapping)
- 딕셔너리 (dict)  
\- Key-Value 쌍으로 구성됨  
\- Key 값은 중복❌
  ```python
  fruits = {'apple' : '사과', 'banana' : '바나나', 'cherry' : '체리'}
  type(fruits)
  >>> dict

  fruits['cherry']
  >>> 체리
  ```  
<br/>

## 연산
### 숫자형(Numeric)
  ```python
  a = 3
  b = 4

  # 덧셈
  print(a + b)
  >>> 7

  # 뺄셈    
  print(a - b)
  >>> -1

  # 곱셈
  print(a * b)
  >>> 12

  # 나눗셈
  print(a / b)
  >>> 0.75

  # 몫 - 정수만 반환(소수점 버림)
  print(a // b)
  >>> 0

  # 나머지
  print(a % b)
  >>> 3
  ```  

### 시퀀스(Sequence)
  ```python
  s1 = 'Hello'
  s2 = ' Python

  li1 = [4, 5, 6]
  li2 = [7, 8, 9, 10, 11]

  t1 = (1, 2, 3)
  t2 = (4, 5)
  ```  

- 덧셈 연산은 __각 변수의 데이터를 합침__
  ```python
  # 문자열
  print(s1 + s2)
  >>> Hello Python

  # 리스트
  print(li1 + li2)
  >>> [4, 5, 6, 7, 8, 9, 10, 11]

  # 튜플
  print(t1 + t2)
  >>> (1, 2, 3, 4, 5)
  ```  

- 곱셈 연산은 __각 변수의 데이터가 반복됨__
  ```python
  # 문자열
  print(s1 * 2)
  >>> HelloHello

  # 리스트
  print(li1 * 2)
  >>> [4, 5, 6, 4, 5, 6]

  # 튜플
  print(t1 * 2)
  >>> (1, 2, 3, 1, 2, 3)
  ```  
<br/>

## 제어문
### 조건문
- 조건에 맞는 동작 수행  
- <span style="background-color: #fff5b1">들여쓰기</span>로 코드 블럭을 구분  
  
- if문
  ```python
  # 점수가 90점 이상이면 'A' 출력
  # 점수가 80점 이상 ~ 90점 미만이면 'B' 출력
  # 점수가 80점 미만이면 'C' 출력

  score = int(input('점수를 입력하세요: '))

  if score >= 90 : 
    print('A')
  elif score >= 80 : 
    print('B')
  else :
    print('C')
  ```  

### 반복문
- 동일한 작업 반복 수행  
- <b>break(반복 종료), continue(현재 반복 생략)</b> 로 반복 제어
  
- while문  
\- 조건을 충족할 때까지 반복  
  ```python
  # 1부터 10까지 숫자 중에서 홀수만 출력
  i = 0
  while i < 10 :
    if i % 2 :
      print(i)
    i += 1

  # 퀴즈 맞추기 - 무한루프에서 break 중요!
  while True :
    answer = input('대한민국의 수도는? ')

    if answer in ['서울', 'SEOUL', 'Seoul', 'seoul'] : 
      print('정답입니다.')
      break
    else :
      print('오답입니다.')
  ```  

- for문  
\- 지정된 횟수만큼 반복  
  ```python
  # 1부터 10까지 숫자 중에서 홀수만 출력
  for i in range(10) :
    if i % 2 :
      print(i)

  # 1부터 20까지 숫자 중에서 짝수만 출력
  for i in range(1, 21) :
    if i % 2 :
      continue
    print(i)
  ```  
<br/>

## 함수
- 입력을 받아 특정 동작을 수행하고 결과를 만들어줌  
  
### 기본 함수  
- 함수 1개당 가변 매개변수는 1개만 사용 가능  
  ```python
  # 입력에 따라 연산 결과를 반환하는 함수
  def stat(type, *args) :
    result = 0

    # 합
    if type == 'sum' :
      for i in args :
        result += i
    # 최소값
    elif type == 'min' :
      result = args[0]

      for i in args :
        if i < result :
          result = i
    # 최대값
    elif type == 'max' :
      result = args[0]

      for i in args :
        if i > result :
          result = i

    return result
  
  print(stat('sum', 1, 2, 3))
  >>> 6

  print(stat('min', 51, -2, 3, 20))
  >>> -2

  print(stat('max', 10, 11, 12))
  >>> 12
  ```  

### 람다 함수  
- 익명 함수로 함수를 <span style="background-color: #fff5b1">한 번만 사용하거나 함수를 인자로 전달해야 할 때 유용하게 사용</span>  
- <b>lambda 매개변수 : 표현식</b>  
  ```python
  # 입력값을 거듭제곱한 결과를 반환하는 함수

  # 기본 함수
  def square(n) : 
    return n ** 2

  # 람다 함수로 변환
  square = lambda n : n ** 2
  print(square(3))
  >>> 9
  ```  

### 고차 함수  
- 함수를 인자로 받거나 리턴하는 함수  
  
- map(함수명, iterable) - 모든 요소에 함수 적용  
  ```python
  # 숫자 → 문자열 데이터로 변경
  nums = [1, 2, 3, 4]
  print(list(map(str, nums)))
  >>> ['1', '2', '3', '4']
  ```  
- filter(함수명, iterable) - 조건을 만족하는 요소만 필터링  
  ```python
  # 10보다 작은 수만 출력
  nums = [3, 12, 54, 39, 7, 187, 5, 2, 26, 84]
  print(list(filter(lambda n : n < 10, nums)))
  >>> [3, 7, 5, 2]
  ```  
- enumerate(iterable, start=0) - 각 요소의 인덱스와 값을 순서쌍(튜플)으로 반환  
  ```python
  # 인덱스 변경
  for index, value in enumerate(['red', 'green', 'blue'], start=1) :
    print(index, value)
  >>> 1 red
      2 green
      3 blue
  ```  
<br/>

## 클래스  
### 클래스  
- 동일한 무언가를 계속 만들어낼 수 있는 틀(설계도)로 __데이터와 기능(메서드)를 함께 가지고 O__  
  ```python
  class Calculator :
    # 생성자 - 객체 생성 시, 자동으로 호출됨
    def __init__(self, first, second) :
      self.first = first
      self.second = second
    
    # 메서드
    def add(self) :
      return self.first + self.second

    def sub(self) :
      return self.first - self.second

    def mul(self) :
      return self.first * self.second

    def div(self) :
      return self.first / self.second 
  ```  

### 객체  
- 클래스로 만든 것  
  ```python
  # cal1 객체 생성
  cal1 = Calculator(3, 4)
  print(cal1.first, cal1.second)
  >>> 3 4
  print(cal.add())
  >>> 7
  print(cal.sub())
  >>> -1

  # cal2 객체 생성
  cal2 = Calculator(5, 2)
  print(cal2.first, cal2.second)
  >>> 5 2
  print(cal2.mul())
  >>> 10
  print(cal2.div())
  >>> 2.5
  ```  

### 상속   
- 클래스를 상속받아 <span style="background-color: #fff5b1">그 클래스의 기능을 그대로 혹은 변경(오버라이딩)하여 사용 가능</span>
  ```python
  # Character 클래스 - 부모 클래스
  class Character :
    def __init__(self, nickname, server) :
      self.nickname = nickname
      self.server = server
    
    def info(self) :
      print(f'{self.nickname}님이 속한 서버는 {self.server} 입니다.')
  

  # Warrior 클래스 - 자식 클래스
  class Warrior(Character) :
    # super() 로 상속받은 Character 클래스 생성자 호출
    def __init__(self, nickname, server, level) :
      super().__init__(nickname, server)
      self.level = level
    
    # 상속받은 Character 클래스 info() 메소드 재정의(Override)
    def info(self) :
      super().info()
      print(f'레벨 : Lv.{self.level}')


  # Warrior 객체 생성
  w = Warrior('bestwarrior', 'Azshara', 50)
  w.info()
  >>> bestwarrior님이 속한 서버는 Azshara 입니다.
      레벨 : Lv.50
  ```  