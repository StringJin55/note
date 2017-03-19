해당 유저의 프로필 이미지를 바꾼다
    0. 유저모델에 img_profile필드 추가 mig
    1. change_profile_image.html 작성
    2. profileImageForm생성
    3. 해당 Form을 템플릿에 렌더링
    4. request.method == 'POST'일때 request.FILES의 값을 이용해서 request.user의
    img profile을 변경 저장
    5. 처리 완료 후 member:profile로 이동
    6. profile.html에서 user의 프로필 이미지를 img 태그를 사용해서 보여줌 {{ MEDIA_URL }}
    
    
### 프로필 이미지 바꾸기 

단위 테스트작성(멤버 뷰)

모델 폼 사용

모델폼은 인스턴스를 모델루 미리 담아두고 추가 값도 거기에 업데이트됨.
[The save() method](https://docs.djangoproject.com/en/1.10/topics/forms/modelforms/#the-save-method)
폼.save()하면 
모델 객체가 생성되고 저장됨 
인스턴스가 있을 경우 업데이트, 인스턴스가 없을경우 새로 생성


인스타그램 끗



# YouTube API
[YouTube API시작하기](https://developers.google.com/youtube/v3/getting-started)

API 키를 받는다.

API키는 유출되면 안됨!!!
그래서 따로 관리하고 유출안되게 하기 위해.gitignore에 아래를 추가해준다
```
# Config folders
.conf/
```

**djnago_app(프로젝트폴더)를 소스루트로 설정하는것 잊지말기**

```json
{
  "youtube": {
    "API_KEY": "AIzaSyDCwRLdf4wMHXm9ZVflH30ljG19WNtoX-c"
  }
}
```

.conf/settings_local.json
에 API키를 딕셔너리 형태로 담아준다. JSON (자바스크립트에서 객체를 나타내는 방식) 

```python
ROOT_DIR = os.path.dirname(BASE_DIR)
CONF_DIR = os.path.join(ROOT_DIR, '.conf')
config = json.loads(open(os.path.join(CONF_DIR, 'settings_local.json')).read())
```

settings.py에 CONF_DIR설정, config로 json읽어오게설정
json라이브러리로 loads해서 읽어오면 JSON객체를 파이썬 딕셔너리 형태로 읽어온다

