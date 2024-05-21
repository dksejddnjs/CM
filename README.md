**0521 스프링 **

 스프링과 스프링 부트의 차이점:
   
스프링을 더 쉽게 사용할 수 있도록 구성 정보와 설정을 제공하는 것이 스프링 부트이다 

JAVA Spring:

자바를 기반으로 만들어진 애플리케이션 프레임워크를 의미한다. 프레임워크란 뼈대나 근간을 이루는 코드들의 묶음을 의미한다. 
개발에 필요한 구조를 이미 코드로 만들어 놓았기 때문에 필요한 부분은 조립하는 형태로 개발이 가능하고, 이러한 장점으로 프레임워크를 사용한 프로젝트는 일정한 품질을 보장하며 개발시간을 단축시킬 수 있다. 
자바 스프링은 경량 프레임워크이며 기존의 복잡한 구동환경을 벗어나 특정 기능 위주로 간단한 jar파일을 이용하여 개발을 가능하게 구성된 프레임워크 이다.

JAVA Spring의 주요 특징

- POJO(Plain Old Java Object) 기반의 구성
스프링은 객체간의 관계를 구성할 수 있는 특징을 가지고 있다. 일반적인 JAVA코드를 이용하여 객체를 구성하는 방식을 그대로 사용할 수 있다는 것이다. 따라서 개발자가 가장 일반적인 형태로 코드를 작성하고 실행 가능하여 유연하게 작업할 수 있다고 한다.

- 의존성 주입을 통한 관계 구성
  
의존성 주입이란 어떠한 객체가 필요한 객체를 외부에서 밀어넣는 방식을 말한다. 스프링에서 ApplicationContext라는 존재가 이러한 역할을 수행한다.  스프링에서 ApplicationContext가 관리하는 객체를 빈(Bean)이라고 부르며 빈과 빈사이의 의존관계를 처리하는 방식으로 XML설정, @어노테이션설정 등을 이용한다.

@어노태이션 종류 중 4가지

@Configuration: 이 어노테이션이 있는 클래스는 빈 설정을 담당하는 클래스가 된다. 이 클래스 안에서 @Bean 어노테이션이 동봉된 메소드를 선언하면, 그 메소드를 통해 스프링 빈을 정의하고 생명주기를 설정하게 된다.

@Autowired: 정말 자주 쓰는거 같다. 뭘지 궁금했는데 찾아보니 필드, 메소드, 생성자에다가 넣을 수 있고 스프링 빈을 가져오는 가장 기본적인 방법이라고 한다.

@Value:  사진 전송 구현할 때 사용하였는데 생성자, setter 따위의 메소드, 필드 등에다가 스프링에서 설정한 값을 주입할 수 있다. 주로application.properties 및 스프링이나 자바 property 값을 가져올 때 쓴다. 

@CrossOrigin
@CrossOrigin: CORS 보안상의 문제로 브라우저에서 리소스를 현재 origin에서 다른 곳으로의 AJAX요청을 방지하는 것이다.

AOP (Aspect-Oriented-Programming)지원:

AOP방식은 관점 지향 프로그래밍이다. 이는 횡단 관심사(반드시 처리가 필요한 부분)을 모듈로 분리하는 프로그래밍 패러다임이다. 이를 통해 개발자는 핵심 비지니스 로직에 집중하여 코드를 개발할 수 있고, 각 프로젝트마다 다른 관심사를 적용할때 코드 수정을 최소화할 수 있으며 원하는 관심사의 유지보수가 수월한 코드를 구성할 수 있다고 한다.

CORS(Cross-Origin Resource Sharing):

CORS는 영어 그대로 교차 출처를 공유할 수 있는 권한을 부여하도록 브라우저에 알려주는 정책이다. 서로 다른 출처를 가진 Application이 서로의 Resource 에 접근할 수 있도록 해준다.

Origin:

기본적으로 프로토콜, 호스트, 포트 를 통틀어서 Origin(출처) 라고 한다. 
즉 서로 같은 출처는 이 셋이 동일한 출처를 말하고, 여기서 하나라도 다르다면 Cross Origin, 즉 교차출처가 되는 것이다.
* http://localhost:8080 : Spring Boot
* http://localhost:3000 : React
보안상의 이유로, 브라우저는 스크립트에서 시작한 Cross Origin HTTP Request를 제한한다. 즉, SOP(Same Origin Policy)를 따른다.
React와 Spring Boot의 port 가 서로 다르기 때문에 cors정책 위반 에러가 나왔던 것이다. 이건 따로 코드 추가를 하여 수정할 수 있다.
