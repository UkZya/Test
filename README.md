# 분산 자판기 시스템 아키텍처 면접 답변 준비

## Object-Oriented Concepts

### 1. What are the basic concepts for object-orientation?

객체지향의 기본 개념은 4가지입니다:

- **Encapsulation (캡슐화)**: 데이터와 메서드를 하나의 단위로 묶고, 내부 구현을 숨기는 것
- **Inheritance (상속)**: 기존 클래스의 속성과 메서드를 재사용하여 새로운 클래스를 만드는 것
- **Polymorphism (다형성)**: 같은 인터페이스를 통해 다른 구현체를 사용할 수 있는 것
- **Abstraction (추상화)**: 복잡한 시스템에서 핵심적인 개념만을 추출하는 것

### 2. How is the concept of polymorphism implemented in programming language?

다형성은 주로 세 가지 방식으로 구현됩니다:

- **Method Overloading (오버로딩)**: 같은 이름의 메서드를 매개변수만 다르게 정의
- **Method Overriding (오버라이딩)**: 상속받은 메서드를 재정의
- **Interface/Abstract Class**: 인터페이스나 추상 클래스를 통한 구현

**프로젝트 적용 예시:**
제 분산 자판기 시스템에서 Command Pattern을 사용할 때, IRemoteCommand 인터페이스를 통해 다양한 명령(SetPrice, UpdateInventory 등)을 다형적으로 처리했습니다.

### 3. Explain the concept of information hiding.

정보 은닉(Information Hiding)은 모듈의 내부 구현 세부사항을 외부로부터 감추는 설계 원칙입니다.

**목적:**
- 모듈 간 의존성 감소
- 변경의 영향 범위 최소화
- 인터페이스와 구현의 분리

**프로젝트 적용:**
VMC의 Core Microkernel이 내부 구현을 숨기고, 명확한 API만 Plugin Component들에게 노출하여 플러그인 개발자가 내부 복잡도를 알 필요 없도록 했습니다.

---

## UML

### 1. What is the purpose of UML?

UML(Unified Modeling Language)의 목적:
- 시스템의 구조와 동작을 시각화
- 이해관계자 간 의사소통 도구
- 설계 문서화 및 청사진 제공
- 코드 생성의 기반

### 2. What kinds of diagrams are in UML?

UML 다이어그램은 크게 두 가지로 분류됩니다:

**구조 다이어그램 (Structural Diagrams):**
- Class Diagram
- Component Diagram
- Deployment Diagram
- Package Diagram
- Object Diagram
- Composite Structure Diagram

**행위 다이어그램 (Behavioral Diagrams):**
- Use Case Diagram
- Sequence Diagram
- Activity Diagram
- State Machine Diagram
- Communication Diagram
- Timing Diagram
- Interaction Overview Diagram

### 3. List a few behavioral diagrams in UML?

- **Use Case Diagram**: 시스템 기능과 액터의 상호작용
- **Sequence Diagram**: 시간 순서에 따른 객체 간 메시지 교환
- **State Machine Diagram**: 객체의 상태 변화
- **Activity Diagram**: 워크플로우나 비즈니스 프로세스

### 4. What is the difference in use between state diagram and sequence diagram?

**State Diagram:**
- 단일 객체의 상태 변화에 집중
- 이벤트에 따른 상태 전이 표현
- "무엇이 언제 발생하는가"

**Sequence Diagram:**
- 여러 객체 간의 상호작용에 집중
- 시간 순서대로 메시지 흐름 표현
- "누가 누구에게 무엇을 보내는가"

**프로젝트 적용:**
- Sequence Diagram: UC-01 음료 구매의 시스템 시퀀스 다이어그램으로 VMC와 Payment Gateway 간의 메시지 흐름을 표현
- State Diagram: 필요시 자판기의 상태(대기, 결제중, 제품 배출중 등)를 표현할 수 있음

---

## SOLID

### 1. What is the purpose of SOLID?

SOLID 원칙의 목적:
- **유지보수성(Maintainability)** 향상
- **확장성(Extensibility)** 확보
- **재사용성(Reusability)** 증가
- **테스트 용이성** 개선
- 코드의 이해와 수정을 쉽게 만듦

### 2. What are the fundamental concepts of SRP?

**Single Responsibility Principle (단일 책임 원칙):**

"A class should have only one reason to change"
- 하나의 클래스는 하나의 책임만 가져야 함
- 변경의 이유가 하나여야 함
- 높은 응집도(High Cohesion)를 추구

### 3. What is the difference between SRP and cohesion?

**SRP (Single Responsibility Principle):**
- 설계 원칙 (Design Principle)
- "변경의 이유"에 초점
- 책임의 단일성 강조

**Cohesion (응집도):**
- 품질 메트릭 (Quality Metric)
- 모듈 내부 요소들이 얼마나 밀접하게 관련되어 있는가
- 측정 가능한 지표

**관계:** SRP를 잘 지키면 높은 cohesion을 얻을 수 있습니다. SRP는 "왜"를 설명하고, cohesion은 "얼마나"를 측정합니다.

### 4. Explain the concept of SRP with a specific example

**프로젝트 예시 - VMC 구조:**

잘못된 설계 (SRP 위반):
```
class VendingMachine {
    // 재고 관리
    void updateInventory()
    // 결제 처리
    void processPayment()
    // 로그 기록
    void writeLog()
    // UI 표시
    void displayMessage()
}
```

좋은 설계 (SRP 준수):
```
class InventoryManager {
    void updateInventory()
}

class PaymentProcessor {
    void processPayment()
}

class Logger {
    void writeLog()
}

class DisplayController {
    void displayMessage()
}
```

제 시스템에서는 VMC Core를 여러 Plugin Component로 분리하여 각각이 하나의 책임만 갖도록 설계했습니다:
- Payment Plugin: 결제 처리만 담당
- Inventory Plugin: 재고 관리만 담당
- Dispenser Plugin: 제품 배출만 담당

