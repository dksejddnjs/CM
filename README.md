**오류 관련한 모든 것**

**20240516 docker 세팅, 스프링 부트 실행**    
오류: 스프링 부트 실행했을 때 whitelabel Error Page 뜨지만 새로고침 했을 때 사진처럼 오류 발생
<https://github.com/dksejddnjs/CM/blob/main/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202024-05-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.43.30.png>

사진과 상관없는 오류 => 실행했을 때 url 찾기 불가, db 연결 관련 문제

------------------------------------------------------------------------------------------
**20240517 스프링 부트 실행 시 오류 및 DB 설정 관련 오류**
1. Application.propertise에서 spring.jpa.database-platform=…/ spring.jpa.properties.hibernate.dialect=…

   -> 이 명령어는 기본적으로 잡아주는 거라 필요없다는 오류
3. constructor parameter 0: Error creating bean with name 'userRepository' defined in com.example.demo.repository.UserRepository defined in @EnableJpaRepositories declared on JpaRepositoriesRegistrar.EnableJpaRepositoriesConfiguration: Not a managed type: class com.example.demo.entity.UserEntity

   -> entity나 repository의 코드 오류
4. org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Class org.springframework.orm.jpa.vendor.SpringHibernateJpaPersistenceProvider does not implement the requested interface jakarta.persistence.spi.PersistenceProvider

   -> Spring Boot와 Jakarta Persistence API 간의 버전 충돌로 인한 오류/ 애플리케이션이 시작될 때 **`BeanCreationException`**이 발생했음을 알 수 있고 이 문제는 **`entityManagerFactory`** 빈을 생성하는 중에 발생함. 에러 메시지를 보면 **`SpringHibernateJpaPersistenceProvider`** 클래스가 **`jakarta.persistence.spi.PersistenceProvider`** 인터페이스를 구현하지 않는다는 것... 

entityManagerFactory: entity나 hibernate, jakarta.persistence 등 DB 설정 관련

Dependency 디펜던시: 의존성

------------------------------------------------------------------------------------------
**240518  의존성 문제 해결 및 스프링 부트 재실행시 연동된 db안에 테이블 정보가 사라지는 것, chat**

1. 0517의 오류 중  3번째 오류

오류났던 파일의 build.gradle 파일과 수정하여 실행된 build.gradle 파일의 dependency 부분을 비교해본 결과  implementation 'org.hibernate.validator:hibernate-validator' 이게 없었어서 오류가 나지 않았나 싶다. 사무실에 있는 환경과 맥북 환경 모두 동일하고, 오류난 프로젝트를 깃에서 받고 수정하여 ./gradlew clean build를 해본 결과 오류 문제 없이 실행이 된다.

'org.hibernate.validator:hibernate-validator'는 종속성(Dependency) 식별자라고 한다. 이 식별자는 프로젝트에서 Hibernate Validator 라이브러리를 사용하기 위해 지정된다.

1. 스프링 부트 실행 후 postman으로 post, get 테스트 한 것들이 users table 안에 저장된 것을 확인할 수 있는데 스프링 부트를 재실행했을 때 그 정보들이 사라지는 것을 확인하였다. 이 부분은 application.propertise에서 

`spring.jpa.hibernate.ddl-auto=update` 이 부분이 update가 아닌 create으로 되어 있어서 그랬던 것을 알게되었다.

`spring.jpa.hibernate.ddl-auto` 는 Spring Boot에서 Hibernate의 데이터베이스 스키마 자동 생성 및 업데이트 동작을 제어하는 설정 옵션이다. 이 설정을 통해 Hibernate가 애플리케이션의 엔티티 모델과 데이터베이스 스키마 간의 관계를 어떻게 관리할지 정의할 수 있
다.

------------------------------------------------------------------------------------------
**240519 socket 오류**

1. http://localhost:8098/chat 으로 들어가면 404오류와 함께 반환한 chater를 찾지 못 하는 오류가  뜸.

application.properties에서 ⇒

`spring.thymeleaf.prefix=classpath:/templates/`

`spring.thymeleaf.suffix=.html`

이렇게 선언한 부분을  ⇒

`spring.mvc.view.suffix=.html`

원래 templates 안에 chater.html이 있었는데 정적 리소스여서 static 폴더 안에 있어야 했다. 이동시킨 후 /templates/를 지우지 않아서 인지 계속 404 오류가 났었다.

