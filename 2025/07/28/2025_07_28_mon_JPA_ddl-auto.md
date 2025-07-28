# 2025.07.28.mon JPA ddl-auto

## JPA의 ddl-auto

### ddl-auto

- Springboot에서 JPA 구현체(Hibernate 등)을 사용할 때 데이터베이스 스키마를 관리하기 위한 옵션
- [application.properties](http://application.properties) 또는 application.yml에 설정
    - application.properties
        
        ```
        spring.jpa.hibernate.ddl-auto:XXXX
        ```
        
    - application.yml
        
        ```yaml
        spring:
        	jpa:
        		hibernate:
        			ddl-auto: XXXX
        ```
        
- SpringFramework 단독 사용 시에는 자동으로 적용되지 않으며 hibernate.hbm2ddl.auto로 직접 설정해야 한다.
    
    ```java
    Properties props = new Properties();
    props.put("hibernate.hbm2ddl.auto", "update"); // create, update, validate, none 등
    props.put("hibernate.dialect", "org.hibernate.dialect.MySQL8Dialect");
    
    entityManagerFactoryBean.setJpaProperties(props);
    ```
    

### options

- **none**
    - DB 스키마와 관련된 어떠한 작업도 수행하지 않는다.
    - DB 스키마를 수동으로 관리할때 사용한다.
    - 운영 환경과 같이 DB 스키마를 신중하게 변경해야 할 경우 사용된다.
- **validate**
    - 애플리케이션 시작 시 DB 스키마와 JPA 엔티티 매핑이 일치하는지 확인한다.
    - 별도의 DB 스키마 변경은 수행하지 않는다.
    - 운영 환경에서 DB 스키마와 엔티티 매핑이 일치하는지 확인할때 주로 사용
- **update**
    - JPA 엔티티 매핑과 DB 스키마를 반영하여 JPA 엔티티 매핑의 반영되지 않은 부분을 DB 스키마에 반영한다.
    - 기존 데이터는 유지되고 새로운 엔티티에 해당하는 테이블이 추가되거나 추가/변경된 엔티티 필드에 해당하는 컬럼이 추가/변경된다.
    - 모든 변경 사항이 자동으로 반영되는 것은 아니다.
        - 기존 필드명을 변경할 경우, 기존 필드의 컬럼은 놔두고 변경된 필드명으로 새로운 필드 생성
        - 필드 타입 변경 시 반영되지 않거나 에러가 발생한다.
        - VARCHAR의 length가 감소되는 변경은 반영되지 않는다.
        - 엔티티에서 필드를 제거해도 스키마에서는 컬럼이 제거되지 않는다.
        - UNIQUE, Index 제약조건을 추가해도 DB 스키마에는 반영되지 않는다.
        - 복합키를 추가해도 반영되지 않는다.
        - **대체로 데이터 손실 가능성이 있는 변경은 자동으로 반영되지 않는다. 인덱스 추가 등 구조 변경도 자동으로 반영되지 않는다.**
    - 모든 변경 사항을 완벽히 반영하고 싶다면 데이터 백업 후 create를 사용하자. (개발/테스트 환경 한정)
    - 운영 환경에서는 사용하지 않는 것이 권장된다. none으로 설정해두고 직접 스키마를 변경하는 것이 안전하다.
- **create**
    - 애플리케이션 시작 시 기존 스키마를 제거하고 JPA 엔티티 매핑 기반으로 새로 생성한다.
    - 개발 초기에 DB 스키마가 빈번히 변경될 경우 유용하다.
    - **기존 데이터가 전부 삭제되므로 운영 환경에서는 절대로 사용하면 안된다.**
- **create-drop**
    - 애플리케이션 시작 시 JPA 엔티티 기반으로 스키마를 새로 생성하고 애플리케이션 종료 시 스키마를 제거한다.
    - 테스트 환경에서 일시적인 스키마가 필요할 시 유용하다.
    - **기존 데이터가 전부 삭제되므로 운영 환경에서는 절대로 사용하면 안된다.**