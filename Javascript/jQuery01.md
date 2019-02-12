﻿

![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190212-lightgray.svg?style=flat-square)

# jQuery

- 웹 표준이 정착되기 이전, 브라우저 간 호환성이 미미할 때 혜성처럼 등장한 언어이다
- 크로스 브라우징이 가능하며, 표현 방식이 간편해 많은 개발자가 이용함
- jQeury 를 이용하는 주요 이유 중 하나가 Effects 이다. 이는 동적 효과를 나타낼 때 활용된다
- **window.onload=function(){};** 와 **$(document).ready(function(){});** 비교
	* 전자는 지정된 주요 페이지인지, 로드된 서브 페이지인지에 따라 동작하지 않을 수도 있다
	* 그러나 후자는 언제나 동작이 보장된다

## jQuery 주의점

- 고수가 알려주는 팁
- 선택자에서 전체를 지정하는 방식 (**$('*')** 혹은 범위가 넓은 선택자) 은 절대 사용하지 말 것
- 인터프리터 방식이므로 한줄한줄 해석하느라 퍼포먼스 부하가 극심해진다

## 개요

- 기본 문법
	* 따옴표 구분 없음
	* 계층별 요소명을 명시적으로 지정해주는 것이 셀렉터 속도 향상에 도움됨
	* 기본적으로 셀렉터는 주로 **$([selector])** 식으로 지정된다
	* 접근 방법 : HTML DOM / DOM 계층구조 / 속성을 통한 접근 (id, class 등)
	* 이벤트 : 접두사 **on** 이 붙지 않는다. 대신 각 이벤트명에 따른 함수가 존재한다
- 설치
	* 라이브러리 : cdn 에 비해 번거롭지만, 프로젝트에 포함시켜 배포 가능, 안정성 보장
	* cdn 활용 : 사용이 간편하지만, 네트워크가 안정적이어야 함 (접속자가 네트워크를 통해 직접 다운받음)
	* 안정성 및 네트워크 트래픽 감소 측면에서 라이브러리 포함을 권장. 그러나 편리성 측면에서 cdn 도 괜찮음
- jQuery API 구조
	* **jQuery CORE**
	* **Selectors**
	* **Attributes**
	* **Traversing**
	* **Manipulation**
	* ------------ 여기까지는 태그를 선택하는 방법 ------------
	* **CSS**
	* **Events**
	* **Effects** : 동적 이펙트. 빈번히 활용됨
	* **Ajax** : 더욱 간결한 방식으로 사용 가능. 매우 강려크
	* **Utilities**
	* DOM

### 문법 상세

- **선택자 문법**
	* css 에서 각 html 요소를 선택하는 방식과 유사하다
	* 정규식에서의 방식도 일부 존재 (**^** 시작, **$** 끝) >> 이런 식으로 이쪽 언어에서 쓰이던 규칙이 저쪽 언어에서도 활용되는 일이 빈번함
	* $(), $('*'), $('#id'), $('.class name'), $('element name'),
	* $('element#id'), $('element.class'), $('s1, s2, s3, ...'), $('#container element'), $('.container element')
- **속성값을 사용한 Selector**
	* $(Selector[attr]), $(Selector[attr='value']), $(Selector[attr!='value']), $(Selector[attr^='value']), $(Selector[attr$='value'])
- **CSS 문법**
	* 기본적으로 **[선택자].css([스타일 속성], [값]);** 이런 식이지만,
	* 모든 스타일 속성들을 json 맵 방식으로 한 번에 나열해서 사용할 수 있다

**선택자, css 예제**
```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jQuery Select Test02</title>
<script src='/js/jquery-3.3.1.min.js'></script>
<script>
	/*
	 * 1. 모든 span 의 넓이와 높이 외곽선 지정
	 * 2. id 가 ab 로 시작하는 요소의 배경색은 파랑
	 * 3. id 가 d 로 끝나는 요소의 문자색은 빨강
	 * 4. id 에 e 가 포함된 요소의 외곽선 색 지정
	 */
	$(document).ready(function () {
		$('#cont > span').css({"display":"inline-block", "width":"350px", "height":"100px", "margin-bottom":"10px", "border":"1px solid black"});
		$('#cont span[id^=ab]').css("background-color", "#0000ff");
		$('#cont span[id$=d]').css("color", "#ff0000");
		$('#cont span[id*=e]').css('border', '2px solid #00ff00');
	});
</script>
</head>
<body>

	<div id='selector02'>
		<h3>jQuery Select Test02</h3>
		<h4>&nbsp;&nbsp;ID, CLASS, Tag name 으로 선택</h4>
		
		<div id='cont'>
			<span id='abc'>SPAN</span><br/>
			<span id='abd'>SPAN</span><br/>
			<span id='abe'>SPAN</span><br/>
			<span id='ddd'>SPAN</span><br/>
		</div>
	</div>

</body>
</html>
```

- **Filter**
	* 선택된 요소 중 필요한 요소만을 걸러내는 방법
	* 키워드 앞에 접두어로 콜론(:) 이 붙는다. 필터 자체는 선택한 요소이름 뒤에 붙는다
	* 필터와 필터를 연결해서 사용 가능
	* 필터의 종류 : 기본필터, 폼 필터, 자식필터, traverse
- **Traverse**
	* 기본적으로 셀렉터로 필요한 요소를 가져올 수 있으나, **traverse** 를 사용하면 1차로 선택된 항목들을 대상으로 2, 3차 작업을 더 할수 있다
	* 그러한 작업은 특별하게 정의된 함수를 통해 수행된다
	* 종류 : 필터링 함수, Miscellaneous traverse, tree traverse

- **CORE**
	* 코어는 단순한 선택자의 의미를 넘어서 특정한 데이터 처리 로직도 수행한다
	* 각 기능에 맞는 함수를 사용한다

- **Attribute**
	* .attr() : 엘리먼트 집합에 속한 첫 번째 엘리먼트의 속성을 가져온다
	* 태그 내의 값 가져오기
		- value 속성으로 세팅된 값은 val(), 태그 영역 내 데이터는 text(), html 코드까지 포함한 데이터는 html() 함수로 가져온다
		- 그러므로, val() 같은 경우 태그에 value 속성이 지정되어 있어야 하며, 없다면 text() 나 html() 을 활용해야 한다
- **Event**
















