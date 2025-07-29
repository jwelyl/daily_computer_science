# 2025.07.29 Persistence Context, Entity Manager

## Persistence Context, Entity Manager

### Persistence Context (영속 컨텍스트)

- JPA Entity를 영구 저장하는 환경
- Entity를 Persistence Context에 저장하거나 DB에서 불러오면 해당 Entity는 Persistence Context에 에 의해 관리된다. (Managed)
- Persistence Context에 의해 관리되는 Entity는 다음과 같은 장점이 있다.
    - 1차 캐시
    - 동일성 보장
    - 쓰기 지연
    - 변경 감지
    - 지연 로딩

### Entity Manager

- Persistence Context와 상호작용하여 Entity의 저장, 수정, 삭제, 조회 등을 담당하는 interface
- Entity Manager를 통해 Persistence Context의 Entity를 관리한다.
- Persistence Context는 Entity Manager가 생성될 때 생성되는 논리적인 개념에 가깝다.
- **Entity Manager Factory vs Entity Manager**
    - Entity Manager Factory
        - 연결된 DB 하나 당 하나만 생성하며 Entity Manager를 생성하는 역할을 한다.
        - 애플리케이션 전체에서 공유한다.
        - Thread-safe하다.
    - Entity Manager
        - 하나의 Transaction/DB Connection 당 하나씩 생성되어 Transaction을 처리한다.
        - Thread-safe하지 않으므로 Thread끼리 공유하면 안된다.

### Entity Lifecycle

- Entity의 상태는 4가지가 존재한다.
    - 비영속(New/Transient) : Entity가 갓 생성되어 Persistence Context에 저장된 적 없는 상태, 데이터베이스와 아무 상관이 없는 그냥 Java 객체임 (ID 없음)
    - 영속(Managed) : Persistence Context에 저장되어 관리 받는 상태 (ID 할당 받음)
        - 비영속 상태의 Entity를 persist로 저장하는 경우
        - DB에 저장된 데이터를 조회해 불러오는 경우
    - 준영속(Detached) : Persistence Context에 저장되었다가 분리되어 더 이상 관리받지 않는 상태 (ID 할당된 상태)
        - 준영속 상태의 Entity에서 일어난 변화는 DB에 반영되지 않는다.
    - 삭제(Removed) : Persistence Context에서 제거되어 DB에서도 제거될 예정인 상태 (ID 할당된 상태)
        - 준영속 상태의 Entity와의 차이점은 DB에서도 제거될 예정인것이 다르다.

![entity_life_cycle.png](2025%2007%2029%20Persistence%20Context,%20Entity%20Manager%2023f154b2a3b880c08437ca55a9ae320c/entity_life_cycle.png)