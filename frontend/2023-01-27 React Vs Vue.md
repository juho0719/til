
---
title: React Vs Vue
date: 2023-01-27 22:35:00 +09:00
categories: [Blog, Frontend, SPA]
tags: [frontend spa]
---


# Why use frontend library(framework)?

- 사실  `React`나 `Vue`가 하는 일을 바닐라 JS로 모두 구현 가능
- 데이터 변화를 화면에 쉽게 적용
- 컴포넌트, 모듈화 하기 쉬움
- 소규모 개발에서는 티가 잘 안나지만, 관리 데이터가 많아지고 대규모 프로젝트로 갈수록 생산성도 좋고 편하게 개발할 수 있음


# React

- UI 자바스크립트 라이브리러
- SPA의 UI를 생성하는데 집중하는 라이브러리
- `JSX(Javascript XML)` 문법과 `단방향 데이터 바인딩(One-way Data Binding)`를 사용
- `가상 돔(Virtual DOM)`을 이용하여 퍼포먼스 최적화
- 선언적(Declarative), 컴포넌트 기반(Component-based), 한번 배워서 어디에서나 사용(Learn once, Write anyware)


# Vue

- UI 라이브러이이자 프레임워크
- UI 생성 기능뿐만 아니라 라우터, 상태관리, 테스팅등을 쉽게 결합할 수 있도록 제공
- `가상 돔(Virtual Dom)`으로 화면 요소를 변경 및 조작하고 최종 결과물을 `DOM Tree`에 반영
- 점진적인 프레임워크(progressive framework)
- 단방향, 양방향 모두 지원
- 상대적으로 배우기 쉬워 러닝커브가 낮은 편


# 차이점

- 서로 다른 데이터 흐름 라이브러리
	- `React`는 `Redux`, `Vue`는 `Vuex`
- 상태 관리
	- `React`는 `Redux`를 통해 가능, `Vue`는 자체 도구 제공
- `React`는 라이브러리, `Vue`는 프레임워크
- `Vue`는 `SFC(Simple File Component)`로 구성, `React`는 `JSX` 사용


# 결론

- 초보자가 접근하기에는 `Vue`가 유리
	- 기능이 잘 구성되어 있고, 문법이 상대적으로 쉬운편
- 커스터마이징 및 자유도가 높은 것을 원하면 `React`가 유리
