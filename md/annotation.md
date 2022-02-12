# 📢 어노테이션(@Annotation)이란?
> 사전적 의미로는 주석이라는 뜻이다. 자바에서 사용될 때의 어노테이션은 코드 사이에 
> 주석처럼 쓰여서 특별한 의미, 기능을 수행하도록 하는 기술

## `어노테이션` 의 용도

> -컴파일러에게 코드 작성 **문법 에러를 체크**하도록 정보를 제공 </br>
-소프트웨어 개발툴이 **빌드나 배치시 코드를 자동으로 생성**할 수 있도록 정보 제공 </br>
-**실행시(런타임시)특정 기능을 실행**하도록 정보를 제공 </br>

## `어노테이션`을 사용하는 순서
> 1.어노테이션(@***)을 정의한다. <br/>
2.클래스에 어노테이션을 배치한다.  <br/>
3.코드가 실행되는 중에 Reflection을 이용하여 추가 정보를 획득하여 기능을 실시한다.

## `어노테이션`의 종류
## 🔎 @Annotation
- 사전적 의미로는 '주석', 자바코드에 주석을 달아 특별한 의미를 부여한 것
- 즉, 자바코드에 주석처럼 달아 프로그램에게 추가적인 정보를 제공해주는
'메타데이터' <br/>
자바나 스프링이 제공해주는 것도 있고, 사용자가 직접 만들 수도 있음

### 📌 @ComponentScan
- 빈으로 등록 될 준비를 마친 클래스들을 스캔하여, 빈으로 등록해주는 것
  빈으로 등록 될 준비를 하는 것이란? <br/>
@Component, @Service, @Repository, @Controller, @Configuration이 붙은 빈들을 찾아서
Context에 빈으로 등록 될 준비를 한 것 <br/>
= 해당 애너테이션이 작성된 패키지 이하의 
클래스들을 순회하며 빈으로 등록될 객체들을 탐색

#### 📌 component-scan 사용방법
- component-scan 을 사용하는 방법은
xml 파일에 설정하는 방법과 자바파일 안에서 설정하는 방법이 있다. <br/>

- AppCtx 설정 클래스에 @ComponentScan 애노테이션을 붙인 예제(자바파일)
``` 
@Configuration
// @Component 애노테이션을 붙인 클래스를 스캔해서 빈으로 등록하도록 한다.
// basePackages 속성은 스캔 대상 패키지 목록을 지정한다. 해당 패키지와 그 하위 패키지에 속한 클래스를 스캔 대상으로 설정한다.
// 스캔 대상 중 @Component 애노테이션이 붙은 클래스의 객체를 생성해서 빈으로 등록한다.
@ComponentScan(basePackages= {"spring"})
public class AppCtx {
  @Bean
  @Qualifier("example")
  public ExService exService(){
    return new ExService();
  }
  @Bean
  public SampleService sampleService(){
    return new SampleService ();
  }
}
```

### 📌 @Component
- 개발자가 직접 작성한 Class를 Bean으로 등록하기 위한 어노테이션

@ComponentScan 선언에 의해 특정 패키지 안의 클래스들을 자동 스캔하여<br/>
@ComponentScan 어노테이션이 있는 클래스들에 대하여 빈 인스턴스를 생성한다.<br/>
@Component 어노테이션은 Controller, Service, DAO 세가지 이외의 클래스에만 사용 권장
<br/>
@Component -> (구체화) -> @Controller, @Service, @Repository
<br/>

[ 사용할 수 있는 위치 ] <br/>
설정 위치: 클래스 선언부 앞
<context:component-scan> 태그를 설정파일에 추가하면 해당 어노테이션이 적용된 클래스를 빈으로 등록하게 된다. 
범위는 디폴트로 singleton이며 @Scope를 사용하여 지정할 수 있다.
사용하려면 XML 설정파일에 <context:component-scan>을 정의하고 적용할 
기본 패키지를 base-package 속성으로 등록한다.