### 5. Explain the concept of DIP with a specific example

**Dependency Inversion Principle (의존성 역전 원칙):**
- 고수준 모듈이 저수준 모듈에 의존하지 않아야 함
- 둘 다 추상화에 의존해야 함
- 추상화는 세부사항에 의존하지 않아야 함

**프로젝트 예시 - Command Pattern 적용:**

잘못된 설계:
```
class VMCCore {
    SetPriceCommand setPriceCmd;
    UpdateInventoryCommand updateInvCmd;
    // 새로운 명령 추가 시 VMCCore 수정 필요
}
```

좋은 설계 (DIP 적용):
```
interface IRemoteCommand {
    void execute();
}

class VMCCore {
    List<IRemoteCommand> commands;
    
    void executeCommand(IRemoteCommand cmd) {
        cmd.execute();
    }
}

class SetPriceCommand implements IRemoteCommand {
    void execute() { /* ... */ }
}

class UpdateInventoryCommand implements IRemoteCommand {
    void execute() { /* ... */ }
}
```

VMCCore는 구체적인 Command 클래스가 아닌 IRemoteCommand 인터페이스에 의존하므로, 새로운 명령 추가 시 VMCCore 수정이 불필요합니다.

### 6. Explain the concept of ISP with a specific example

**Interface Segregation Principle (인터페이스 분리 원칙):**
- 클라이언트는 사용하지 않는 인터페이스에 의존하지 않아야 함
- 큰 인터페이스보다 작고 구체적인 인터페이스가 낫다

**예시:**

잘못된 설계:
```
interface IVendingMachine {
    void dispenseBeverage();
    void processCashPayment();
    void processCardPayment();
    void processMobilePayment();
    void generateSalesReport();
    void updateInventory();
}

// 음료 배출만 필요한 클라이언트도 모든 메서드를 구현해야 함
```

좋은 설계:
```
interface IDispenser {
    void dispenseBeverage();
}

interface IPaymentProcessor {
    void processPayment();
}

interface IReportGenerator {
    void generateReport();
}

interface IInventoryManager {
    void updateInventory();
}
```

각 클라이언트는 필요한 인터페이스만 사용합니다.

### 7. Explain the concept of OCP with a specific example

**Open-Closed Principle (개방-폐쇄 원칙):**
- 확장에는 열려있고, 수정에는 닫혀있어야 함
- 기존 코드 변경 없이 새로운 기능 추가 가능해야 함

**프로젝트 예시 - Microkernel Architecture + Plugin:**

제 분산 자판기 시스템에서 Microkernel Architecture를 채택하여 OCP를 달성했습니다:

```
// Core Microkernel (변경 없음)
class VMCCore {
    Map<String, IPlugin> plugins;
    
    void registerPlugin(IPlugin plugin) {
        plugins.put(plugin.getName(), plugin);
    }
}

// 새로운 기능 추가 시 Core 수정 없이 Plugin만 추가
class FaceRecognitionPlugin implements IPlugin {
    void initialize() { /* ... */ }
    void execute() { /* ... */ }
}

class VoiceOrderPlugin implements IPlugin {
    void initialize() { /* ... */ }
    void execute() { /* ... */ }
}
```

**장점:**
- Core 수정 없이 새로운 Plugin 추가 가능
- Hot Swap을 통한 런타임 업데이트
- OCP 완벽 준수

---

## Architectural Driver

### 1. What is an architecture driver?

Architecture Driver는 아키텍처 설계를 주도하는 핵심 요구사항입니다:

**구성 요소:**
1. **Primary Functionality**: 시스템의 핵심 기능 (Use Cases)
2. **Quality Attribute Scenarios**: 품질 속성 시나리오
3. **Constraints**: 제약사항 (기술, 비용, 일정 등)

이들은 아키텍처 설계 결정에 가장 큰 영향을 미치는 요소들입니다.

### 2. Give a few specific examples of architectural drivers for your system

**제 분산 자판기 시스템의 Architecture Drivers:**

**Primary Functionality:**
- UC-01: 음료 구매 (현장 구매)
- UC-02: 음료 예약/선결제 (모바일 앱)
- UC-03: 예약 음료 픽업 (얼굴 인식)

**Quality Attributes:**
- **QA-01 Performance**: 얼굴 인식 기반 픽업의 종단 간 응답 시간 < 3초 (90% 케이스)
- **QA-02 Reliability**: 서버 장애 시에도 로컬 판매 기능 지속 (99% 가용성)
- **QA-03 Security**: 분산 거래 기록의 위변조 방지 (블록체인 기반)
- **QA-04 Maintainability**: 새로운 기능 추가 시 Core 수정 없이 Plugin만 추가 가능
- **QA-05 Usability**: 앱 예약부터 픽업까지 3단계 이내 완료

**Constraints:**
- VMC는 Linux 기반 임베디드 시스템
- 네트워크 불안정 환경 대응 필요
- 기존 자판기 하드웨어와 호환성 유지

### 3. How can you make sure that the architectural drivers are reflected into the architecture design?

**제가 사용한 방법:**

1. **ADD (Attribute-Driven Design) 프로세스:**
   - 각 QA Scenario에 대해 Candidate Design Approach를 도출
   - 장단점 분석 후 최적 솔루션 선택

2. **Tactics 적용:**
   - Performance: Caching, Load Balancing
   - Reliability: Active Redundancy, Heartbeat
   - Security: Digital Signature, Limit Access
   - Maintainability: Microkernel Architecture

3. **Architectural Pattern 선택:**
   - Microkernel: Maintainability 달성
   - Master-Slave: Reliability와 Synchronization
   - Layered Architecture: Separation of Concerns

