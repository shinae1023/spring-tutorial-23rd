<details>
<summary><h1>spring-tutorial-23rd</h1></summary>

<img width="1674" height="628" alt="image" src="https://github.com/user-attachments/assets/58a0006a-2362-409c-845a-f7d9f3691201" />

<img width="694" height="372" alt="image" src="https://github.com/user-attachments/assets/b72c3223-8fdc-4128-b2c0-bea23f35a6c6" />
</details>

<details>
<summary><h1>spring이 지원하는 기술들</h1></summary>
    
## 1. POJO란?

**POJO (Plain Old Java Objection) : 특정 기술이나 프레임워크에 종속되지 않은 순수한 자바 객체**

### 1) POJO의 탄생 배경

2000년대 초반, 자바 엔터프라이즈 개발의 주류였던 EJB(Enterprise JavaBeans)는 매우 강력했지만, 치명적인 단점이 존재

- **높은 의존성:** 특정 인터페이스를 상속받거나 구현해야만 동작
- **어려운 테스트:** 프레임워크가 없으면 객체 하나도 테스트 불가
- **복잡성:** 비즈니스 로직보다 프레임워크를 위한 코드가 더 많아짐

마틴 파울러 등의 "왜 우리는 이렇게 복잡하게 개발해야 하는가? 그냥 **평범하고 오래된 자바 객체**로 개발하자!"라는 생각에서 출발

### 2) POJO의 조건

어떤 객체가 POJO라고 불리려면 다음 세 가지 제약에서 자유로워야함.

1. **extends 하지 않기:** 특정 규약(Framework)의 클래스를 상속받지 않음
2. **implements 하지 않기:** 특정 규약의 인터페이스를 구현하지 않음
3. **종속되지 않기:** 특정 환경에 종속적인 어노테이션이나 라이브러리가 비즈니스 로직에 침투하지 않음

```java
// POJO인 경우 (순수 자바)
public class User {
    private String name;
    public String getName() { return name; }
}

// POJO가 아닌 경우 (특정 프레임워크에 종속)
public class User extends FrameworkSpecificBaseClass {
// ...
}
```

### 3) 스프링과 POJO의 관계

스프링 프레임워크의 가장 큰 목표 :  POJO를 이용한 엔터프라이즈 애플리케이션 개발

개발자는 비즈니스 로직에만 집중해서 순수한 POJO 클래스를 작성하고, 스프링은 그 뒤에서 다음과 같은 기술로 기능을 덧붙여줌

- **IoC/DI :** 순수한 객체 상태로 있으면 필요한 의존성(객체)을 알아서 주입
- **AOP :** 비즈니스 로직에만 집중해서 개발하면 트랜잭션이나 로깅 같은 공통 기능을 밖에서 감싸줌
- **PSA :** 인터페이스만 보고 개발하면 뒤에서 기술이 바뀌어도(DB, 서버 등) 코드 변화 없음

### 4) POJO의 장점

- **코드의 단순화 :** 프레임워크 관련 코드가 사라져 가독성이 좋아짐
- **테스트 난이도 하락 :** 환경의 영향을 받지 않으므로 JUnit 등을 이용해 빠르게 단위 테스트 가능
- **객체지향적 설계 :** 특정 기술의 제약이 없으므로 객체지향 설계(상속, 다형성 등)를 적용하기 쉬움

## 2. Spring이 지원하는 기술

### 1) 스프링은 어떻게 애플리케이션 전반을 제어하는가

과거의 프레임워크(EJB 등)는 상속을 강요 → 스프링은 “너는 그냥 평소처럼 편하게 있어(POJO). 내가 너를 불러줄게(Callback)"

이를 할리우드 원칙이라고 함

> *"Don't call us, we'll call you." (우리한테 전화하지 마세요. 우리가 당신을 부를게요.)*
> 

스프링은 다음과 같은 기술들로 POJO를 제어

