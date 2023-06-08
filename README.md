# chap59_boot_gradle
project spring starter project -> type = gradle = groovy   Frequently Used -> lombok, springboot Devtools, spring web

1. build.gradle 설정
plugins {
	id 'java'
	id 'org.springframework.boot' version '2.7.12'
	id 'io.spring.dependency-management' version '1.0.15.RELEASE'
}

group = 'com.javalab'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	developmentOnly 'org.springframework.boot:spring-boot-devtools'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	
   // 타임리프
   implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
   implementation 'nz.net.ultraq.thymeleaf:thymeleaf-layout-dialect' /* 레이아웃 */   
	
}

tasks.named('test') {
	useJUnitPlatform()
}

2. application.properties 설정
# 포트번호
server.port:8080

# 기본 폴더/뷰리졸버(jsp)
#spring.mvc.view.prefix:/WEB-INF/views/
#spring.mvc.view.suffix:.jsp

# 뷰리졸버(Thymeleaf)
spring.thymeleaf.enabled=true
spring.thymeleaf.cache=false
spring.thymeleaf.check-template-location=true
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html

# 컨텍스트 패스
server.servlet.context-path=/

# 데브툴스 설정
spring.devtools.livereload.enabled=true
spring.devtools.restart.enabled=true 

3. src/main/java 작업
- com.javalab.board.controller
package com.javalab.board.controller;

import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import com.javalab.board.vo.UserVo;

import lombok.extern.slf4j.Slf4j;

@Controller
@Slf4j
public class BoardController {
	@GetMapping("/")
	public String home(Model model, HttpSession session) {
		log.info("여기는 BoardController home()");
		
		// 컬렉션 생성
		UserVo user = new UserVo("dream", "홍길동", 18);
		model.addAttribute("user", user);
		
		return "index";
	}
}

- com.javalab.board.vo
package com.javalab.board.vo;

import groovy.transform.ToString;
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Getter
@Setter
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class UserVo {
	
	private String id;
	private String name;
	private int age;
	
}

4. 타임리프 뷰 페이지 생성 templates
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h3>User Info</h3>
	<br>
	1. <p th:text="${user.id}"></p>
	2. <p th:text="${user.name}"></p>
	3. <p th:text="${user.age}"></p>
</body>
</html>
