[RESTful Web Service HTTP methods](https://spoqa.github.io/2012/02/27/rest-introduction.html)



# [Requests](http://www.django-rest-framework.org/api-guide/requests/#requests)



DRF의 `Request` 객체는 `HttpRequest`를 확장하여 request파싱과 인증을 해준다.



## Request parsing

REST 프레임 워크의 Request 객체는 유연한 요청 구문 분석 기능을 제공하므로 사용자가 일반적으로 양식 데이터를 처리하는 것과 같은 방식으로 JSON 데이터 또는 다른 미디어 유형으로 요청을 처리 할 수 있습니다.



### .data

`request.data`는 파싱된 request body를 반환한다. `request.POST`나 `request.FILES`과 비슷하다. 

`POST`이외에 `PUT`과`PATCH`에도 접근 가능하다. 

[parsers documentation](http://www.django-rest-framework.org/api-guide/parsers/)



### .query_params

`request.query_params`는 `request.GET`이랑 똑같은데 코드의 명확성을 위해 (HTTP method는 GET 요청 뿐만 아니라 query parameter도 포함할 수 있다.) request.GET	보다 request.query_params사용을 권장한다.



### .parsers

APIView 클래스 또는 @api_view 데코레이터는 뷰에 설정된 parser_classes 또는 DEFAULT_PARSER_CLASSES 설정에 따라이 속성이 자동으로 Parser 인스턴스 목록으로 설정되도록합니다.

일반적으로이 속성에 액세스 할 필요는 없습니다.



## [Content negotiation](http://www.django-rest-framework.org/api-guide/requests/#content-negotiation)

### .accepted_renderer

인스턴스. 

### .accepted_media_type

문자열. the content negotiation stage에서 수락한 미디어 타입.



## [Authentication](http://www.django-rest-framework.org/api-guide/requests/#authentication)

REST 프레임 워크는 다음과 같은 기능을 제공하는 유연한 요청 별 인증을 제공합니다.

- API의 다른 부분에 대해 서로 다른 인증 정책을 사용하십시오.
- 다중 인증 정책의 사용을 지원합니다.
- 들어오는 요청과 관련된 사용자 및 토큰 정보를 제공하십시오.



### .user

request.user는 일반적으로 django.contrib.auth.models.User의 인스턴스를 반환하지만 동작은 사용되는 인증 정책에 따라 다릅니다. 요청이 인증되지 않은 경우 request.user의 기본값은 django.contrib.auth.models.AnonymousUser의 인스턴스입니다. 자세한 내용은 [인증 문서](http://www.django-rest-framework.org/api-guide/authentication/)를 참조하십시오.



### .auth

request.auth는 추가 인증 컨텍스트를 리턴합니다. request.auth의 정확한 작동은 사용되는 인증 정책에 따라 다르지만 대개 요청이 인증 된 토큰의 인스턴스 일 수 있습니다. 요청이 인증되지 않았거나 추가 컨텍스트가없는 경우, request.auth의 기본값은 None입니다.



### .authenticatiors

APIView 클래스 또는 @api_view 데코레이터는 뷰에 설정된 authentication_classes 또는 DEFAULT_AUTHENTICATORS 설정에 따라 이 속성이 자동으로 Authentication 인스턴스 목록으로 설정되도록합니다. 일반적으로이 속성에 액세스 할 필요는 없습니다.



## [Browser enhancements](http://www.django-rest-framework.org/api-guide/requests/#browser-enhancements)

REST 프레임 워크는 브라우저 기반 PUT, PATCH 및 DELETE 양식과 같은 몇 가지 브라우저 개선 사항을 지원합니다.



### .method

request.method는 HTTP method를 나타내는 대문자 문자열을 반환한다. 브라우저 기반 PUT, PATCH, DELETE form은 명백히(?) 지원된다. 자세한 정보는 [브라우저 개선사항 문서](http://www.django-rest-framework.org/topics/browser-enhancements/) 참조. 



### .content_type

request.content_type은 HTTP request body의 content type을 나타내는 문자열 객체를 반환하거나 미디어 유형이 제공되지 않은 경우 empty string`''`을 반환합니다. 일반적으로 REST 프레임 워크의 기본 request parsing 동작에 의존하므로 일반적으로 요청의 콘텐츠 형식에 직접 액세스 할 필요가 없습니다. request의 콘텐츠 형식에 액세스해야하는 경우 브라우저 기반 비 형식 콘텐츠에 대한 투명한 지원을 제공하므로 request.META.get ( 'HTTP_CONTENT_TYPE')을 사용하는 것보다 .content_type 속성을 사용해야합니다.



### .stream

`request.stream` 요청 본문의 내용을 나타내는 스트림을 반환합니다.



## [Standard HttpRequest attributes](http://www.django-rest-framework.org/api-guide/requests/#standard-httprequest-attributes) 

REST 프레임 워크의 request은 Django의 HttpRequest를 확장하므로 다른 모든 표준 속성과 메소드도 사용할 수 있습니다. 예를 들어 request.META 및 request.session 딕셔너리는 정상적으로 사용 가능합니다.

구현 이유로 인해 Request 클래스는 HttpRequest 클래스에서 상속하지 않고 대신 [composition 조립](http://aroundck.tistory.com/617)을 사용하여 클래스를 확장합니다.

