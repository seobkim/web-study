# 2장 백엔드 개발

## 2.1.3 스프링 프레임워크와 의존성 주입

의존성 주입이란 이 클래스가 의존하는 다른 클래스들을 외부에서 주입시킨다.

- Field(필드) 주입
    
    ```java
    public class TodoService {
    	@Autowired
    	private final ITodoPersistence persistence;
    }
    ```
    
- Setter(수정자) 주입
    
    ```java
    public class TodoService {
    	private final ITodoPersistence persistence;
    	
    	public setITodoService(ITodoPersistence persistence) {
    		this.persistence = persistence;
    	}
    }
    ```
    
- Constructor(생성자) 주입
TodoService 오브젝트를 **생성할 때** 의존성 주입
    
    <aside>
    💡 Lombok의 @RequiredArgsConstructor
    `final`이 붙거나 `@NotNull`이 붙은 필드의 생성자 자동 생성
    
    </aside>
    
    ```java
    public class TodoService {
    	private final ITodoPersistence persistence;
    	
    	public TodoService(ITodoPersistence persistence) {
    		this.persistence = persistence;
    	}
    }
    ```
    
    - **생성자 주입의 장점**
        1. 불변 객체를 만들 수 있음
        2. 순환참조를 막을 수 있음
            
            ```java
            @Service
            public class AService {
            	private final BService bService;
            
            	public AService(BService bService) {
            		this.bService = bService;
            	}
            }
            
            @Service
            public class BService {
            	private final AService aService;
            
            	public BService(AService bService) {
            		this.aService = aService;
            	}
            }
            ```
            
        3. NullPointerException 방지

## 2.1.6 메인 메서드와 @SpringBootApplication

@SpringBootApplication : 스프링 부트를 설정하는 클래스

@ComponentScan : 베이스 패키지와 그 하위 패키지에서 @Component 가 달린 클래스를 찾는다

@Component : 해당 클래스를 자바 빈으로 등록시키라고 알려주는 어노테이션

@Bean : 스프링 빈 등록, 자동으로 연결될 빈 외에 다른 빈을 사용할 때

## 2.2 백엔드 서비스 아키텍처

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/87427863-c2a3-41b3-8802-0a2c7a2e00a4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220609%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220609T101115Z&X-Amz-Expires=86400&X-Amz-Signature=1c1b9697d85a9d7e60245f7835878dc555935df04cef2766f40a55c6a6811d95&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" width="500" height="300">

### Lombok annotation

- @Builder : 오브젝트 생성을 위한 디자인 패턴 중 하나, 매개변수의 순서를 기억할 필요 x
- @NoArgsConstructor : 매개변수가 없는 생성자를 구현해줌
- @AllArgsConstructor : 모든 멤버 변수를 매개변수로 받는 생성자를 구현해줌
- @Data : Getter/Setter 메서드를 구현해줌

### DTO

1. 비즈니스 로직을 캡슐화
    
    외부 사용자에게 서비스 내부의 로직, 데이터베이스의 구조 등을 숨길 수 있다.
    
2. 클라이언트가 필요한 정보를 모델이 전부 포함하지 않는 경우가 많기 때문에
    
    ex) 서비스 로직과는 상관없는 에러 메시지
    

### 컨트롤러 레이어 : 스프링 REST API controller

**요청 매개변수** 

@PathVariable : /{id}
@RequestParam : ?id={id} 
@RequestBody, Responseody : 오브젝트처럼 복잡한 자료형 요청, 응답

### 서비스 레이어 : 비즈니스 로직

### 퍼시스턴스 레이어 : 스프링 데이터 JPA

@Entity(name= “[테이블이름]”) 어노테이션 추가

@Id : 기본 키

@GeneratedValue : 자동 생성

@GenericGenerator
