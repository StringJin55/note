#Markdown
##Markdown이란
웹상에서 텍스트 스타일을 지정하는 방법. GitHub에서 주로 사용한다.

##예제
###Text
**bold**
*italic*

###List
numbered list:

1.일
2.이
3.삼

bullet point:

* 일
* 이
* 하위분류가 필요할때는
	* 공백 두칸을 들여쓰기하고(tab으로 공백처리) dash 나 star을 찍어준다
  

### 이미지 넣기
![돌고래 이미지]
(http://www.eroun.net/wp-content/themes/advanced-newspaper/timthumb.php?src=http://www.eroun.net/wp-content/uploads/2012/05/%EC%A0%9C%EB%8F%8C%EC%9D%B4.jpg&q=90&w=479&zc=1)

### Headers&인용구

'#'를 붙인다(갯수가 많아질수록 문서구조 상 더 하위 분류)

인용구의 앞에는 > 를 붙인다
> 저는 돌고래 입니다


### 코드 삽입
한줄의 코드일때는 ``로 감싸준다

`var example = true`
장문의 코드일때는 4스페이스(탭 2번)으로 시작한다.

		if (isAwesome){
			return true
		}	

혹은 들여쓰기없이 ```으로 감싸주어도 된다

```
if (isAwesome){
	return true
}	
```

구문 강조를 하고싶으면 사용한 언어를 표기해주면 된다

```javascript
if (isAwesome){
	return true
}	
```


### 추가기능(깃허브)
다른 사용자에게 코멘트 보내기

안녕 @kneath - 스웨터가 좋아요

체크박스

- [x] 예
- [ ] 아니오

