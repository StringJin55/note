# instagram 실습

170213

## 프로젝트로 사용할 폴더 생성
1. pyenv virtualenv 3.4.3 <환경명>
2. pyenv local <환경명>
3. pip install django
4. django-admin startproject <프로젝트명>
5. mv <프로젝트명> django_app
6. Pycharm interpreter세팅
7. django_app폴더를 Sources root로 설정
8. django_extensions, ipython pip로 설치(models.py내용이 자동임포트)
9. source_root (mark directory as) 설정


프로젝트명
instagram

## DB모델 설계

### member app

#### MyUser모델

Attributes

* username
* last_name
* first_name
* nickname
* email
* date_joined
* last_modified
* following (팔로우하고 있는 User목록)

```python
username = models.CharField(max_length=30, unique=True)
    last_name = models.CharField('성', max_length=20)
    first_name = models.CharField('이름', max_length=20)
    nickname = models.CharField('닉네임', max_length=24)
    email = models.EmailField('이메일', blank=True)
    date_joined = models.DateTimeField(auto_now=True)
    last_modified = models.DateTimeField(auto_now=True)
    following = models.ManyToManyField('self', symmetrical=False, blank=True)

    def __str__(self):
        return self.first_name + self.last_name
```

객체를 자동으로 생성해주는 메소드를 만들었다. 

```python
@staticmethod
    def create_dummy_user(num):
        """
        num개수 만큼 User1 ~ User<num>까지 임의의 유저를 생성한다
        :return: 생선된 유저 수
        """
        import random
        last_name_list = ['박', '신', '김', '정', '송', '이', '백']
        first_name_list = ['보현', '준성', '은별', '재희', '선아', '다희', '민주', '규원']
        nickname_list = ['찬이맘', '아재', '세글자', '티부', '해방촌가수']
        created_count = 0
        for i in range(num):
            try:
                MyUser.objects.create(
                    username='User{}'.format(i + 1),
                    last_name=random.choice(last_name_list),
                    first_name=random.choice(first_name_list),
                    nickname=random.choice(nickname_list),
                )
                created_count += 1
            except IntegrityError as e:
                print(e)
        return created_count
```
`random`을 사용해서 리스트안에서 랜덤하게 담아줌!





객체를 변수에 자동으로 할당해주는 메소드를 만들었다. 

```python
# 자동으로 변수에 할당해주는 메소드를 만들었다!!!
    @staticmethod
    def assign_global_variables():
        # sys모듈은 파이썬 인터프리터 관련 내장 모듈
        import sys
        # __main__모듈을 module변수에 할당
        module = sys.modules['__main__']
        # MyUser객체 중 'User'로 시작하는 객체들만 조회하여 user변수에 할당
        users = MyUser.objects.filter(username__startswith='User')
        # users를 순회하며
        for index, user in enumerate(users):
            # __main__모듈에 u1,u2,u3... 이름으로 각 MyUser객체를 할당
            setattr(module, 'u{}'.format(index+1), user)
```

확인해보니 변수에 자동으로 잘 할당되었다.
```shell
In [2]: MyUser.assign_global_variables()

In [3]: globals()
...
 'u1': <MyUser: 재희김>,
 'u10': <MyUser: 선아이>,
 'u2': <MyUser: 은별박>,
 'u3': <MyUser: 다희백>,
 'u4': <MyUser: 은별김>,
 'u5': <MyUser: 규원김>,
 'u6': <MyUser: 준성김>,
 'u7': <MyUser: 규원백>,
 'u8': <MyUser: 다희박>,
 'u9': <MyUser: 보현백>}
```

아직 모델 메소드는 구현하지 않은 상태.

`u1.following.add(u2)`

참조된 값의 역참조를 해서 누가 팔로잉하고 있는지 확인해본다.

`u2.myuser_set.all()`

Methods

* follow (팔로우 처리)
* unfollow (언팔로우 처리)
* followers (자신을 팔로우하고 있는 User목록, Property처리)
* change_nickname

```python
    def follow(self, user):
        self.following.add(user)

    def unfollow(self, user):
        self.following.remove(user)

    # 마음대로 수정못하게 읽기전용으로
    @property
    def followers(self):
        return self.follower_set.all()

    def change_nickname(self, new_nickname):
        self.nickname = new_nickname
        self.save()
        
```


### post app

#### Post 모델

Attributes

* author (ForienKey, User)
* photo
Methods

* add_comment
#### Comment 모델

Attributes

* author (ForienKey, User)
* post (ForienKey, Post)


로그인 숙제

+ 수업시간에 로그인폼 이용해서 로그인 페이지 구현

### 유저모델 상속받아서 확장하기
`from django.contrib.auth.base_user import AbstractBaseUser`

커맨드+클릭 으로 AbstractBaseUser안을 살펴보았다.

rm db.sqlite3 
데이터베이스를 날리고 시작한다.
migrations도 다 지움

setting.py 에 AUTH_USER_MODEL 지정

[Specifying a custom user model](https://docs.djangoproject.com/en/1.10/topics/auth/customizing/#specifying-a-custom-user-model)
보고 실습한다

1. username field 만들기
2. baseusermanager 상속받는 클래스 작성
3. create_user, create_superuser 만들기

PermissionsMixin 상속
REQUIRED_FIELDS 채워준다.
