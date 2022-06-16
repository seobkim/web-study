# 4장 인증 백엔드 통합

인증 : 누구인지

인가 : 사용할 수 있는 자원

## 4.1 REST API 인증 기법

### Basic 인증

최초 로그인한 후 모든 요청에 아이디와 비밀번호를 같이 보내는 방법
HTTP요청 헤더의 Authorization부분 → <ID>:<Password>

**[ 단점 ]**

1. 디코더로 아이디와 비밀번호가 노출된다는 점(MITM), 따라서 반드시 HTTPS로 사용
2. 사용자를 로그아웃 시킬 수 없음
3. 사용자의 계정 정보가 있는 DB에 과부하가 걸릴 확률이 높음
4. 인증 서버가 단일 장애점 (시스템의 한 부분이나 오류가 나는 경우 전체 시스템 다운)

### Bearer 인증

최초 로그인 시 서버가 토큰을 만들어주면 클라이언트는 이후 요청에 아이디와 비밀번호 대신 토큰 전달
헤더의 Authorization부분 → Bearer <TOKEN>
하지만 여전히 스케일 문제가 해결되지 않음!

### JSON 웹 토큰(JWT)

서버에서 전자 서명된 토큰을 이용하면 인증에 따른 스케일 문제를 해결할 수 있다.
{header}.{payload}.{signature}

- Header
typ : 토큰의 Type
alg : 토큰의 서명을 발행하는 데 사용된 해시 알고리즘
- Payload
sub : 토큰의 주인, ID처럼 유일한 식별자
iss : 토큰을 발행한 주체
iat : 토큰이 발행된 날짜와 시간
exp : 토큰이 만료되는 시간
- Signature
토큰을 발행한 iss가 발행한 서명으로 토큰의 유효성 검사에 사용

**[ 과정 ]**

1. 최초 로그인 시 서버는 사용자의 아이디, 비밀번호를 DB와 비교한다.
2. 인증된 사용자인 경우 사용자의 정보를 이용해 {header}.{payload}부분을 작성하고 자신의 시크릿키로 전자 서명한다.
3. 전자 서명의 결과로 나온 값을 {header}.{payload}.{signature}로 이어붙이고 Base64로 인코딩한 후 반환한다.
4. 이후에 누군가 이 토큰으로 리소스 접근을 요청하면 서버는 해당 토큰을 Base64로 디코딩, 각 부분으로 나눈 후 {header}.{payload}.secret키로 전자서명 후 기존 서명과 비교 → 일치하면 검증 완료

## 4.3 스프링 시큐리티 통합

dependency에 jjwt라이브러리 추가
TokenProvider : 사용자 정보를 받아 JWT생성

### 스프링 시큐리티와 서블릿 필터

API가 실행될 때마다 사용자를 인증해 주는 부분

토큰 인증을 위해 컨트롤러 메서드의 첫 부분마다 인증 코드를 작성해야 한다는 점 → 서블릿 필터로 해결

서블릿 필터가 전부 살아남은 HTTP 요청은 디스패처 서블릿으로 넘어와 컨트롤러에서 실행되고 아니면 HttpSerletResponse의 상태를 403 Forbidden으로 바꾼다. 
이처럼 예외가 발생할 경우 디스패처 서블릿을 실행하지 않고 리턴할 것이다.

서블릿 필터는 꼭 1개일 필요는 없다. 이 서블릿 필터가 여러개 있다면 FilterChain을 이용해 연쇄적으로 순서대로 실행할 수 있다.
스프링 시큐리티가 FilterChainProxy라는 필터를 서블릿 필터에 끼워 넣어준다.

OncePerRequestFilter,WebSecurityConfigurerAdapter라는 클래스를 상속해 서블릿 필터를 설정한다.

### JWT를 이용한 인증 구현

spring-boot-starter-security 디펜던시를 build.gradle에 추가한다.

1. 요청의 헤더에서 Bearer 토큰을 가져온다. parseBearerToken()
2. TokenProvider를 이용해 토큰을 인증하고 토큰을 인증하고 UsernamePasswordAuthenticationToken을 작성한다. 이 오브젝트에 사용자의 인증 정보를 저장하고 SecurityContext에 인증된 사용자를 등록한다. 
    
    ```java
    SecurityContext securityContext = SecurityContextHolder.createEmptyContext();
    securityContext.setAuthentication(authentication);
    SecurityContextHolder.setContext(securityContext);
    ```
    

### 스프링 스큐리티 설정

구현한 서블릿 필터를 사용하려면 설정을 해줘야한다.

WebSecurityConfigurerAdapter를 상속하는 클래스
configure메서드 오버라이딩

### 인증된 사용자 사용하기

@AuthenticationPrincipal을 이용해 userId찾아냄
