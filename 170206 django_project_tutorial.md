# Django Docs - Tutorial

## 프로젝트 폴더 생성 
```shell
➜  projects git:(master) ✗ pwd
/Users/kizmo04/Dropbox/projects


➜  projects git:(master) ✗ mkdir django_tutorial


➜  projects git:(master) ✗ cd django_tutorial


➜  django_tutorial git:(master) ✗ pyenv virtualenv 3.4.3 django_tutorial_env
Ignoring indexes: https://pypi.python.org/simple
Requirement already satisfied (use --upgrade to upgrade): setuptools in /usr/local/var/pyenv/versions/3.4.3/envs/django_tutorial_env/lib/python3.4/site-packages
Requirement already satisfied (use --upgrade to upgrade): pip in /usr/local/var/pyenv/versions/3.4.3/envs/django_tutorial_env/lib/python3.4/site-packages


➜  django_tutorial git:(master) ✗ pwd
/Users/kizmo04/Dropbox/projects/django_tutorial


➜  django_tutorial git:(master) ✗ pyenv local django_tutorial_env


(django_tutorial_env) ➜  django_tutorial git:(master) ✗ pip install django
You are using pip version 6.0.8, however version 9.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
Collecting django
  Downloading Django-1.10.5-py2.py3-none-any.whl (6.8MB)
    100% |################################| 6.8MB 92kB/s
Installing collected packages: django

Successfully installed django-1.10.5


(django_tutorial_env) ➜  django_tutorial git:(master) ✗ django-admin startproject mysite
(django_tutorial_env) ➜  django_tutorial git:(master) ✗ tree
.
└── mysite
    ├── manage.py
    └── mysite
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py

2 directories, 5 files
```

쟝고는 프로젝트 폴더 안쪽에 프로젝트명으로된 세팅폴더가 있고 그안에 세팅파일이 모두 들어가있다. 그래서 헷갈리기 때문에 startproject해서 만든 폴더 이름을 바꿔준다.(바깥폴더) 세팅폴더의 이름은 바꿀 수 없다. 

```shell
(django_tutorial_env) ➜  django_tutorial git:(master) ✗ mv mysite django_app
(django_tutorial_env) ➜  django_tutorial git:(master) ✗ tree
.
└── django_app
    ├── manage.py
    └── mysite
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py

2 directories, 5 files
```

`pip freeze > requirements.txt`


## 프로젝트 폴더 깃 설정 
`.gitignore` 만들기 
`git init` 하기

```shell

(django_tutorial_env) ➜  django_tutorial git:(master) ✗ tree
.
├── django_app
│   ├── manage.py
│   └── mysite
│       ├── __init__.py
│       ├── settings.py
│       ├── urls.py
│       └── wsgi.py
└── requirements.txt

2 directories, 6 files
```
django_tutorial 에서 git init 해주었다.
(requirements.txt 와 .gitignore도 생성!)

```shell
(django_tutorial_env) ➜  django_tutorial git:(master) ✗ git add -A
(django_tutorial_env) ➜  django_tutorial git:(master) ✗ git commit -m "first commit"
```
`git add .` 은 현재폴더 추가

```shell
(django_tutorial_env) ➜  django_tutorial git:(master) git push -u origin master
```
푸시해주고

```shell
(django_tutorial_env) ➜  django_tutorial git:(master) ✗ git remote -v
origin	https://github.com/kizmo04/django_tutorial.git (fetch)
origin	https://github.com/kizmo04/django_tutorial.git (push)
```
깃 리모트 확인해보는 명령어 `git remote -v`


```shell
(django_tutorial_env) ➜  django_app git:(master) ./manage.py runserver
```

>`~/.zshrc` 의 PATH 경로중에 파이썬도 포함되어 있어서 `python` 명령어로 실행해주지 않아도 된다. 그냥 `manage.py`경로만 써주면됨!

서버가 잘 돌아가는 지 확인!!


## pycharm 설정 
프로젝트 인터프리터 설정해주기!
`/usr/local/var/pyenv/versions/~~~~~`


* 작은 단위로 구분된 기능이 어플리케이션(앱)
* `tree -I '__pycache__'` : 이걸로 캐시구조는 무시. zshrc에 알리아스로 등록해놨음 `tree-py` 


## url 설정 
polls/urls.py
mysite/urls.py


## 데이터베이스 

