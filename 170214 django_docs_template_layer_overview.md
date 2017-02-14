# Templates
the basics:overview

## Support for template engines

### Configuration

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            # ... some options here ...
        },
    },
]
```
BACKEND is a dotted Python path to a template engine class implementing Django’s template backend API. The built-in backends are django.template.backends.django.DjangoTemplates and django.template.backends.jinja2.Jinja2.


DIRS - defines a list of directories where the engine should look for template source files, in search order.

## The Django template language

### Syntax

#### Variables

```
My first name is {{ first_name }}. My last name is {{ last_name }}.
```
With a context of {'first_name': 'John', 'last_name': 'Doe'}, this template renders to:

```
{{ my_dict.key }}
{{ my_object.attribute }}
{{ my_list.0 }}
```
.으로 접근한다.

#### Tags

A [reference of built-in tags](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#ref-templates-builtins-tags) is available as well as [instructions for writing custom tags](https://docs.djangoproject.com/en/1.10/howto/custom-template-tags/#howto-writing-custom-template-tags).

#### Filters

#### Comments
장고 템플릿 에서 템플릿언어로 변환되지 않았으면 좋겠는 부분은 이렇게 감싸줘야한다. 

```
{# this won't be rendered #}
```

### Components
#### Context processors


# The Django template language
for designers:language overview

## Templates

## Variables

## Filters

변수가 어떻게 보여질지 변경하는 것. 

linebreaks 개행문자를 모두 반영해줌 (br,p태그등으로 바꿔서)

## Template inheritance
base.html이 있으면 각 섹션별로 서브 템플릿을 만들어라.
{{ block.super }} 상위요소 가져올 수 있다.

## Automatic HTML escaping
신뢰할 수 있는 html태그는 escape 필터로 장고가 렌더링 할 수 있도록 해준다. 

* < is converted to &lt;
* > is converted to &gt;
* ' (single quote) is converted to &#39;
* " (double quote) is converted to &quot;
* & is converted to &amp;
### How to turn it off
```
This will be escaped: {{ data }}
This will not be escaped: {{ data|safe }}
```

```
This will be escaped: &lt;b&gt;
This will not be escaped: <b>
```


## Accessing method calls


# Humanization
