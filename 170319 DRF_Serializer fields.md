# [Serializer fields](http://www.django-rest-framework.org/api-guide/fields/#serializer-fields)

> "Form 클래스의 각 필드는 데이터 유효성 검사뿐 아니라 일관된 형식으로 정규화하여 "정리"하는 역할을 담당합니다."

Serializer 필드는 원시값과 내부데이터 타입간의 변환을 처리한다. 또한 input값의 유효성 검사와 부모 객체에서 값 검색 및 값 설정을 한다.

> serializer 필드는 fields.py에 선언되어 있지만, `from rest_framework import serializers`에 따라 가져와서 `serializers.<FieldName>`로 참조해야 한다.



## [Core arguments](http://www.django-rest-framework.org/api-guide/fields/#core-arguments)

각 serializer 필드 클래스 생성자는 최소한 다음의 인수를 취합니다. 일부 Field 클래스는 필드 별 인수를 추가로 가져 오지만 다음을 항상 가져야합니다.



#### [`read_only`](http://www.django-rest-framework.org/api-guide/fields/#read_only)

읽기 전용 필드는 API output에 포함되지만 작성 또는 갱신 조작 중 input에 포함되면 안됩니다. serializer input에 잘못 포함 된 'read_only'필드는 무시됩니다. 표현식을 직렬화 할 때 이 필드를 사용하지만 반직렬화 과정에서 인스턴스를 생성하거나 업데이트 할 때 필드를 사용하지 않으려면 이 값을 True로 설정하십시오. 기본값은 False입니다.



#### [`write_only`](http://www.django-rest-framework.org/api-guide/fields/#write_only)

True로 설정하면 `read_only`와 반대로 반직렬화 할때 인스턴스를 생성하거나 업데이트 하면서 이 필드가 사용될 수 있지만, 표현식을 직렬화 할 때는 이 필드가 포함되지 않는다. 기본값은 False.



#### [`required`](http://www.django-rest-framework.org/api-guide/fields/#required)

반직렬화 과정에서 필드가 제공되지 않으면 오류가 발생한다. 반직렬화 중에 이 필드가 필요하지 않으면 False로 설정해야 한다. False일 경우 인스턴스를 직렬화 할 때 객체 속성이나 딕셔너리 키가 생략될 수 있다. 키가 없으면 출력 표현에 포함되지 않는다. (직렬화 결과로 생긴 JSON등에 키와 밸류가 포함되지 않는다.) 기본값은 True.



#### [`allow_null`](http://www.django-rest-framework.org/api-guide/fields/#allow_null)

일반적으로 serializer 필드에 None이 전달되면 에러가 발생한다. None도 유효한 값으로 간주해야 하는 경우 이 인수를 True로 설정한다. 기본값은 False.



#### [`default`](http://www.django-rest-framework.org/api-guide/fields/#default)

이 인자의 값이 설정되면 필드에 입력값이 제공되지 않을 때 사용될 기본값이 제공된다. 

부분 업데이트의 조작중에는 기본값이 적용되지 않는다. 부분 업데이트의 경우 데이터가 입력된(들어온) 필드에만 유효성 검사를 거친 값이 반환된다.

May be set to a function or other callable, in which case the value will be evaluated each time it is used. When called, it will receive no arguments. If the callable has a `set_context` method, that will be called each time before getting the value with the field instance as only argument. 유효성 검사기와 동일한 방식으로 작동한다.

인스턴스를 직렬화 할 때 오브젝트 속성 또는 사전 키가 인스턴스에없는 경우 기본값이 사용됩니다. `default`를 설정하면 `required=False`이다. `default` 와 `required`  인수를 모두 포함하면 올바르지 않으며 오류가 발생합니다.



#### [`source`](http://www.django-rest-framework.org/api-guide/fields/#source)

필드를 채우는 속성의 이름이다. `URLField(source='get_absolute_url')`같은 `self`인수만 갖는 메소드나 `EmailField(source='user.email')`처럼 `.`으로 속성에 접근하는 점표기법을 사용할 수 있다. 

 `source = '*'`는 전체 객체가 필드로 전달되어야 함을 나타낸다. 중첩된 표현을 작성하거나 출력 표현을 결정하기 위해 complete 객체에 접근해야하는 필드에 유용합니다.

기본값은 필드의 이름이다.



#### [`validators`](http://www.django-rest-framework.org/api-guide/fields/#validators)

A list of validator functions which should be applied to the incoming field input, and which either raise a validation error or simply return. Validator functions should typically raise `serializers.ValidationError`, but Django's built-in `ValidationError` is also supported for compatibility with validators defined in the Django codebase or third party Django packages.

#### [`error_messages`](http://www.django-rest-framework.org/api-guide/fields/#error_messages)

A dictionary of error codes to error messages.

#### [`label`](http://www.django-rest-framework.org/api-guide/fields/#label)

A short text string that may be used as the name of the field in HTML form fields or other descriptive elements.

#### [`help_text`](http://www.django-rest-framework.org/api-guide/fields/#help_text)

A text string that may be used as a description of the field in HTML form fields or other descriptive elements.

#### [`initial`](http://www.django-rest-framework.org/api-guide/fields/#initial)

A value that should be used for pre-populating the value of HTML form fields. You may pass a callable to it, just as you may do with any regular Django `Field`:

```
import datetime
from rest_framework import serializers
class ExampleSerializer(serializers.Serializer):
    day = serializers.DateField(initial=datetime.date.today)
```

#### [`style`](http://www.django-rest-framework.org/api-guide/fields/#style)

A dictionary of key-value pairs that can be used to control how renderers should render the field.

Two examples here are `'input_type'` and `'base_template'`:

```
# Use <input type="password"> for the input.
password = serializers.CharField(
    style={'input_type': 'password'}
)

# Use a radio input instead of a select input.
color_channel = serializers.ChoiceField(
    choices=['red', 'green', 'blue'],
    style={'base_template': 'radio.html'}
)
```

For more details see the [HTML & Forms](http://www.django-rest-framework.org/topics/html-and-forms/) documentation.