**0521 스프링 **

 스프링과 스프링 부트의 차이점:
   
스프링을 더 쉽게 사용할 수 있도록 구성 정보와 설정을 제공하는 것이 스프링 부트이다 

JAVA Spring:

자바를 기반으로 만들어진 애플리케이션 프레임워크를 의미한다. 프레임워크란 뼈대나 근간을 이루는 코드들의 묶음을 의미한다. 
개발에 필요한 구조를 이미 코드로 만들어 놓았기 때문에 필요한 부분은 조립하는 형태로 개발이 가능하고, 이러한 장점으로 프레임워크를 사용한 프로젝트는 일정한 품질을 보장하며 개발시간을 단축시킬 수 있다. 
자바 스프링은 경량 프레임워크이며 기존의 복잡한 구동환경을 벗어나 특정 기능 위주로 간단한 jar파일을 이용하여 개발을 가능하게 구성된 프레임워크 이다.

---------------------------

JAVA Spring의 주요 특징

- POJO(Plain Old Java Object) 기반의 구성
스프링은 객체간의 관계를 구성할 수 있는 특징을 가지고 있다. 일반적인 JAVA코드를 이용하여 객체를 구성하는 방식을 그대로 사용할 수 있다는 것이다. 따라서 개발자가 가장 일반적인 형태로 코드를 작성하고 실행 가능하여 유연하게 작업할 수 있다고 한다.

- 의존성 주입을 통한 관계 구성
  
의존성 주입이란 어떠한 객체가 필요한 객체를 외부에서 밀어넣는 방식을 말한다. 스프링에서 ApplicationContext라는 존재가 이러한 역할을 수행한다.  스프링에서 ApplicationContext가 관리하는 객체를 빈(Bean)이라고 부르며 빈과 빈사이의 의존관계를 처리하는 방식으로 XML설정, @어노테이션설정 등을 이용한다.

---------------------------

@어노태이션 종류 중 4가지

@Configuration: 이 어노테이션이 있는 클래스는 빈 설정을 담당하는 클래스가 된다. 이 클래스 안에서 @Bean 어노테이션이 동봉된 메소드를 선언하면, 그 메소드를 통해 스프링 빈을 정의하고 생명주기를 설정하게 된다.

@Autowired: 정말 자주 쓰는거 같다. 뭘지 궁금했는데 찾아보니 필드, 메소드, 생성자에다가 넣을 수 있고 스프링 빈을 가져오는 가장 기본적인 방법이라고 한다.

@Value:  사진 전송 구현할 때 사용하였는데 생성자, setter 따위의 메소드, 필드 등에다가 스프링에서 설정한 값을 주입할 수 있다. 주로application.properties 및 스프링이나 자바 property 값을 가져올 때 쓴다. 

@CrossOrigin: CORS 보안상의 문제로 브라우저에서 리소스를 현재 origin에서 다른 곳으로의 AJAX요청을 방지하는 것이다.

------------------------------------

CORS(Cross-Origin Resource Sharing):

CORS는 영어 그대로 교차 출처를 공유할 수 있는 권한을 부여하도록 브라우저에 알려주는 정책이다. 서로 다른 출처를 가진 Application이 서로의 Resource 에 접근할 수 있도록 해준다.

------------------------------------

Origin:

기본적으로 프로토콜, 호스트, 포트 를 통틀어서 Origin(출처) 라고 한다. 
즉 서로 같은 출처는 이 셋이 동일한 출처를 말하고, 여기서 하나라도 다르다면 Cross Origin, 즉 교차출처가 되는 것이다.
* http://localhost:8080 : Spring Boot
* http://localhost:3000 : React
보안상의 이유로, 브라우저는 스크립트에서 시작한 Cross Origin HTTP Request를 제한한다. 즉, SOP(Same Origin Policy)를 따른다.
React와 Spring Boot의 port 가 서로 다르기 때문에 cors정책 위반 에러가 나왔던 것이다. 이건 따로 코드 추가를 하여 수정할 수 있다.

-------------------------------------------------------------------------------------------------------------

**240522 스프링**

AOP (Aspect-Oriented-Programming)지원:

AOP방식은 관점 지향 프로그래밍이다. 이는 횡단 관심사(반드시 처리가 필요한 부분)을 모듈로 분리하는 프로그래밍 패러다임이다. 이를 통해 개발자는 핵심 비지니스 로직에 집중하여 코드를 개발할 수 있고, 각 프로젝트마다 다른 관심사를 적용할때 코드 수정을 최소화할 수 있으며 원하는 관심사의 유지보수가 수월한 코드를 구성할 수 있다고 한다.

- 중심적 관심사: 실현해야 할 기능을 나타내는 프로그램
- 횡단적 관심사: 본질적인 기능은 아니지만 품질이나 유지보수 등의 관점에서 반드시 필요한 기능을 나타내는 프로그램
  