4. **Traceability 확보:**
   - 각 설계 결정이 어떤 QA를 만족시키는지 문서화
   - View 별로 어떻게 반영되었는지 추적 가능

---

## Use Case

### 1. What is a use case model?

Use Case Model은 시스템의 기능적 요구사항을 사용자 관점에서 표현한 모델입니다.

**구성 요소:**
- **Actor**: 시스템과 상호작용하는 외부 엔티티
- **Use Case**: 시스템이 제공하는 기능 단위
- **Relationship**: Actor와 Use Case, Use Case 간의 관계

**목적:**
- 이해관계자와의 의사소통
- 기능 요구사항 명세
- 테스트 케이스 도출 기반

### 2. What are criteria for identifying good use cases?

**좋은 Use Case의 기준:**

1. **Actor의 목표 달성**: 사용자의 명확한 목표를 달성
2. **완결성**: 시작부터 종료까지 완전한 흐름
3. **가치 제공**: 액터에게 의미있는 가치 제공
4. **적절한 크기**: 너무 크지도 작지도 않은 크기
5. **독립성**: 다른 use case와 독립적으로 실행 가능
6. **측정 가능성**: 성공/실패를 명확히 판단 가능

**프로젝트 예시:**
- UC-01 "음료 구매": 고객이 "음료를 구입한다"는 명확한 목표
- 너무 세분화하지 않음 (예: "동전 투입", "거스름돈 반환" 등으로 쪼개지 않음)
- 독립적으로 실행 가능하며 가치 제공

---

## QA Scenario

### 1. What are the six parts for describing a quality attribute?

Quality Attribute Scenario는 6가지 요소로 구성됩니다:

1. **Source**: 자극의 출처 (누가/무엇이)
2. **Stimulus**: 자극 (어떤 이벤트)
3. **Artifact**: 영향을 받는 대상 (시스템의 어느 부분)
4. **Environment**: 환경 조건 (어떤 상황에서)
5. **Response**: 시스템의 응답 (어떻게 반응)
6. **Response Measure**: 측정 기준 (얼마나)

### 2. Give a specific sample of QA Scenario for a QA such as performance

**프로젝트 예시 - QAS-01: 얼굴 인식 기반 픽업의 종단 간 응답 시간**

| 요소 | 내용 |
|------|------|
| **Source** | 예약 고객 (Mobile App User) |
| **Stimulus** | VMC 앞에서 얼굴 인식 요청 |
| **Artifact** | VMC + CMS (얼굴 인식 서비스) |
| **Environment** | 정상 운영 상태, 네트워크 지연 100ms 이하 |
| **Response** | 얼굴 인식 완료 후 예약 내역 확인 및 제품 배출 시작 |
| **Response Measure** | 종단 간 응답 시간 3초 이내 (90% 케이스) |

**또 다른 예시 - QAS-02: 서버 장애 시 가용성**

| 요소 | 내용 |
|------|------|
| **Source** | CMS (Central Management System) |
| **Stimulus** | 서버 장애 발생 |
| **Artifact** | VMC (Vending Machine Controller) |
| **Environment** | 피크 시간대 판매 진행 중 |
| **Response** | 로컬 DB를 활용하여 현장 판매 기능 지속 제공 |
| **Response Measure** | 서버 복구 전까지 99% 가용성 유지 |

---

## Architectural Views

### 1. What are architectural views? Why are they necessary?

**Architectural Views의 정의:**
아키텍처를 서로 다른 관점에서 표현한 것으로, 각 이해관계자의 관심사(Concern)를 다룹니다.

**필요한 이유:**

1. **복잡도 관리**: 전체 시스템을 한 번에 이해하기 어려움
2. **이해관계자별 관심사**: 개발자, 운영자, 보안 담당자 등이 각기 다른 정보 필요
3. **Separation of Concerns**: 관심사의 분리를 통한 명확성 확보
4. **의사소통 도구**: 서로 다른 배경의 사람들과 효과적으로 소통

**예시:**
- 개발자: Module View (어떻게 구현할 것인가)
- 운영자: Deployment View (어디에 배포할 것인가)
- 보안 담당자: C&C View (데이터 흐름과 보안 경계)

### 2. List architectural views proposed by CMU SEI.

**CMU SEI의 3가지 주요 View:**

1. **Module View**
   - 코드 단위의 구조
   - 예: Decomposition, Uses, Layered, Data Model

2. **Component-and-Connector (C&C) View**
   - 런타임 구조
   - 예: Pipe-Filter, Client-Server, Peer-to-Peer, Service-Oriented

3. **Allocation View**
   - 시스템과 환경의 매핑
   - 예: Deployment, Implementation, Work Assignment

**프로젝트 적용:**
- Module View: VMC의 Microkernel + Plugin 구조
- C&C View: VMC와 CMS 간의 Master-Slave 구조
- Allocation View: VMC(Edge), CMS(Cloud) 배포 구조

---

## Design Techniques - Component

### 1. What is a component?

**Component의 정의:**
- 독립적으로 배포 가능한 소프트웨어 단위
- 명확한 인터페이스를 통해 서비스 제공
- 내부 구현이 캡슐화됨
- 교체 가능(Replaceable)

**특징:**
- Self-contained
- Well-defined interface
- Encapsulation
- Reusability

### 2. What is the difference between classes and components?

| 측면 | Class | Component |
|------|-------|-----------|
| **추상화 수준** | 낮음 (코드 레벨) | 높음 (아키텍처 레벨) |
| **크기** | 작음 | 크고 여러 클래스 포함 |
| **배포 단위** | 아니오 | 예 (독립 배포 가능) |
| **인터페이스** | Public methods | Well-defined API |
| **런타임** | 객체로 인스턴스화 | 프로세스/서비스로 실행 |
| **관점** | 구현 관점 | 아키텍처 관점 |

