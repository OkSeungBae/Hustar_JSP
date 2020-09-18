# 2020 09 18
=============

## 캐릭터 인코딩 필터

* util/CharacterEncodingFilter.java
```
package util;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class CharacterEncodingFilter implements Filter {

	private String encoding;
	
	@Override
	public void init(FilterConfig config) throws ServletException {
		encoding = config.getInitParameter("encoding");
		if (encoding == null) {
			encoding = "UTF-8";
		}
	}

	@Override
	public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
			throws IOException, ServletException {
		req.setCharacterEncoding(encoding);
		chain.doFilter(req, res);
	}

	@Override
	public void destroy() {
	}
}
```

- 해당 인코딩을 사용하려면 다음과 같이 web.xml에 다음과 같은 설정을 추가하면 된다.
* web.xml
```
<filter>
	<filter-name>encodingFilter</filter-name>
	<filter-class>util.CharacterEncodingFilter</filter-class>
	<init-param>
		<param-name>encoding</param-name>
		<param-value>utf-8</param-value>
	</init-param>
</filter>

<filter-mapping>
	<filter-name>encodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```