* pycharm - keymap 에 reformat 단축키 지정(커맨드L) 파이썬 코드 규격에 맞게 맞춰준다. 자주해줄것!!

* .git 폴더 날리기 : `rm -rf 폴더명` 폴더명 안써주면 다날라가니까 조심!! 

* .gitignor 안에서 pycharm 부분 수정해줌. .idea/ 추가


1. mysite/setting.py 의 installed_app에 'polls.apps.PollsConfig' 추가

2. `./manage.py makemigrations polls`

polls/migrations 에 0001_initial.py 로 파일이 생긴다! 마이그리이션 할때마다 생긴다 

3. `./manage.py migrate`로 마이그레이션 데이터베이스에 적용

pip 로 ipython과 django_extensions 설치후 installed_apps에 djnago_extensions 추가

installed_apps의 순서 는 맨위에서부터 내장앱, 외부 라이브러리앱, 내가만드는앱 순서

django_extensions 설치했으면 manage.py shell_plus로 쉘 실행시킨다 


* q.save()해야 데이터 베이스에 저장됨. 그전에는 메모리에만 남아있는거다. 

```shell
In [19]: for item in Question.objects.all():
    ...:     print(item.question_text)
What's new?
What's new?
What's up?

```
질문이 세개나 들어가서 확인하기위해 출력해보았다! 

모델에 `__str__()`메소드 추가하기 


sqlitebrowser 설치하기

프로젝트 폴더에 있는 db.slite3 파일 열어서 본다.


```shell
In [1]: from polls.models import Choice, Question

In [2]: Question.objects.all()
Out[2]: <QuerySet [<Question: What's new?>, <Question: What's new?>, <Question: What's up?>]>

In [3]: Question.objects.filter(id=1)
Out[3]: <QuerySet [<Question: What's new?>]>

In [4]: print(Question.objects.filter(id=1).query)
SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 1
```
쿼리를 보면 이해가 쉬움

```shell
In [12]: print(Question.objects.all().query)
SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question"

```

```shell
In [17]: print(Question.objects.filter(pub_date__year=current_year).query)
SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."pub_date" BETWEEN 2017-01-01 AND 2017-12-31
```

get() 해당조건에 맞는 객체 한개만 가져오고 없으면 에러남
filter()는 리스트로 다 가져오고 없어도 에러안남

역참조할때 자기클래스명(소문자)_set
1대 다의 관계에서 1쪽에서 다를 참조할때는 이렇게 하고
다에서 1을 참조할때는 하나밖에 없으니까 그냥 속성명으로 참조

초이스에서는 퀘스천으로 바로 접근. 퀘스천에서는 choice_set으로 접근


>개발자는 API를 통해 관계를 자동으로 필요한만큼 계속 따라갈 수 있습니다.
계를 분리하려면 이중 밑줄을 사용합니다.
원하는 만큼 중첩이 가능하며, 따라갈 수 있는 관계 개수에는 제한이 없습니다.

.delete() 삭제

## 관리자 사이트 

polls/admin.py

```shell
from django.contrib import admin

from .models import Question

admin.site.register(Question)

```

## 뷰 만들기 -3	부

```python
from django.http import HttpResponse
from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ', '.join([q.question_text for q in latest_question_list])
    return HttpResponse(output)

```

    
pub_date 앞에 - 는 내림차순 하라는 것


디버그 config
python - 인터프리터, 스크립트, 스크립트 파라미터, 워킹디렉토리 설정하기

polls/views.py , urls.py 코드 추가

templates/index.html 추가하고

### 실제로 뭔가하는 뷰 작성하기 
까지는 똑같이 함!!
이후
settings.py에 

```
# 템플릿 디렉토리 path
TEMPLATES_DIR = os.path.join(BASE_DIR, 'templates')

'DIRS': [
            TEMPLATES_DIR,
        ],
```
를 추가함. 

BASE_DIR 은 장고앱을 가리킴.

* 서버 올릴때 setting.py는 두번 로드돼서 두번 실행된다

빈 객체는 False로 판단됨.

커맨드키 누르고 메소드 클릭하면 연결된 페이지로 넘어간다

#### 지름길:render() 

* shift + alt + command + L 해서 옵티마이즈 임포트 옵션 주면 안쓰는 임포트 없어짐


스텁 메소드 - 임시로 만든 메소드, 이걸로 작동확인만 간단하게 할 수 있다.