**프로젝트 예시:**
- Class: `PaymentProcessor`, `InventoryManager` (개별 클래스)
- Component: `Payment Plugin`, `Inventory Plugin` (여러 클래스를 포함하는 배포 가능한 플러그인)

### 3. What is the difference between components and packages?

| 측면 | Package | Component |
|------|---------|-----------|
| **목적** | 논리적 그룹화 | 독립적 배포 단위 |
| **런타임** | 런타임 개념 아님 | 런타임 엔티티 |
| **인터페이스** | 네임스페이스 제공 | 명확한 API 제공 |
| **의존성** | 컴파일 타임 | 런타임 의존성 |
| **배포** | 소스 코드 조직화 | 독립 배포 |

**프로젝트 예시:**
- Package: `com.vmc.payment` (클래스들의 네임스페이스)
- Component: `PaymentPlugin.jar` (독립적으로 배포되고 실행되는 플러그인)

---

## Design Techniques - Tactics

### 1. What is tactics?

**Tactics의 정의:**
Quality Attribute를 달성하기 위한 설계 기법으로, Pattern보다 더 세밀한 수준의 설계 결정입니다.

**특징:**
- 특정 QA에 초점
- 구체적인 구현 기법
- Pattern의 구성 요소로 사용됨
- 아키텍처 설계 도구

### 2. What is the difference between tactics and patterns?

| 측면 | Tactics | Patterns |
|------|---------|----------|
| **추상화 수준** | 낮음 (구체적) | 높음 (추상적) |
| **범위** | 단일 QA 목표 | 여러 QA 조합 |
| **구성** | 단일 기법 | 여러 tactics 조합 |
| **재사용성** | 특정 컨텍스트 | 일반적 문제 |
| **문서화** | 간단한 설명 | 구조화된 템플릿 |

**예시:**
- Tactic: "Caching", "Heartbeat", "Load Balancing"
- Pattern: "Microkernel" (여러 tactics의 조합)

### 3. Give a few specific examples of tactics for performance

**제 프로젝트에서 사용한 Performance Tactics:**

1. **Caching (캐싱)**
   - 적용: VMC에서 자주 접근하는 재고 정보를 로컬 캐시에 저장
   - 효과: CMS 조회 횟수 감소, 응답 시간 단축

2. **Load Balancing (부하 분산)**
   - 적용: 여러 VMC의 얼굴 인식 요청을 CMS가 분산 처리
   - 효과: 피크 시간대 처리량 증가

3. **Reduce Computational Overhead**
   - 적용: 얼굴 인식 전처리를 VMC에서 수행, CMS는 매칭만 담당
   - 효과: 네트워크 전송량 감소, 전체 지연시간 단축

4. **Bound Queue Sizes**
   - 적용: 이벤트 큐 크기 제한으로 메모리 오버플로우 방지
   - 효과: 안정적인 성능 유지

### 4. Give a few specific examples of tactics for availability/reliability

**제 프로젝트에서 사용한 Reliability Tactics:**

1. **Active Redundancy (능동 중복)**
   - 적용: VMC가 로컬 DB를 유지하여 CMS 장애 시에도 운영
   - 효과: 서버 장애에도 99% 가용성 유지

2. **Heartbeat (하트비트)**
   - 적용: VMC가 주기적으로 CMS에 상태 전송
   - 효과: 장애 신속 감지 및 복구

3. **Exception Handling**
   - 적용: 네트워크 오류 시 로컬 모드로 자동 전환
   - 효과: 서비스 연속성 보장

4. **Monitor (모니터링)**
   - 적용: CMS가 VMC 상태를 실시간 모니터링
   - 효과: 장애 예방 및 조기 대응

5. **Transactions**
   - 적용: 결제 처리에 트랜잭션 적용
   - 효과: 데이터 일관성 보장

---

## Design Techniques - Patterns

### 1. What are patterns?

**Pattern의 정의:**
반복적으로 발생하는 설계 문제에 대한 검증된 해결책입니다.

**구성 요소:**
- Context: 문제가 발생하는 상황
- Problem: 해결해야 할 설계 문제
- Solution: 해결책의 구조
- Consequences: 장단점 및 트레이드오프

**종류:**
- Architectural Patterns (아키텍처 패턴)
- Design Patterns (디자인 패턴)
- Idioms (관용구)

### 2. What are the differences between architectural patterns and design patterns?

| 측면 | Architectural Patterns | Design Patterns |
|------|----------------------|-----------------|
| **범위** | 전체 시스템 구조 | 서브시스템/모듈 내부 |
| **추상화** | 높음 | 중간 |
| **영향** | 전체 시스템 | 특정 컴포넌트 |
| **QA 영향** | 여러 QA에 큰 영향 | 특정 QA에 영향 |
| **예시** | Layered, Microkernel | Strategy, Observer |
| **변경 비용** | 매우 높음 | 상대적으로 낮음 |

**프로젝트 예시:**
- Architectural Pattern: Microkernel Architecture (전체 VMC 구조)
- Design Pattern: Command Pattern (VMC 내부 원격 명령 처리)

---

## Architectural Patterns

### 1. Give a few examples of architecture patterns? What are the advantages of those patterns?

**제 프로젝트에서 사용한 Architectural Patterns:**

**1. Microkernel Architecture**
- **적용**: VMC의 Core + Plugin 구조
- **장점**:
  - 높은 확장성: 새 기능을 Plugin으로 추가
  - OCP 만족: Core 수정 없이 확장
  - Hot Swap: 런타임 업데이트 가능
  - 유지보수 용이

**2. Master-Slave Architecture**
- **적용**: CMS(Master) - VMC(Slave) 구조
- **장점**:
  - 중앙 집중 관리
  - 데이터 일관성 유지
  - 장애 격리
  - 모니터링 용이

