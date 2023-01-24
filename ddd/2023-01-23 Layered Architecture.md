
---
title: Layered Architecture
date: 2023-01-23 12:50:00 +09:00
categories: [Blog, Architecture, DDD]
tags: [architecture java ddd]
---


# Layered Architecture?

- `관심사 분리(Separation of Concern)`에 따라 시스템을 유사한 관심사로 레이어를 분해하고 각각의 레이어가 하위 레이어에만 의존하도록 구성하는 아키텍처
- `Layer Architecture`의 목적은 각 레이어들이 특정 관심사에 관련된 항목만 포함하도록 만들어 시스템의 결합도를 낮추고 재사용성, 유지보수성을 향상시키는 것
- 구성되는 계층의 숫자에 따라 N-계층 아키텍처라고도 함 (`N-tier Architecture`)
	- 4-Tier Architecture
		- Presentation Layer
		- Business Layer
		- Persistence Layer
		- Database Layer
- 높은 유지보수성과 쉬운 테스트는 장점


## 4-Tier Architecture
 
```mermaid
graph
	subgraph 4-Tier Architecture
		subgraph Presentation Layer
			id1[Component]
			id2[Component]
			id3[Component]
		end
		subgraph Business Layer
			id4[Component]
			id5[Component]
			id6[Component]
		end
		subgraph Persistence Layer
			id7[Component]
			id8[Component]
			id9[Component]
		end
		subgraph Database Layer
			id10[(DB)]
			id11[(DB)]
			id12[(DB)]
		end
	end
```

### Presentation Layer
- 사용자와의 상호 작용으로 UI에 데이터를 표시하고 서버와의 통신을 처리
- `Client`의 요청 처리 방법에는 관심이 없고, 요청을 어떻게 하고 어떻게 받을지에 대해 관심이 있는 계층
- 스프링에서는 `Controller`나 `View`, 화면에서는 `HTML`, `CSS`, `JavaScript`가 여기에 해당

### Business Layer
- 비즈니스 로직 수행
- `Client`요청에 대한 처리를 담당
- 스프링에서는 `Service`가 여기에 해당

### Persistence Layer
- 영속성 구현을 위한 계층
- `Business Layer`의 요청에 따라 데이터베이스에서 데이터를 조회, 저장, 삭제등 로직을 수행
- 스프링에서는 `Repository`가 여기에 해당

### Database Layer
- 데이터베이스가 위치한 계층


## Layers of Isolation
- 각각의 나눠진 수평 계층은 수직적으로 배치
- 특정 레이어는 바로 하위 레이어에만 연결


## Anti Pattern
- 싱크홀 안티패턴
	- 특정 레이어가 아무런 로직도 수행하지 않고 들어온 요청 그대로 다시 하위 레이어로 보내는 경우
	- 불필요한 리소스 낭비 초래
	- 전체 흐름 중 약 20%가 싱크홀이라면 나쁘지 않은 수준



## DDD(Domain Driven Design)에서 말하는 Layered Architecture

- `DDD(Domain Driven Design)`에서는 주로 아래의 4가지로 분류
	- `Presentation Layer`
	- `Application Layer`
	- `Domain Layer`
	- `Infrastructure Layer`
- 높은 유지보수성과 쉬운 테스트는 장점

#### Presentation Layer
- HTTP 요청과 같은 사용자 요청을 처리하는 `View`나 `Controller`가 여기에 해당

#### Application Layer
- 애플리케이션의 흐름 제어 목적
- 단일 객체 서비스가 아닌 단일 책임을 가지는 여러 개의 서비스로 구성하는게 좋음
- 높은 응집도, 낮은 결합도를 갖도록 설계

#### Domain Layer
- 해당 레이어는 고수준의 비즈니스 논리를 해결하고, 저수준의 기술 구현 및 외부 인프라에 의존하지 않도록 구성
- 해당 레이어를 추상적으로 설계할수록 단위 테스트 및 리팩토링 난이도가 쉬워짐

#### Infrastructure Layer
- Data Access나 Ioc 컨테이너등 상위 계층을 지원하는 역할
	- 환경 구성(Spring IoC Container)
	- 보안(Spring Security)
	- Data Access(Hibernate, Mybatis)
	- 메시지 큐(Rabbit MQ, Kafka, SQS)
	- 메일


#### 해당 아키텍처를 적용하면 좋은 경우
- 프로젝트 도메인이 복잡하지 않은 경우
- 확장성보다는 일관성을 가져가는 게 목표인 경우
- 소규모 팀인 경우

#### 단점
- 프로젝트가 커질수록 확장성이 떨어짐
- 추후 각 레이어들로 분리된 관심사외 다른 관심사가 발견된 경우 패키지 분리 및 코드 배치가 어려움
- 성능적인 이점을 갖긴 어려움