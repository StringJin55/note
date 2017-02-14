# Models 
모델은 데이터에 대한 정보 소스이다. 저장된 데이터의 필수적인 필드(속성)와 행동(메소드)을 담고있다. 일반적으로 하나의 모델은 하나의 데이터베이스 테이블에 매핑된다.

기본:

* 각 모델은 파이썬 서브클래스다. (django.db.models.Model)

* 모델의 각 속성은 데이터베이스 필드(컬럼)를 나타낸다.
 
* 이것들을 통해 쟝고는 자동으로 생성된 데이터베이스에 접근할 수 있는 API를 제공한다.(ORM)

## 빠른 예 
first_name과 last_name 속성을 갖는 Person모델의 예

```python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```
여기서 first_name과 last_name은 클래스 속성으로 지정되며 데이터베이스 테이블의 컬럼에 매핑된다. Person은 데이터베이스의 테이블을 만든것이다. 다음과 같은 테이블을 만든다.

```SQL
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```
테이블 이름인 myapp_person은 자동으로 모델 메타데이터에서 자동으로 생기지만 오버라이드 할 수 있다.

id필드는 자동으로 추가되지만, 이것도 오버라이드 할 수 있다. autofield(primary_key=True)

CREATE TABLE 쿼리는 PostgreSQL(장고랑 가장 잘 맞는다)구문을 따르지만 쟝고에  지정한 데이터베이스가 있으면 그것의 SQL을 따른다.

## 모델의 사용 
모델을 정의하면 쟝고 세팅파일을 수정해야 한다. INSTALLED_APPS에 models.py를 담고있는 모듈이름을 추가해야한다. 
예를들어 myapp.models모듈에 앱의 모델이 있다면 다음처럼 수정해준다.

```python
INSTALLED_APPS = [
    #...
    'myapp',
    #...
]
```

INSTALLED_APPS에 새로운 앱을 추가하면, manage.py migrate를 해서 마이그레이션(변경사항)을 데이터베이스 스키마에 적용한다. 이 변경사항을 따로 저장하고 싶으면 먼저 manage.py makemigrations로 마이그레이션을 저장해준다.

써드파티앱의 경우 마이그레이션이 이미 존재할 수 있다. 그런경우 그냥 마이그레이트만 하면 되지만 새로생성해서 마이그레이션이 없는 앱의 경우는 메이크마이그레이션을 먼저 해서 마이그레이션을 먼저 만들어줘야 한다. 그 후 변경사항이 등록된 마이그레이션을 마이그레이트 한다. 

## 필드 

데이터베이스 필드는 중요하다. (모델이 중요한이유!!) 이 필드는 모델의 클래스 속성으로 지정된다. **modelsAPI와 충돌할 수 있는 이름으로 필드이름을 지정하지 말 것(clean, save, delete)**

### 필드 타입 
모델의 각 필드는 적합한 필드 클래스(속성이 클래스를 갖는다)의 인스턴스이어야 한다. 장고는 몇가지를 결정하기 위해 필드 클래스 타입을 이용한다.  

* The column type, which tells the database what kind of data to store (e.g. INTEGER, VARCHAR, TEXT).
* The default HTML widget to use when rendering a form field (e.g. <input type="text">, <select>). 장고가 검증해준다. 
* The minimal validation requirements, used in Django’s admin and in automatically-generated forms.

장고는 많은 내장 필드 타입을 가지고 있다. 
필드 타입은 직접 만들 수 있다.

### 필드 옵션 

각 필드는 필드특정 인수를 받는다. 예를들어 CharField는 VARCHAR문자열 이 담을 수 있는 크기를 정하는 max_length인수를 받는다. 

모든 필드타입에 사용가능한 일반적인 인수들이 있다. 물론 사용은 선택적이다. 

* null
	True면 데이터 베이스에 NULL(이라는 빈값)을 저장한다. 기본은 null=False
* blank
	True면 필드가 공백(비어있는 스트링)인것이 허용된다. 기본은 blank=False. 
	>null과는 다르다. null은 데이터베이스와 관련있고, blank는 검증과 관련있다.(?) blank=True이면 form 검증때 빈값인 것이 허용된다. blank=False면 필드에 값을 채워야 한다.   
 
