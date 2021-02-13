# spring-template
* 만든 이유
```
스프링을 더 깊게 이해하기 위하여 만든 템플릿 프로젝트
쉽게 스프링 프로젝트를 시작하기 위하기 위해 제작
Docker 활용하여 자동 빌드, 이미징, 배포, 실행

Windows10 또는 Linux환경에서 개발 중에
개발자가 로컬에서 간단히 테스트 가능하게함
```

* 포함
    - hibernate, hikariCP, h2, mysql-connector
    - spring webmvc
    - lombok
    





# 실행 
* 필요사항
    - Openjdk8
    - Maven 
    - Docker
    - WSL2(windows 10)
* 실행 순서
```

```




# Manual Maven Build & Docker
* Maven Build
    - mvn clean -f .\
    - mvn package -f .\
* Dockfile
    - openjdk8 tomcat9 환경
    - docker build --tag spring-template:1.0 .






# spring 키워드
* 빈 주입
    - Autowired, Qualifier
    - Inject, Named

---

* 값 유효성 검사
    - Validator 인터페이스
    - JSR380(빈 검증 2.0)
        ```java
        @NotNull, @Min, @Max, @NotBlank, @Size
        ```
    - 스프링의 JSR380지원 LocalValidatorFactoryBean
        ```java
        @Autowired private Validator validator;
        // org.springframework.validation.beanvalidation.LocalValidatorFactoryBean
        // jsr380의 validator, validatorFactory인터페이스 구현하는 동시에, 스프링 Validator인터페이스도 구현한다

        // 스프링 validator
        BeanPropertyBindingResult = bindingResult = new BeanPropertyBindingResult(대상, "Errors");
        validator.validate(대상, bindingResult);
        if(bindingResult.getErrorCount() > 0){
            logger.error("Error were found");
        }else{
            대상.작업
        }

        // jsr380 api
        Set<ConstraintViolation<대상.class>> violations = validator.validate(대상);
        Iterator<ConstraintViolation<대상.class>> iter = violations.iterator();
        if(itr.hasNext()) logger.error("Error were found");
        else 대상.작업
        ```
    - 메서드 검증
        ```java
        // 메서드의 인수와 반환값을 검증
        // org.springframework.validation.beanvalidation.MethodValidationPostProcessor

        //@Validated가 설정된 빈 클래스를 검색해 jsr380 제약 사항 애너테이션을 사용해 검증 지원한다

        @Validated
        public interface CustomerRequestService{
            @Future // 반환값은 미래날짜여야한다.
            Calendar submitRequest(@NotBlank String type, @Size(min=20, max=100) String description, @Past Calendar accountOpeningTime);
        }
        ```

--- 

* 프로파일
    - 빈 정의 프로파일
    - 빈 집합과 프로파일을 연결시켜서 환경에 따라 다른 빈을 사용하고싶을때 사용
    - ex) 개발환경에는 내장db, 프로덕션에서는 독립db
    - spring.profiles.active 프로퍼티 값으로 프로파일 이름을 설정
    - java 기반 config일때는 설정클래스또는 메소드에 @Profile({"dev", "~~"}) 등으로 프로파일 설정이 가능
        
--- 

* 스프링으로 데이터베이스 상호작용
    - 스프링은 JDBC 위에 추상계층을 추가해 데이터베이스와 상호작용을 편리하게 해줌
