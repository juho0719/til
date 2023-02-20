
# Start
- Jetbrain에서 개발한 언어
- 2011년에 발표, 코틀린섬으로부터 지어졌고 2017년 구글에 의해 안드로이드 공식언어 지정
- 자바 대체 가능한 언어로 다음과 같은 프로그래밍이 가능한 멀티 플랫폼
	- Kotlin/JVM : 가상머신 위에 동작하는 앱 개발
	- Kotlin/JS : 자바스크립트에 의한 풀스택 웹 개발
	- Kotlin/Native : 임베디드, IoT와 같은 프로그램 개발

# Feature
- 정적 타입 지정 언어
	- 모든 프로그램의 구성요소를 컴파일 시점에 알 수 있음. 컴파일러가 타입을 검증
	- 자바와 달리 타입을 선언하지 않아도 됨(타입추론)
- Null 안정성
	- 널이 될 수 있는 타입도 지원
	- 널 값 허용여부를 컴파일 단계에서 검사
- 함수 타입에 대한 지원
	- 함수형 프로그래밍 지원
	- 함수를 통해 불변데이터 구조를 사용, 다중스레드를 사용해도 안정적

# Why?
- 자바 호환성
	- 자바에서 적용하던 것을 모두 코틀린에서 가능
- 실용적
- 간결성
	- getter, setter, 생성자 파라미터 대입로직 기본 제공
	- 람다 표현식 지원
- 안전성
	- JVM을 사용
		- 메모리 안전성 보장
		- 버퍼오버플로우 방지
		- 동적 메모리 할당
	- 타입 자동 추론을 통한 안전성 보장
	- NullPointException으로 인한 프로그램 오류 방지