**3. Layered Architecture**
- **적용**: VMC의 계층 구조 (Presentation - Business - Data)
- **장점**:
  - Separation of Concerns
  - 독립적 개발/테스트
  - 재사용성
  - 이해 용이

### 2. List a few architectural patterns that are applied in the architectural design

**제 시스템에 적용된 Architectural Patterns:**

1. **Microkernel Architecture**
   - 위치: VMC Core
   - 목적: Maintainability, Extensibility
   - QA: QA-04 (새 기능 추가 시 Core 수정 불필요)

2. **Master-Slave Architecture**
   - 위치: CMS-VMC 간 관계
   - 목적: Centralized Control, Data Synchronization
   - QA: QA-02 (Reliability)

3. **Distributed Deployment Pattern**
   - 위치: 시스템 전체
   - 목적: Scalability, Security
   - QA: QA-03 (Security), Performance

4. **Event-Driven Architecture**
   - 위치: VMC 내부 EventBus
   - 목적: Loose Coupling, Asynchronous Processing
   - QA: QA-04 (Maintainability)

---

## Design Patterns

### 1. What is design pattern?

**Design Pattern의 정의:**
객체지향 설계에서 반복적으로 발생하는 문제에 대한 재사용 가능한 해결책입니다.

**GoF (Gang of Four) Design Patterns:**
- 23개의 검증된 디자인 패턴
- 재사용성, 유지보수성 향상
- 공통 어휘 제공

### 2. For what are GoF patterns used? What quality can be achieved through GoF Patterns?

**GoF Patterns의 목적:**

주로 **Maintainability**와 **Reusability**를 달성합니다:

- **Loose Coupling**: 객체 간 의존성 감소
- **High Cohesion**: 관련 기능을 함께 묶음
- **OCP**: 확장에 열려있고 수정에 닫혀있음
- **DIP**: 추상화에 의존
- **Flexibility**: 런타임에 동작 변경 가능

**달성되는 Quality Attributes:**
- Maintainability (유지보수성)
- Extensibility (확장성)
- Reusability (재사용성)
- Testability (테스트 용이성)

### 3. What are the three categories of GoF design patterns? List a few patterns for each category

**1. Creational Patterns (생성 패턴)** - 객체 생성 메커니즘
- Factory Method
- Abstract Factory
- Builder
- Prototype
- Singleton

**2. Structural Patterns (구조 패턴)** - 클래스/객체 조합
- Adapter
- Bridge
- Composite
- Decorator
- Facade
- Flyweight
- Proxy

**3. Behavioral Patterns (행위 패턴)** - 객체 간 상호작용
- Chain of Responsibility
- Command
- Interpreter
- Iterator
- Mediator
- Memento
- Observer
- State
- Strategy
- Template Method
- Visitor

### 4. Strategy Pattern

**What is strategy pattern?**

Strategy Pattern은 알고리즘군을 정의하고 각각을 캡슐화하여 교환 가능하게 만드는 패턴입니다.

**구조:**
```java
interface IStrategy {
    void execute();
}

class ConcreteStrategyA implements IStrategy {
    void execute() { /* 알고리즘 A */ }
}

class ConcreteStrategyB implements IStrategy {
    void execute() { /* 알고리즘 B */ }
}

class Context {
    IStrategy strategy;
    
    void setStrategy(IStrategy s) {
        strategy = s;
    }
    
    void executeStrategy() {
        strategy.execute();
    }
}
```

**프로젝트 적용:**
제 시스템에서 Command 객체들이 각각 독립적인 실행 로직을 캡슐화하는 Strategy 역할을 수행합니다.

**What is the difference between strategy pattern and command pattern?**

| 측면 | Strategy Pattern | Command Pattern |
|------|-----------------|-----------------|
| **목적** | 알고리즘 교체 | 요청의 캡슐화 |
| **초점** | "어떻게" (How) | "무엇을" (What) |
| **컨텍스트** | 실행 방법 선택 | 요청 큐잉, 로깅, Undo |
| **수신자** | 없음 | 있음 (Receiver) |
| **사용** | 런타임 알고리즘 교체 | 요청-응답 분리 |

**예시:**
- Strategy: 정렬 알고리즘 선택 (QuickSort, MergeSort)
- Command: 리모컨 버튼 (각 버튼이 다른 명령 실행)

**What is the difference between strategy pattern and state pattern?**

| 측면 | Strategy Pattern | State Pattern |
|------|-----------------|---------------|
| **목적** | 알고리즘 교체 | 상태 기반 동작 변경 |
| **컨텍스트** | 클라이언트가 전략 선택 | 상태가 자동 전이 |
| **변경** | 외부에서 변경 | 내부에서 자동 변경 |
| **의존** | 전략들 독립적 | 상태들 서로 인지 |
| **예시** | 압축 알고리즘 선택 | TCP 연결 상태 |

**What is the difference between strategy pattern and observer pattern?**

| 측면 | Strategy Pattern | Observer Pattern |
|------|-----------------|------------------|
| **목적** | 알고리즘 교체 | 일대다 의존성 |
| **관계** | 일대일 | 일대다 |
| **방향** | Context → Strategy | Subject → Observers |
| **통지** | 명시적 호출 | 자동 통지 |
| **예시** | 결제 방식 선택 | 이벤트 리스너 |

### 5. Template Method Pattern

**What is template method pattern?**

Template Method Pattern은 알고리즘의 골격을 정의하고, 일부 단계를 서브클래스에서 구현하도록 하는 패턴입니다.

**구조:**
```java
abstract class AbstractClass {
    // Template Method
    final void templateMethod() {
        step1();
        step2();
        step3();
    }
    
    void step1() { /* 공통 구현 */ }
    abstract void step2(); // 서브클래스가 구현
    void step3() { /* 공통 구현 */ }
}

class ConcreteClass extends AbstractClass {
    void step2() { /* 구체적 구현 */ }
}
```

