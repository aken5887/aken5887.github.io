---
layout: post
title:  "Maven 3.8.1 이후부터 HTTP 저장소는 차단됩니다."
date:   2023-08-14
categories: TroubleShooting
tags : 전자정부프레임워크
---

전자정부 프레임워크를 가져와 intellij에서 maven을 새로 빌드하였을 때 해당 오류 발생하였다.

maven 3.8.1 버전 이후에는 http에 대한 외부 연결을 기본적으로 막는다고 한다. 

# 에러 내용

---

```
org.terracotta.forge:forge-parent:pom:2.0 failed to transfer from http://0.0.0.0/ during a previous attempt. This failure was cached in the local repository and resolution is not reattempted until the update interval of maven-default-http-blocker has elapsed or updates are forced. Original error: Could not transfer artifact org.terracotta.forge:forge-parent:pom:2.0 from/to maven-default-http-blocker (http://0.0.0.0/): Blocked mirror for repositories: [terracotta-repository (http://www.terracotta.org/download/reflector/releases, default, releases+snapshots)]

Maven 3.8.1 이후부터 HTTP 저장소는 차단됩니다.

가능한 해결법:
- Maven settings.xml에 HTTP 저장소가 없는지 확인
- Maven POM 파일에 HTTP 저장소 http://www.terracotta.org/download/reflector/releases이(가) 없는지 확인
- Maven settings.xml에서 HTTP URL을 허용하는 http://www.terracotta.org/download/reflector/releases의 미러를 추가
- 설정에서 Maven을 3.8.1 이전의 버전으로 다운그레이드
```

# 해결 방법

---

maven의 settings.xml 파일에 http url 을 허용하는 코드를 삽입하였다.

### `settings.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
	http://maven.apache.org/xsd/settings-1.0.0.xsd">
<mirrors>
	<mirror>
		<id>maven-default-http-blocker</id>
		<mirrorOf>external:dont-match-anything-mate:*</mirrorOf>
		<name>Pseudo repository to mirror external repositories initially using HTTP.</name>
		<url>http://www.terracotta.org/download/reflector/releases</url>
	</mirror>
</mirrors>
</settings>
```
