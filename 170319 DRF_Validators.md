# [Validators](http://www.django-rest-framework.org/api-guide/validators/#validators)

대부분 REST 프레임 워크에서 유효성 검사를 처리하는 경우 기본 필드 유효성 검사를 사용하거나  serializer또는 필드 클래스에 대한 유효성 검사 메소드를 작성하면 됩니다.



## [Validation in REST framework](http://www.django-rest-framework.org/api-guide/validators/#validation-in-rest-framework)

DRF serializer의 유효성 검사는 Django ModelFrom클래스에서의 유효성 검사와 조금 다르게 작동한다. 

ModelForm을 사용하면 유효성 검사가 부분적으로는 폼 양식에서, 부분적으로는 모델 인스턴스에서 수행된다. DRF에서 유효성 검사는 전부 serializer 클래스에서 수행된다. 이것은 다음과 같은 이유로 유리하다. 

* 적절한 구분을 제공하여 코드 동작을보다 명확하게 만듭니다. 
* ModelSerializer 클래스를 사용하거나 Serializer 클래스를 사용하는 것은 쉽게 전환 할 수 있습니다. ModelSerializer에 사용되는 모든 유효성 검증 동작은 복제가 간단합니다. serializer 인스턴스의 repr을 인쇄하면 적용되는 유효성 검사 규칙이 정확하게 표시됩니다. 
* 모델 인스턴스에서 추가 숨겨진 유효성 검사 동작이 호출되지 않습니다.

ModelSerializer를 사용하면이 모든 것이 자동으로 처리됩니다. 대신 Serializer 클래스를 사용하려면 유효성 검사 규칙을 명시적으로 정의해야합니다.

### 예제

유니크 제약 필드가 있는 모델을 만들고,

```python
class CustomerReportRecord(models.Model):
    time_raised = models.DateTimeField(default=timezone.now, editable=False)
    reference = models.CharField(unique=True, max_length=20)
    description = models.TextField()
```

CustomerReportRecord 인스턴스의 생성과 업데이트에 사용할 간단한 ModelSerializer 정의 했다. 

```python
class CustomerReportSerializer(serializers.ModelSerializer):
    class Meta:
        model = CustomerReportRecord
```

python shell에서 다음과 같이 해본다. 

```python
>>> from project.example.serializers import CustomerReportSerializer
>>> serializer = CustomerReportSerializer()
>>> print(repr(serializer))
CustomerReportSerializer():
    id = IntegerField(label='ID', read_only=True)
    time_raised = DateTimeField(read_only=True)
    reference = CharField(max_length=20, validators=[<UniqueValidator(queryset=CustomerReportRecord.objects.all())>])
    description = CharField(style={'type': 'textarea'})
```

reference 필드를 보면 고유성 제약이 serializer 필드의 유효성 검사기에 의해 명시적으로 적용되고 있다. 

이런점들 때문에 DRF는 Django에서 사용할 수 없는 몇가지 유효성 검사기 클래스를 포함한다. 



## [UniqueValidator](http://www.django-rest-framework.org/api-guide/validators/#uniquevalidator)

이 유효성 검사기를 통해서 모델 필드에 unique=True 제약 조건을 적용할 수 있다. 인수는 다음과 같다.

* queryset ***required*** - unique 제약 조건을 걸어야 하는 쿼리셋
* message - 검증에 실패했을 경우에 사용하는 에러 메세지 
* lookup - 검증되고있는 값을 가지는 기존의 인스턴스를 찾는 데 사용되는 조회. 기본값은 'exact'입니다.

```python
from rest_framework.validators import UniqueValidator

slug = SlugField(
    max_length=100,
    validators=[UniqueValidator(queryset=BlogPost.objects.all())]
)
```

이렇게 유니크 제약을 걸 필드에 넣어준다. 



## [UniqueTogetherValidator](http://www.django-rest-framework.org/api-guide/validators/#uniquetogethervalidator)

두개의 필수 인수가 있다. 

* queryset ***required*** - 유일성을 강요해야하는 queryset입니다. 
* fields ***required***- 고유한 세트를 만들어야하는 필드 이름의 목록 또는 튜플. 이들은 serializer 클래스의 필드로 존재해야합니다. 
* message - 검증에 실패했을 경우에 사용하는 에러 메세지

```python
from rest_framework.validators import UniqueTogetherValidator

class ExampleSerializer(serializers.Serializer):
    # ...
    class Meta:
        # ToDo items belong to a parent list, and have an ordering defined
        # by the 'position' field. No two items in a given list may share
        # the same position.
        validators = [
            UniqueTogetherValidator(
                queryset=ToDoItem.objects.all(),
                fields=('list', 'position')
            )
        ]
```

