# 제어문

## 조건문 if

파이썬은 중괄호대신 들여쓰기로 구조를 나타낸다. 

실습


is_holiday에 True또는 False값을 할당한 후, if문과 조건표현식을 사용해서 각각 ‘Good’과 ‘Bad’를 출력하는 코드를 짜본다.
```
In [7]: if is_holiday:
   ...:     print ('Good')
   ...: else:
   ...:     print ('Bad')
   ...:     
Good
```

조건표현식으로는 
```
In [8]: print('Good') if is_holiday else print('Bad')
Good
```

>vacation에 1에서 10중 아무 값이나 할당 후, if, elif, else문과 중첩 조건표현식을 사용해서 각각 vacation이 7이상이면 ‘Good’, 5이상이면 ‘Normal’, 그 이하면 ‘Bad’를 출력하는 코드를 짜본다.


```
In [11]: if vacation >= 7:
    ...:     print ('Good')
    ...: elif vacation >= 5:
    ...:     print ('Normal')
    ...: else:
    ...:     print ('Bad')
    ...:             
Normal

```

>중첩조건표현식으로 
```
In [9]: vacation = 6

In [10]: print ('Good') if vacation >= 7 else print ('Normal') if vacation >=5 else print ('Bad')
Normal

```

## 반복문 for

내부함수 range()
```
In [12]: range(10)
Out[12]: range(0, 10)


In [14]: for i in range(5):
    ...:     print(i)
    ...:     
0
1
2
3
4
```

generator는 메모리에 내용을 전부갖고있지않다
리스트는 메모리에 내용을 갖고 있음 

zip????

list(range(1,5))

###리스트 컴프리헨션
In [33]: [item for item in range(1,6)]
Out[33]: [1, 2, 3, 4, 5]

In [34]: [item*2 for item in range(1,6)]
Out[34]: [2, 4, 6, 8, 10]


In [36]: [item for item in range(1,6) if item % 2 == 0]
Out[36]: [2, 4]

In [37]: [item for item in range(15) if item % 2 == 0]
Out[37]: [0, 2, 4, 6, 8, 10, 12, 14]


중첩도가능
In [40]: com_list = []
    ...: for color in colors:
    ...:     for fruit in fruits :
    ...:         com_list.append((color,fruit))
    ...:         

In [41]: com_list
Out[41]: 
[('red', 'apple'),
 ('red', 'banana'),
 ('red', 'melon'),
 ('yellow', 'apple'),
 ('yellow', 'banana'),
 ('yellow', 'melon'),
 ('green', 'apple'),
 ('green', 'banana'),
 ('green', 'melon'),
 ('purple', 'apple'),
 ('purple', 'banana'),
 ('purple', 'melon')]

2중까지는 알아볼 수 있지만 3중부터는 알아보기 어렵기때문에 for문을 쓰는게 낫다





In [42]: g = (item for item in range(10))

In [43]: print (type(g))
<class 'generator'>

In [44]: for i in g:
    ...:     print (i)
    ...:     
0
1
2
3
4
5
6
7
8
9


1. for문을 2개 중첩하여 (0,0), (0,1), (0,2), (0,3), (1,0), (1,1)..... (6,3)까지 출력되는 반복문을 구현한다.
```
for i in range(7):
	for j in range(4):
		print('({},{})'.format(i,j))
```

2. 리스트 컴프리헨션을 중첩하여 위 결과를 똑같이 출력한다.
```
[ print('({},{})'.format(x,y)) for x  in range(7) for y in range(4)]

```
```
item_list = [(x,y) for x  in range(7) for y in range(4)]
for item in item_list:
         print(item)
```

3. 1, 2번의 반복문에서 튜플의 첫 번째 항목이 짝수일때만 출력하도록 조건을 변경한다.
>필요한 값만 출력하고 continue로 넘겨줘서 효율적이다. 
```
In [59]: for i in range(7):
    ...:     if i % 2 != 0:
    ...:         continue
    ...:     for j in range(4) :
    ...:         print((i,j))
    ...:         
(0, 0)
(0, 1)
(0, 2)
(0, 3)
(2, 0)
(2, 1)
(2, 2)
(2, 3)
(4, 0)
(4, 1)
(4, 2)
(4, 3)
(6, 0)
(6, 1)
(6, 2)
(6, 3)

```

>아래 코드는 이중for문을 끝까지 다돌게 돼서 성능이 더 떨어진다. 
```
In [61]: for i in range(7):
    ...:     for j in range(4) :
    ...:         if i % 2 ==0:
    ...:             print((i,j))

```
4. 1000에서 2000까지의 숫자 중, 홀수의 합을 구해본다.
```
In [65]: sum = 0
    ...: for i in range(1000,2001):
    ...:     if i % 2 != 0:
    ...:         sum += i
    ...:         
    ...:     

In [66]: print (sum)
75000

```
5. 리스트 컴프리헨션을 사용하여 구구단 결과를 갖는 리스트를 만들고, 해당 리스트를 for문을 사용해 구구단 형태로 나오도록 출력해본다.
각 단마다 한 번 더 줄바꿈을 넣어준다.
```
In [67]: ['{} x {} = {}'.format(x,y,x*y) for x in range(2,10) for y in range(1,10)]

```

```
In [70]: for i  in range(2,10):
    ...:     for j in range (1,10):
    ...:         gugu_list.append('{} x {} = {}'.format(i,j,i*j))

```


```
In [75]: result_list = [x*y for x  in range(2,10) for y in range(1,10)]

In [76]: index = 0

In [78]: for i in range (2,10):
    ...:     for j in range(1,10):
    ...:         print('{} X {} = {}'.format(i,j,result_list[index]))
    ...:         index +=1

```


6. 1에서 99까지의 정수 중, 7의 배수이거나 9의 배수인 정수인 리스트를 생성한다. 단, 7의 배수이며 9의 배수인 수는 한 번만 추가되어야 한다.
```
for i in range(1,100):
	if i % 7 == 0 or i % 9 ==0:
		list.79.append(i)
```

```
for i in range (1,100):
    ...:     if i % 7 == 0:
    ...:         com_list.append(i)
    ...:     elif i % 9 ==0:
    ...:         com_list.append(i)
```


enumerate() 함수!!!

* 컴프리헨션 
for문보다 리스트 컴프리헨션 사용하는게 성능면에서 유리하다. 