### 📌 @Bean
- 개발자가 직접 제어가 불가능한 외부라이브러리 등을 Bean으로 등록할 때 사용<br/>
- ex) ArrayList같은 라이브러리 등을 Bean으로 등록하기 위해서는<br/>
별도로 해당 라이브러리 객체를 반환하는 Method를 만들고 @Bean을 사용하면 됨<br/>
- ex)  DB 연동에 사용할 DataSource를 스프링 빈으로 등록한 후, 
DB 연동 기능을 구현한 빈 객체는 DataSource를 주입받아 사용
- 자바파일ver
```  
@Configuration
public class DBSet {
    @Bean
    public DriverManagerDataSource getConnection() throws SQLException {
        DriverManagerDataSource driverManagerDataSource = new DriverManagerDataSource();
        driverManagerDataSource.setDriverClassName();
        driverManagerDataSource.setUrl();
        driverManagerDataSource.setUsername();
        driverManagerDataSource.setPassword();

        return driverManagerDataSource;
    }
} 
```
- xml ver
```  
<!-- mybatis setting -->
   <beans:bean name="dataSource"
      class="org.springframework.jdbc.datasource.DriverManagerDataSource">
      <beans:property name="driverClassName" value="oracle.jdbc.driver.OracleDriver" />
      <beans:property name="url"
         value="jdbc:oracle:thin:@localhost:1521:xe" />
      <beans:property name="username" value="hr" />
      <beans:property name="password" value="123456" />
   </beans:bean>
```


### 📌 @Autowired
- 필요한 의존 객체의 "타입"에 해당하는 빈을 찾아 주입

[ @Autowired ]
- required : 기본값 : true
(못 찾으면 구동 실패 )

[ 사용할 수 있는 위치 ]
- 생성자 (스프링 4.3부터는 생략 가능) <br/>
- setter <br/>
- 필드 <br/>

### 📌 @Qualifier("alias")
- 특정한 객체를 찾기위한 이름을 지정 <br/>
- ex) WebClinet를 Bean으로 등록했는데 요청해야 할 url이 많아서 여러개의 WebClient를 등록하면서 문제가 발생하였다.
물론 그때그때 생성해서 쓰면되지만 효율적이지 못하다.<br/>
이러한 문제를 해결하기 위해서 자주 사용하고 재사용이 가능하다면 
@Quailifer를 사용하면 된다. <br/>

[사용할 수 있는 위치 ]
- @Autowired 바로 아래 쓰인다.(@Autowired 어노테이션과 함께 사용)
- @Autowired의 목적에서 동일 타입의 빈객체가 존재시 특정빈을 
삽입할 수 있게 설정한다. 
@Qualifier("mainBean")의 형태로 @Autowired와 같이 사용하며 
해당 <bean>태그에 <qualifire value="mainBean" /> 
태그를 선언해주어야 한다. 메서드에서 두개이상의 파라미터를 사용할 경우는
파라미터 앞에 선언해야한다.

사용하려면 동일타입의 빈객체 설정에서 <qualifier value="[alias명]" />를 추가해 준다.
```  
<bean id="user2" class="com.sp4.UserImpl"> 
    <property name="name" value="스프링"/> 
    <property name="age" value="20"/> 
    <property name="tel" value="000-0000-0000"/>
</bean> 
<bean id="userService1" class="com.sp4.UserService"/>
```
```  
public class UserService { 
    @Autowired 
    @Qualifier("user2") 
    private User user; 
    
    public String result() { 
        return user.getData(); 
    } 
}

```


### 📌 @Required
- 필수 프로퍼티임을 명시하는 것으로 필수 프로퍼티를 설정하지 않을 경우 
빈 생성시 예외를 발생시킴 <br/>

[사용할 수 있는 위치 ]
- setter 메서드 앞

```  
import org.springframework.beans.factory.annotation.Required 
public class TestBean { 
    @Required 
    private TestDao testDao; 
  
    public void setTestDao(TestDao testDao) { 
        this.testDao = testDao; 
    } 
}
```

```  
<bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanpostProcessor"/> 
<bean name="testBean" class="han.test.TestBean"> 
    <property name="testDao" ref="testDao"/> 
    <!-- @Required 어노테이션을 적용하였으므로 설정하지 않으면 예외를 발생시킨다. --> 
</bean>
```
RequiredAnnotationBeanPostProcessor 클래스는 
스프링 컨테이너에 등록된 bean 객체를 조사하여
@Required 어노테이션으로 설정되어 있는 프로퍼티의 값이 설정되어 있는지 
검사 <br/>
사용하려면 <bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor" /> 
클래스를 빈으로 등록시켜줘야 함