* choices
	2차원 튜플값이 선택사항으로 쓰인다.(튜플을 모은 튜플) 이게 주어지면 폼 양식이 선택박스형으로 제공된다. 선택항목은 주어진 선택사항으로 제한된다. 각 튜플의 첫번째값은 데이터베이스에 저장된 값이다. 두번째 값은 기본 폼 위젯에서 보여지거나 get_foo_display()(models.Model 안에 정의되어있다. 해당 속성에 접근해서 매칭되는 초이스값을 갖고옴. 없으면 데이터베이스 안에 있는 값을 그냥 보여준다)로 접근할 수 있다. 
이런식으로 쓰면 데이터베이스 공간을 절약할 수 있다.

실습 	
	
	
* dafult
	
* help_text
* primary_key

> flat=True
> 모델클래스명.objects.values_list() 모델 객체 다 보여줌. 이렇게 다 가져와서 파이썬으로 정제하는 것보다 쿼리로 필요한 밸류만 가져오는게 성능이 더 좋다!
> flat=True는 튜플값에 밸류가 하나면 그냥 리스트로 만들어서 가져와줌
> ```
> In [4]: Person.objects.values_list('name')
>Out[4]: <QuerySet [('Fred Flintstone',)]>
>
>In [6]: Person.objects.values_list('name', flat=True)
>Out[6]: <QuerySet ['Fred Flintstone']>

>```

* unique
  
  
## 관계 
### 다대일
외래키, 위치인자, 
재귀적관계 - 자기가 자기 자신을 외래키로 갖는 경우 (선생님-학생 관계:선생님한명은 많은 학생을 가질 수 있다)

일-다 로 연결되어있을때 일 요소가 사라지면 다 요소도 같이 사라진다. (참조된 키가 사라지면 외래키들도 사라짐)

on_delete=models.CASCADE
장고 2.0부터는 on_delete는 기본인자.
CASCADE면 포린키를 담고잇는게 지워지면 같이 지워지는옵션

instructor 
p1, p2

>>> p1.instructor : 일대다 관계 맺기

related_name : 나를 참조하는 self 객체의 이름
이 이름을 역참조 이름대신 써도된다. (역참조할때는 클래스_set 으로 해준다. )

ForeignKey.related_name¶
The name to use for the relation from the related object back to this one. It’s also the default value for related_query_name (the name to use for the reverse filter name from the target model). See the related objects documentation for a full explanation and example. Note that you must set this value when defining relations on abstract models; and when you do so some special syntax is available.

If you’d prefer Django not to create a backwards relation, set related_name to '+' or end it with '+'. For example, this will ensure that the User model won’t have a backwards relation to this model:

```
user = models.ForeignKey(
    User,
    on_delete=models.CASCADE,
    related_name='+',
)
```

* CASCADE: 참조되는 테이블의 행이 삭제되었을 경우에는 참조하는 테이블과 대응되는 모든 행들이 삭제된다. 참조되는 테이블의 행이 갱신되었을 경우에는 참조하는 테이블의 외래 키 값은 같은 값으로 갱신된다.
* RESTRICT: 참조하는 테이블의 행이 남아 있는 경우에는 참조되는 테이블의 행을 갱신하거나 삭제할 수 없다. 이 경우에는 데이터 변경이 이루어지지 않는다.
* NO ACTION:참조되는 테이블에 대해 UPDATE 또는 DELETE가 실행된다. DBMS에서 SQL 문장의 실행 종료시에 참조 정합성을 만족하는지를 검사한다. RESTRICT와 차이점은, 트리거 또는 SQL 문장의 시멘틱스 자체가 외래 키의 제약을 채울 것이라는 데에 있다. 이 때는 SQL 문장 실행이 성공한다. 외래 키의 제약이 만족되지 않은 경우에는 SQL 문장 실행이 실패한다.
* SET NULL:참조되는 테이블에 대해 행이 갱신 또는 삭제되었을 경우, 참조하는 테이블의 행에 대한 외래 키 값은 NULL로 설정된다. 이 옵션은 참조하는 테이블의 외래 키에 NULL을 설정할 수 있는 경우에만 가능하다. NULL의 시멘틱스에 의해, 참조하는 테이블에 대해 NULL이 있는 행은, 참조되는 테이블의 행을 필요로 하지 않는다.
* SET DEFAULT: SET NULL과 비슷하지만, 참조되는 테이블의 행이 갱신 또는 삭제되었을 경우, 참조하는 테이블의 외래 키 값은 속성의 기본값(default)으로 설정된다.

