---
layout: post
current: post
cover: assets/built/images/java/springboot/spring-logo.png
navigation: True
class: post-template
subclass: 'post tag-java'
author: kilhyeonjun
title: "springboot 블로그 8.테이블 생성하기" 
comments: true
categories:
- java
tags:
- springboot
- blog
date: "2021-09-17 00:45"
---
{% include springboot/springboot-blog-table-of-contents.html %}
해당 게시물은 이 [강의](https://edu.goorm.io/lecture/24605/스프링부트-나만의-블로그-만들기)를 보고 제작하게되었습니다.

# 테이블 생성하기

## 1. Blog 테이블 만들기 (User, Board, Reply)
- User.java
~~~java
package com.kbox.blog.model;

import java.security.Timestamp;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity // User 클래스가 Mysql에 테이블이 생성된다
public class User {
	
		@Id //Primary key
		@GeneratedValue(strategy = GenerationType.IDENTITY) // 프로젝트에서 연결된 DB의 넘버링 전략을 따라간다.
		private int id; // 시퀀스(오라클), auto_increment(mysql)
		private String username; //아이디
		private String password;
		private String email;
		private Timestamp createDate;
}
~~~
- Board.java
~~~java
~~~
- Reply.java
~~~java
~~~