1. **설정 정보(Metadata) :** `@Service`, `@Controller` 같은 어노테이션이나 XML 설정을 통해 해당 클래스를 관리
2. **리플렉션(Reflection) :** 자바의 리플렉션 기술을 사용하면 상속을 받지 않아도 클래스의 이름만 알면 강제로 객체를 만들고 메서드를 호출할 수 있음
3. **의존성 주입(DI) :** 내가 직접 `new`를 하지 않아도, 스프링이 알아서 필요한 객체를 가져다 끼워줌

| **특징** | **설명** |
| --- | --- |
| **컨테이너 제공** | 객체(Bean)의 생명주기를 직접 관리하는 '통' 역할 |
| **확장 포인트 제공** | 개발자가 비즈니스 로직만 채워 넣으면 작동하도록 미리 설계된 틀(Template) |
| **비침투적** | 프레임워크의 코드가 사용자 코드에 직접적으로 드러나지 않으면서도 강력한 기능을 수행 |

### 2) 스프링 삼각형

<img width="956" height="764" alt="image" src="https://github.com/user-attachments/assets/2f689443-d2ab-468f-b232-0638aff7bf24" />

스프링 삼각형 : IoC/DI, AOP, PSA

### 3) IoC/DI

**IoC (제어의 역전) :** 프로그램의 제어 흐름을 개발자가 직접 관리하는 것이 아니라 외부(프레임워크)에 맡기는 설계 원칙

- **전통적인 방식 :** 개발자가 객체를 언제 생성할지 어떤 객체를 사용할지 직접 결정하고 실행
- **IoC 방식 :** 객체의 생명주기(생성, 설정, 실행, 소멸)를 개발자가 아닌 **스프링 컨테이너**가 관리

**DI (의존성 주입)** : IoC를 구현하기 위한 핵심 패턴으로 어떤 객체가 필요로 하는 의존 객체를 직접 생성하는 것이 아니라 **외부에서 주입**해 주는 방식

DI를 사용하지 않는 코드 : 이 방식은 `Store` 클래스가 어떤 키보드를 팔지 **직접 결정**. 만약 키보드 종류를 바꾸려면 `Store` 클래스의 코드를 직접 수정해야 하므로 결합도가 높음.

```java
public class Store {
    private Keyboard keyboard;

    public Store() {
        // Store가 직접 특정 구현체를 생성함 (강한 결합)
        this.keyboard = new GamingKeyboard();
    }
}
```

DI를 사용하는 코드 (스프링 방식) : 이제 `Store`는 어떤 키보드가 들어올지 모름. 단지 `Keyboard` 인터페이스를 따르는 무언가가 들어올 것이라고 믿고 사용.

```java
public class Store {
    private Keyboard keyboard;

    // 생성자를 통해 외부에서 '주입'받음 (의존성 주입)
    public Store(Keyboard keyboard) {
        this.keyboard = keyboard;
    }
}
```

**이게 왜 제어의 역전?** 

위 코드에서 `Store`는 이제 `Keyboard`객체를 생성할 권한이 없음 누군가(스프링 컨테이너)가 밖에서 던져주는 대로 받아야 함. 즉, **객체 생성의 주도권 역전**

| **구분** | **IoC (Inversion of Control)** | **DI (Dependency Injection)** |
| --- | --- | --- |
| **성격** | 추상적인 **설계 원칙** | 구체적인 **패턴** |
| **핵심 질문** | "누가 제어권을 가지고 있는가?" | "의존성을 어떻게 전달받는가?" |

### 3-1) new로 생성한 객체가 테스트하기 어려운 이유