> UniqueTogetherValidation 클래스는 적용되는 모든 필드를 required=True로 취급한다. default 값이 있는 경우는 예외.



## [UniqueForDateValidator](http://www.django-rest-framework.org/api-guide/validators/#uniquefordatevalidator)

## [UniqueForMonthValidator](http://www.django-rest-framework.org/api-guide/validators/#uniqueformonthvalidator)

## [UniqueForYearValidator](http://www.django-rest-framework.org/api-guide/validators/#uniqueforyearvalidator)

이 유효성 검사기는 model 인스턴스에 대해 unique_for_date, unique_for_month 및 unique_for_year 제약 조건을 적용하는 데 사용할 수 있습니다.

* queryset ***required*** - 유일성을 강요해야하는 queryset입니다. 
* field ***required*** - 지정된 날짜 범위의 고유성에 대한 유효성을 검사하는 필드 이름입니다. 이것은 serializer 클래스의 필드로 존재해야합니다. 
* date_field 필수 - 고유성 제한 조건의 날짜 범위를 결정하는 데 사용할 필드 이름입니다. 이것은 serializer 클래스의 필드로 존재해야합니다. 
* message - 검증에 실패했을 경우에 사용하는 에러 메세지

유효성 검사에 사용되는 날짜 필드는 항상 serializer 클래스에 있어야합니다. 유효성 검사가 실행될 때까지 기본값에 사용되는 값이 생성되지 않기 때문에 모델 클래스 default = ...에 의존 할 수 없습니다.

ModelSerializer를 사용해 DRF가 제공하는 기본값을 사용하지 않고, Serilaizer 클래스나 다른 방식으로 사용하려는 경우 아래의 스타일을 참고한다. 

#### [Using with a writable date field.](http://www.django-rest-framework.org/api-guide/validators/#using-with-a-writable-date-field)

```django
published = serializers.DateTimeField(required=True)
```

#### [Using with a read-only date field.](http://www.django-rest-framework.org/api-guide/validators/#using-with-a-read-only-date-field)

기본값은 validated_data로 전달된다.

```
published = serializers.DateTimeField(read_only=True, default=timezone.now)
```

#### [Using with a hidden date field.](http://www.django-rest-framework.org/api-guide/validators/#using-with-a-hidden-date-field)

기본값은 serializer에서 validated_data로 반환한다.

```python
published = serializers.HiddenField(default=timezone.now)
```

> UniqueFor<Range>Validation 클래스가 적용되는 필드에는 항상 암시적으로 required=True 제약조건이 있다. default 값이 있는 필드는 예외.



## [Advanced field defaults](http://www.django-rest-framework.org/api-guide/validators/#advanced-field-defaults)

시리얼 라이저의 여러 필드에 적용되는 유효성 검사기는 API 클라이언트가 제공해서는 안되지만 유효성 검사기의 입력으로 사용할 수있는 필드 입력이 필요할 수 있습니다.

이런 유형의 유효성 검사에 사용할 수 있는 두가지 패턴:

* HiddenField 사용하기. 이 필드는 validated_data에 있지만 serializer 출력 표현에서는 사용되지 않습니다. 
* 일반 필드에 read_only = True사용하기, 여기는 default = ... 인수도 포함합니다. 이 필드는 serializer 출력 표현에 사용되지만 사용자가 직접 설정할 수는 없습니다.

REST 프레임 워크는이 컨텍스트에서 유용 할 수있는 몇 가지 기본값(다음을 참고)을 포함합니다. 

#### [CurrentUserDefault](http://www.django-rest-framework.org/api-guide/validators/#currentuserdefault)

현재 사용자를 나타내는 데 사용할 수있는 기본 클래스입니다. 이것을 사용하기 위해서, serializer를 인스턴스화 할 때 'request'가 context 딕셔너리로 제공되어야합니다.

```python
owner = serializers.HiddenField(
    default=serializers.CurrentUserDefault()
)
```

#### [CreateOnlyDefault](http://www.django-rest-framework.org/api-guide/validators/#createonlydefault)

생성 중의 기본인수를 설정하는데 사용되는 기본 클래스이다. 업데이트 중 필드는 생략된다. 

하나의 인수를 갖는데, 생성 동작에 필요한 기본값이나 호출가능한 값을 갖는다.

