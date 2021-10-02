---
layout: post
current: post
cover: assets/built/images/java/springboot/spring-logo.png
navigation: True
class: post-template
subclass: 'post tag-java'
author: kilhyeonjun
title: "springboot 블로그 10.화면구현" 
comments: true
categories:
- java
tags:
- springboot
- blog
date: "2021-10-02 16:51+09:00"
---
{% include springboot/springboot-blog-table-of-contents.html %}
해당 게시물은 이 [강의](https://edu.goorm.io/lecture/24605/스프링부트-나만의-블로그-만들기)를 보고 제작하게되었습니다.

# 화면구현

## 1. 메뉴와 푸터
- header.jsp  

~~~jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %>
<sec:authorize access="isAuthenticated()">
	<sec:authentication property="principal"  var="principal"/>
</sec:authorize>
<!DOCTYPE html>
<html lang="en">
<head>
<title>Bootstrap Example</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">

<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</head>
<body>
	<nav class="navbar navbar-expand-md bg-dark navbar-dark">
		<a class="navbar-brand" href="/">kbox</a>
		<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#collapsibleNavbar">
			<span class="navbar-toggler-icon"></span>
		</button>
		<div class="collapse navbar-collapse" id="collapsibleNavbar">
			<c:choose>
				<c:when test="${empty principal}">
					<ul class="navbar-nav">
						<li class="nav-item"><a class="nav-link" href="/auth/loginForm">로그인</a></li>
						<li class="nav-item"><a class="nav-link" href="/auth/joinForm">회원가입</a></li>
					</ul>
				</c:when>
				<c:otherwise>
					<ul class="navbar-nav">
						<li class="nav-item"><a class="nav-link" href="/board/form">글쓰기</a></li>
						<li class="nav-item"><a class="nav-link" href="/user/form">회원정보</a></li>
						<li class="nav-item"><a class="nav-link" href="/logout">로그아웃</a></li>
					</ul>
				</c:otherwise>
			</c:choose>
		</div>
	</nav>
	<br>
~~~
- footer.jsp

~~~jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<br />
<div class="jumbotron text-center" style="margin-bottom: 0">
	<p>Created by kbox</p>
	<p>📞 010-0000-0000</p>
	<p>🏴 인하공업전문대학 컴퓨터정보과</p>
</div>
</body>
</html>
~~~

## 2. 회원가입 화면
- joinForm.jsp

~~~jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<%@ include file="../layout/header.jsp"%>

<div class="container">
	<form>
		<div class="form-group">
			<label for="username">Username</label> <input type="text" class="form-control" placeholder="Enter username" id="username">
		</div>

		<div class="form-group">
			<label for="password">Password</label> <input type="password" class="form-control" placeholder="Enter password" id="password">
		</div>
				
		<div class="form-group">
			<label for="email">Email</label> <input type="email" class="form-control" placeholder="Enter email" id="email">
		</div>

	</form>
	<button id="btn-save" class="btn btn-primary">회원가입완료</button>
</div>

<script src="/js/user.js"></script>
<%@ include file="../layout/footer.jsp"%>
~~~
## 3. 로그인 화면
- loginForm.jsp

~~~jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>

<%@ include file="../layout/header.jsp"%>

<div class="container">
	<form action="#" method="POST">
		<div class="form-group">
			<label for="username">Username</label> <input type="text"
				class="form-control" name="username" placeholder="Enter username"
				id="username">
		</div>

		<div class="form-group">
			<label for="password">Password</label> <input type="password"
				class="form-control" name="password" placeholder="Enter password"
				id="password">
		</div>
		<div class="form-group form-check">
			<label class="form-check-label"> <input
				class="form-check-input" name="remember" type="checkbox">
				Remember me
			</label>
		</div>
		<button id="btn-login" class="btn btn-primary">로그인</button>
	</form>
</div>

<%@ include file="../layout/footer.jsp"%>
~~~
## 4. 회원 수정 화면

## 5. 글 목록 화면(메인화면)

## 6. 글 상세보기 화면

## 7. 글 수정 화면