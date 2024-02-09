# 📌 1차. 객체지향과 SOLID

# 객체지향 패러다임; 협력, 책임, 역할

## 객체지향 세계의 특징

1) 객체도 사회적 동물이다
    - 객체지향 세계에서는 공동체를 만들어 서로 상호작용하며 자신의 존재여부를 증명한다.
    - 여러 객체가 독립적인 존재가 아니라 공동체를 만들면 기능에 대한 책임을 나누고 목표를 이루는데 참여한다.
2) 객체지향 세계에서 객체는 자아를 가지는 로봇이 된다.
    - 실제 세계에서의 수동적인 존재가 능동적인 존재가 된다
    - Theater 객체는 종업원 없이도 혼자서 티켓을 확인하고 관람을 입장시킨다.
3) 객체지향 세계에서는 독점은 없다
    - 목표를 달성하기 위해 필요한 기능을 쪼개서 적절한 객체에 분배하고 객체들 간의 적절한 협력을 만들어 주는 것이 목표다

## 협력

- Application의 주요 기능을 구현하기 위해서 **작은 단위로 쪼개진 기능**을 설명하는 한 줄 요약본
- 협력은 객체 간의 상호작용을 통해서 이루어짐
- 객체 간의 상호작용은 메시지를 통해서만 가능함

### 협력: 작은 단위로 쪼개진 기능

- 음료 주문 시스템
    - 주문 받기
    - 주문 결제 금액 계산
    - 커피 제조
    - 커피 전달

## 책임

- 협력에 참여하기 위해서 **객체가 책임지고 있는 전체 기능 정의서**

### 책임: 협력에 참여하기 위해 필요한 세부 기능 목록

- 음료 주문 시스템
    - 주문 받기
        - 주문한 메뉴와 수량 확인
        - 주문 고객정보 등록
        - 커피 제조 요청
    - 주문 결제 금액 계산
        - 커피 판매금액 계산
        - 통신사 할인금액 계산
        - 멤버십 할인금액 계산
    - 커피 제조
        - 적절한 커피 제조
    - 커피 전달
        - 주문한 커피의 고객정보 확인
        - 전달

## 역할

- 동일한 책임을 수행하는 객체의 집합체
- 협력을 위한 책임 수행 중 겹치는 일이 있을 경우 협력을 개별적으로 만들지 않는다.
- 동일한 책임을 수행하는 객체들을 대표할 특별할 역할을 부여하고 그것을 슬롯 형태로 관리 → 동적으로 적절한 객체로 결합

### 역할: 동일한 책임을 수행하는 객체 목록

- 음료 주문 시스템
    - 주문 받기
    - 주문 결제 금액 계산
        - 커피 판매금액 계산
        - 통신사 할인금액 계산 → *TelecomeDiscountPolicy*
        - 멤버십 할인금액 계산 → *MembershipDiscountPolicy*
    - 커피 제조
        - 적절한 커피 제조 → *CoffeeMaker*
        - 적절한 티 제조 → *TeaMaker*
    - 커피 전달
- *TelecomeDiscountPolicy*와 *MembershipDiscountPolicy*는 isSatisfied, calculateDiscountAmount와 같은 메서드를 공유 가능
- *CoffeeMaker*와 *TeaMaker*는 getRecipe와 make과 같은 메서드를 공유 가능

# 의존성; 객체의 영원한 친구

## 의존성이란?

- 객체 A가 객체 B에게 메시지를 보낼 때 (상호작용을 할 때) 객체 B가 객체 A에 대해 알고 있는 정보를 의존성이라고 함
- 알고 있는 정보의 수준에 따라 **높은 의존성(강한 결합도), 낮은 의존성 (느슨한 결합도)**라고 표현함

## 강한 결합도, 낮은 결합도

- 결합도: 객체 B에 변경이 발생하는 경우 객체 B에 의존하고 있는 다른 객체들에게 전이되는 정도의 수준
- 강한 결합도를 가지는 구조는 구조의 변경의 여파가 크고, 모든 객체에게 전파되기에 느슨한 결합도로 바꾸는 과정이 필요
- 느슨한 결합도를 유지하기 위해 필요한 목표
    - 올바른 의존성 주입
    - 변경을 방해하는 의존성을 제거

## 올바른 의존성 관계 형성

### Constructor 함수를 통한 의존 관계 형성

- 디폴트 방식 (스프링 팀에서 공식적으로 권고하는 방식)
- 의존하는 객체의 전체적인 행동에 대해 지속적으로 의존 객체에게 메시지를 보내야 하는 경우에 사용하는 것이 좋음

### Parameter of Method를 통한 의존 관계 형성

