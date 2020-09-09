# 2020 09 09
=============

# JSP액션태그

1. <jsp:include> 액션 태그
- chap07의 include부분 확인

2. <jsp:forward> 액션 태그

2-1. 사용방법

* pro01/WebContent/ch07_1/from/from.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%--
    	<jsp:foward>액션 태그를 실행하면
    	생성했던 출력 결과는 모두 제거된다.
     --%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

이 페이지는 from.jsp가 생성한 것입니다.
<jsp:foward page="/ch07_1/to/to.jsp" />

</body>
</html>
```

* pro01/WebContent/ch07_1/to/to.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>to.jsp의 실행결과</title>
</head>
<body>

이 페이지는 to.jsp가 생성한 것입니다.

</body>
</html>
```

- 실행결과 URL은 from.jsp에 요청을 했지만 내부의 흐름에 의해 to.jsp가 화면에 보여진다.

2-2. 활용방법

* /pro01/WbeContent/ch07_1/select.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>옵션 선택 화면</title>
</head>
<body>

<form action="<%=request.getContextPath() %>/ch07_1/view.jsp">

보고 싶은 페이지 선택 :
	<!-- 드랍 다운 리스트(select태그) 이고 아이템들(option태그)이다 -->
	<select name="code">
		<option value="A">A페이지</option>
		<option value="B">B페이지</option>
		<option value="C">C페이지</option>
	</select>

	<input type="submit" value="이동">
</form>

</body>
</html>
```

* /pro01/WbeContent/ch07_1/view.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	String code = request.getParameter("code");
	String viewPageURL = null;
	
	if(code.equals("A"))
	{
		viewPageURL = "/ch07_1/viewModule/a.jsp";
	}
	else if(code.equals("B"))
	{
		viewPageURL = "/ch07_1/viewModule/b.jsp";
	}
	else if(code.equals("C"))
	{
		viewPageURL = "/ch07_1/viewModule/c.jsp";
	}
	
%>
<jsp:forward page="<%= viewPageURL %>"/>

<%-- 오직 JSP코드만 필요하기 때문에 HTML코드는 지웠다. --%>
```

- select.jsp에서 각각 리스트를 선택하고 이동하면 view.jsp페이지로 이동하지만 각 다른 페이지를 표시한다.

3. <jsp:param> 액션 태그

```
<jsp:forward page="moveTo.jsp">
	<jsp:param name="first" value="BK">
	<jsp:param name="last" value="choi">
</jsp:forward>
```
다음과 같이 파라미터를 넘길 수 있다.

## 4. 기본 객체의 속성을 이용해서 값 전달하기 ( 두 페이지 간의 데이터 넘길 때 사용하는 방법중 가장 많이 사용한다.)

* /from/makeTime.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%@ page import="java.util.Calendar" %>
    
<%
	Calendar cal = Calendar.getInstance();
	request.setAttribute("time", cal);
%>
<jsp:forward page="/ch07_1/to/viewTime.jsp"/>
```

* /to/viewTime.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%@ page import="java.util.Calendar" %>


<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<% Calendar cal = (Calendar)request.getAttribute("time"); %>
	현재시간은 <%=cal.get(Calendar.HOUR) %>시 <%= cal.get(Calendar.MINUTE) %>분 <%= cal.get(Calendar.SECOND)%>초 입니다.
</body>
</html>
```

- 두 페이지 간 request라는 기본 객체를 사용하기 때문에 request.setAttribute / request.getAttribute를 사용해서 변수를 공유한다.