**What is the difference between strategy pattern and template method pattern?**

| 측면 | Strategy Pattern | Template Method |
|------|-----------------|-----------------|
| **구조** | Composition (위임) | Inheritance (상속) |
| **변경** | 런타임 교체 가능 | 컴파일 타임 고정 |
| **알고리즘** | 전체 교체 | 일부만 변경 |
| **결합도** | 낮음 (인터페이스) | 높음 (상속) |
| **유연성** | 더 유연함 | 덜 유연함 |
| **사용** | 알고리즘 전체 교체 | 알고리즘 일부만 변경 |

**예시:**
- Strategy: 정렬 알고리즘 전체를 다른 것으로 교체
- Template Method: 데이터 처리의 특정 단계만 다르게 구현

### 6. Factory Method Pattern

**What is factory method pattern?**

Factory Method Pattern은 객체 생성 인터페이스를 정의하되, 어떤 클래스의 인스턴스를 생성할지는 서브클래스가 결정하도록 하는 패턴입니다.

**구조:**
```java
abstract class Creator {
    abstract Product factoryMethod();
    
    void operation() {
        Product p = factoryMethod();
        p.use();
    }
}

class ConcreteCreatorA extends Creator {
    Product factoryMethod() {
        return new ConcreteProductA();
    }
}

class ConcreteCreatorB extends Creator {
    Product factoryMethod() {
        return new ConcreteProductB();
    }
}
```

**What is the difference between factory method pattern and abstract factory pattern?**

| 측면 | Factory Method | Abstract Factory |
|------|---------------|------------------|
| **범위** | 하나의 제품 | 제품군 (Family) |
| **메서드** | 하나의 생성 메서드 | 여러 생성 메서드 |
| **구조** | 상속 기반 | 객체 조합 기반 |
| **목적** | 생성 메서드 위임 | 관련 객체군 생성 |
| **복잡도** | 단순 | 복잡 |

**예시:**
- Factory Method: 특정 타입의 Document 생성
- Abstract Factory: GUI 컴포넌트 세트 생성 (Windows, Mac)

### 7. Facade Pattern

**What is Facade pattern?**

Facade Pattern은 복잡한 서브시스템에 대한 단순화된 인터페이스를 제공하는 패턴입니다.

**구조:**
```java
// 복잡한 서브시스템
class SubsystemA { void operationA() {} }
class SubsystemB { void operationB() {} }
class SubsystemC { void operationC() {} }

// Facade
class Facade {
    SubsystemA a = new SubsystemA();
    SubsystemB b = new SubsystemB();
    SubsystemC c = new SubsystemC();
    
    void simpleOperation() {
        a.operationA();
        b.operationB();
        c.operationC();
    }
}

// 클라이언트는 Facade만 사용
Facade facade = new Facade();
facade.simpleOperation();
```

**프로젝트 적용:**
VMC Core가 복잡한 Plugin들에 대한 Facade 역할을 하여 외부에는 단순한 API만 노출합니다.

**What is the difference between Facade pattern and mediator pattern?**

| 측면 | Facade | Mediator |
|------|--------|----------|
| **목적** | 단순화된 인터페이스 | 객체 간 통신 중재 |
| **방향** | 단방향 (Client → Facade) | 양방향 통신 |
| **결합** | 서브시스템 독립적 | 서브시스템이 Mediator 인지 |
| **복잡도** | 외부 복잡도 감소 | 내부 복잡도 감소 |
| **예시** | 라이브러리 래퍼 | 채팅방 중재자 |

**What is the difference between Facade pattern and composite pattern?**

| 측면 | Facade | Composite |
|------|--------|-----------|
| **목적** | 인터페이스 단순화 | 부분-전체 계층 구조 |
| **구조** | 평면적 | 트리 구조 |
| **통일성** | 다른 인터페이스 통합 | 동일 인터페이스 |
| **재귀** | 없음 | 재귀적 구조 |
| **예시** | API 래퍼 | 파일 시스템 |

---

## Design Principles - Cohesion

### 1. What is cohesion?

**Cohesion (응집도):**
모듈 내부의 요소들이 얼마나 밀접하게 관련되어 있는지를 나타내는 척도입니다.

**원칙:** High Cohesion (높은 응집도)을 추구합니다.

**종류 (낮음 → 높음):**
1. Coincidental (우연적)
2. Logical (논리적)
3. Temporal (시간적)
4. Procedural (절차적)
5. Communicational (통신적)
6. Sequential (순차적)
7. Functional (기능적) ← 가장 이상적

### 2. How to measure it for a function, a class, a package, and a component?

**Function (함수) 수준:**
- **LCOM (Lack of Cohesion in Methods)**: 메서드들이 공유하는 변수의 정도
- 하나의 명확한 목적만 수행하는가?
- 모든 변수가 함수 내에서 사용되는가?

**Class (클래스) 수준:**
- **LCOM4**: 메서드들이 인스턴스 변수를 얼마나 공유하는가
- 모든 메서드가 클래스의 목적에 부합하는가?
- SRP를 만족하는가?

**측정 예시:**
```java
// 낮은 응집도
class VendingMachine {
    void dispenseBeverage() {}
    void generateReport() {}  // 다른 책임
    void sendEmail() {}       // 다른 책임
}

// 높은 응집도
class Dispenser {
    void checkInventory() {}
    void dispenseBeverage() {}
    void updateStock() {}
    // 모두 배출 관련 기능
}
```

**Package (패키지) 수준:**
- 패키지 내 클래스들이 공통 목적을 가지는가?
- 패키지 간 의존성보다 내부 의존성이 높은가?
- **Relational Cohesion** = (내부 관계 수) / (가능한 관계 수)