- 생성자 방식과 반대로 메서드가 실행되는 동안에만 일시적으로 의존 관계를 맺어도 무방할 때 사용
- 매서드가 실행될 때마다 의존 대상이 매번 달라져야 하는 경우
- Setter 방식과의 차이점
    - 특정 함수(Setter) 호출해서 의존 관계를 맺는 것이 아니라 단순히 인자로 전달하는 점이 다름

### Setter 함수를 통한 의존 관계 형성

- 의존하고 있는 대상을 변경할 수 있는 가능성을 열어두고 싶을 경우
- 유의점
    - 의존 관계를 맺지 않은 상태에서 의존 대상에게 메시지를 요청하게 되면 NPE이 발생하여 설계 시 주의
    - Setter를 통해 정의되지 않은 상태(해당 객체가 아직 null)에서 호출될 경우

## 변경을 방해하는 의존성 관계

- 변경에 따른 영향이 너무 커서 변경할 수 없는 상황
- 실무에서는 흔히 레거시하다고 표현

### 변경을 방해하는 의존성 주입 방식

- Constructor 함수에서 new 키워드를 사용하여 구현체를 직접 할당하는 방식
    - 구현부에 숨겨져있어 찾기가 힘듦
    - 구현체에 직접적으로 의존하는 것은 변경을 방해하는 시작점
    - 무조건 나쁜 것일까?
        - 표준 클래스 또는 디폴트로 할당이 필요한 경우 생성자 체이닝을 이용해 직접 할당하는 편이 나음

## 응집도와 결합도

### 응집도와 결합도는 비례 혹은 반비례 관계인가?

- 응집도
    - 클래스에 포함된 내부 요소가 하나의 책임과 연관된 정도와 책임을 수행하기 위해 필요한 기능들이 하나의 연관 클래스에 잘 정의되어 있는지를 알려주는 척도
- 응집도에 따라 결합도에는 영향을 줄 수 없는 구조
    - 낮은 응집도와 높은 결합도 예시
        - 만약 하나의 책임이 여러 개의 클래스들로 분산 (낮은 응집도). 분산된 클래스를 의존하는 객체들이 분산된 클래스의 내부 구현까지 알고 있을 경우 (높은 결합도)
    - 높은 응집도와 낮은 결합도 예시
        - 캡슐화를 통한 내부 구현을 숨기는 형태 (높은 응집도 + 낮은 결합도)

![image](https://github.com/sangeun99/wanted-pre-onboarding/assets/63828057/d4b5a12b-4651-4126-ae9b-5ba5ace8056c)

# 상속; 코드 재사용의 함정

## 상속

- 부모 클래스가 가지고 있는 상태와 행동을 자식 클래스가 물려 받아 코드를 재사용할 수 있게 하는 기법

## 상속의 필요성

- 변경에 방해하는 코드의 중복을 제거하기 위함
- 기능 추가 시 기존에 있는 클래스를 이용해 빠르게 구현 가능

## 상속을 통해 코드 재사용 시의 문제

- 1) 캡슐화 규칙을 위반해야 한다
    - 상속을 통해 부모 클래스의 행동 + 상태까지 상속받음
    - 그렇기에 상속을 제대로 활용하기 위해서는 내부 구현을 알아야 함
        - public 인터페이스에 의존하는 것이 아니라 코드에 의존
- 2) 부모 클래스와 자식 클래스 간에 강한 결합이 생긴다
    - 부모 클래스의 내부 구현 변경 시 자식 클래스 또한 변경을 적용해야 함
    - 자식 클래스에서 메서드 오버라이딩 시 예상치 못한 오류를 만날 수 있음
    - 변경의 유연한 설계(객체지향 이론과 SOLID 규칙을 코딩하는 이유) 를 무력하게 만듦
- 3) 변경의 유연함과 규칙이 깨진다
    - OCP 위반
        - 상속 관계는 컴파일 타임에 결정됨
        - 객체지향이 추구하는 방향성은 작은 기능을 조합해 유연하게 문맥을 완성시키는 것임
        - 하지만 상속 관계의 경우 컴파일 타임 이후 런타임 시점에 문맥의 변경이 어려움
    - 불필요한 기능 상속
        - 의도하지 않은 기능을 부모 클래스에게 상속 받아 자식 클래스의 기능 설계와 상이하게 동작할 수 있는 잠재적 오류가 발생할 수 있는 요소가 내포할 수 있는 문제를 가짐

### *. OCP (Open Close Principle: 개방 폐쇄의 원칙)