1. 테스트 격리의 불가능 : 단위 테스트의 목적은 "특정 클래스 한 곳의 로직"만 검증하는 것. 하지만 내부에서 `new`로 의존 객체를 직접 생성하면, 테스트 대상 클래스와 의존 객체가 하나의 덩어리가 됨. 의존 객체에 버그가 있다면 테스트 대상 클래스도 실패하게 되어, 어디가 문제인지 파악하기 어려움
2. 외부 의존성 제어 불가 :  만약 `new`로 생성된 객체가 실제 데이터베이스에 접속하거나, 외부 API를 호출하거나, 파일을 생성하는 로직을 담고 있다면 테스트 환경에서 이를 제어할 방법이 없음. 테스트를 실행할 때마다 실제 DB 값이 변하거나 네트워크 상태에 따라 테스트 성공 여부가 달라지는 비결정적 테스트
3. Mock 객체 교체 불가 : Mockito 같은 모킹 프레임워크는 대상 객체를 가짜(Mock)로 갈아끼워 동작을 정의해야 함. 하지만 코드 내부에 `new`가 박혀 있으면 외부에서 이 가짜 객체를 끼워 넣을 수 없음
- **테스트하기 어려운 코드 (`new` 직접 생성)**
    
    아래 코드에서 `UserService`는 `UserRepository`와 강하게 결합
    
    ```java
    public class UserService {
        private UserRepository userRepository;
    
        public UserService() {
            // 내부에서 직접 생성 -> 외부에서 UserRepository를 조작할 방법이 없음
            this.userRepository = new UserRepository(); 
        }
    
        public boolean registerUser(String email) {
            if (userRepository.existsByEmail(email)) {
                return false;
            }
            userRepository.save(email);
            return true;
        }
    }
    ```
    
    **이 코드가 테스트하기 어려운 이유:**
    
    - `UserRepository`가 실제 DB를 사용한다면, 테스트 코드를 돌릴 때마다 실제 DB 설정이 필요
    - `userRepository.existsByEmail(email)`이 항상 `true`를 반환하는 상황을 가정하고 싶어도, 코드 내부에 `new`가 고정되어 있어 가짜 객체를 넣을 수 없음
    
- 테스트하기 쉬운 코드 (DI 적용)
    
    의존성을 외부에서 주입받도록 수정하면 결합도가 낮아짐
    
    ```java
    public class UserService {
        private final UserRepository userRepository;
    
        // 생성자를 통해 외부에서 주입받음 (DI)
        public UserService(UserRepository userRepository) {
            this.userRepository = userRepository;
        }
    
        public boolean registerUser(String email) {
            if (userRepository.existsByEmail(email)) {
                return false;
            }
            userRepository.save(email);
            return true;
        }
    }
    ```
    
- 실제 테스트 코드 (Mockito 활용)
    
    이제 `UserService`를 테스트할 때, 실제 DB를 건드리는 `UserRepository` 대신 가짜 객체를 주입하여 **순수하게 `UserService`의 로직만** 검증
    
    ```java
    @ExtendWith(MockitoExtension.class)
    class UserServiceTest {
    
        @Mock
        UserRepository userRepository; // 가짜 객체 생성
    
        @InjectMocks
        UserService userService; // 가짜 객체를 UserService에 주입
    
        @Test
        void 이미_존재하는_이메일이면_회원가입에_실패한다() {
            // 1. Given: userRepository가 특정 이메일에 대해 true를 반환하도록 설정
            String email = "test@example.com";
            when(userRepository.existsByEmail(email)).thenReturn(true);
    
            // 2. When: 가입 시도
            boolean result = userService.registerUser(email);
    
            // 3. Then: 결과 확인 (DB를 건드리지 않고 로직만 검증)
            assertThat(result).isFalse();
            verify(userRepository, never()).save(anyString()); // save가 호출되지 않았는지 검증
        }
    }
    ```
    

### 3-2) 의존성 주입 방식

① 필드 주입 (Field Injection) : 변수에 `@Autowired` 어노테이션을 붙여서 직접 주입하는 방식

```java
@Service
public class OrderService {
    @Autowired
    private UserRepository userRepository; // 필드에 직접 주입
}
```

- **특징:** 코드가 가장 간결
- **단점:** 외부에서 수정을 할 수 없어 테스트하기 매우 어렵고, 스프링 컨테이너가 없으면 작동하지 않는 "강한 결합" 상태

② 수정자 주입 (Setter Injection) : `setXXX` 메서드를 통해 의존성을 주입하는 방식