### 다대다 관계

피자의 재료 하나는 여러가지 피자에 들어갈 수 잇다.
Pizza 앱에 실습!!

models.py

```python
from django.db import models


class Topping(models.Model):
    title = models.CharField(max_length=30)

    def __str__(self):
        return self.title


class Pizza(models.Model):
    title = models.CharField(max_length=100)
    # 피자와 토핑은 다대다 관계
    # 포함관계에서 상위요소에 해당하는 클래스에 매니투매니를 선언한다
    toppings = models.ManyToManyField(Topping)

    def __str__(self):
        return '{}, Topping : {}'.format(
            self.title,
            ', '.join(self.toppings.values_list('title', flat=True))
            # ', '.join([toppings.title for toppings in self.toppings.all()])
        )

```

```shell
In [5]: p1 = Pizza.objects.create(title='Cheese Pizza')

In [6]: t1 = Topping.objects.create(title='peperoni')

In [7]: p1.toppings.add(t1)

In [8]: p1
Out[8]: <Pizza: Cheese Pizza, Topping : peperoni>

```

피자와 토핑 테이블은 서로 참조관계가 없고

그 사이에 새로운 피자_토핑 테이블이 생겨서 양쪽을 포린키로 참조함

피자와 토핑은 포함관계가 있지만 토핑이 피자 역참조 가능
Pizza_set

피자_토핑 테이블은 자기의pk와 피자아이디와 토핑아이디 값만 가지고있다.

매니투매니의 테이블 이름은 복수형이 추천
매니투 매니도 재귀관계가 가능. (예, 친구관계, 팔로우관계)

#### 다대다의 재귀관계 
blog/models.py - User클래스

```python
class User(models.Model):
    # followers = models.ManyToManyField(
    #     'self'
    # )
    # following = models.ManyToManyField(
    #     'self'
    # )
    friends = models.ManyToManyField('self')
    name = models.CharField(max_length=50)

    def __str__(self):
        return self.name
```

```shell
In [2]: u1 = User.objects.create(name='lhy')

In [3]: u2 = User.objects.create(name='나 대 진')

In [4]: u3 = User.objects.create(name='최 용 일')



In [9]: u1.friends.add(u3)

In [10]: u3.friends.all()
Out[10]: <QuerySet [<User: lhy>]>

```
u1에만 친구로 넣어줘도 u3의 친구에도 추가가되어있다. 

반면에 양쪽이 매니두매니로 재귀일때 

```python
followers = models.ManyToManyField(
        'self'
    )
    following = models.ManyToManyField(
        'self'
    )
```
이때는 한쪽에만 넣어주면 다른쪽에 나오면 안되는데 나온다. 
그래서 옵션을 넣어준다.

```python
 followers = models.ManyToManyField(
        'self',
        related_name='following_set',
        symmetrical=False
    )
    following = models.ManyToManyField(
        'self',
        related_name='follower_set',
        symmetrical=False
    )
    
```
그러면

```shell
In [2]: u1 = User.objects.create(name='이한영')

In [3]: u2 = User.objects.create(name='나대진')

In [4]: u1.followers.add(u2)

In [5]: u1.followers.all()
Out[5]: <QuerySet [<User: 나대진>]>

In [6]: u2.followers.all()
Out[6]: <QuerySet []>

```

원하는 결과를 얻었다. symetrical(대칭적) 옵션에 false로 줘서 가능. 하나의 필드만 갖고도 팔로우-팔로잉 관계 만들 수 있다. 

