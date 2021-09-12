---
layout: post
current: post
cover: assets/built/images/java/springboot/spring-logo.png
navigation: True
class: post-template
subclass: 'post tag-java'
author: kilhyeonjun
title: "springboot 3.동작원리"
comments: true
categories:
- java
tags:
- springboot
date: "2021-09-12 21:00"
---
{% include springboot/springboot-table-of-contents.html %}
해당 게시물은 이 [강의](https://edu.goorm.io/learn/lecture/24604/스프링부트-개념정리)를 보고 제작하게되었습니다.
# ※ 동작원리

## 1. 내장 톰캣을 가진다.
- 톰캣을 따로 설치할 필요가 없이 내장되어있다.

### Socket 이란
- 네트워크 상에서 데이터를 보내거나 받기 위한 실제적인 창구 역할을 하는 것입니다.
- 프로세스가 데이터를 보내거나 받기 위해서는 반드시 소켓을 통해서 해야합니다.
- 소켓은 프로토콜, ip 주소, port 번호로 정의됩니다.

#### Socket 통신
- 한 port에 클라이언트 1개로 제한됩니다.
- 처음 통신시 Main 쓰레드인 소켓이 연결 되면 자식 쓰레드 소켓으로 연결하고 메인 쓰레드와의 통신은 끊습니다.
- 쓰레드로 작동되기 때문에 동시에 여러 클라이언트와 통신하는 것 처럼 보입니다.
- 계속 연결 되어있어 클라이언트 식별이 가능합니다.
- 부하가 많이 걸려서 느려질 수 가 있습니다.

### HTTP (Hypertext Transfer Protocol) 란
- 웹서버와 클라이언트가 사이에서 문자(문서)를 주고받기 위해 사용되는 프토토콜입니다.

#### HTTP 통신
- Stateless 방식을 사용하여 계속 연결되어 있지는 않고 필요한 정보만 주고받고 연결을 끊습니다.
- 계속 연결이 되어있는 형식이 아니기 때문에 부하가 많지 않고 하나의 포트로만 통신이 가능합니다.
- 정보가 필요할 때만 새로 연결이 되기 때문에 클라이언트 식별이 불가능합니다.
- 소켓통신을 기반으로 만들어졌습니다.
- static한 자원을 제공하여 정보가 실시간으로 반영되지 않습니다.

### 웹서버
- 가지고 있는 파일/문서를 공유할 수 있게 해주는 서버입니다.
- 클라이언트가 서버에 문서를 요청(Request)하면 서버는 해당 문서를 응답(Response)합니다.
- 서버는 클라이언트의 IP를 몰라도 됩니다.
- 자바를 이해하지 못합니다.
- 이해하지 못한 Jsp 코드는 톰캣에게 보냅니다.

### 톰캣
- 웹브라우저에서 읽을수 없는 jsp 코드를 컴파일하고 HTML로 번역해서 웹서버 돌려주는 역할을 합니다.
- 서블릿 컨테이너가 있습니다.

## 2. 서블릿 컨테이너
![img](assets/built/images/java/springboot/servletContaine.png)
- 스프링은 URL(자원) 접근을 막고 URI(식별자)를 통해서만 자원에 접근이 가능합니다.
- 모든 Request는 톰캣을 거친 후 Response 됩니다.
- Request가 들어오면 스레드가 생성되고 스레드에 대한 서블릿 객체가 생성됩니다.
- 스레드를 정해진 개수만큼 생성하고 그 후에는 기존 스레드를 재사용 합니다.
- 스레드를 재사용 함으로써 삭제와, 생성 과정을 안하기에 처리 시간이 단축됩니다.
- 스레드를 사용 함으로써 여러개의 Reqeust를 동시에 처리할 수 있습니다.

### URL 이란
-URL은 흔히 웹 주소라고도 하며, 컴퓨터 네트워크 상에서 리소스가 어디 있는지 알려주기 위한 규약입니다.
- ex) https://kilhyeonjun.github.io/a.png

### URI 란
- URI는 특정 리소스를 식별하는 통합 자원 식별자(Uniform Resource Identifier)를 의미합니다.
- ex) https://kilhyeonjun.github.io/picture/a

## 3. web.xml

### ServletContext의 초기 파라미터
- 초기 파라미터는 한 번 정해지면 어디서든지 사용 가능한 값입니다.

### Session의 유효시간 설정
####  Session 이란?
- 인증을 통해서 접속하는 방법입니다.
- 유효시간을 정하게 되면 각 Session을 해당 시간동안만 접속이 가능합니다.
- 유호시간을 늘리고 싶으면 재인증을 통해 갱신하면 됩니다.

### Servlet/JSP에 대한 정의

### Servlet/JSP 매핑
- 요청한 자원, 로케이션, 식별자가 어디있는지 알려주고 찾아갈 수 있게 도와주는 것입니다.
- 모든 클래스 매핑을 적용하기에는 코드가 너무 복잡하므로 FrontController 패턴을 이용합니다.

### Mime Type 매핑
- Mime Type을 식별 한 후 데이터가 필요한 곳으로 매핑하는 것입니다.
- HTTP get방식은 데이터를 가지고 오지 않습니다. 
  - 데이터를 가지고 가기 위해 요청되었기 때문입니다.
  
#### MimeType 이란?
- 서버에 접속할때 가지고 오는 데이터의 타입입니다.

### Welcome File list
- 요청의 목적이 불명확실할 때에 보내는 곳입니다.

### Error Pages 처리
- 에러가 난 페이지를 따로 처리하는 것입니다.

### 리스너/필터 설정

#### 필터 란 ?
- 요청을 거르거나 가지고 있는 데이터를 필터링 하는 것입니다.  

#### 리스너 란 ?
- 주어진 행동에 대해서 그 행동이 일어나는지 감시하는 것입니다.

### 보안
- 보안 기능을 제공합니다.

## 4. FrontController 패턴