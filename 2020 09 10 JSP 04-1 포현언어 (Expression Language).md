# 2020 09 10
=============

## EL

1. EL에서 객체의 메서드 호출

* src/Thermometer.java
```
package chap11;

import java.util.HashMap;
import java.util.Map;

public class Thermometer {

	private Map<String, Double> locationCelsiusMap = new HashMap<String, Double>();

	public void setCelsius(String location, Double value) {
		locationCelsiusMap.put(location, value);
	}

	public Double getCelsius(String location) {
		return locationCelsiusMap.get(location);
	}

	public Double getFahrenheit(String location) {
		Double celsius = getCelsius(location);
		if (celsius == null) {
			return null;
		}
		return celsius.doubleValue() * 1.8 + 32.0;
	}

	public String getInfo() {
		return "온도계 변환기 1.1";
	}
}
```

* ch11/thermometer.jsp
```
<%@page import="chap11.Thermometer"%>
<%@ page contentType="text/html; charset=utf-8"%>
<%
	Thermometer thermometer = new Thermometer();
	request.setAttribute("t", thermometer);
%>
<html>
<head>
	<title>온도 변환 예제</title>
</head>
<body>
	${t.setCelsius('서울', 27.3)}
	서울 온도: 섭씨 ${t.getCelsius('서울')}도 / 화씨 ${t.getFahrenheit('서울')}

	<br/>
	정보: ${t.info}
</body>
</html>

```