---
layout: post
title:  "JSP EL(Expression Language)이 동작하지 않을 때"
date:   2023-08-14
categories: TroubleShooting
tags : JSP, EL
---

# 에러 내용

---

```html
Hello, ${greeting}
```

`JSP` 페이지에 `EL`이 동작하지 않고 ${} 문자열이 그대로 출력하는 현상 발생하였다. 

# 원인

---

`web.xml`의 web-app 태그 안의 버전에 서블릿 버전이 2.3 이하로 표시될 경우에 `EL`문이 동작하지 않을 수 있다.

## **`web.xml`**

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >
<web-app>
  <display-name>Spring Study Application</display-name>
</web-app>
```

# 해결 방법

---
## 1. isELIgnored="false" 를 jsp 상단에 추가

## 2. web.xml 스키마 헤더 변경

Servlet 4.0에 해당하는 `web.xml` 스키마 헤더로 변경해주었다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0">
  <display-name>Spring Study Application</display-name>
</web-app>
```

`web.xml`을 변경 한 후에 해결 되었다.

```html
Hello, World
```

# 참고

---

**Tomcat & Servlet & JSP 스펙**

![Untitled](/assets/images/posts/2023-08-14/tomcat-servlet-jsp.png)

*출처 : [https://zetawiki.com/wiki/톰캣_버전별_서블릿_스펙](https://zetawiki.com/wiki/%ED%86%B0%EC%BA%A3_%EB%B2%84%EC%A0%84%EB%B3%84_%EC%84%9C%EB%B8%94%EB%A6%BF_%EC%8A%A4%ED%8E%99)*

예) tomcat 9.0.26 버전을 사용 하는 경우 maven 설정

```xml
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>4.0.1</version>
  <scope>provided</scope>
</dependency>
<dependency>
  <groupId>javax.servlet.jsp</groupId>
  <artifactId>javax.servlet.jsp-api</artifactId>
  <version>2.3.3</version>
</dependency>
```