```java
@Service
public class OrderService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

- **특징:** 의존성을 선택적으로 주입하거나, 런타임에 의존성을 변경해야 할 때 사용
- **단점:** 주입받는 객체가 변경될 가능성이 열려 있어 객체의 안정성이 떨어짐

③ 생성자 주입 (Constructor Injection) : 생성자를 통해 의존성을 주입받는 방식 **스프링 4.3 버전부터는 생성자가 하나라면 `@Autowired` 생략 가능**

```java
@Service
public class OrderService {
    private final UserRepository userRepository; // final 키워드 사용 가능

    public OrderService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

- 가장 권장되는 방식

### 3-3) 생성자 주입이 가장 권장되는 이유

**1) 불변성 (Immutability) :** 생성자 주입을 사용하면 필드에 `final` 키워드를 사용할 수 있음. 이는 객체가 생성될 때 의존성이 한 번 설정되면 **절대로 변하지 않음**을 보장. 객체 지향 프로그래밍에서 상태의 불변성은 멀티스레드 환경에서도 안전하고 예측 가능한 코드를 만들 수 있음

**2) 객체의 완전성 (Null 안전성) :** 생성자 주입은 객체를 생성하는 시점에 모든 의존성을 요구. 만약 필요한 의존성을 전달하지 않으면 **컴파일 오류**가 발생. 반면 필드/수정자 주입은 의존성 없이도 객체가 생성될 수 있어, 메서드 호출 시점에 `NullPointerException`이 발생할 위험이 큼

**3) 순환 참조 방지 :** 두 객체가 서로를 참조할 때(A -> B, B -> A), 생성자 주입을 사용하면 애플리케이션 **구동 시점**에 에러를 띄워줌. 필드나 수정자 주입은 실제로 메서드가 호출될 때 비로소 에러가 터지기 때문에 문제를 미리 발견하기 어려움

**4) 순수 자바 테스트 가능** : 생성자 주입은 스프링 컨테이너 도움 없이도 **단순히 `new`를 통해 테스트 코드를 작성**할 수 있음 `Mock` 객체를 생성자에 넣어주기만 하면 됨

| **비교 항목** | **필드 주입** | **수정자 주입** | **생성자 주입** |
| --- | --- | --- | --- |
| **의존성 불변** | ❌ 불가 | ❌ 불가 | **✅ 가능 (final)** |
| **필수 의존성** | ❌ 누락 위험 | ❌ 누락 위험 | **✅ 강제 가능** |
| **테스트 용이성** | ❌ 어려움 (Spring 필수) | △ 보통 | **✅ 매우 쉬움 (Pure Java)** |
| **순환 참조 확인** | ❌ 런타임에 발생 | ❌ 런타임에 발생 | **✅ 컴파일/구동 시 발생** |

### 3-4) 스프링 컨테이너의 역할

1. **Bean의 생성 및 관리**
    
    빈(Bean) : 개발자가 `@Component`, `@Service` 등으로 등록한 객체 → 컨테이너는 이 빈들의 태생부터 죽음까지를 책임지는 역할
    
    - **객체 생성 :** 애플리케이션이 시작될 때 필요한 객체들을 리플렉션을 이용해 직접 생성 (더 이상 개발자가 `new`를 호출하지 않음)
    - **생명주기 콜백 :** 객체가 생성된 직후(`@PostConstruct`)나 소멸하기 직전(`@PreDestroy`)에 특정 로직을 수행할 수 있도록 관리
2. **의존성 주입**
    
    객체들 간의 연관 관계를 분석하여 필요한 객체를 서로 연결
    
    - **의존성 분석 :** 어떤 클래스가 어떤 인터페이스에 의존하고 있는지 파악
    - **주입 :** 생성자 주입이나 필드 주입을 통해 컨테이너가 가지고 있는 빈 부품들을 끼워 넣음 이를 통해 객체들은 서로의 구체적인 구현체를 몰라도 됨
3. **설정 메타데이터 처리**
    
    컨테이너는 무엇을 객체로 만들고, 어떻게 연결할지를 결정하기 위해 가이드북을 읽음
    
    - **소스 :** 과거에는 XML을 많이 썼지만, 현재는 자바 설정 클래스(`@Configuration`)와 어노테이션(`@Component`, `@Autowired`)을 주로 사용
    - **역할 :** 이 가이드북을 해석해서 어떤 객체를 싱글톤으로 만들지, 어떤 구현체를 주입할지 결정
