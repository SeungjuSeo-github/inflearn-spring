# 📢 DAO 란?
> Data Access Object의 약어로 실질적으로 DB에 접근하여 데이터를 조회하거나 조작하는 기능을 전담하는 객체를 말한다. DAO의 사용 이유는 효율적인 커넥션 관리와 보안성 때문이다. DAO는 저수준의 Logic과 고급 비즈니스 Logic을 분리하고 domain logic으로부터 DB관련 mechanism을 숨기기 위해 사용한다.

# 📢 Mapper 란?
> Mybatis 매핑XML에 기재된 SQL을 호출하기 위한 인터페이스이다. Mybatis3.0부터 생겼다.

##  repository := dao (비슷함)
>repotiroy는 엔티티 객체를 보관하고 관리하는 저장소이고, dao는 데이터에 접근하도록 DB접근 관련 로직을 모아둔 객체이다. 둘다 개념의 차이일뿐 실제로 개발할 때는 비슷하게 사용된다.

<br>

# 📢 DAO VS Mapper
#### 1) 클라이언트 - Controller - Service - DAO - mapper.xml

- DAO는 인터페이스와 클래스의 결합된 형태
#### 2) 클라이언트 - Controller - Service - Mapper.java - mapper.xml

- Mapper.java는 단순 인터페이스

<br>

##  **📒 DAO 사용법 및 특징**
- SqlSession을 등록해줘야 한다.
- DAO인터페이스와 인터페이스를 구현한 DAO클래스를 생성해줘야한다.
- Mapper인터페이스를 사용하지 않았을 때는 네임스페이스 + “.” + SQL ID로 지정해서 SQL을 호출해야한다.(예를들면 sesseion.selectOne(“com.test.mapper.TimeMapper.getReplyer, bno ))
- selectOne, insert, delete 등 제공하는 메소드를 사용해야 한다.
- 문자열로 작성하기 때문에 버그가 생길 수 있다.
- IDE에서 제공하는 code assist를 사용할 수 없다.

<br>

### `UserService.java`
```java
@Service
@Slf4j
@RequiredArgsConstructor
public class UserService {

    private final UserDao userDao;

    public User getUser(String username) {
        return UserDao.getUser(username);
    }

}
```

### `UserDao.java`
```java
@Repository
public class UserDao {

    private final SqlSession sqlSession;

    private static String nameSpace = "com.example.dao.UserDao";

    @Override
    public User getUser(String username) {
        return sqlSession.selectOne(nameSpace +"getUser", username);
    }
}

```
### `UserMapper.xml`
```xml
<mapper namespace="com.example.dao.UserDao">
    
    <select id="getUser" parameterType="String" resultType="com.example.entity.User">
        select user_id, user_name, user_pw, nick_name, activated
        from users
        where user_name = #{user_name}
    </select>

</mapper>
``` 




##  **📒 Mapper인터페이스를 사용법 및 특징**
- Mapper인터페이스는 개발자가 직접 작성한다.
- mapper 네임스페이스는 패키지명을 포함한 인터페이스 명으로 작성한다.
- SQL id는 인터페이스에 정의된 메서드명과 동일하게 작성한다

<br>

### `UserService.java`
```java
@Service
@Slf4j
@RequiredArgsConstructor
public class UserService {

    private final UserMapper userMapper;

    public User getUser(String username) {
        return userMapper.getUser(username);
    }

}
``` 

### `UserMapper.java`
```java
@Mapper
public interface UserMapper {
    User getUser(String username);
}
``` 

### `UserMapper.xml`
```xml
<mapper namespace="com.example.mapper.UserMapper">
    
    <select id="getUser" parameterType="String" resultType="com.example.entity.User">
        select user_id, user_name, user_pw, nick_name, activated
        from users
        where user_name = #{user_name}
    </select>

</mapper>
``` 

