# spring-practice
초보 웹 개발자를 위한 스프링 입문5


ORM(Object-Relational Mapping) = 객체와 관계형 데이터베이스를 매핑
JPA(Java Persistnece API) = 자바 진영의 ORM 기술 표준, JPA를 사용하려면 JPA를 구현한 ORM 프레임 워크를 선택해야 하는데 가장 대중적인건 hibernate.
JDBC(Java DataBase Connectivity) = java와 db연결을 위한 표준 api


Bean 객체는 기본적으로 싱글톤 범위를 갖는다. (singleton scope) -> 모든 Bean 객체는 하나씩만 생성됨
<자동 주입>
Annotation -> 일종의 캡션, 조금의 기능을 갖춘
의존 자동 주입 기능을 프로젝트 전반에 걸쳐 사용 @Autowired(주로), @Resource
의존 주입 대상을 스프링 빈으로 등록하는 것이 보통 @Bean
@Bean 설정 class를 @Configuration으로 등록, 이 class를 설정 class라고함, 이 클래스내에 @Bean 생성


@Qualfier

방법1. @Autowired(required = false) Autowired annotation은 자동 주입할 Bean이 없으면 exception을 발생시키기 때문에 저렇게 처리하면 자동 주입이 필수가 아니게 되면서 exception이 발생하지 않는다.
스프링5부터는 의존주입대상(파라미터)에 Optional을 사용할 수 있다.
방법2. 파라미터에 @Nullable annotation 사용
방법1은 Bean이 없으면 setter 메소드 호출X, 방법2는 Bean이 없어도 setter 메소드 호출


<컴포넌트 스캔> : spring이 직접 클래스를 검색해서 Bean으로 등록해주는 기능
@Component, 설정 클래스에 @ComponentScan(basePackges = {"spring"}, excludeFilters = @Filter(type = , pattern = , classes =  ), AspectJ패턴이 작동하려면 의존 대상에 aspectjweaver 추가(pom.xml)
어떤 애너테이션을 붙인 타입을 컴포넌트 대상에서 제외할때 classes에 애너테이션이름.class 적기 이때 type = FilterType.ANNOTATION 으로 설정
컴콘콘서레에 -> 컴포넌트 스캔 기본 스캔 대상
컴포넌트 스캔 과정에서 같은 빈 이름 사용시 충돌(익셉션)이 일어남
수동 생성 빈이 가장 우선적으로 생성된다.

챕터6. Bean 라이프 사이클과 범위
빈 객체 라이프 사이클 : 객체 생성 -> 의존 설정 -> 초기화 -> 소멸
초기화 : InitializingBean 인터페이스 상속 후 afterPropertiesSet() 구현
소멸 : DisposableBean 인터페이스 상속 후 destroy() 구현
그러나 두 인터페이스를 구현할 수 없거나 사용하고 싶지 않은 경우 스프링 설정에서 직접 메소드 지정 가능 - > @Bean의 속성값으로 가능 ex) @Bean(initMethod = "~~", destroyMethod = "~~")


Bean객체를 구할 때마다 새로운 객체를 생성하는 것 -> Bean의 범위를 프로토타입으로 설정
ex)@Bean /n @Scope("prototype") 프로토타입 범위를 갖는 빈은 완전한 라이프 사이클을 따르지 않는다.

챕터7. AOP 프로그래밍 (Aspect Oriented Programming)
기본적으로 스프링이 AOP를 구현할때는 pom.xml 파일에 aspectweaver의존을 추가해야한다.
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.13</version>
</dependency>

기존 코드를 수정하지 않고 코드 중복을 피할 수 있는 방법 -> 프록시 객체
핵심 기능의 실행은 다른 객체에 위임하고 부가적인 기능을 제공하는 객체를 프록시 객체라고 한다.
AOP의 핵심은 공통 기능 구현과 핵심 기능 구현을 분리하는 것
ex) 생성자로 객체를 주입받고 인터페이스의 메소드 구현부에는 그 객체의 메소드를 사용하고
부가적인 기능만 만드는 것

AOP : 여러 객체에 공통으로 적용할 수 있는 기능을 분리해서 재사용성을 높이는 것
     = 핵심기능에 공통 기능을 삽입하는 것.
방법1. 컴파일 시점에 코드에 공통 기능을 삽입하는 방법
방법2. 클래스 로딩 시점에 바이트 코드에 공통 기능을 삽입하는 방법
방법3. 런타임에 프록시 객체를 생성해서 공통 기능을 삽입하는 방법 - > 널리 사용되는 방법


스프링AOP 구현
1. Aspcet로 사용할 클래스에 @Aspect 애노테이션을 붙인다.
2. @Pointcut 애노테이션으로 공통 기능을 적용할 Pointcut을 정의한다.
3. 공통 기능을 구현한 메서드에 @Around 애노테이션을 적용한다.
설정 클래스에 @EnableAspectJAutoProxy 적용


@Aspect, @Pointcut, @Around
7장 다시보기 cache 내용

챕터8. DB연동 JDBC, JDBC 템플릿 사용법
DriverManager -> Connection -> Statement -> ResultSet
               를 통해 인스터스를 얻고, 통해 얻고, 이용해 얻는다.       

DA0 = Data Access Object
DB에 접속하여 데이터를 조회/수정을 하는 기능을 전담하도록 만든 객체


TDD = Test Driven Development
DBMS = DataBase Management System
