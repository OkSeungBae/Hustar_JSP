# 2020 09 09
=============

## 표현언어 (Expression Language)

* 표현식과 표현언어의 차이

```
<%-- 표현식 --%>
<%= member.getAddress().getZipcode() %>

<%-- 표현언어 --%>
${member.address.zipcode}
```

```
<%@ page contentType = "text/html; charset=utf-8" %>
<%
	request.setAttribute("name", "옥승배");
%>
<html>
<head><title>EL Object</title></head>
<body>

<%-- name이란 속성을 찍을려면 다음과 같이 작성해야한다. --%>
<%= request.getAttribute("name") %>
<br>
<%-- EL사용 --%>
${name}
<br>
${requestScope.name}

<br>
요청URI : ${pageContext.request.requestURI}
<br>
request의 name속성 : ${requestScope.name}
<br>
code 파라미터 : ${param.code}

</body>
</html>

```

- EL로 표현한 ${name} 과 ${requestScope.name} 은 같은 값을 의미한다
- EL은 pageContext -> request -> session -> application 순으로 우선순위를 가진다
따라서 같은 이름의 Attribute가 있다면 우선순위가 높은 Attribute의 값을 먼저 접근한다.

- param.code는 파라미터가 없기 때문에 null값 이지만 nullPointException이 뜨지 않는다
파라미터가 있다면 값이 나오고 없어도 Exception이 뜨지 않는다.
- http://localhost:8080/pro01/ch11/useELObject.jsp?code=1 다음과 같이 쓰면 값이 들어감

1-2. 연산자

EL의 수치 연산자
* +, -, *, /, %, 단항연산자-

비교연산자
* ==, !=, <, >, <=, >=

논리연산자
* &&, || , !

### empty 연산자
* empty<값>
검사할 객체가 텅 빈 객체인지를 검사하지 위해 사용

