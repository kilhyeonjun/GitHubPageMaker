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

import java.sql.Timestamp;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

import org.hibernate.annotations.ColumnDefault;
import org.hibernate.annotations.CreationTimestamp;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;


@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder // 빌더 패턴
@Entity // User 클래스가 Mysql에 테이블이 생성된다 // ORM -> 언어(ex) JAVA)  Object -> 테이블로 매핑해주는 기술
public class User {
	
		@Id //Primary key
		@GeneratedValue(strategy = GenerationType.IDENTITY) // 프로젝트에서 연결된 DB의 넘버링 전략을 따라간다.
		private int id; // 시퀀스(오라클), auto_increment(mysql)
		
		@Column(nullable =false, length = 30)
		private String username; //아이디
		
		@Column(nullable =false, length = 100) // 해쉬 (비밀번호 암호화)
		private String password;
		
		@Column(nullable =false, length = 50)
		private String email;

		@ColumnDefault("'user'")
		private String role;  // Enum을 쓰는게 좋다. // admin, user, manager
	
		@CreationTimestamp // 시간이 자동 입력
		private Timestamp createDate;
}
~~~
- Board.java
~~~java
package com.kbox.blog.model;

import java.sql.Timestamp;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.Lob;
import javax.persistence.ManyToOne;

import org.hibernate.annotations.ColumnDefault;
import org.hibernate.annotations.CreationTimestamp;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;


@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
public class Board {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY) //auto_increment
	private int id;
	
	@Column(nullable = false, length = 100)
	private String title;
	
	@Lob // 대용량 데이터
	private String content; // 섬머노트 라이브러리 사용 예정, html태그가 섞여서 디자인 됨.
	
	@ColumnDefault("0")
	private int count; //조회수
	
	@ManyToOne  // Many = Board, User = one
	@JoinColumn(name="userId")
	private User user; // DB는 오브젝트를 저장할 수 없다(그래서 FK 사용), but 자바는 오브젝트를 저장할 수 있다.
	
	@CreationTimestamp
	private Timestamp createDate;
}
~~~
- Reply.java
~~~java
package com.kbox.blog.model;

import java.sql.Timestamp;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.OneToMany;

import org.hibernate.annotations.ColumnDefault;
import org.hibernate.annotations.CreationTimestamp;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Builder
@AllArgsConstructor
@NoArgsConstructor
@Data
@Entity
public class Reply {
	@Id //Primary key
	@GeneratedValue(strategy = GenerationType.IDENTITY) // 프로젝트에서 연결된 DB의 넘버링 전략을 따라간다.
	private int id; // 시퀀스(오라클), auto_increment(mysql)
	
	@Column(nullable = false, length = 200)
	private String content;
	
	@ManyToOne
	@JoinColumn(name="boardId")
	private Board board;
	
	@ManyToOne
	@JoinColumn(name="userId")
	private User user;
	
	@CreationTimestamp
	private Timestamp creatDate;
}
~~~
