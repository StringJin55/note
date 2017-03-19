# Sending email

## send_mail()
mail.send()를 반환한다.

파이썬은 [smtplib](https://docs.python.org/3/library/smtplib.html#module-smtplib)로 비교적 쉽게 메일을 보낼 수 있게 해준다. 장고는 이걸 더 빠르게 해줌. SMTP를 못쓰는 플랫폼에도 지원해줌.

django.core.mail 모듈을 이용한 코드 샘플

```python
from django.core.mail import send_mail

send_mail(
    'Subject here',
    'Here is the message.',
    'from@example.com',
    ['to@example.com'],
    fail_silently=False,
)
```
그리고 settings.py

```python
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_USE_TLS = True
EMAIL_HOST_USER = config['email']['user']
EMAIL_HOST_PASSWORD = config['email']['password']
```
구글계정과 비밀번호를 사용했음. 메일을 보내기 위한 최소한의 설정은 위와 같다.

메일은 EMAIL_HOST와 EMAIL_PORT에 세팅된 SMTP호스트와 포트로 보내진다. (위의 네가지 세팅으로 해결됨.)