```shell
In [8]: u1.following.all()
Out[8]: <QuerySet []>

In [9]: u1.following.add(u2)

In [10]: u1.following.all()
Out[10]: <QuerySet [<User: 나대진>]>

In [11]: u2.follower_set.all()
Out[11]: <QuerySet [<User: 이한영>]>

```
내가 팔로잉 하는 사람 추가하면 그사람 입장에서 나의 팔로워를 볼 수 있다. 
참조와 역참조만 갖고도 관계를 나타낼 수 있다. 


* ForeignKey() - 인자에 문자열로 넣으면 모듈상에서 위치에 상관없이 참조가 가능. 문자열이 아니면 컴파일될때 참조되는 클래스가 위쪽에 위치해야 한다. 문자열로 하면 다른 모듈의 클래스도 '모듈명.클래스명'으로 쓰는것이 가능!

### Extra fields on many-to-many relationships¶

다대다 관계에서는 자동으로 생겼던 다대다 테이블. 
그 테이블에 추가 필드를 넣은것이 중간자 테이블이다.
이것은 직접 포린키 지정해주면서 직접 만들어야 한다.

기존의 다대다 테이블처럼 포린키를 두개 갖는데 추가로 date_joined를 갖는다.



다대다 재귀모델에서도 thtough field로 소스와 타겟 순서를 적어줘야 한다. 여기에 적어준것만 사용됨





사람 
	친구관계
	
	사람1
	사람2
	

친구관계에 대한 정보
사람1
사람2
누가 이 둘을 소개시켜줬냐( 사람3) <-여기서 사람클래스가 3번쨰 사용되었다
언제 친구를 맺었냐



* python manage.py showmigrations : 마이그레이션들을 보여준다

# 170210

from datetime import datetime, date, time, timedelta
요 네가지 많이 쓴다

---
다대다 관계 실습

```
In [10]: Idol.objects.values_list('name', flat=True)
Out[10]: <QuerySet ['나나', '레이나', '리지', '유이', '레이나', '나나', '리지', '이영', '가은']>

In [11]: afterschool = Group.objects.create(name='애프터 스쿨')

In [12]: orangecaramel = Group.objects.create(name='오렌지캬라멜')

In [13]: from datetime import date

In [14]: m1 = Membership(idol=reina, group=orangecaramel, date_joined=date(2010, 6, 17))

In [15]: m1.save()
/usr/local/var/pyenv/versions/django_doc/lib/python3.4/site-packages/django/db/models/fields/__init__.py:1372: RuntimeWarning: DateTimeField Membership.date_joined received a naive datetime (2010-06-17 00:00:00) while time zone support is active.
  RuntimeWarning)

```
아이돌을 추가해주고, 그룹을 각 각 만든뒤에 그룹에 아이돌멤버를 추가해준다.

```
In [16]: from datetime import datetime, date, time, timedelta

In [17]: m1.date_joined
Out[17]: datetime.date(2010, 6, 17)

In [18]: Membership.objects.create(idol=nana, group=orangecaramel, date_joined=date(2010, 6, 17))
/usr/local/var/pyenv/versions/django_doc/lib/python3.4/site-packages/django/db/models/fields/__init__.py:1372: RuntimeWarning: DateTimeField Membership.date_joined received a naive datetime (2010-06-17 00:00:00) while time zone support is active.
  RuntimeWarning)
Out[18]: <Membership: Membership object>

```
계속 추가추가!

```python
In [24]: Membership.objects.all()
Out[24]: <QuerySet [<Membership: Membership object>, <Membership: Membership object>, <Membership: Membership object>, <Membership: Membership object>]>

In [25]: Membership.objects.get(id=3)
Out[25]: <Membership: Membership object>

In [26]: Membership.objects.get(id=3).delete()
Out[26]: (1, {'blog.Membership': 1})

In [27]: Membership.objects.count()
Out[27]: 3

```
잘못들어간 멤버를 지우기, 숫자 세보기

```python
In [31]: orangecaramel.members.all()
Out[31]: <QuerySet [<Idol: 레이나>, <Idol: 나나>, <Idol: 리지>]>

In [32]: nana.group_set.all()
Out[32]: <QuerySet [<Group: 오렌지캬라멜>]>

In [33]: nana.membership_set.all()
Out[33]: <QuerySet [<Membership: Membership object>]>

In [34]: nana.membership_set.values_list('group__name', 'date_joined')
Out[34]: <QuerySet [('오렌지캬라멜', datetime.datetime(2010, 6, 17, 0, 0, tzinfo=<UTC>))]>

```

