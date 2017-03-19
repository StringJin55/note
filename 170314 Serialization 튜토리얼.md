# Serialization 튜토리얼

[pastebin.com](http://pastebin.com/) 같은 사이트 만들기



LEXERS : 어떤 언어에서 어떤 확장자를 사용할 것인지 나와있음



```python
Snippet.objects.create(**validated_data)
```

딕셔너리를 넣어서 create할 수 있다.



```python
class BaseSerializer(Field): 
    def update(self, instance, validated_data):
        raise NotImplementedError('`update()` must be implemented.')

    def create(self, validated_data):
        raise NotImplementedError('`create()` must be implemented.')
```



serializer 내부를 보면 update와 create를 반드시 만들어주도록 되어있다. 없으면 에러메세지를 호출하도록 만들어져 있음. 



django RESTframework 는 장고 모델이 아니라도 다룰 수 있지만 그렇게는 잘 안쓴다. [instances를 저장하는 방법](http://www.django-rest-framework.org/api-guide/serializers/#saving-instances) 



```python
serializer = SnippetSerializer(Snippet.objects.all(), many=True)  
serializer.data
```

여기에서 스니펫 객체를 여러개 넣을때 쿼리셋으로 넣어주는데, many=True옵션 반드시 추가해야한다.



class JSONResponse (HttpResponse):

HttpResponse를 상속받아 안에서 content내용만 바꿔서 반환한다. 



def snippet_list(): 까지 만들고 테스트 해봄. (스크린샷)



### [class-based  views](https://docs.djangoproject.com/en/1.10/topics/class-based-views/#class-based-views)

[`as_view()`](https://docs.djangoproject.com/en/1.10/ref/class-based-views/base/#django.views.generic.base.View.as_view) 

클래스기반 뷰를 통해 객체를 만들어주는 메소드

[HEAD요청](https://docs.djangoproject.com/en/1.10/topics/class-based-views/#supporting-other-http-methods)

믹스인

함수 기반의 제너릭 뷰 - 이제 쓰지 않는다.

함수 기반의 제너릭 뷰로 해결 불가능한 문제를 해결하기 위해 만든 클래스 기반 제너릭 뷰



함수기반 뷰의 예:

```python
from django.http import HttpResponse

def my_view(request):
    if request.method == 'GET':
        # <view logic>
        return HttpResponse('result')
```

클래스 기반 뷰의 예:

```python
from django.http import HttpResponse
from django.views import View

class MyView(View):
    def get(self, request):
        # <view logic>
        return HttpResponse('result')
```



```python
# urls.py
from django.conf.urls import url
from myapp.views import MyView

urlpatterns = [
    url(r'^about/$', MyView.as_view()),
]
```

as_view()로 클래스 안에서 HTTP요청에 알맞은 함수가 반환된다. 



[custom parser](http://www.django-rest-framework.org/api-guide/parsers/#jsonparser)



git commit —amend : 이전 커밋을 덮어씌움 (push 하기 전에 해야함!!)

'auth.User' 보다 settings.AUTH_USER_MODEL 쓰는게 편함. 나중에 변경되어도 



[pygments](http://pygments.org/)



터미널 개발자 

```python
 linenos = self.linenos and 'table' or False
```

```python
linenos = 'table' if self.linenos else False
```




```python
class CreateModelMixin(object):
    """
    Create a model instance.
    """
    def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        self.perform_create(serializer)
        headers = self.get_success_headers(serializer.data)
        return Response(serializer.data, status=status.HTTP_201_CREATED, headers=headers)

    def perform_create(self, serializer):
        serializer.save()
```

```
generics.ListCreateAPIView > CreateModelMixin
```

안에 들어가보면 이렇게 생성함수있다. 이걸 오버라이드 해서 커스텀 할 수 있음.





BasePermission을 보면 permission policies에 `SAFE_METHODS = ('GET', 'HEAD', 'OPTIONS')`로 정의되어 있음. 



5부 API의 최상단에 대한 엔드 포인트 만들기

예제에 나온 함수 기반 뷰를

```python
from rest_framework.decorators import api_view  
from rest_framework.response import Response  
from rest_framework.reverse import reverse


@api_view(('GET',))
def api_root(request, format=None):  
    return Response({
        'users': reverse('user-list', request=request, format=format),
        'snippets': reverse('snippet-list', request=request, format=format)
    })
```

를 클래스 기반 뷰로 만들어보기

django_app/config/views.py

```python
from rest_framework.response import Response
from rest_framework.reverse import reverse
from rest_framework.views import APIView


class ApiRoot(APIView):
    def get(self, request, format=None):
        return Response({
            'users': reverse('member:user_list', request=request, format=format),
            'snippets': reverse('snippet:snippet_list', request=request, format=format)
        })

```





[pagination](http://www.django-rest-framework.org/api-guide/pagination/)

settings.py

```python
REST_FRAMEWORK = {  
    'PAGE_SIZE': 10
}
```



cbv.py에 추가

```python
class SnippetList(generics.ListCreateAPIView):
    pagination_class = pagination.PageNumberPagination
```





[Router](http://www.django-rest-framework.org/api-guide/routers/#simplerouter)

router사용시 namespace 자동으로 만들어준다. 

라우터는 뷰셋들을 모아서 나눠준다. 