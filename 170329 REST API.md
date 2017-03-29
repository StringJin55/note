# REST API



## REST 구성

| 구성요소            | 내용                | 표현방법                  |
| --------------- | ----------------- | --------------------- |
| resource        | 자원을 정의            | HTTP URI              |
| verb            | 자원에 대한 행위를 정의     | HTTP Method           |
| representations | 자원에 대한 행위의 내용을 정의 | HTTP message pay load |



## HTTP method

|        |           |
| ------ | --------- |
| POST   | 생성        |
| GET    | 조회        |
| DELETE | 삭제        |
| PUT    | 한꺼번에 업데이트 |
| PATCH  | 부분 업데이트   |

![](/Users/kizmo04/Dropbox/note/img/스크린샷 2017-03-29 오후 3.12.57.png)



## 리소스

* 모든 개체를 리소스 단위로 표현
* /{리소스 그룹명}/{리소스 ID}/
* RESTS는 리소스에 대한 CRUD(Create/Read/Update/Delete)행위로 표현



## URL

* 직관적이고 2 depth 정도로만
* snake_case , camelCase로 명명(소문자 원칙,단어는 전부 붙여서, 다른단어가 이어질때는 뒤에오는 단어의 앞글자를 대문자로)
* 단수대신 복수형으로 /dogs/uki
* 동사대신 명사형 HTTP POST: /dogs/{puppy}/owner/{terry}



## 에러처리

* 200: 성공

* 400: BAD REQUEST - field validation 실패

* 401: 인증, 인가 실패

* 404: 해당 리소스를 찾을 수 없음

* 500: 서버에러

* 에러 상세내용은 HTTP body에 표현

* ```json
  {
    "error": 코드,
    "message": "Authorization failed",
    "detail": "User doesn't have permission to update user profile"
  }
  ```



## 버전관리

* 하위 호환성 보장 
* API 버전 정의 방법 
  * api.server.com/{서비스 명}/{버전}/{리소스}/
  * v2.0



## 페이징

* 페이스북 스타일이 직관적
  * /records?offset=100&limit=25 (100번째 레코드부터 25개 출력)



## 부분 응답(partial response)

* REST API 응답 중 일부만 응답 받는 방식
* 페이스북 스타일이 직관적
  * /terry/friends?fields=id,name



## API 보안

* 인증(authentication)
* 인가(authorization)
* 네트워크 레벨 전송 암호화
* 메세지 무결성 보장
* 메세지 본문 암호화
* HTTPS 사용, session은 Stateless에 위배됨. 



## API 문서화

* 2단계(설계용, 배포용)로 진행하는 것이 좋음

![](/Users/kizmo04/Dropbox/note/img/스크린샷 2017-03-29 오후 3.08.36.png)



## API 관리 시스템

* Amazon API Gateway



## 레퍼런스

[REST API 설계](https://www.slideshare.net/Byungwook/rest-api-60505484)

[RESTful API 설계](https://www.slideshare.net/brotherjinho/restful-api-64494716)

[REST API 디자인 개요](https://www.slideshare.net/nexusz99/rest-api-48600643)

[RESTful API 제대로 만들기](https://www.slideshare.net/ssuser7887b3/restful-api?next_slideshow=1)