[http://localhost:8080/chat](http://localhost:8080/chat이) 들어가서 개발자용 도구로 js 콘솔 표시로 보면 socket 연결이 됐다고 뜬다 

1. "Handshake failed due to invalid Upgrade header: null"

 brew install nginx;

 nano /usr/local/etc/nginx/nginx.conf 

location / {
proxy_pass [http://localhost:8080](http://localhost:8080/);
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_http_version 1.1;
} <= 이걸로 수정해주기

컨트롤 O, 엔터, 컨트롤 X

nginx -t   

nginx -s reload

------------------------------------------------------------------------------------------
**240520 소켓 로컬 통신**

1. 다른 컴퓨터에서 보낸 메시지가 안보였던 이유

`websocket = new WebSocket("ws://localhost:8080/ws/chat")`  이 부분을 

`websocket = new WebSocket("ws://192.168.101.81:8080/ws/chat")`  ← 이걸로 바꾸니 상대방이 보낸 메시지들도 `payload` log로 잘 뜨는걸 확인함

2. 405 오류, 클라이언트가 해당 경로에 POST 요청을 보내면 서버는 해당 요청을 처리할 수 없으며 "Method Not Allowed" 오류가 발생
   
  "status": 405,
  
  "error": "Method Not Allowed"
  
  "message": "Method 'POST' is not supported.",
  
  "path": "/chat"

  -----------------------------------------------------------------------------------
  **240521 사진 전송 구현**

파일 선택해서 업로드 했을 때 파일은 보이지 않고 업로드 완료라는 창만 뜨게됨.

또한 내가 보냈을 때는 그 창이 보이지만 다른 컴퓨터에서 보냈을 때는 그 창이 보이지 않음

그래서 수정했더니

POST <http://192.168.101.81:8080/api/image> 405 (Method Not Allowed) 오류가 뜬다.

image controller에서 @PostMapping 등등 Post 관련으로 처리 했음에도 저 오류가 뜨고 있다.

-----------------------------------------------------------------------------------------
**240522 파일 전송**

1. POST <http://192.168.101.81:8080/api/image> 405 (Method Not Allowed) 오류가 뜨는데 PostMapping을 했음에도 불구하고 계속 떠서 코드를 다시 구현했다.
->  사용자가 파일을 업로드하면, 웹 애플리케이션은 해당 파일을 서버에 저장하고 파일이 성공적으로 업로드되면 그 파일에 대한 고유한 URL을 생성하게 함. 그 후 js 콘솔창을 띄우면 그 URL이 나오고 그걸 복사해서 상대방에게 보내어 파일을 다운로드할 수 있게 수정해줌

   <https://github.com/dksejddnjs/CM/commit/c9c41afd55f1bebee55ee3eb6a89cc3cee60f94d>

2. 상대방은 내 url로 다운이 되지만 상대방이 보낸 url은 내가 사용하지 못 함

1. 사진 안뜨는 오류 및 다운로드 창에서 안보이는 오류가 있었다.

사진이 안뜨는 이유는 서버 저장이 안된 것 같다. demo1 8 프로젝트가 아닌 finder 다운로드에 이미지가 저장되었다.

 uploads 폴더로 사진을 받으려고 설정했다.
`file.upload-dir=/Users/user/Downloads/demo1 8 <- uploads 경로`

파일은 uploads 안에서 받아야 하기에
`file.upload-dir=/Users/user/Downloads/demo1 8/uploads <- 수정했다가`
`file.upload-dir=/Users/user/Downloads/demo1 8/uploads/ <- 한 번 더 수정함`

현재로서 파일은 →
<https://github.com/dksejddnjs/CM/blob/main/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202024-05-22%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.05.48.png>

이미지는 →
<https://github.com/dksejddnjs/CM/blob/main/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202024-05-22%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.06.40.png>


이 상태로 나올 수 있게 수정하였다. 그러나 파일 url을 반환하여 채팅창에 올라간거라 저 상태는 상대방에게 보이지 않는다. 채팅창에 올라온 url을 복사하여 보내야 상대방이 url을 볼 수 있고 url 눌렀을 때 다운로드 창에서 다운로드 되는걸 볼 수 있다. 

저 url은 파일 다운로드를 권하는 url이라 눌렀을 때 페이지로 넘어가지 않고 다운로드만 되고 있다.

-----------------------------------------------------------------------------------------
**0523 소켓 이미지, 파일 전송**

1. url이 한 번에 안보내지는 것을 해결하기 위해서

`function appendImageURLToChat(imageUrl){}`

`function appendLinkToChat(linkUrl){}`  선언한 function에 ⇒

`websocket.send(username + ":" + imageUrl);`/ `websocket.send(username + ":" + linkUrl);` 로 수정해주었다. 그랬더니 바로 상대방이 볼 수 있게 되었다.

----------
코드 이해

1. WebSocket 연결:

사용자가 로그인하면 connectWebSocket() 함수가 호출. 이 함수는 서버에 WebSocket 연결을 설정.

function connectWebSocket() {

    websocket = new WebSocket("ws://192.168.101.81:8080/ws/chat");


    websocket.onmessage = onMessage;
    
    websocket.onopen = onOpen;
    
    websocket.onclose = onClose;
}

2. 메시지 전송:

사용자가 메시지를 입력하고 전송 버튼을 클릭하면 send() 함수가 호출. 이 함수는 WebSocket을 통해 서버로 메시지를 전송.

function send(message) { let msg = message || $("#msg").val(); websocket.send(username + ":" + msg); $("#msg").val(''); }

3. 메시지 수신:

서버에서 메시지를 받으면 onMessage() 함수가 호출. 이 함수는 메시지를 파싱하고, HTML을 생성하여 #msgArea에 추가.

function onMessage(msg) { var data = msg.data; var arr = data.split(":"); varsessionId = arr[0]; var message = arr.slice(1).join(":"); var str = "<div class='col-6'>"; 

if(sessionId == username){ str += "<div class='alert alert-secondary'>"; } else { str += "<div class='alert alert-warning'>"; } str += "<b>"+ sessionId + " : " + 

formatMessage(message) + "</b>"; str += "</div></div>"; $("#msgArea").append(str); }

4. 사용자 상태 알림:

WebSocket 연결이 열리거나 닫힐 때, 사용자 입장 및 퇴장 메시지가 전송.

function onOpen(evt) { var str = username + ": 님이 입장하셨습니다."; websocket.send(str); } 

function onClose(evt) { var str = username + ": 님이 방을 나가셨습니다."; websocket.send(str); }

파싱이란 일반적으로 문자열을 구조화된 데이터로 변환하는 과정을 말함. 
메시지 파싱은 클라이언트와 서버 간의 통신과 직접적으로 관련이 있다. 파싱은 메시지를 받아 적절히 화면에 표시하는 과정이다.

----------------------------------------------

**0524 메시지 전송 시 db 저장**

1. WebSocket 세션이 예외로 인해 닫혔다는 오류
Closing session due to exception for StandardWebSocketSession[id=47e5ae52-68b8-adf1-3dc6-6e9890c06be4, uri=ws://192.168.101.81/ws/chat]
`.orElseThrow(() -> new RuntimeException("사용자를 찾을 수 없음"));` ⇒ 사용자를 찾을 수 없음이 뜨고 있음.
username 찾는 log를 넣어 봤더니 username 자체를 찾지 못 하고 있음. 

`RuntimeException` : 정상적인 작동 중에 발생할 수 있는 예외의 슈퍼 클래스

시퀀스 (sequence) : 시퀀스는 연속적인 숫자 값을 자동으로 증가시키는 숫자를 발생시키는 객체. 시퀀스를 이용하면 기본 키의 값을 자동으로 생성할 수 있기 때문에 훨씬 간편해진다.

-------------------------------------------------------

**0527 메시지 불러오기**
1. smc로 로그인하고 채팅을 한 후 새로고침하여 다시 로그인했을 때 smc(내 계정) 자신이 보낸 메시지만 불러와지고 있음. 상대방이 보낸 메시지는 보이지 않는 문제가 있다.
   
   findAll() 사용하면 모든 메시지가 불러와짐. 현재로서는 굳이 ChatRoomId 만들어서 하기보다는 findAll()을 사용하는게 적합하다고 본다는 피드백을 받았다. 또한 findAll()로 불러온 메시지를 구별하기 위해 메시지 창의 색상 차이를 주는 것이 좋을 것이고 그건 프론트에서 해야한다. 

------------------------------------------------------------------------------------------
**0528 웹 소켓 자동 끊김**

웹 소켓이 끊기는 것을 수정하기 위해 찾아 본 결과 =>

소켓 서버(브로커)에서는 보통 특정 클라이언트와 일정 시간 동안 아무 메시지를 주고받지 않으면 연결을 끊는다. 그래서 클라이언트에서는 일정 주기로 ping을 보내고, 서버로부터 그에 대한 응답을 받아 연결이 유지되고 있음을 확인한다.


-------------------------------------------------------------
**개발 도구**

intellij

mysql
