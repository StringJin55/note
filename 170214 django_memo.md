170213

과제하다 에러남

url()¶

url(regex, view, kwargs=None, name=None)[source]¶
urlpatterns should be a list of url() instances. For example:

from django.conf.urls import include, url

urlpatterns = [
    url(r'^index/$', index_view, name='main-view'),
    url(r'^weblog/', include('blog.urls')),
    ...
]
The regex parameter should be a string or ugettext_lazy() (see Translating URL patterns) that contains a regular expression compatible with Python’s re module. Strings typically use raw string syntax (r'') so that they can contain sequences like \d without the need to escape the backslash with another backslash.

The view parameter is a view function or the result of as_view() for class-based views. It can also be an include().

The kwargs parameter allows you to pass additional arguments to the view function or method. See Passing extra options to view functions for an example.

See Naming URL patterns for why the name parameter is useful.



url()에서 name= 이 가르키는 것과 view.py의 메소드는 상관없다고 생각했는데?

>Naming URL patterns¶
>
>In order to perform URL reversing, you’ll need to use named URL patterns as done in the examples above. The string used for the URL name can contain any characters you like. You are not restricted to valid Python names.

>When you name your URL patterns, make sure you use names that are unlikely to clash with any other application’s choice of names. If you call your URL pattern comment, and another application does the same thing, there’s no guarantee which URL will be inserted into your template when you use this name.

>Putting a prefix on your URL names, perhaps derived from the application name, will decrease the chances of collision. We recommend something like myapp-comment instead of comment.
>

urls.py에서 url() 안의 name=이름 은 템플릿의 {% url 'appname:이름' %} 과 같다. (urls.py에 app_name=을 지정해주었을 때) 실제 url과 매칭되는 view는 url()의 두번재 파라미터로. 