역참조 하는것

```python
In [36]: for idol in Idol.objects.all():
    ...:     Membership.objects.create(idol=idol, group=afterschool, date_joined=date(2009, 1, 15))
    ...:     
    ...:     
/usr/local/var/pyenv/versions/django_doc/lib/python3.4/site-packages/django/db/models/fields/__init__.py:1372: RuntimeWarning: DateTimeField Membership.date_joined received a naive datetime (2009-01-15 00:00:00) while time zone support is active.
  RuntimeWarning)
```

for문으로 한꺼번에 여섯명 넣어주기

```python
In [3]: Membership.objects.all()
Out[3]: <QuerySet [<Membership: 오렌지캬라멜 - 레이나 (2010-06-17 00:00:00+00:00)>, <Membership: 오렌지캬라멜 - 나나 (2010-06-17 00:00:00+00:00)>, <Membe지 (2010-06-17 00:00:00+00:00)>, <Membership: 애프터 스쿨 - 유이 (2009-01-15 00:00:00+00:00)>, <Membership: 애프터 스쿨 - 레이나 (2009-01-15 00:00:00+00:p: 애프터 스쿨 - 나나 (2009-01-15 00:00:00+00:00)>, <Membership: 애프터 스쿨 - 리지 (2009-01-15 00:00:00+00:00)>, <Membership: 애프터 스쿨 - 이영 (2009-0, <Membership: 애프터 스쿨 - 가은 (2009-01-15 00:00:00+00:00)>]>

```
결과확인 


idol과 group은 membership이라는 중간자모델이 있어서
idol에서 그룹에 바로 add하거나 create식으로 할 수 없다.
(피자와 토핑의 예와는 다름!!)
역참조 가능하지만, 멤버십 클래스를 꼭 이용해줘야 한다.

clear()하면 연결된 관계가 사라진다. (중간자 테이블의 정보가 사라지는것!)

* 중간자테이블에는 참조하는 프라이머리키값만 포린키로 저장되어 있다. 인스턴스를 참조하는 데이터만 저장되었다고 봐도 됨. (참조하는 테이블의 레코드를 카피해오는 것이 아니라.)


### 일대일 관계
사용자의 추가프로필 정보 같은 경우

## Models across files
다른 모듈의 모델을 사용하기

custom field types
필드의 형식을 지정하고 유효성 검사 하는 경우, 필드의 디스플레이 방식을 지정하는 경우, (핸드폰 번호 저장할때 등)

## meta options
ordering 테이블에서 데이터를 꺼낼때 어떤 순서로 꺼내느냐

메타데이터에는 필드를 제외한 모든 정보가 들어가있다.
반드시 쓸 필요는 없다

## model attributes

Model.objects : Manager객체. 가장 중요한 객체다. 
manager는 테이블 전체 작업. 모델 메소드는 인스턴스 메서드로 레코드 하나당 작업

property 함수로 접근 안하고 속성처럼 접근해도 함수의 리턴값이 반환 (@property 데코레이터부분...)

### 오버라이딩 메소드 
save() 예를들어 블로그의 게시글 수를 미리 저장해두고 업데이트가있을때마다 수정하는경우 (매번 카운트하지 않고)

super(부모클래스, self) 이 문법은 파이썬 2 방식. 이제는 안스고 그냥 super()한다

# Model inheritance 모델 상속

### 추상 기본 클래스 (abs = abstract base class)
추상 클래스 자체는 테이블로 만들어지지 않고, 추상 기본 클래스를 상속받은 클래스만 테이블로 만들어진다. 클래스 메타속성에 abstract True로 준다. 

참조할 테이블이 결과적으로 1개

```python
from django.db import models

class CommonInfo(models.Model):
    name = models.CharField(max_length=100)
    age = models.PositiveIntegerField()

    class Meta:
        abstract = True

class Student(CommonInfo):
    home_group = models.CharField(max_length=5)
```

