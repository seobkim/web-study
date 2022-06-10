# 3장 프론트엔드 개발

### Node.js

자바스크립트 런타임 환경, js를 서버 언어로 사용 가능

```jsx
npm install
mkdir react-workspace
cd react-workspace
npx create-react-app todo-react-app
npm start
```

리액트에서 html파일은 index.html 밖에 없음, 다른 페이지들은 React.js를 통해 생성되고 index.html에 있는 root 엘리먼트 아래에 동적으로 렌더링된다.

### SPA (Single Page Application)

한 번 웹 페이지를 로딩하면 사용자가 임의로 새로고침하지 않는 이상 페이지를 새로 로딩하지 않는 애플리케이션

## 3.3 서비스 통합

fetch API로 백엔드에 API 요청 

### Cross-Origin Resource Sharing

처음 리소스를 제공한 도메인이 현재 요청하려는 도메인과 다르더라도 요청을 허락해 주는 웹 보안 방침

![image](https://user-images.githubusercontent.com/56287836/173046805-7fc991d0-cb97-48a6-8aec-dbafbd9625dc.png)

1. OPTIONS 메서드를 사용하는 요청
이 리소스에 대해 어떤 HTTP 메서드를 사용할 수 있는지 확인하고 싶을 때
    
    → CORS 여부 및 GET 요청 사용 가능 여부 확인
    
2. 원래 보내려고 했던 요청
헤더에 요청의 Origin을 함께 보냄

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

	private final long MAX_AGE_SECS = 3600;

	@Override
	public void addCorsMappings() {
		registry.addMapping("/**")
			.allowedOrigins("http://localhost:3000")
			.allowedMethods("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS")
			.allowedHeaders("*")
			.allowedCredentials(true)
			.maxAge(MAX_AGE_SECS);
	}
}
```

### Promise

비동기 오퍼레이션 (Asynchronous Operation)에서 사용

1. Pending : 오퍼레이션이 끝나길 기다리는 상태
2. Resolve : 오퍼레이션이 성공적으로 끝날 때
3. Reject : 오퍼레이션 중 에러가 나는 경우

### Fetch API

Promise 오브젝트 리턴

then, catch에 콜백 함수를 전달해 응답을 처리할 수 있다.
