### `DTO(Data Transfer Object)` 란? 
-  Controller 같은 클라이언트와 직접 마주하는 계층에서 DTO 를 사용하여 데이터를 요청/응답을 하며, 주로 View 와 Controller 사이에서 데이터를 주고받을 때 활용
- 데이터 교환만을 위해 사용하므로 다른 비즈니스 로직은 갖지 않고 only getter/setter 메소드만 갖음
  (순수하게 데이터 전달 만을 위한 객체이기 때문) <br/>
  ㄴ 보내는 쪽에서 setter를 사용해 데이터를 DTO에 담아보내고 받는곳에서 getter를 사용해 전달받은 DTO로부터 데이터를 꺼내는 방식이다.
- ex) 시험 볼 떄 OMR카드라고 생각, 답을 하나 다르게 쓰면 결과 값이 다름

### `DTO 예제 코드`
#### `MemberDto.java`
```java
@Getter
@Setter
public class MemberDto {
  private String name;
  private int age;
}
```

️️️️✒️ 위 코드에서와 같이 setter 메소드를 가지는 경우 가변 객체로 활용할 수 있다

✒️ 만약, 불변 객체로 활용하고자 한다면 setter 대신 생성자를 이용해서 초기화하여 사용할 수 있다

📒 불변 객체로 만들면 데이터를 전달하는 과정에서 데이터가 변조되지 않음을 보장할 수 있다
```java
@Getter
@AllArgsConstructor
public class MemberDto {
  private String name;
  private int age;
}
```
<br><br>

### `VO(Value Object)` 란? 
- 직역과 같이 값 그 자체를 표현하는 객체라는 의미 (값으로만 비교가 됨)
  ex)  지폐의 경우 같은 만원이지만 고유번호가 다름(고유번호를 각 객제의 주소라고 생각) -> 하지만 다같은 만원임
- VO는 값 자체를 표현하기 때문에 불변객체이다
- 따라서 getter 메소드는 가질수 있지만, setter 성격의 메서드는 포함X, 생성자를 만들어야함 (ReadOnly 특징을 가짐)
- 비지니스 로직을 포함할 수 있음 <br/>
 - ※ 핵심 역할은 equals() 와 hashCode() 를 오버라이딩 하여 객체의 불변성을 보장
  즉, 객체들의 주소 값이 달라도 데이터 값이 같으면 동일한 것으로 판단


### `VO 예제 코드`
#### `UserVo.java`
```java
@Getter
@AllArgsConstructor
public class UserVo {

  private String name;
  private Integer age;
  private String address;

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    UserVo userVo = (UserVo) o;
    return Objects.equals(name, userVo.name) && Objects.equals(age, userVo.age) && Objects.equals(address, userVo.address);
  }

  @Override
  public int hashCode() {
    return Objects.hash(name, age, address);
  }
}
```
️️✒️ 만약 equals(), hashCode() 메소드 없이 테스트를 실행하면 테스트 통과에 실패!
  <br/>
  ㄴ hashCode() 리턴값 -> (같음) -> equals() 리턴값 -> (true) -> 동등객체

#### `VoTest.java`
```java
public class VoTest {

    @DisplayName("VO 동등비교")
    @Test
    void isSameVo(){
        UserVo vo1 = new UserVo("개발자", 20, "서울");
        UserVo vo2 = new UserVo("개발자", 20, "서울");

        Assert.assertEquals(vo1,vo2);

        assertThat(vo1).isEqualTo(vo2);
        assertThat(vo1).hasSameHashCodeAs(vo2);
    }
}
```

<br><br>

### `Entity` 란?
- Entity는 실제 DB 테이블과 매핑되는 핵심 클래스, 데이터베이스의 테이블에 존재하는 컬럼들을 필드로 가지는 객체
<br/>✒️ 상속을 받거나 구현체여서는 안되며, DB 테이블내에 존재하지 않는 컬럼을 가져서도 안된다
- 이를 기준으로 테이블이 생성되고 스키마가 변경됨 따라서, 절대로 Entity를 요청이나 응답값을 전달하는 클래스로 사용해선 X
- Entity는 id로 구분되며, VO와 마찬가지로 비즈니스 로직을 포함할 수 있음
#### `User.java`
```java
@Entity
@Builder
@Getter
@NoArgsConstructor
@AllArgsConstructor
public class User {
   @Id
   @GeneratedValue(strategy = GenerationType.IDENTITY)
   private Long id;
  
   @Column(nullable = false)
   private String name;
  
   @Column(nullable = false)
   private String email;
   
   private String phoneNumber;
}
```
️✒️ Entity 클래스에서는 setter 메서드의 사용을 지양해야 한다

- 이유는 변경되면 안되는 인스턴스나 데이터에 대해서 setter 를 통해 접근이 가능해지기 때문에 객체의 일관성, 안전성을 보장하지 어렵다
- 그렇기 때문에 Entity 에서는 Constructor(생성자) 또는 Builder 를 사용해야한다

<br><br>

### `DTO vs VO vs Entity` 
<table>
	<tr>
		<th></th>
        <th>DTO</th>
        <th>VO</th>
        <th>Entity</th>
	</tr>
	<tr>
		<td>용도</td>
        <td>레이어간 데이터 전송용 객체</td>
        <td>값자체표현 객체</td>
        <td>DB 테이블 매핑용 객체</td>
	</tr>
    <tr>
		<td>동등결정</td>
        <td>속성값이 모두 같다고 해서 같은 객체X</td>
        <td>속성값이 모두 같으면 같은 객체O</td>
        <td></td>
	</tr>
    <tr>
		<td>가변/불변</td>
        <td>가변 또는 불변 객체<br/>
        (setter 존재 시 가변, <br/> setter 비존재시 불변)</td>
        <td>불변</td>
        <td>가변 객체 생성 후 상태 변경 O</td>   
	</tr>
    <tr>
		<td>로직</td>
        <td>only g/s</td>
        <td>g/s외 로직 가질수 있음</td>
        <td>로직 포함O</td>
	</tr>
</table>



 


