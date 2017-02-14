# 웹 크롤러 만들기

## 환경설정

crawling.py 생성

[뷰티풀숩 설치하기](http://cryptosan.github.io/pythondocuments/documents/beautifulsoup4/#installing-beautiful-soup)


(pyenv깔린 폴더에서 작업해야함)

pip install beautifulsoup4

html parser : html문서를 긁어오기편하게해주는것

우리는 파서를 활용해서 크롤링한다

pip install requests

커맨드+1하면 파이참 목록창 열고 닫기



크롤링할 url정하기

preference-project-project interpreter 

show all해서
+눌러서 add local 하면 파인더 뜨는데 '/usr/local/var/pyenv/versions/fc-python/bin/python'

pychar에러 때문에 2016 2.3버전으로 다운그레이드

preference-build,,-console-python console 에서 디폴트 인터프리터는 3.4.3 해주고

프로젝트 설정

preference-project-project interpreter  
에서 가상환경으로 바꿔준다.
(우리는 리퀘스트랑 뷰티풀숲을 가상환경에만 설치했기 때문에)

### passing parameters in urls

[passing~ ](http://docs.python-requests.org/en/master/user/quickstart/#passing-parameters-in-urls)
에서 설명이 나와있다

request.get은 키+밸류 형태로 보낸다

### response content
리스폰스를 읽어준다.
r.text

### 뷰티풀수프 빨리기시작하기
print(type(html_doc))
해보면 해석기 설치하는게 좋을것같음

뷰티플숲 객체를 생셩 인자는 html text
soup = BeautifulSoup(html_doc)

pip install lxml
xml해석기 설치 
[해석기설치](http://cryptosan.github.io/pythondocuments/documents/beautifulsoup4/#installing-a-parser)

제목꺼내기 성공


테이블 로우 하나마다 통째로 꺼내오도록 수정하기

--

모듈화하기 !!!
딕셔너리 형태로 출력되도록 하기!!!

+ 임포트 문제 해결을 위해 크롤링 폴더 자체를 mark directory as source root 로 해준다!

. 아무문자
* 앞에 패턴이 0~무한대까지 반복된다
no=를 만나기 직전까지를 *이 차지

[?&] ?나 &중 하나 

(\d+) 숫자가 한개이상

.* &이후의 아무문자들 전부


from 모쥴명 import 메소드

------
## 모듈과 패키지
package/
game.py
lol.py
shop.py

game.play_game()
game. 으로 game클로저에 접근한다.

import한거 불러올때 코드에 있는 함수호출이 다시 되는 문제

brew install tree


## 패키지

__init__.py 는 패키지 초기화 파일

패키지에 임포트 코드 써두면

다른데서 패키지 않에있는 모듈의 메소드도

패키지명.메소드명 이런식으로 마치 패키지 안에 있는 것처럼 꺼내서 쓸 수 있다!!!!!


----
이제 이내용대로 리팩토링을 해본다.

parser 패키지안에 page.py와 communication.py로 역할에 따라 나눔. 
__init__.py 에 임포트해줌

이제 메인에서 parser.으로 메소드 꺼내 써지는지 테스트


shift+command+ - : 함수닫아준다
상수로쓰는 변수는 대문자로 (WEBTOON_ID)

 * 딕셔너리의 키의 순서는 랜덤!!
 * 여기서도 반환된 리스트 딕셔너리 결과를 보면 순서가 랜덤하다
 
 
 