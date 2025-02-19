### Section 01 - 1

- base project 생성
  - spring boot 3.4.2
  - java 21
  - cards, accounts, loans 생성
- 관련 dependency 추가
  - actuator
    - `org.springframework.boot:spring-boot-starter-actuator`
    - Spring Boot Actuator는 애플리케이션의 상태를 모니터링하고 관리
  - jpa
    - `org.springframework.boot:spring-boot-starter-data-jpa`
    - Java 진영에서 ORM(Object-Relational Mapping) 기술 표준으로 사용하는 인터페이스 모음
    - 단점: learning curve 가 높다. 디버깅이 복잡하다. 데이터베이스 설계의 영향이 크다.
  - validation
    - `org.springframework.boot:spring-boot-starter-validation`
    - 필드 유효성 검사
  - web
    - `org.springframework.boot:spring-boot-starter-web`
    - REST API 개발
  - openapi (swagger)
    - `org.springdoc:springdoc-openapi-starter-webmvc-ui:2.1.0`
    - REST API 웹 서비스를 설계, 빌드, 문서화
  - log4j2
    - `org.springframework.boot:spring-boot-starter-log4j2:3.4.2`
    - Log4j2가 아파치에 따르면 멀티쓰레드 환경에서 비동기 로거의 경우 처리량이 18배 높고 대기시간이 더 짧다.
  - devtool
    - `org.springframework.boot:spring-boot-devtools`
    - Spring boot에서 제공하는 개발 편의를 위한 모듈
  - database driver
    - `com.h2database:h2`
    - `com.mysql:mysql-connector-j`
- properties 파일 분리
  - 구동 환경에 따른 properties 파일 분리
    - 현재는 실습용 `h2`, 개발서버의 mysql 연결용 `mysql`, 개인로컬 데이터베이스(postgresql) 연결용 `local` 로 분리
  - h2
  - mobigen
  - local
- database init sql 추가
  - 실습에 필요한 database table 생성 쿼리 추가
    - properties 파일에 관련 설정 설명
    - 단, 현재 `h2` 연결용 schema 쿼리만 추가한 상태 (**mysql 버전의 쿼리 필요**)
- log4j2 관련 파일 추가
  - log4j2.xml
    - 콘솔 로그 출력 포맷 변경
    - log 저장 경로 및 파일 저장용 포맷, 백업 등의 설정은 보류


#### h2 데이터베이스 접속

`http://[IP]:[실행 application 포트]/h2`

- JDBC URL: `jdbc:h2:mem:testdb`
- User Name: sa
- Password: 없음(공백)

좌측 메뉴에서 schema.sql 에 정의된 테이블 확인

### Section 01 - 2

- BaseEntity 클래스 생성
  - BaseEntity
- Entity 클래스 생성
  - Account
  - Customer
- `@EnableJpaAuditing` 적용
  - `@EntityListeners(AuditingEntityListener.class)` 과 같이 사용

### Section 01 - 3

- DTO 생성
  - DTO 생성... @Entity 또는 (VO 등등)의 경우 raw 데이터로 간주하고 dto의 경우 raw 데이터를 담는 그릇이라고 보면된다.(개인)
    - AccountsDto
    - CustomerDto
  - Swagger, Validation annotation 적용
    (참고: [Hibernate는 @Column과 @Size 사용 시 길이를 어떻게 판단할까 / 주의할 점](https://melonturtle.netlify.app/hibernate-column-size/))

### Section 01 - 4

- mapper 클래스 추가 (convert Entity to DTO)
  - AccountsMapper
    - Accounts -> AccountsDto, AccountsDto -> Accounts 변환
  - CustomerMapper
    - Customer -> CustomerDto, CustomerDto -> Customer 변환

### Section 01 - 5

  - Repository 와 Service 생성, 공통 예외 처리 클래스 추가
    - 간단하게 Repository 는 데이터베이스와 연결되어 쿼리수행하는 클래스, Service는 Repository 를 통해 가져온 데이터를 `서비스`하는 클래스
  - Repository
    - AccountsRepository
    - CustomerRepository
    - (참고: [[Java] Optional이란? Optional 개념 및 사용법](https://mangkyu.tistory.com/70))
    - (참고: [[JPA] @Modifying이란? 그리고 주의할점  (벌크 연산)](https://hstory0208.tistory.com/entry/JPA-Modifying%EC%9D%B4%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%A3%BC%EC%9D%98%ED%95%A0%EC%A0%90-%EB%B2%8C%ED%81%AC-%EC%97%B0%EC%82%B0))
    - (참고: [[Spring JPA] @Modifying 어노테이션](https://frogand.tistory.com/174))
  - Service
    - IAccountsService
    - AccountsServiceImpl
  - 예외 클래스
    - CustomerAlreadyExistsException
    - ResourceNotFoundException
  - 공통 String 변수 클래스
    - AccountsConstants

### Section 01 - 6

  - Controller 추가, Swagger 코드 추가
    - AccountsController
  - 요청 및 응답 DTO 추가
    - ErrorResponseDto
    - ResponseDto
  