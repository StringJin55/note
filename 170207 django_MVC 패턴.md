# MVC 패턴 

쟝고는 웹 서버 앱에서 들어온 url요청을 url.py를 통해 알맞는 뷰에 보내줌. 뷰.py는 템플릿 불러옴 다시 요청을 웹서버로 보냄



# Django girls tutorial

1.가상환경생성
2.프로젝트 폴더생성
3. 프로젝트 폴더 내부에 가상환경 적용
4. 장고 설치
5. 장고 어드민 스타트 프로젝트 마이사이트
6. 장고 프로젝트 컨테이너 폴더 이름 장고 앱으로 변경
7. 깃 저장소 초기화
8. 깃이그노어 작성 (.idea/추가)
9. 깃 애드 후에 깃 스테이터스로 깃이그노어 잘 작동하는지 확인
10. setting.py - 시간대,언어 바꾸기, python manage.py migrate , 스태틱dir 설정, python manage.py startapp blog 하고 인스톨드앱에 블로그앱 적어주기


blank 빈문자열, 빈값 
null 값이없음

타임존 - 쟝고에서 시간은 다 똑같이 들어가는데 표현만 설정해준 시간대에 따라서 다르게 표현된다

auto_now - 수정될때마다 기록됨
auto_now_add - 생성될때 기록됨

[https://docs.djangoproject.com/en/1.10/ref/models/fields/](https://docs.djangoproject.com/en/1.10/ref/models/fields/)

manage.py 를 그냥 실행하면 사용가능한 서브커맨드들 나옴. 
or
manage.py help 하면 서브커맨드 확인 가능


git diff
git reset

11. 블로그 안에 포스모델 만들고 메잌마이그레이션즈


쿼리문에서 부등호 쓸 수 없어서 대신 써준다. 
lte
lt 작은
gt 큰

setting.py 에 디버그 모드 잇다 (boolean)

1. 파이썬 쉘에서 Post를 추가하고 저장해보기
+2. id 가 2인 Post 객체를 발행해보기
+3. 템플릿의 post_list키에 빈 리스트가 ㅇ전달 될 경우 포스트 없음을 출력


12. post-list,
post-detail 뷰 템플릿 구현

Detail view를 만들어 봅니다.
+ 1. view에 post_detail 함수 추가
+    post_detail은 인자로 post_id를 받는다.
+
+ 2. urls.py에 해당 view와 연결하는 url 추가
+    정규표현식으로 패턴네밍 post_id이름을 갖는 숫자 1개이상의 패턴을 등록
+ 3. post_detail뷰가 원하는 URL에서 잘 출력되는지 확인 후(stub메서드 사용)
+    get 퀘리셋을 사용해서 (Post.objects.get(...))


13. urls.py {% url 'name' args=parameter %}url 바꾸기. 템플릿에서 url을 역참조해서 찾아간다. name 키워드에 따라서 동적으로 url 생성 (args까지 오면 같이 받아서 url패턴이 있는지 찾아보고 잇으면 거기에 맞게 url을 만들어줌)

## 템플릿 확장하기 

템플릿의 확장기능 - 반복되는 부분은 미리 구성해서 재사용할 수 잇다. (html상단 부분)
{% block %}

## 폼 만들기 

post-add

views.py
	def post_add(request)
	
urls.py
	'post/add/'에서 작동하도록, name='post_add'
템플릿
	post-list.html에서 write post버튼을 만들고 해당 버튼에 post_add로 가는 링크 생성
	
## 서버로 데이터 보내기 

csrf 검증 : 현재 페이지가 이전에 요청한 페이지가 맞다는 인증이 필요함. (내가 나인가?!!!)
사용자가 매번 페이지를 로드(서버에서 정보가 올때)할때마다 서버에서 고유값을 줌. 그래서 다음 페이지 요청시에 그 고유값을 계속 갖고있는지 검사함. 폼태그에 추가해줘야한다. {% csrf token %}

post_add() 에서 받은 데이터로 디비에 포스트 추가하기 

Post.objects.create()하면 save()안해두된당

리다이렉트로 유알엘 패턴의 네임을 받아 다시 포스트 리스트로 돌아가도록한다. 리턴 리다이렉트는 반환된 요청을 받았을때 브라우저가 해당 주소로 이동하도록한다. 주소를 동적으로 받기 위해 주소대신 유알엘 패턴이름을 적어준다. 

리버스-역참조는 데이터를 가지고 표현식에 매칭되는 유알엘 문자열을 만들어준다. 리다이렉트랑은 다른것

유투브 검색 필터링

배포 - 도커
가상서버? - aws
