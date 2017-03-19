# Serializers

serializer는 쿼리셋이나 모델 인스턴스같은 복잡한 데이터를 JSON이나 XML로 렌더링 하기 쉬운 native Python 데이터타입으로 변환시켜 준다. 또한 반직렬화 기능도 제공해서, 파싱된 데이터를 유효성 검증 후에 다시 복잡한 데이터 형식(쿼리셋이나 모델 인스턴스)로 변환해 준다. 

DRF의 serializer는 Django의 Form 클래스나 ModelForm클래스와 비슷하게 작동한다. Serializer 클래스뿐만 아니라 모델 인스턴스와 쿼리셋을 다루기 좋은 ModelSerializer 클래스도 제공한다. 



## Serializers 선언

form을 정의하는 것과 매우 유사하다.



## 객체를 직렬화 하기 

```python
serializer = CommentSerializer(comment)
serializer.data
# {'email': 'leila@example.com', 'content': 'foo bar', 'created': '2016-01-27T15:17:10.375877'}
```

직렬화 한 결과 native python 데이터 타입이 되었다. 직렬화를 마무리 하기 위해 JSON타입으로 변환시켜 준다. 

```python
from rest_framework.renderers import JSONRenderer

json = JSONRenderer().render(serializer.data)
json
# b'{"email":"leila@example.com","content":"foo bar","created":"2016-01-27T15:17:10.375877"}'
```

JSONRenderer를 이용해서 JSON으로 렌더링 해주면 직렬화가 마무리된다.

> `b'...'` 나`B'...'`는 바이트 형식의 데이터 표현이다. 



## 객체를 반직렬화 하기

직렬화 과정과 비슷하다. JSON타입 등의 데이터를 먼저 Python native 데이터타입으로 스트림(끊기지 않은 연속적인 데이터)을 파싱한다. 

```python
from django.utils.six import BytesIO
from rest_framework.parsers import JSONParser

stream = BytesIO(json)
data = JSONParser().parse(stream)
```

`django.utils.six` 내부의 `BytesIO`는 메모리 내 이진 스트림([file object](https://docs.python.org/3/glossary.html#term-file-object)와 같다. )을 BytesIO객체로 사용할 수 있게 해준다. (python3:[io.BytesIO](https://docs.python.org/3/library/io.html#binary-i-o)) BytesIO객체는 파일과 비슷한 객체, 읽고 쓸 수 있게 만들어준다.?!?!? 그리고 나서 파싱한다. 

```python
serializer = CommentSerializer(data=data)
serializer.is_valid()
# True
serializer.validated_data
# {'content': 'foo bar', 'email': 'leila@example.com', 'created': datetime.datetime(2012, 08, 22, 16, 20, 09, 822243)}
```

Serializer로 반직렬화!



class인스턴스 = 사용자정의 자료형

딕셔너리 등등 = 기본자료형, primitive data types