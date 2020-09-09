# 2020 09 09
=============

# 자바빈과 <jsp:useBean>액션태그

1. <jsp:useBean>태그를 이용한 자바 객체 사용

1-1. <jsp:setProperty> / <jsp:getProperty> 사용 방법

- <jsp:useBean>태그를 사용할 때 객체가 존재하면 새로 생성하지 않고 기존에 존재하는 객체를 그대로 사용한다.

* /pro01/Java Resources/src/ch08.member/MemberInfo.java
```
package ch08.member;

import java.util.Date;

public class MemberInfo {

	private String id;
	private String password;
	private String name;
	private Date registerDate;
	private String email;
	
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Date getRegisterDate() {
		return registerDate;
	}
	public void setRegisterDate(Date registerDate) {
		this.registerDate = registerDate;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
}
```

* makeObject.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<jsp:useBean id="member" scope="request" class="ch08.member.MemberInfo"/>

<% 
	member.setId("test");
	member.setName("옥승배");

%>
<jsp:forward page="/ch08/usePbject.jsp"/>
```
=====

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<jsp:useBean id="member" scope="request" class="ch08.member.MemberInfo"/>
<jsp:setProperty name="member" property="name" value="옥승배" />	<!-- name은 객체이름 / property는 클래스의 변수 이름 / value는 값 -->
<jsp:setProperty name="member" property="id" value="test" />	
<jsp:setProperty name="member" property="email" value="test@test.com" />	
<jsp:forward page="/ch08/useObject.jsp"/>
```

- 위 두개의 코드는 동일한 동작을 한다.
- 이름이 member인 객체를 생성해서 request기본 객체에 저장한다.
- <jsp:useBean>액션 태그 ID속성에 지정한 이름을 변수명으로 사용하기 때문에 , 스크립트 코드에서 이 이름을 사용하여 생성한 객체에 접근할 수 있다.

* useObject.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<jsp:useBean id="member" scope="request" class="ch08.member.MemberInfo" />

<%=member.getName() %>(<%= member.getId() %>)회원님 안녕하세요
```

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<jsp:useBean id="member" scope="request" class="ch08.member.MemberInfo" />
<jsp:getProperty name="member" property="name" /> (<jsp:getProperty name="member" property="id" />)회원님.
<br>
이메일은
<jsp:getProperty name="member" property="email" />
```

- 위 두 코드는 동일한 동작을 한다.
- <jsp:useBean>액션 태그에서 id값이 makeObject.jsp에서 생성한 객체 이름과 다른 값을 사용하면 request기본 객체에 속성이 존재하지 않기 때문에 이전 페이지에서 생성하여 request에 넘겨준 값과 동일한 id를 사용하여야 한다.

==============================================================================

1-2. <jsp:setProperty> / <jsp:getProperty> 사용 방법

* membershipForm.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원가입 폼</title>
</head>
<body>

<form action="/pro01/ch08/processJoining.jsp" method="post">
	<table border="1" cellpadding="0" cellspacing="0" >
		<tr>
			<td>아이디</td>
			<td colspan="3"><input type="text" name="id" size="10"></td>
		</tr>
		<tr>
			<td>이름</td>
			<td><input type="text" name="name" size="10"></td>
			<td>이메일</td>
			<td><input type="text" name="email" size="10"></td>
		</tr>
		<tr>
			<td colspan="4" align="center">
			<input type="submit" value="회원가입">
			</td>
		</tr>
	</table>
</form>

</body>
</html>
```

- form을 통해서 processJoining으로 request기본 객체로 데이터를 넘긴다.

* processJoining.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<% request.setCharacterEncoding("utf-8");%>

<jsp:useBean id="memberInfo" class="ch08.member.MemberInfo" scope="request"/>
<jsp:setProperty name="memberInfo" property="*"/>		
<jsp:setProperty name="memberInfo" property="password" value="<%= memberInfo.getId() %>"/> <!-- 사용자가 입력한 값이 아니기 때문에 따로 작성한것이다 id값이랑 동일하게 처리한 것 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>가입</title>
</head>
<body>


<table width="400" border="1" cellpadding="0" cellspacing="0" >
	<tr>
		<td>아이디</td>
		<td><jsp:getProperty name="memberInfo" property="id"/></td>
		<td>암호</td>
		<td><jsp:getProperty name="memberInfo" property="password"/></td>
	</tr>
	<tr>
		<td>이름</td>
		<td><jsp:getProperty name="memberInfo" property="name"/></td>
		<td>이메일</td>
		<td><jsp:getProperty name="memberInfo" property="email"/></td>
	</tr>
</table>

</body>
</html>
```

- setProperty를 통해 requset기본 객체의 데이터를 memberInfo로 받는다.


