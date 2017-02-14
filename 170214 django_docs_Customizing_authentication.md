# Customizing authentication in Django

## Extending the existing User model

프록시모델

어쩔 수 없는 경우


## Substituting a custom User model
내장 유저 모델을 바꿔서 쓴당

```
AUTH_USER_MODEL = 'myapp.MyUser'
```

### Using a custom user model when starting a project
프로젝트 시작할 때 유저모델 상속받아 확장해서 처음부터 쓰는걸 권장한다. 디폴트 유저모델이 잘맞아도 쓰지말아라. it’s highly recommended to set up a custom user model, even if the default User model is sufficient for you. 

### Changing to a custom user model mid-project
이건 힘들다. 안될거니까 알아서 잘하시요..


### Reusable apps and AUTH_USER_MODEL
재사용 가능 앱을 만들땐 커스텀 유저 모델 안쓴다

### Referencing the User model

#### get_user_model()
직접 참조하는대신 이걸로. 커스텀 사용자 모델이 auth_user_model에 지정된 경우 가져와준다. 아니면 기본 유저모델

```python
class Article(models.Model):
    author = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )
```
유저 모델 참조하는 포린키의 기본형


### [Specifying a custom user model](https://docs.djangoproject.com/en/1.10/topics/auth/customizing/#specifying-a-custom-user-model)

 inherit from [AbstractBaseUser](https://docs.djangoproject.com/en/1.10/topics/auth/customizing/#django.contrib.auth.models.AbstractBaseUser).
저걸 상속받고 나머지들을 구현해준다

#### class models.CustomUser
#### USERNAME_FIELD
#### REQUIRED_FIELDS
##### is_active
기본필드

#### class models.CustomUserManager

### Extending Django’s default User