-------------------------------------------------------------------------------------
AOP 고유 용어

어드바이스 (Advice): 횡단적 관심사의 구현(메서드), 로그 출력 및 트랜잭션 제어 등 이다.

애스펙트 (Aspect) : 어드바이스를 정리한 것(클래스)이다.

조인포인트 (JoinPoint) : 어드바이스를 중심적인 관심사에 적용하는 타이밍, 메서드(생성자) 실행 전, 메서드(생성자) 실행 후 등 실행되는 타이밍이다.

포인트컷 (Ponitcut) : 어드바이스를 삽입할 수 있는 위치, 예를 들어, 메서드 이름이 get으로 시작할 때만 처리하는 조건을 정의할 수 있다.

인터셉터 (Interceptor): 처리의 제어를 인터셉트하기 위한 구조 또는 프로그램이다. 스프링 프레임워크에서는 인터셉트라는 매커니즘으로 어드바이스를 중심 관심사에 추가한 것처럼 보이게 한다.

타깃 (Target): 어드바이스가 도입되는 대상을 말한다.

-----------------------------------------
어드바이스 종류

- Before Advice : 중심적 관심사가 실행되기 이전에 횡단적 관심사를 실행 => @Before

- After Returning Advice : 중심적 관심사가 정상적으로 종료된 후에 횡단적 관심사를 실행 => @AfterReturning

- After Throwing Advice : 중심적 관심사로부터 예외가 던져진 후로 횡단적 관심사를 실행 => @AfterTrowing

- After Advice : 중심적 관심사의 실행 후에 횡단적 관심사를 실핼 (정상 종료나 예외 종료 등의 결과와 상관없이 실행) => @After

- Around Advice : 중앙적 관심사 호출 전후에 횡단적 관심사를 실행 => @Around

---------------------------------------------------------------

레이어별로 사용할 인스턴스 생성 어노테이션          도메인 주도 설계의 라이어

- 애플리케이션 레이어 (Application Layer) : 애플리케이션 레이어는 클라이언트와의 데이터 입출력을 제어하는 레이어이다.
  
- 도메인 레이어 (Domain Layer) : 도메인 레이어는 애플리케이션의 중심이 되는 레이어로서 업무 처리를 수행하는 레이어이다.

- 인프라스트럭처 레이어 (Intrastructure Layer) : 인프라스트럭처 레이어는 데이터베이스에 대한 데이터 영속성 등을 담당하는 레이어이다.
  
=> 인스턴스 생성 어노테이션은 레이어별로 구분할 수 있다.
@Controller : 애플리케이션 레이어의 컨트롤러에 부여

@Service : 도메인 레이어의 업무 처리에 부여

@Repository : 인프라 레이어의 데이터베이스 엑세스 처리에 부여

@Component : @Controller, @Service, @Repository의 용도 이외의 인스턴스 생성 대상 클래스에 부여

---------------------------------------------
**0523 스프링 CORS**

다시 기억하는 부분

CORS(Cross-Origin Resource Sharing):

CORS는 영어 그대로 교차 출처를 공유할 수 있는 권한을 부여하도록 브라우저에 알려주는 정책이다. 서로 다른 출처를 가진 Application이 서로의 Resource 에 접근할 수 있도록 해준다.


Origin:

기본적으로 프로토콜, 호스트, 포트 를 통틀어서 Origin(출처) 라고 한다. 즉 서로 같은 출처는 이 셋이 동일한 출처를 말하고, 여기서 하나라도 다르다면 Cross Origin, 즉 교차출처가 되는 것이다.

---------------------------------------------

동일 출저 정책 (Same-Origin Policy)

SOP 정책은 단어 그대로 동일한 출처에 대한 정책을 말한다. 그리고 SOP 정책은 동일한 출처에서만 리소스를 공유할 수 있다 라는 것을 가지고 있다. 

Server A: A_image. png 

Server B: B_image. png

www.serverA.com에서 A_image. png는 가져올 수 있지만 B_image. png는 가져올 수 없다.

---------------------------------------------------------------------
동일 출처 정책이 필요한 이유

동일 출처가 아닌 경우 접근을 차단하는 이유는 출처가 다른 두 어플리케이션이 자유로이 소통할 수 있는 환경은 꽤 위험한 환경이다. 만약 제약이 없다면, 해커가 CSRF나 XXS 등의 방법을 이용하여 우리가 만든 어플리케이션에서 해커가 심어놓은 코드가 실행되어 개인 정보를 가로챌 수 있다.

-----------------------------------
출처 비교와 차단

이것은 브라우저에서 한다. 출처를 비교하는 로직은 서버에 구현된 스펙이 아닌 브라우저에 구현되니 스펙에서 진행한다.

--------------------------------------------------------------------------------------