4. BeanFactory vs ApplicationContext
    
    
    | **구분** | **BeanFactory** | **ApplicationContext (주로 사용)** |
    | --- | --- | --- |
    | **특징** | 스프링 컨테이너의 최상위 인터페이스 | BeanFactory를 상속받은 확장 인터페이스 |
    | **주요 기능** | 빈의 생성과 주입 등 기본적인 IoC 기능 | 빈 관리 + 부가 기능 (국제화, 이벤트, AOP 연동 등) |
    | **빈 로딩** | 지연 로딩 (Lazy Loading) | 즉시 로딩 (Pre-loading) |

컨테이너가 일하는 과정

1. **Read :** 설정 정보(어노테이션, 자바 설정 등)를 읽음
2. **Register :** 관리할 객체들을 '빈 정의(BeanDefinition)'로 등록
3. **Instantiate :** 등록된 정보를 바탕으로 객체(Bean)를 생성
4. **Inject :** 생성된 객체들 사이에 필요한 의존성을 주입
5. **Provide :** 개발자가 필요할 때(`getBean()`) 완성된 객체를 제공

### 4) AOP

AOP(Aspect Oriented Programming, 관점 지향 프로그래밍) : 공통 관심사를 비즈니스 로직에서 분리해내는 기술

- **Aspect :** 공통 기능들을 모아놓은 모듈
- **Advice (할 일) :** 실제로 언제 무엇을 할 것인지에 대한 정의
- **Join Point (합류 지점) :** Advice가 적용될 수 있는 모든 시점 (주로 메서드 실행 시점)
- **Pointcut (대상 선정) :** Join Point 중에서 실제로 Advice를 적용할 메서드를 선별하는 규칙
- **Proxy (대리인):** 호출을 가로채서 Advice를 실행하고 비즈니스 로직을 호출해 주는 중간 객체

(1) 비즈니스 로직 (순수한 pojo)

```java
@Service
public class OrderService {
    public void createOrder() {
        // 주문 로직만 작성 (로깅이나 시간 측정 코드가 없음!)
        System.out.println("주문이 생성되었습니다.");
    }
}
```

(2) aspect class

```java
@Aspect
@Component
public class PerformanceAspect {

    // Pointcut: com.example.service 패키지 하위의 모든 메서드 대상
    @Around("execution(* com.example.service..*(..))")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();

        try {
            // 실제 비즈니스 로직 실행 (OrderService.createOrder() 등)
            return joinPoint.proceed(); 
        } finally {
            long end = System.currentTimeMillis();
            System.out.println(joinPoint.getSignature().getName() + " 실행 시간: " + (end - start) + "ms");
        }
    }
}
```

### 4-1) 스프링에서의 AOP 구현

스프링은 내부적으로 **Proxy** 객체를 생성해서 클라이언트가 `OrderService`를 호출하면, 진짜 `OrderService`가 아닌 스프링이 만든 '가짜(Proxy) OrderService'가 호출됨

이 프록시가 먼저 `Advice`를 실행하고, 그다음에 진짜 `OrderService`의 메서드를 호출하는 방식

- advice : 언제(When) 어떤 부가 기능(What)을 수행할 것인가
    - **클라이언트의 호출 :** 클라이언트는 `target.doSomething()`을 호출한다고 생각하지만, 실제로는 `proxy.doSomething()`이 호출됨
    - **프록시의 가로채기 :** 프록시 객체가 호출을 받아서 등록된 **Advice**를 확인
    - **Advice 실행 (Before) :** 메서드 실행 전 처리해야 할 로직(로그 기록 등)을 먼저 수행
    - **타겟 메서드 호출 :** 프록시가 비로소 진짜 객체(Target)의 메서드를 호출
    - **Advice 실행 (After) :** 메서드 실행이 끝난 후 처리해야 할 로직을 수행
    - **결과 반환 :** 최종 결과를 클라이언트에게 전달합니다.