**Component (컴포넌트) 수준:**
- 컴포넌트 내 모듈들이 함께 변경되는가?
- 공통 closure를 가지는가?
- **CCP (Common Closure Principle)**: 함께 변경되는 것들을 함께 묶기

**프로젝트 예시:**
- Payment Plugin: 결제 관련 기능만 포함 (높은 응집도)
- Inventory Plugin: 재고 관련 기능만 포함 (높은 응집도)

---

## Design Principles - Coupling

### 1. What is coupling?

**Coupling (결합도):**
모듈 간의 상호 의존 정도를 나타내는 척도입니다.

**원칙:** Loose Coupling (낮은 결합도)을 추구합니다.

**종류 (높음 → 낮음):**
1. Content (내용) - 최악
2. Common (공통)
3. External (외부)
4. Control (제어)
5. Stamp (스탬프)
6. Data (데이터) - 가장 좋음

### 2. How to measure it for a function, a class, a package, and a component?

**Function (함수) 수준:**
- 매개변수 개수 (적을수록 좋음)
- 전역 변수 사용 여부
- 다른 함수 호출 횟수

**Class (클래스) 수준:**
- **CBO (Coupling Between Objects)**: 다른 클래스와의 결합 수
- **RFC (Response For Class)**: 호출하는 메서드 수
- 의존하는 클래스 수

**측정 예시:**
```java
// 높은 결합도 (나쁨)
class OrderProcessor {
    InventoryManager inv = new InventoryManager(); // 구체 클래스 의존
    PaymentGateway pay = new PaymentGateway();     // 구체 클래스 의존
}

// 낮은 결합도 (좋음)
class OrderProcessor {
    IInventory inv;    // 인터페이스 의존
    IPayment pay;      // 인터페이스 의존
    
    OrderProcessor(IInventory i, IPayment p) {
        inv = i; pay = p;  // 의존성 주입
    }
}
```

**Package (패키지) 수준:**
- **Afferent Coupling (Ca)**: 패키지에 의존하는 외부 클래스 수
- **Efferent Coupling (Ce)**: 패키지가 의존하는 외부 클래스 수
- **Instability = Ce / (Ca + Ce)** (0~1, 낮을수록 안정적)

**Component (컴포넌트) 수준:**
- 컴포넌트 간 인터페이스 수
- 공유 데이터 구조
- **ADP (Acyclic Dependencies Principle)**: 순환 의존 없어야 함

**프로젝트 예시:**
- VMC Core와 Plugin 간: 인터페이스 기반 낮은 결합도
- Plugin 간: 직접 의존 없음 (EventBus 통한 간접 통신)

---

## Design Principles - SOLID

### What is OCP?

**Open-Closed Principle (개방-폐쇄 원칙):**
"Software entities should be open for extension but closed for modification"

- **Open for Extension**: 새로운 기능 추가 가능
- **Closed for Modification**: 기존 코드 변경 불필요

### How to achieve OCP?

**OCP 달성 방법:**

1. **Abstraction (추상화)**
   - 인터페이스나 추상 클래스 사용
   - 구체적 구현에 의존하지 않음

2. **Polymorphism (다형성)**
   - 런타임에 구현체 교체 가능
   - 의존성 주입 활용

3. **Design Patterns**
   - Strategy Pattern
   - Template Method Pattern
   - Decorator Pattern

**프로젝트 예시 - Microkernel + Plugin:**
```java
// Core (변경 없음)
class VMCCore {
    Map<String, IPlugin> plugins;
    
    void registerPlugin(IPlugin plugin) {
        plugins.put(plugin.getName(), plugin);
    }
}

// 새 기능 추가 (Core 수정 없이)
class NewFeaturePlugin implements IPlugin {
    void initialize() {}
    void execute() {}
}
```

### What is SRP?

**Single Responsibility Principle (단일 책임 원칙):**
"A class should have only one reason to change"

- 하나의 클래스는 하나의 책임만
- 변경의 이유가 하나여야 함

### What is the difference between SRP and cohesion?

이전 섹션 "SOLID - 3번" 참조

### What is DIP?

**Dependency Inversion Principle (의존성 역전 원칙):**
"Depend upon abstractions, not concretions"

1. 고수준 모듈이 저수준 모듈에 의존하지 않아야 함
2. 둘 다 추상화에 의존해야 함
3. 추상화는 세부사항에 의존하지 않아야 함

**프로젝트 예시:**
```java
// 나쁜 예 (구체 클래스 의존)
class VMCCore {
    MySQLDatabase db = new MySQLDatabase();
}

// 좋은 예 (추상화 의존)
class VMCCore {
    IDatabase db;
    
    VMCCore(IDatabase database) {
        db = database;
    }
}
```

---

## Refactoring

### 1. What is the goal of refactoring?

**Refactoring의 목표:**

1. **코드 품질 향상**
   - 가독성 개선
   - 유지보수성 향상
   - 복잡도 감소

2. **설계 개선**
   - 높은 응집도
   - 낮은 결합도
   - SOLID 원칙 준수

3. **기능 변경 없음**
   - 외부 동작은 동일
   - 내부 구조만 개선

**핵심:** "동작을 유지하면서 코드 구조를 개선"

### 2. Give a few refactorings to improve cohesion of classes

**Cohesion 향상을 위한 Refactoring:**

1. **Extract Class**
   - 하나의 클래스가 여러 책임을 가질 때
   - 관련 데이터와 메서드를 새 클래스로 추출

```java
// Before
class VendingMachine {
    // 재고 관리
    int stock;
    void updateStock() {}
    
    // 결제 처리
    void processPayment() {}
    
    // 로그 관리
    void writeLog() {}
}

// After
class InventoryManager {
    int stock;
    void updateStock() {}
}

class PaymentProcessor {
    void processPayment() {}
}

class Logger {
    void writeLog() {}
}
```

