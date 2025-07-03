# application-properties
스프링부트 애플리케이션 속성 환경 파일

## 환경속성 분리 필요성

application.properties에 데이터베이스를 연결하기 위한 정보가 포함될 수 있는데 이러한 정보가 깃 또는 도커 레포지토리에 공개되면 노출된 데이터베이스는 보안에 취약하게 됩니다.


> 데이터베이스 아이디 패스워드가 포함된 application.properties 파일
``` properties
spring.application.name=example
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/db
spring.datasource.username=root
spring.datasource.password=1234
```

이를 해결하기 위해서 환경파일을 분리할 필요가 있습니다.

여러 방식 중 application.properties 파일에서 spring.config.import을 사용하여 외부 파일을 가져와 Override 하는 방식으로 분리 해 보았습니다.

## 과정

**application.properties와 같은 디렉토리에 application-{프로필 이름}.properties 파일을 생성**

<br>

예시
```
application-dev.properties

application-prod.properties
```

<br>


**분리할 환경 파일에 Override할 속성을 작성**

예시
```
spring.datasource.url=jdbc:mysql://123.123.123.123/store
spring.datasource.username=root
spring.datasource.password=OMGMYPASSWORD123!
```
<br>

**application.properties 파일에 import**
``` properties
spring.config.import=optional:file:./${SPRING_PROFILES_ACTIVE}.properties 
```
<br>

> application.properties

``` properties
spring.application.name=example

spring.jpa.properties.hibernate.show_sql=true
spring.jpa.hibernate.ddl-auto=update

spring.datasource.url=jdbc:mysql://127.0.0.1:3306/db
spring.datasource.username=root
spring.datasource.password=1234

spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# 추가 된 문장
spring.config.import=optional:file:./${SPRING_PROFILES_ACTIVE}.properties 
```
<br>
이렇게 되면 기존 설정된 환경 속성들은 Override되어 마지막으로 지정된 속성 값이 적용됩니다.
<br><br><br>

빌드할 때 다음 옵션을 추가해서 분리된 환경 속성이 적용될 수 있도록 합니다.

``` 
-Dspring.profiles.active=dev
```


## 주의사항
분리된 properties파일들은 .gitignore 처리를 해야합니다. 도커도 마찬가지.



## 정리
프로필에 따라서 환경속성을 분리하고, 보안 정보를 노출하는 것을 방지할 수 있다.