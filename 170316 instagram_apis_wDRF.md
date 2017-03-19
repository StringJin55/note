# instagram_api_DRF



## test case 작성 



## DRF setting

우선 DRF를 사용하기 위해 pip설치하고 Installed_apps에 추가. 그리고 DRF에 맞도록 세팅해주어야 한다. 

먼저 apis, views, urls 패키지 생성



`apis/__init__.py`

요기에 뷰를 하나 만들어줌

```python
from rest_framework import generics

from post.models import Post


class PostList(generics.ListCreateAPIView):
    queryset = Post.objects.all()
    serializer_class = None
   
```



urls/views.py

기존의 urls.py에 있던 내용 즉 일반 뷰를 담당하는 urls내용들을 여기서 관리한다.

```python
from django.conf.urls import url

from post import views


urlpatterns = [
    url(r'^create/$', views.post_create, name='post-create'),
    url(r'^photo/add/$', views.post_photo_add, name='photo-add'),
    url(r'^$', views.post_list, name='post-list'),

]

```



urls/apis.py

여기에서는 이제 api들의 url을 관리한다.

```python
from django.conf.urls import url

from . import apis as views

urlpatterns = [
    url(r'^$', views.PostList.as_view(), name='post-list'),
]
```



config/urls.py

여기에도 api를 위한 url 패턴 추가

```python
from django.conf.urls import url, include
from django.contrib import admin

from post.urls import apis as post_apis_urls
from post.urls import views as post_urls

api_urlpatterns = [
    url(r'^post/', include(post_apis_urls)),
]
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^post/', include(post_urls)),
    url(r'^api/', include(api_urlpatterns, namespace='api')),
]
```