2. **Move Method**
   - 메서드가 다른 클래스의 데이터를 더 많이 사용할 때
   - 해당 메서드를 적절한 클래스로 이동

3. **Extract Interface**
   - 클래스의 일부 책임만 사용하는 클라이언트가 있을 때
   - 해당 부분을 인터페이스로 추출

### 3. Give a few refactorings to improve coupling of classes

**Coupling 감소를 위한 Refactoring:**

1. **Introduce Parameter Object**
   - 여러 매개변수를 객체로 묶기
   - 데이터 결합도 감소

```java
// Before
void createOrder(String name, int quantity, double price, String address) {}

// After
class OrderInfo {
    String name;
    int quantity;
    double price;
    String address;
}

void createOrder(OrderInfo info) {}
```

2. **Replace Data Value with Object**
   - 원시 타입을 객체로 교체
   - 의미있는 타입 제공

```java
// Before
String phoneNumber;

// After
class PhoneNumber {
    String number;
    boolean isValid() {}
}
```

3. **Hide Delegate**
   - 중간 객체를 통한 위임
   - 직접 결합 제거

```java
// Before
manager = employee.getDepartment().getManager();

// After
manager = employee.getManager();

// Employee class
Manager getManager() {
    return department.getManager();
}
```

### 4. What is the difference between the following refactorings

**Replace Type Code with Class**
- 타입 코드를 클래스로 교체
- 타입 안정성 향상, 유효성 검증 가능

```java
// Before
class Employee {
    static final int ENGINEER = 0;
    static final int MANAGER = 1;
    int type;
}

// After
class EmployeeType {
    private int value;
    static EmployeeType ENGINEER = new EmployeeType(0);
    static EmployeeType MANAGER = new EmployeeType(1);
}

class Employee {
    EmployeeType type;
}
```

**Replace Type Code with Subclass**
- 타입 코드를 서브클래스로 교체
- 타입별 다른 동작이 필요할 때
- Factory Method와 함께 사용

```java
// Before
class Employee {
    int type;
    
    int getPayment() {
        switch(type) {
            case ENGINEER: return 1000;
            case MANAGER: return 2000;
        }
    }
}

// After
abstract class Employee {
    abstract int getPayment();
}

class Engineer extends Employee {
    int getPayment() { return 1000; }
}

class Manager extends Employee {
    int getPayment() { return 2000; }
}
```

**Replace Type Code with State/Strategy**
- 타입 코드를 State/Strategy 패턴으로 교체
- 런타임에 타입 변경이 가능해야 할 때
- 서브클래스 방식보다 유연함

```java
// Before
class Employee {
    int type;
    void changeType(int newType) {
        type = newType; // 서브클래스로는 불가능
    }
}

// After
interface EmployeeType {
    int getPayment();
}

class Engineer implements EmployeeType {
    int getPayment() { return 1000; }
}

class Manager implements EmployeeType {
    int getPayment() { return 2000; }
}

class Employee {
    EmployeeType type;
    
    void setType(EmployeeType newType) {
        type = newType; // 런타임 변경 가능
    }
    
    int getPayment() {
        return type.getPayment();
    }
}
```

**차이점 요약:**

| Refactoring | 타입 변경 | 다형성 | 사용 시점 |
|------------|---------|--------|----------|
| **Type Code with Class** | 불가 | 없음 | 타입 안정성만 필요 |
| **Type Code with Subclass** | 불가 | 있음 | 타입별 동작 다름, 변경 없음 |
| **Type Code with State/Strategy** | 가능 | 있음 | 타입별 동작 다름, 런타임 변경 |

---

## 프로젝트별 구체적 답변

### Architectural Drivers (내 시스템)

**제 분산 자판기 시스템의 핵심 Architectural Drivers:**

1. **UC-01: 음료 구매** - 현장에서 즉시 구매
2. **UC-02: 음료 예약/선결제** - 모바일 앱으로 사전 주문
3. **UC-03: 예약 음료 픽업** - 얼굴 인식 기반 자동 픽업

**Quality Attributes:**
- Performance: 얼굴 인식 3초 이내
- Reliability: 99% 가용성
- Security: 거래 위변조 방지
- Maintainability: Plugin 기반 확장
- Usability: 3단계 이내 완료

### Design Patterns (내 시스템)

**사용한 Design Patterns:**

1. **Command Pattern**
   - 원격 명령(SetPrice, UpdateInventory)을 객체로 캡슐화
   - OCP 만족: 새 명령 추가 시 VMC Core 수정 불필요

2. **Strategy Pattern**
   - Command 객체들이 독립적인 실행 로직을 캡슐화
   - 각 명령이 다른 전략으로 동작

3. **Observer Pattern**
   - EventBus를 통한 비동기 이벤트 처리
   - Plugin 간 느슨한 결합

4. **Decorator Pattern**
   - Digital Signature 기능 추가
   - 기존 명령 로직 변경 없이 보안 기능 확장

### Tactics (내 시스템)

**Performance Tactics:**
- Caching: 재고 정보 로컬 캐시
- Load Balancing: 얼굴 인식 요청 분산

**Reliability Tactics:**
- Active Redundancy: VMC 로컬 DB
- Heartbeat: 주기적 상태 전송
- Monitor: CMS의 VMC 모니터링

**Security Tactics:**
- Digital Signature: 명령 위변조 방지
- Limit Access: 권한 기반 접근 제어

**Maintainability Tactics:**
- Microkernel Architecture
- Hot Swap
- Interface-based Design

---

이렇게 준비하시면 면접에서 충분히 답변하실 수 있을 것 같습니다!

핵심은:
1. **일반적인 개념**은 정확하게 설명
2. **프로젝트 예시**는 실제 보고서 내용을 인용
3. **비교 질문**은 명확한 차이점 제시

혹시 특정 질문에 대해 더 자세한 답변이 필요하시면 말씀해주세요!
