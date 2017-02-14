# User authentication in Django
>Common Web application tools/Authentication:
>

## Overview
authrization : 권한
authentication : 권한이 있는지

The auth system consists of:

* Users
* Permissions: Binary (yes/no) flags designating whether a user may perform a certain task.
* Groups: A generic way of applying labels and permissions to more than one user.
* A configurable password hashing system
* Forms and view tools for logging in users, or restricting content
* A pluggable backend system


middleware 클라이언트와 서버으 ㅣ중간과정 

migrate하면 정의된 migrations을 테이블에 적용

## Usage

Using the Django authentication system

### User objects

The primary attributes of the default user are:

* username
* password
* email
* first_name
* last_name

### Creating superusers
```shell
$ python manage.py createsuperuser --username=joe --email=joe@example.com
```

### Changing passwords

python manage.py changepassword username

```shell
>>> from django.contrib.auth.models import User
>>> u = User.objects.get(username='john')
>>> u.set_password('new password')
>>> u.save()
```

### Authenticating users
**authenticate(**credentials)**
사용자인증 == 로그인

```python
from django.contrib.auth import authenticate
user = authenticate(username='john', password='secret')
if user is not None:
    # A backend authenticated the credentials
else:
    # No backend authenticated the credentials
```