commonInfo는 테이블로 생기지 않고 대신 Student의 테이블에 name과 age 필드도 같이 생긴다. 상속받았기 때문에. 
당연히 commonInfo는 쉘에서 사용하지 못 한다. 


### meta inheritance
ordering 이 메타로 들어가 잇어서 네임으로 정렬해준다

### Be careful with related_name and related_query_name
related_name : 역방향으로 참조할때
related_query_name : 역방향으로 참조하는것을 필터링할때의 키값 이름을 정해줌


[ForeignKey.related_query_name](https://docs.djangoproject.com/en/1.10/ref/models/fields/#django.db.models.ForeignKey.related_query_name)
The name to use for the reverse filter name from the target model. It defaults to the value of related_name or default_related_name if set, otherwise it defaults to the name of the model:

```python
# Declare the ForeignKey with related_query_name
class Tag(models.Model):
    article = models.ForeignKey(
        Article,
        on_delete=models.CASCADE,
        related_name="tags",
        related_query_name="tag",
    )
    name = models.CharField(max_length=255)

# That's now the name of the reverse filter
Article.objects.filter(tag__name="important")
```

Tag가 Article을 참조함 Article은 Tag를 역참조.
Article.tag_set.get 이런식으로 써야함
related_name덕분에 Article.tags. 이렇게 사용가능
하지만 Article.tags.filter(tags__name=) 이렇게 필터링할때의 키 이름도 바뀐다. 그래서 related_query_name으로 저걸 지정해줄수잇음

'%(class)s' is replaced by the lower-cased name of the child class that the field is used in.
'%(app_label)s' is replaced by the lower-cased name of the app the child class is contained within. Each installed application name must be unique and the model class names within each app must also be unique, therefore the resulting name will end up being different.


얘네 둘 조심해줘야함.

## Multi-table inheritance

상속해주는 상위 테이블은 그대로 있고, 추가적인 정보만 상속받는 테이블에 기록된다. (id값은 기존 상위테이블의 id에 이어서) 대신에 레코드를 가져올때는 두 테이블을 다 검색해야 하기 때문에 성능이 느리다. (테이블 조인) 참조할 테이블이 결과적으로 2개

```ㅔㅛ쇄ㅜ
from django.db import models

class Place(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=80)

class Restaurant(Place):
    serves_hot_dogs = models.BooleanField(default=False)
    serves_pizza = models.BooleanField(default=False)
All of the fields of Place will also be available in Restaurant, although the data will reside in a different database table. So these are both possible:

>>> Place.objects.filter(name="Bob's Cafe")
>>> Restaurant.objects.filter(name="Bob's Cafe")
If you have a Place that is also a Restaurant, you can get from the Place object to the Restaurant object by using the lower-case version of the model name:

>>> p = Place.objects.get(id=12)
# If p is a Restaurant object, this will give the child class:
>>> p.restaurant
<Restaurant: ...>
```
## 상속과 역참조

### Specifying the parent link field

## Proxy model
이름만 다르게 지정하고 싶다던가 같은 테이블에 특정한 부분만 기능 추가할 때 사용

자식테이블이 따로 안생기고 같은 데이터에 대해 다른 객체 형태가 생김. 실제 테이블은 하나지만. 

```python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

class MyPerson(Person):
    class Meta:
        proxy = True

    def do_something(self):
        # ...
        pass
The MyPerson class operates on the same database table as its parent Person class. In particular, any new instances of Person will also be accessible through MyPerson, and vice-versa:

>>> p = Person.objects.create(first_name="foobar")
>>> MyPerson.objects.get(first_name="foobar")
<MyPerson: foobar>
```

부모 모델 하나에 대해서만 정의 할 수 있다. 

## 다중상속 

클래스 메타에서 준 속성은 하나만 유지됙 나머지 추가 속성은 삭제된다

오토필드로 pk필드를 정해준다. (상속하는 부모에)

```python
class Piece(models.Model):
    pass

class Article(Piece):
    ...

class Book(Piece):
    ...

class BookReview(Book, Article):
    pass
```
다중 테이블 상속. 여기서 piece만 아이디를 갖는다