| **종류** | **설명** | **시점** |
| --- | --- | --- |
| **@Before** | 타겟 메서드가 실행되기 **전**에 실행 | 비즈니스 로직 시작 전 보안 체크, 로그 기록 등 |
| **@AfterReturning** | 메서드가 **성공적으로 완료**된 후에 실행 | 결과값 반환 후 후처리 |
| **@AfterThrowing** | 메서드 실행 중 **예외가 발생**했을 때 실행 | 예외 처리, 에러 로그 기록 |
| **@After (Finally)** | 성공/실패 여부와 상관없이 **무조건** 실행 | 리소스 해제 등 |
| **@Around (가장 강력)** | 메서드 호출 **전/후/예외**를 모두 제어 | 실행 시간 측정, 트랜잭션 관리 등 |

ex) @Transactional

트랜잭션 어노테이션도 스프링이 미리 만들어놓은 advice로 직접 구현한다면 아래 코드와 같음

```java
@Aspect
@Component
public class TransactionAspect {

    // @Around: 메서드 실행 전후를 다 가로채겠다 (언제)
    @Around("execution(* com.myapp.service..*(..))") 
    public Object handleTransaction(ProceedingJoinPoint joinPoint) throws Throwable {
        try {
            System.out.println("트랜잭션 시작 (Before)"); // 무엇을 1
            
            Object result = joinPoint.proceed(); // 실제 비즈니스 로직 실행
            
            System.out.println("트랜잭션 커밋 (AfterReturning)"); // 무엇을 2
            return result;
        } catch (Exception e) {
            System.out.println("트랜잭션 롤백 (AfterThrowing)"); // 무엇을 3
            throw e;
        } finally {
            System.out.println("리소스 정리 (After)"); // 무엇을 4
        }
    }
}
```

### 5) PSA

- PSA란?
    - **Portable (이식 가능한) :** 이 기술 저 기술로 옮겨 다녀도 코드가 그대로 작동함.
    - **Service Abstraction (서비스 추상화) :** 특정 기술의 복잡한 사용법을 인터페이스로 감춰버림.
- **PSA가 없는 경우**
    
    ```java
    // JDBC를 직접 쓸 때
    public void saveUser() {
        Connection conn = dataSource.getConnection();
        conn.setAutoCommit(false); // 트랜잭션 시작
        try {
            // 비즈니스 로직 (SQL 실행 등)
            conn.commit();
        } catch (Exception e) {
            conn.rollback();
        }
    }
    ```
    
    → 이 경우 jdbc를 쓰다가 jpa를 쓰고 싶으면 코드를 전부 수정해야함
    
- PSA가 있는 경우
    
    ```java
    **@Service
    public class UserService {
        // 내부가 JDBC든 JPA든 상관없이 이 어노테이션
        @Transactional 
        public void saveUser(User user) {
            userRepository.save(user);
        }
    }**
    ```
    
    스프링이 제공하는 `PlatformTransactionManager`라는 추상화 인터페이스를 사용
    
    실제 DB 연동 기술이 JDBC에서 JPA로 바뀌어도 이 코드는 **단 한 줄도 수정할 필요가 없음.**
    
- 또 다른 PSA
    
    
    | **PSA 대상** | **구체적인 기술들** | **스프링의 추상화 인터페이스** |
    | --- | --- | --- |
    | **Web** | Servlet, Reactive (Netty, Jetty) | `@Controller`, `@RequestMapping` |
    | **Database** | JDBC, JPA, MyBatis, Redis | `PlatformTransactionManager` |
    | **Cache** | EhCache, Redis, Caffeine | `@Cacheable`, `CacheManager` |
    | **Mail** | JavaMail, SendGrid | `JavaMailSender` |
- PSA의 장점
    - **낮은 결합도 :** 특정 라이브러리나 서버에 종속되지 않음
    - **테스트 용이성 :** 실제 메일 서버나 실제 DB가 없어도, 테스트용 가짜 구현체를 주입해서 간편하게 테스트 가능
    - **유지보수 향상 :** 신기술이 나와서 라이브러리를 교체해야 할 때 리스크 감소
</details>