### 📌 @Controller
(주용도 view 리턴)클래스의 리턴타입이 String이면 리턴값이 jsp파일명을 의미 <br/>
-> @Controller의 경우 메소드에 @ResponseBody를 사용하여 객체를 리턴할 수 있음<br/>
<table>
@Controller의 실행 흐름
<tr>
Client -> Request -> Dispatcher Servlet -> Handler Mapping 
-> Controller -> View -> Dispatcher Servlet -> Response -> Client
</tr>
@ResponseBody의 실행 흐름
<tr>
Client -> Request -> Dispatcher Servlet -> Handler Mapping -> Controller (ResponseBody) -> Response -> Client
</tr>
</table>

### 📌 @RestController
- **(@Controller + @ResponseBody)** 결합 형태의 어노테이션으로, <br/>
주 용도는 해당 클래스가 ajax 요청을 받아 Json/Xml 형태로 객체 
데이터를 반환
- (주용도 데이터 리턴)클래스의 리턴타입이 String이면 문자열 데이터 의미(jsp파일 사용 X)
<table>
@RestController의 실행 흐름
<tr>
Client -> HTTP Request -> Dispatcher Servlet -> Handler Mapping 
-> RestController (자동 ResponseBody 추가)-> HTTP Response -> Client
</tr>
</table>

### 📌 @ResponseBody
- 자바 객체를 HTTP 요청의 body 내용으로 변환/매핑하는 어노테이션

### 📌 @RequestBody
- HTTP 요청의 body 내용을 전달받아 자바 객체로 변환/매핑하는 어노테이션

### 📌 @Service
- @Service는 계층 구조상 주로 비즈니스 영역을 담당하는 객체임을 표시하기 위해 사용<br/>
  @Service, @Repository, @Controller모두 @Component의 역할을 수행(즉 ioc컨테이너에 bean객체를 생성해주는 역할) 하지만 각 어노테이션마다 특화된 계층이 다르다.

### 📌 @Repository
- DAO를 인식시켜주기 위한 설정
- 해당 클래스가 DAO라는 것을 알리기 위해서 @Repository라고 기재<br/>
  @Service, @Repository, @Controller모두 @Component의 역할을 수행(즉 ioc컨테이너에 bean객체를 생성해주는 역할) 하지만 각 어노테이션마다 특화된 계층이 다르다.

### 📌 @RequestMapping
- 특정 uri로 요청을 보내면 Controller에서 어떠한 방식으로 처리할지 정의 함. 
- 이때 들어온 요청을 특정 메서드와 매핑하기 위해 사용하는 것
- @RequestMapping에서 가장 많이사용하는 부분은 value와 method이다.
  - value는 요청받을 url을 설정하게 된다. 
  - method는 어떤 요청으로 받을지 정의하게 된다.(GET, POST, PUT, DELETE 등)
```  
@RestController
public class HelloController {

    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public String helloGet(...) {
        ...
    }

    @RequestMapping(value = "/hello", method = RequestMethod.POST)
    public String helloPost(...) {
        ...
    }

    @RequestMapping(value = "/hello", method = RequestMethod.PUT)
    public String helloPut(...) {
        ...
    }

    @RequestMapping(value = "/hello", method = RequestMethod.DELETE)
    public String helloDelete(...) {
        ...
    }
}
```
코드를 간결하게 하기 위해서는
```  
@RestController
@RequestMapping(value = "/hello")
public class HelloController {

    @GetMapping()
    public String helloGet(...) {
        ...
    }

    @PostMapping()
    public String helloPost(...) {
        ...
    }

    @PutMapping()
    public String helloPut(...) {
        ...
    }

    @DeleteMapping()
    public String helloDelete(...) {
        ...
    }
}
```
공통적인 url은 class에 @RequestMapping으로 설정을 해주었다.
그리고 @GetMapping, @PostMapping, @PutMapping, @DeleteMapping으로 
간단하게 생략이 가능하다. <br/>
뒤에 추가적으로 url을 붙이고 싶다면 
@GetMapping, @PostMapping, @PutMapping, @DeleteMapping 에 
추가적인 url을 작성하면 된다.
```  
@RestController
@RequestMapping(value = "/hello")
public class HelloController {    
    @GetMapping("/hi")
    public String helloGetHi(...) {
        ...
    }
}
```
위에 있는 helloGetHi에 들어가기 위해서는 /hello/hi로 들어가야 한다.
추가적인 내용으로 @RequestMapping은 Class와 Method에 붙일 수 있고 
@GetMapping, @PostMapping, @PutMapping, @DeleteMapping들은 
Method에만 붙일 수 있다.





