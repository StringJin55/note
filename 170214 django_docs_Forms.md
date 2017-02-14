# Working with forms
Forms:overview

## HTML forms
폼에는 데이터가 반환되는 url과 어떤 httprequest method를 사용해서 보내는지가 있어야한다.

post로 보낼때도 get parameter 받을 수 있다.

?next=

### GET and POST
시스템 상태를 변경하는 데 사용하는 요청만 POST를 사용. 조회같은 시스템에 영향을 주지않는 요청은 GET.

POST, coupled with other protections like Django’s CSRF protection offers more control over access.

## Django’s role in forms


## Forms in Django
html<form>을 의미하기도 하고 장고의 form클래스로 만들어진것을 의미하기도 하고 
end-to-end working(백엔드와 프론트엔드 오가는 작업) 

### The Django Form class

ModelForm

A form’s fields are themselves classes

위젯 - 텍스트 인풋, 라디오버튼 등 

### Instantiating, processing, and rendering forms

### Building a form in Django
#### The Form class
폼 클래스의 변수명과 input의 name= 이 매치된다

#### The view

```python
from django.shortcuts import render
from django.http import HttpResponseRedirect

from .forms import NameForm

def get_name(request):
    # if this is a POST request we need to process the form data
    if request.method == 'POST':
        # create a form instance and populate it with data from the request:
        # post요청으로 온 데이터를 폼에 채운다
        form = NameForm(request.POST)
        # check whether it's valid:
        # 포스트 요청을 채운 폼에서 데이터가 제대로 들어갔는지 유효성 검사
        if form.is_valid():
            # process the data in form.cleaned_data as required
            # ...
            # redirect to a new URL:
            return HttpResponseRedirect('/thanks/')

    # if a GET (or any other method) we'll create a blank form
    else:
        form = NameForm()
	# 잘못된 데이터를 가진 폼 객체를 보내줘서 템플릿안에서 에러가 뜨게함
    return render(request, 'name.html', {'form': form})
```

#### The template

## More about Django Form classes

### More on fields

#### Field data
he validated form data will be in the form.cleaned_data dictionary.


### Working with form templates
#### Form rendering options

Note that you’ll have to provide the surrounding <table> or <ul> elements yourself.

### Further topics
Formsets 제외

#### ModelForm 