- 확장에 대해서는 개방되어야 하지만 변경에 대해서는 폐쇄되어야 함
- 애초에 기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계되어야 함
- 참고자료
    - [https://inpa.tistory.com/entry/OOP-💠-아주-쉽게-이해하는-OCP-개방-폐쇄-원칙](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-OCP-%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84-%EC%9B%90%EC%B9%99)

## 상속은 무조건 나쁜 것인가?

- 개념적으로 명확한 is-a 관계에 있는 경우, 상위 클래스가 확장할 목적으로 설계되었고 상속에 대한 설계도가 문서화 및 최신화되어 있다면 사용해도 괜찮다

![image](https://github.com/sangeun99/wanted-pre-onboarding/assets/63828057/6381e3b7-c686-48e3-a391-196c29c2cbff)

# 합성; 유연한 코드로 가는 길

## 합성이란?

- 클래스의 필드를 통해 의존하는 객체를 참조 (has-a 관계)하게 만드는 설계 기법

## 상속과 다른 점

- 상속
    - 기반이 되는 클래스를 중심으로 변경이 필요한 부분들만 코드를 재사용 (오버라이딩)
    - 그렇게 파생 클래스를 만들어 내는 구조
    - 컴파일 시점에 결정됨
- 합성
    - 기반 클래스에 **필요한 여러 기능**을 제공하는 외부 객체를 런타임 시점에 적절하게 선택하여 주입하는 구조
    - 작은 기능들을 조합해서 다양한 기능을 만들어낼 수 있음
    - OCP, DIP

![image](https://github.com/sangeun99/wanted-pre-onboarding/assets/63828057/9b74b4e0-8bd9-49fb-ba10-bd6958fddcc3)

### *. DIP (Dependency Inversion Principle: 의존 역전 원칙)

- 객체에서 어떤 클래스를 참조해서 사용해야 하는 상황이 생긴다면 직접 참조하지 않고 그 대상의 상위 요소로 참조
- 객체들이 서로 정보를 주고 받을 때 추상성이 낮은 클래스보다 추상성이 높은 클래스와 통신하는 약속이 있음
    - 상위의 인터페이스 타입의 객체로 통신하라는 원칙
- 참고자료
    - [https://inpa.tistory.com/entry/OOP-💠-아주-쉽게-이해하는-DIP-의존-역전-원칙](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-DIP-%EC%9D%98%EC%A1%B4-%EC%97%AD%EC%A0%84-%EC%9B%90%EC%B9%99)

# 새로운 문법; 람다(Lambda)

## 람다식

- 클래스 선언 없이 메서드를 정의하고 이것을 값처럼 사용할 수 있는 함수
- 익명 함수를 표현하는 간결한 문법
- 함수를 **일급 객체**로 취급하여 다른 함수의 인자로 전달하거나 함수에서 반환값으로 사용할 수 있음

### 일급 객체의 조건

- 값으로 취급할 수 있어야 함
    - 모든 일급 객체는 변수가 데이터구조에 담을 수 있어야 함
    - 모든 일급 객체는 메서드의 파라미터로 전달할 수 있어야 함
    - 모든 일급 객체는 메서드의 리턴 값으로 사용할 수 있어야 함

## 람다 표현식을 이용하는 이유

- 간결성
    - 복잡한 코드를 간결하게 표현함으로써 간결성과 가독성을 높임
- 익명 클래스 대체
    - 비즈니스 로직과 상관 없는 코드이면서 여러 곳에서 반복적으로 사용되는 코드
        
        ```java
        // 익명 클래스
        new Comparator<Apple>() {
        	public int compare(Apple a1, Apple a2){
        		return a1.getWeight().compareTo(a2.getWeight());
        	}
        }
        
        // 람다 표현식
        (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
        ```
        
- 스트림 API
    - Java 8에서부터 도입된 API로 컬랙션 처리를 간결하게 처리할 수 있도록 다양한 기능을 제공
    - 불변성 적용
        - 데이터 소스로부터 데이터를 읽기만 할 뿐 변경하지 않음

## 함수형 인터페이스; 람다를 제공하기 위한 문법

- 함수형 인터페이스
    - 오직 하나의 추상 메서드를 정의하는 인터페이스
- 함수형 인터페이스 타입을 기대하는 곳에서 람다 표현식 사용 가능

# 설계; 진정한 객체지향.st

## 잘못된 모습

- 체계적인 설계 없이 생각대로 개발한다
- 객체가 아니라 클래스부터 생각한다
- 객체를 독립적인 존재로 바라본다

## 좀 더 나은 설계 방법

- 필요한 객체 찾기
- 클래스 설계
    - 객체들의 윤곽이 잡히면 공통된 행동과 상태를 분류하고 추상화
    - private 내부 구현과 public 인터페이스 구현
- 변경에 유연한 클래스 구현
    - 캡슐화/상속/추상화/인터페이스 적절히 이용