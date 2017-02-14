# 클래스

## 클래스 
사람(객체)
키, 몸무게 (속성)
먹는다 잔다 (메소드)

형태(타입) - 클래스
형태를 가진 객체 - 인스턴스

사람(타입) - 클래스
각각 한명의 사람 - 인스턴스


>/class 에 가상환경 새로 만들었음 (class-py)
>pyenv 3.4.3 class-py
>

클래스 만들면 초기화 메서드인`__init__` 메서드 반드시 거친다.

self!!

In [1]: from class_sample import Shop

생성한 객체의 초기화 메서드 __init__을 호출한다.
In [2]: lotteria = Shop('Lotteria')

In [3]: lotteria
Out[3]: <class_sample.Shop at 0x106113c50>



>ipytho에서 자동으로 리로드 하기 
>
>```
>In [5]: %load_ext >autoreload
>
>In [6]: %autoreload 2
>```
>안그럼 파일 바꿨을때마다 나갔다 들어와서 임포트 다시 해줘야함


인스턴스를 사용하는 메서드만 self를 가진다. 

self => 생성된 Shop형 객체 자신

### 클래드 메서드 
클래스 메서드는 클래스 속성에 대해 동작하는 메서드이다. 위의 인스턴스 메서드와 달리 호출 주체가 클래스이며, 첫 번째 인자도 클래스이다.
만약 인스턴스가 첫 번째 인자로 주어지더라도 해당 인자의 클래스로 자동으로 바뀌어 전달된다.

클래스메서드는 `@classmethod`데코레이터를 붙여 선언하며, 첫 번째 인자의 이름은 관용적으로 cls를 사용한다.

>실습


>Shop클래스에 클래스 속성 description을 수정하는 클래스 메서드를 작성한다.

```
class Shop:
    description = 'Python shop Class'

    def __init__(self, name, shop_type, address):
        self.name = name
        self.shop_type = shop_type
        self.address = address


    def show_info(self):
        print('상점({})\n 유형:{}\n 주소:{}'.format(self.name, self.shop_type, self.address))

    def change_type(self, shop_type):
        self.shop_type = shop_type

    @classmethod
    def change_desc(cls, description):
        cls.description = description

shop1 = Shop('롯 데 리 아!','패스트푸드','강남구')
shop2 = Shop('홈플러스','음식','마포구')
shop3 = Shop('제노피씨방','식당','종로구')

print(shop1.description)
print(shop2.description)
print(shop3.description)
shop1.show_info()
shop2.show_info()
shop3.show_info()

shop3.change_type('놀이공원')
shop3.show_info()

Shop.change_desc('Change description')
print(shop2.description)
shop1.change_desc('ggg')
shop2.change_desc('hhh')
print(shop1.description)
```

* self나 cls나 관용구이다. 즉 다른걸로 바꿔도 상관없음.

결과는 이렇게 나온다.

```
Python shop Class
Python shop Class
Python shop Class
상점(롯 데 리 아!)
 유형:패스트푸드
 주소:강남구
상점(홈플러스)
 유형:음식
 주소:마포구
상점(제노피씨방)
 유형:식당
 주소:종로구
상점(제노피씨방)
 유형:놀이공원
 주소:종로구
Change description
hhh
```

* 스코프처럼 인스턴스가 클래스보다 우선한다. 

### 스태틱 메서드
일반함수
인자도 없음

스태틱 메서드는 클래스 내부에 정의된 일반 함수이며, 단지 클래스나 인스턴스를 통해서 접근할 수 있을 뿐 해당 클래스나 인스턴스에 영향을 주는 것은 불가능하다.

스태틱메서드는 @staticmethod데코레이터를 붙여 선언한다.

```
@staticmethod
    def print_test():
        print('staticmethod test!')

shop1.print_test()
Shop.print_test()
```

```
staticmethod test!
staticmethod test!
```

## 속성 접근 지정자
### 캡슐화
정보 은닉
함부로 데이터 바꾸거나 하지 못 하도록

### 지정자 
__ : 언더바 두개. private

_클래스명__속성명


shop1.__shop_type 으로 하면 안된다.(직접접근으로 못 바꾼다)

네임 맹글링 : __ 가 있으면 변수 이름을 _클래스명__속성명으로 바꿔준다.
다른 언어처럼 프라이빗 접근을 완던 막는건 아니고 이름을 바꿔놓는다. (그래서 억지로 바구려면 접근해ㅓㅅ 바꿀 수 있다.) 그렇지만 접근하지 않는다. 

dir해보면 저렇게 이름이 바뀐것을 확인할 수 있다

@property 데코레이터

함수역할도 해준다


## 다형성
동일한 실행에 대하여 다른 동작을 수행하도록 하는 것
오는 인자에 대해 다른 실행을 할 수 있다. 

* 파이썬은 동적타입 언어 인자의 타입에 신경을 안쓴다. 

항상 같은 방식의 인자만 받는 것이 아니다. 전혀 다른 메소드지만 (food클래스의 eat()과 drink클래스의 eat() ) 여기서 item은 클래스객체 


```
class User:
    def __init__(self, name):
        self.name = name

    def eat_something(self, item):
        item.eat()

class Food:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print('{}를 먹었다. 배부르다'.format(self.name))


class Drink:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print('{}를 마셨다. 갈증이 해소된다!'.format(self.name))

user = User('kizmo')
food1 = Food('밀스')
drink1 = Drink('맥콜')

user.eat_something(food1)
user.eat_something(drink1)
```

실행결과

```
(class-py) ➜  class git:(master) ✗ python class_sample2.py
밀스를 먹었다. 배부르다
맥콜를 마셨다. 갈증이 해소된다!
```

하나의 함수에 주어진 인자가 다를때 그 인자에 따라서 다른 기능을 한다.

예를들어 내장함수 str도 str(3)과 str(1.4) 같이

* property는 속성처럼 쓰게 해주므로
@property
def aaa():
	return self.__name

aaa는 메소드가 아니라 리턴되는 속성 기준으로 타입이 나온다. type(Shop.aaa) = str

설계한 사람만 이게 메소드인지 알고있으면 됨. 