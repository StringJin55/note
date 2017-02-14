Day 19?


장고걸스 포스트 애드 부분 수정~~

16:00

## 장고 튜토리얼 다시 시작

### 인덱스 만들기 
"""
루트 url (r'^$')에
polls/와 admin/으로 갈 수 있는 링크 페이지를 구현한다

1. 뷰는 mysite/views.py안에 def index(request)에 작성

2. 템플릿은 templates/index.html을 사용
polls/, admin/ 연결하는 부분은 {% url %} 태그 쓰지않고 직접 구현

3. url은 mysite/urls.py에
"""

include()는 name을 따로 줄 수 없다. 

--
### polls.index.html 하드코딩된 거 바꿔주기

### url 네임 스페이스 정해주기
polls/urls.py
polls/index.html


### 폴스 디테일뷰 바꾸기 
polls/views.py
polls/detail.html

"""
    request.method == 'POST' 일때
    전달받은 데이터 출력

    POST형식이 아닐 경우에는 polls/detail.html을 render
"""
<input> 에서 name으로 된 키에 value 값이 묶인 딕셔너리 형태로 POST된다. request.POST['키'] 로 view에서 받아서 씀!

"""
POST요청이 왔을 때 전달받은 객체에서 'choice'키의 값을 HttpResponse로 돌려준다
"""

request.POST['choice']

"""
'choice'키로 전달된 Choice객체의 id를 이용해서 해당 객체의 votes값을 1 늘려주고
    디비에 업데이트 한 후 다시 question detail페이지로 이동

1.request.POST['choice']의 값(초이스객체의 아이디)을 이용해서 초이스 객체를 가져온다
2. 해당 객체의 투표값을 늘려주고, 디비에 변경사항을 업데이트
3. 리다이렉트, question_id를 이용해서 디테일 페이지로 다시 돌아간다.
"""

### result 페이지 구현 

polls/views.py 
def results
인자로 주어진 퀘스쳔아이디에 해당하는 퀘스쳔 객체를 컨텍스트에 담아 렌더에 보낸다

polls/results.html
<!--아래 루프를 돌면서 해당 초이스의 c_text: 해당 초이스객체의 votes값-->
<!--을 출력하도록 작성-->


