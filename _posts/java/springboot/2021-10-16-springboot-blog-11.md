---
layout: post
current: post
cover: assets/built/images/java/springboot/spring-logo.png
navigation: True
class: post-template
subclass: 'post tag-java'
author: kilhyeonjun
title: "springboot 블로그 11.시큐리티 코드 짜기" 
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

# 시큐리티 코드 짜기

## 1. 주소세팅
- /   (컨텍스트 삭제)
- /auth/joinProc
- /auth/loginProc
- /auth/joinForm
- /auth/loginForm
- header.jsp
- joinForm.jsp
- user.js
- UserApiController.java
- UserController.java

## 2. 로그인 페이지 커스터마이징
- SecurityConfig.java
~~~java
package com.kbox.blog.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.annotation.web.servlet.configuration.EnableWebMvcSecurity;


@Configuration // 빈 등록 (객체 생성)
@EnableWebSecurity // 필터 체인에 등록 (스프링 시큐리티 활성화)
@EnableGlobalMethodSecurity(prePostEnabled=true) // 특정 주소 접근시 권한 및 인증을 pre(미리) 체크하겠다.
public class SecurityConfig extends WebSecurityConfigurerAdapter{
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests()
        .antMatchers("/auth/loginForm", "/auth/joinForm")
        .permitAll()
        .anyRequest().authenticated()
        .and()
        .formLogin().loginPage("/auth/loginForm");   
    }
}
~~~
![img](./assets/built/images/java/springboot/annotation.png)

- SecurityConfig.java 수정
  - 로그인 성공, 로그아웃 성공 URL 커스터마이징
~~~java
package com.kbox.blog.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

import com.cos.blog.config.auth.PrincipalDetailService;


@Configuration // 빈 등록 (객체 생성)
@EnableWebSecurity // 필터 체인에 등록 (스프링 시큐리티 활성화)
@EnableGlobalMethodSecurity(prePostEnabled=true) // 특정 주소 접근시 권한 및 인증을 pre(미리) 체크하겠다.
public class SecurityConfig extends WebSecurityConfigurerAdapter{
	
	@Autowired
	private PrincipalDetailService principalDetailService;
	
	// 1. Bean 어노테이션은 메서드에 붙여서 객체 생성시 사용
	@Bean
	public BCryptPasswordEncoder encodePWD() {
		return new BCryptPasswordEncoder();
	}
	
	// 2. 시큐리티가 로그인할 때 어떤 암호화로 인코딩해서 비번을 비교할지 알려줘야 함.
	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		auth.userDetailsService(principalDetailService).passwordEncoder(encodePWD());
	}
	
	// 3. 필터링
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http
        .csrf().disable()
	    .authorizeRequests()
	    .antMatchers("/", "/css/**", "/images/**", "/js/**", "/auth/**")
	    .permitAll()
	    .anyRequest().authenticated()
	    .and()
	    .formLogin().loginPage("/auth/loginForm")
	    .loginProcessingUrl("/auth/loginProc")
	    .defaultSuccessUrl("/");
	}
	
	// 참고 : .headers().frameOptions().disable() // 아이프레임 접근 막기
	// 참고 : .csrf().disable() // csrf 토큰 비활성화 (테스트시 걸어주는 것이 좋음)
}
~~~