```python
created_at = serializers.DateTimeField(
    read_only=True,
    default=serializers.CreateOnlyDefault(timezone.now)
)
```



## [Limitations of validators](http://www.django-rest-framework.org/api-guide/validators/#limitations-of-validators)

ModelSerializer가 생성하는 기본 serializer 클래스를 사용하는 대신 유효성 검사를 명시 적으로(직접) 처리해야하는 경우가 있습니다.



#### [Optional fields](http://www.django-rest-framework.org/api-guide/validators/#optional-fields)

unique together 제약이 있을때 serializer 유효성 검사기는 필드에 required=True 조건을 강제한다. 사용자가 명시적으로 required=False 를 필드에 적용하면, 유효성 검사가 잘 안될 수 있다.

이런 경우 serializer 유효성 검사 대신에 .validate() 메소드를 사용하거나 뷰에서 따로 유효성 검사 로직을 작성해야 한다. 

```python
class BillingRecordSerializer(serializers.ModelSerializer):
    def validate(self, data):
        # Apply custom validation either here, or in the view.

    class Meta:
        fields = ('client', 'date', 'amount')
        extra_kwargs = {'client': {'required': 'False'}}
        validators = []  # Remove a default "unique together" constraint.
```



#### [Updating nested serializers](http://www.django-rest-framework.org/api-guide/validators/#updating-nested-serializers)

기존 인스턴스에 업데이트를 적용할 때 unique 검사기는 현재 인스턴스를 검사에서 제외한다. 현재 인스턴스는 serializer의 속성(attr)으로 존재하기 때문에 serializer를 인스턴스화 할 때 instance = ...를 사용하여 처음에 전달되어서, unique 검사의 컨텍스트에서 사용할 수 있다. 

중첩된 serializer의 업데이트의 경우 인스턴스가 not available이기 때문에 이렇게 현재 인스턴스를 검사에서 제외할 수 없다.

이것도 .validate()메서드나 뷰에 따로 유효성 검사 로직을 작성해야 한다.



#### [Debugging complex cases](http://www.django-rest-framework.org/api-guide/validators/#debugging-complex-cases)

ModelSerializer 클래스가 어떤 동작을하는지 확실히 모를 경우 manage.py 셸을 실행하고 serializer의 인스턴스를 인쇄하면 자동으로 생성되는 필드와 유효성 검사기를 검사 할 수 있습니다.

```python
>>> serializer = MyComplexModelSerializer()
>>> print(serializer)
class MyComplexModelSerializer:
    my_fields = ...
```

복잡한 케이스의 경우에는 ModelSerializer대신 serializer 클래스를 따로 정의하는 것이 낫다.



## [Writing custom validators](http://www.django-rest-framework.org/api-guide/validators/#writing-custom-validators)

커스텀 유효성 검사기 작성하기

### [Function based](http://www.django-rest-framework.org/api-guide/validators/#function-based)

유효성 검사기는 실패할 경우 serializers.ValidationError를 발생하는 호출가능한 함수.

```python
def even_number(value):
    if value % 2 != 0:
        raise serializers.ValidationError('This field must be an even number.')
```

#### [Field-level validation](http://www.django-rest-framework.org/api-guide/validators/#field-level-validation)

필드 레벨 유효성 검증은 당신의 Serializer 하위 클래스에 .validate_<fielde_name> 메소드를 작성해서 할 수 있다. [Serializer 문서](http://www.django-rest-framework.org/api-guide/serializers/#field-level-validation)를 참고.



### [Class-based](http://www.django-rest-framework.org/api-guide/validators/#class-based)

클래스 기반 유효성 검사기를 작성하려면 __call__ 메소드를 사용하십시오. 클래스 기반 유효성 검사기는 동작을 매개 변수화하고 다시 사용할 수 있으므로 유용합니다.

```python
class MultipleOf(object):
    def __init__(self, base):
        self.base = base

    def __call__(self, value):
        if value % self.base != 0:
            message = 'This field must be a multiple of %d.' % self.base
            raise serializers.ValidationError(message)
```

#### [Using `set_context()`](http://www.django-rest-framework.org/api-guide/validators/#using-set_context)

유효성 검사기를 추가 컨텍스트로 사용되는 serializer 필드로 전달해야 할 수 있습니다. 클래스 기반 유효성 검사기에서 set_context 메서드를 선언하여 그렇게 할 수 있습니다.

```python
def set_context(self, serializer_field):
    # Determine if this is an update or a create operation.
    # In `__call__` we can then use that information to modify the validation behavior.
    self.is_update = serializer_field.parent.instance is not None
```

