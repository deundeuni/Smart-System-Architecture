> **다국어 공개 안내:** 본 문서는 동일 내용의 한/영 이중 공개 문서입니다. v3.6 2026-09-13 (영문: [README_EN.md](README_EN.md))  
> **Original Authority Notice:** 본 기술 명세의 법적·공학적 판단 최상위 기준은 한글 원본(`README.ko.md` / `ARCHITECTURE_STRATEGY.md`)에 귀속되며, 영문본은 보조 참조용으로만 기능한다. (PHILOSOPHY.ko.md is authoritative original)

# 스마트시스템 다중 생존 아키텍처 백서 v3.6 (Full-Stack Resilient Smart System Architecture - 첨단 패키징, 사이버-물리 핸드오프, 온디바이스 엣지 및 동적 모듈러 탈부착 통합판)

> 본 문서는 특정 기업이나 특정 브랜드를 배제하고, 구현 수단이나 제어 주체와 무관하게 시스템 생존성과 경제성을 동시에 확보하기 위한 **물리적·논리적 범용 조립 방식(Modular & Disaggregated Architecture)**의 구조적 차이와 표준화 논리를 다룹니다.  
> 본 문서는 백서 원안의 텍스트 표현물에 CC BY 4.0, 기술 사상 및 방어적 특허 통상실시권 호환성에 DPL v1.0을 이원화 적용하며, 방어적 공개(Defensive Publication)를 목적으로 합니다.

---

## 0. 설계자 독자 아키텍처 및 선행기술 공개 선언 (Designer's Philosophical Declaration)

### 0.1 현장에서 출발한 진짜 동기 (Field-Driven Motivation)
본 아키텍처는 가상의 이론에 머물지 않고 실무 현장의 문제의식에서 출발하였다. 피크 타임에 AI 서비스가 지연·먹통이 되는 현상, 공장 현장에서 노후 제어기가 멈추는 사고, 스마트폰·PC 및 자율주행 모빌리티가 과부하 발열로 스로틀링이 걸리거나 튕기는(Crash) 한계를 직접 목격하며 얻은 원칙은 단순하다. **"볼트 하나가 풀려도 전체 시스템이 무너지지 않게 예비 경로가 있어야 하며, 부품 하나가 쓰러지면 옆 부품이 즉시 바통을 이어받아야 한다."**

### 0.2 용량이 아닌 구조 (Structure over Capacity)
본 아키텍처는 거대 단일 구조(Monolithic)의 파편화된 스펙 경쟁을 거부한다. 실리콘 칩, 첨단 패키징 레이어, 소프트웨어 프로세스, 네트워크 노드, 물리 기구 구동체 등 특정 단위에 결함이 생기거나 동적으로 탈부착될 때도 데이터 손실 및 물리 파손 없이 **무중단(Zero-downtime Fail-over)**으로 생존하는 유기체적 구조를 최우선으로 둔다.

### 0.3 비배타적 상호운용성 및 공용 오픈 표준 (Non-Exclusive Interoperability & Open Public Standard)
본 아키텍처는 특정 주체의 독점적 기술 규격을 지향하지 않으며, 반도체·패키징·소프트웨어·로보틱스 생태계 내 다양한 시스템 유닛(APU, GPU, RISC-V, NPU, OS 커널, 물리 기구 모듈 등)이 제약 없이 유연하게 연동될 수 있도록 최소한의 안전 인터페이스를 제공하는 공용 오픈 표준(Open Public Standard)으로 작동한다. 복수의 연산 및 제어 유닛이 물리적 또는 논리적으로 상호 연결되어 자율 재구성되는 유기체적 구조를 정의한다.

### 0.4 현장 기반 우선순위 제어 원칙 (Operational Priority & Load Management)
과부하 및 예외 상황 발생 시 작업의 긴급도에 따라 선순위와 후순위 작업을 분류하고, 자원 부족 시 후순위 작업을 단계적으로 정지·지연·억제하여 선순위 작업의 연속성을 보장한다.

### 0.5 무중단 생존성 및 오류 격리 (Zero-Downtime Continuity & Isolation)
단일 컴퓨트 유닛, 메모리, 물리 인터페이스의 결함 발생 시에도 데이터 손실이나 전체 시스템 중단(Crash) 없이 동작을 무중단(Zero-downtime Fail-over)으로 지속하는 구조를 최우선 가치로 둔다.

### 0.6 상위 스마트시스템 개념 정의 및 포괄적 적용 범위 (Smart System Scope)
**본 아키텍처에 사용된 '칩렛(Chiplet)' 및 '모듈(Module)' 표기는 단일 반도체 실리콘 다이(Die) 분리체에 국한되지 않으며, 기능적으로 분리되어 상호 통신하고 독립 격리 가능한 모든 물리적·논리적 제어 모듈, 첨단 패키징 블록, 소프트웨어 에이전트, 사이버-물리 시스템(CPS) 및 분산 스마트시스템 전체를 포괄하는 상위 개념으로 정의한다.**  
스마트폰 AP, 공장 산업용 제어기, 클라우드 AI 가속기, 자율주행 모빌리티 ECU, EV 스왑 스테이션, 엣지 AI 서버, 재난 대피 인프라 등 하나 이상에 탑재되는 단일/다중 스마트시스템 전체에 적용된다.

### 0.7 공개 목적 및 한계 고지 (Disclosure Purpose & Limitation Notice)
본 문서는 공익적인 목적으로 아이디어 단계의 구상안을 방어적 선행기술(Prior Art)로 공개하는 자료이다. 본 문서 작성 및 정제 과정에서 활용된 텍스트 변환 도구는 수동적 유틸리티에 국한되며, 기재된 구조, 수치, 소재 적용 방향은 실제 구현 및 검증 과정에서 유연하게 변경될 수 있다.

---

## 1. 두 가지 모듈 및 연산 블록 조립 방식 비교

* **단일 통합형 (Single-Die / Monolithic Integration)** — 하나의 단일 구역(Die/Monolith)에 모든 연산, 제어 및 물리 블록을 통합하여 구성하는 방식입니다. 초기 제조 및 구동 단가가 비교적 저렴하며 블록 간 내부 배선 및 인터페이스 구조가 단순하지만, 특정 영역에 결함이 발생할 경우 시스템 전면 마비 위험이 존재하며 영역별 전원 및 보안 격리에 한계가 존재합니다.
* **조립 분리형 (Chiplet / Modular Integration)** — 기능별로 분리된 개별 칩렛, 소프트웨어 모듈, 첨단 패키징 블록 및 물리 구동체들을 패브릭 Interconnect 및 표준 인터페이스로 상호 연결하는 방식입니다. 특정 블록에 장애가 발생해도 해당 영역만 독자 격리하고 인접, 개별, 팀, 중간 관리자, 또는 중앙 지정 예비 블록으로 연산 바통을 넘기는 자가치유를 지향합니다. 전원, 클럭, 보안, 물리적 제어 영역을 블록 단위로 독립화할 수 있습니다.

---

## 2. 이중 트랙 운용 목적 및 완성차 모듈러 플랫폼 원용 세그먼트 파생

두 방식은 우열의 문제가 아닌, 운용 목적과 적용 환경의 차이에 따라 병행됩니다.

* **보급 및 비용 검증 트랙 (단일 통합형)** — 단가 절감 및 일괄 제조·구동 수율 검증을 위해 활용됩니다.
* **생존 및 보안 현장 트랙 (조립 분리형)** — 데이터 센터, 자율주행, 산업 라인, EV 교환 스테이션, 재난 대피 인프라 등 무중단과 데이터/물리 격리가 필수적인 환경에 적용됩니다. 외부 검증되지 않은 블록의 무단 접근을 완화하고 자체 블록을 독립 격리하여 안전성을 확보합니다.
* **완성차 모듈러 플랫폼 공유 메커니즘 원용 (Modular Segment Scaling)** — 완성차 그룹의 공통 모듈러 플랫폼(E-GMP, MQB 등)이 동일 뼈대 위에서 보급형부터 고성능 세그먼트를 파생시키듯, 본 백서의 핵심 안전 뼈대(TL Bridge, 3-포인트 텔레메트리, 0.1ms E-Stop)를 최하위 공통 규격으로 공유하되, 시장 요구에 따라 보급형(Monolithic/Base)부터 확장형(Chiplet/Extended Fusion)까지 수평 파생되는 구조적 범주를 함께 포괄합니다.

---

## 3. 조립형 아키텍처의 핵심 상호호환 논리, 동적 탈부착 및 그린 엣지 부하 완화

* **고성능 및 범용 우회 구성** — 핵심 제어 본체 블록과 고성능 외부 연산/구동 블록을 TL Bridge로 연결하여 최고 성능을 확보하고, 외부 블록 이탈/장애 시 내장 백업 연산/구동 블록으로 바통을 이행하여 시스템 무중단을 도모합니다.
* **동적 모듈 탈부착 및 핫스왑 대응성 (Dynamic Modular Hot-Plugging Resilience)** — 시스템 가동 중 신규 모듈, 칩렛, 소프트웨어 에이전트 또는 물리 구동체가 동적 결합(Hot-Add)되거나 이탈(Hot-Unplug)될 때, 텔레메트리 오토 디스커버리가 이를 실시간 감지하여 분산 관제탑(CCS)의 신뢰도 투표 및 토폴로지 동적 재구성을 유도합니다. 이탈 시에는 0.1ms 이내에 그레이스풀 테어다운(Graceful Teardown)을 수행하여 데이터 정합성 소실이나 시스템 붕괴 위험을 선제 완화합니다.
* **3-포인트 텔레메트리 & 양방향 백프레셔** — 1) 연산/물리 구동 실시간 상태, 2) 인터커넥트/네트워크 지연시간, 3) 전력·열·물리적 응력 상태를 실시간 감시하고, 과부하 시 역방향 백프레셔 신호를 송출하여 시스템 다운을 선제 차단합니다.
* **분산 관제탑(CCS) 유기적 역할 교대 (Raft 기반 Distributed Governance)** — 물리적·논리적 분산 관제(Many as One)를 적용하여 단일 장애점(SPOF)을 원천 차단합니다. 중앙 관제기 노드 이상 시 100ms 이내(가변 범위 10ms~200ms)에 통제 권한이 인접 관제 노드로 동적 순환 이전됩니다.
* **그린 엣지(Green Edge) 및 전력망 부하 완화** — 단말 및 엣지 국소 추론 분산을 통해 중앙 데이터센터 전력 소모 폭주를 억제하고, 대형 발전 시설(원자력·화력·수력·SMR 등) 과잉 건설에 따른 수생태계 섭란 및 자연 생태계 파괴 리스크 완화를 지향합니다.

---

## 4. 기술 축적 단계, 백혈구 면역 억제, 무단 이송 차단 및 물리·논리 격리 명세

* **1단계 기술 축적** — 외부 고성능 블록을 연결하여 초기 시스템 성능 역량을 확보합니다.
* **2단계 기술 축적** — 보급형 및 현장용 실증 환경에서 자체 내장 블록을 병행 운용함으로써 자가치유 및 제어 노하우를 축적합니다.
* **3단계 기술 축적** — 외부 블록이 완전히 배제된 극단적 장애 상황에서도 시스템이 최소 기능(Failsafe)을 유지하며 작동하는 구조를 완성합니다.
* **대역폭 정속 통제 (갓길 관제) — 동적 대역폭 정속화 제어기 (Rate Limiter)** — 우회 경로(갓길) 내 패킷 폭주(Burst)로 인한 2차 충돌 방지를 위해 Token Bucket Policer를 배치하여 패킷을 정속화합니다. (대역폭 점유율 10%~90% 가변 범위 적용)
* **백혈구 스캔 (스텔스 스캔) — 비동기 무작위 트래픽 스캔 (Random Sampling)** — 메인/우회 버스 및 소프트웨어 데이터 흐름을 비동기로 무작위 추출하여 무한 루프나 결함 패킷 감지 즉시 격리 버퍼로 억류합니다.
* **무단 이송 차단 — 비인가 패킷 이송 차단 회로 (Relocation Interception)** — 승인 없이 진입로 주변에서 대기하는 미검증 인터럽트/제어 요청 감지 즉시 해당 통로를 강제 리셋하고 토큰 소지 모듈에만 권한을 부여합니다.
* **물리·논리적 차단 — 트라이스테이트 및 독립 릴레이 격리 (Tri-State / Isolated Containment)** — 정상 제어권을 비정상 침범하려 할 때, 0.1~10클럭 또는 0.1ms 이내에 물리 버스 절단(High-Z), 소프트웨어 프로세스 강제 종료(Kill), 또는 전원 절단을 집행합니다.
* **자원 점유 억제 (T-Reg) — 자가 치유 자원 점유 제어기 (Self-Healing Suppressor)** — 자가 치유 및 격리 모듈이 시스템 자원을 과도하게 점유하는 현상을 막기 위해, 전력·클럭·버스 점유율이 임계치(기본 예시 15%, 가변 범위 5%~30%)를 초과할 경우 하드웨어적으로 동작을 강제 억제(Rate Limit)합니다.

---

## 5. 독립 Safety IP, 안티탬퍼, 인간-AI 시공간 비대칭성 및 CWP 4대 물리 생존 연계

* **독립 Safety IP 및 수 마이크로초($\mu\text{s}$) 물리 안티탬퍼 무력화** — 메인 연산 코어 및 메인 제어 로직과 물리적으로 완전 분리된 독립 전원/클럭 영역의 Safety IP를 탑재합니다. 칩 물리 분석(Decapsulation, 레이저 스캐닝 등) 공격 감지 시, 수 마이크로초($\mu\text{s}$) 이내에 internal eFuse 과전압 인가 및 Key Zeroization을 실행하여 내부 기밀, 암호화 키 및 커스텀 AI 모델 웨이트를 영구 물리 파괴하는 하드웨어 보호를 수행합니다.
* **인간-AI 시공간 비대칭성 기반 2단계 스케줄링 (Cognitive Read-Time Window & Early Exit)** — 인간 사용자의 1~2초 인지·독해 시간과 GHz NPU/연산기의 시공간 비대칭성을 활용하여, 과부하 시 사유 유휴 창(Reasoning Window)을 동적으로 확장하고 칩셋 열 식히기(Thermal Relaxation)를 집행합니다. 단, 지연 할당 대비 검증 정확도 향상이 실증되지 않을 경우 연산 실패로 규정하고 수 ms 이내에 조기 절단(Early Exit)을 집행하는 가변 스케줄링을 포함합니다.
* **CWP 4대 물리 인프라 메커니즘 연동 (L0 Physical Survival)**
  * 진입 유도 (`CWP-Entry`) — 가이드 홈 및 라인 레이저 기반 진입 오차 완화.
  * 오차 흡수 (`CWP-Rolling-Self-Align-Battery-Swap-System`) — V홈 및 캐스터 정렬 메커니즘을 통한 ±5mm 오차 물리 흡수.
  * 저충격 도킹 (`CWP-Battery-Swap`) — N/(N+1) 차동 감속(60T/61T 기어비) 및 회전형 스테이지를 통한 극저속 접속.
  * 무전원 고정 (`CWP-Clamping-Battery-Swap-System`) — EPM 전자기 무전원 자력 체결 및 0.1ms 물리 비상 해제.  
  *(상기 물리 메커니즘은 EV 배터리 팩 및 500kg 이상 범용 중량 모듈러 주택, 대피소, 농기계, 물류 파렛트에 포괄 적용됩니다.)*

---

## 6. 스마트시스템 구현 수단 무관 범용 구조, 다층 제어 및 수송 무관성 포괄 (soma-moa 로드맵 v3.6)

본 규격은 특정 하드웨어 위치나 단일 제어 알고리즘에 국한되지 않으며, **"기능 모듈 분리 및 상태 검증을 통한 이상 격리 및 자가치유"**라는 범용 구조적 목적을 달성하는 모든 기술적 수단, 소프트웨어 정의 체계, 물리 구동체 및 제어 토폴로지를 동일한 선행기술 범주로 포괄 정의합니다.

* **실체적 하드웨어 및 시스템 구현체 연계 (System Implementation Reference)** — 본 아키텍처의 대표 구현 예시로서 CWP 4대 하드웨어 메커니즘(`CWP-Entry`, `CWP-Rolling-Self-Align-Battery-Swap-System`, `CWP-Battery-Swap`, `CWP-Clamping-Battery-Swap-System`), 연산 생존 제어기(`chiplet-apu-multi-system-survival-architecture`), 피난 유도 인프라(`LAST-LIGHT`), 공간 HMI (`POLYLINK-HUD`) 및 엣지 생존 백서(`ON_DEVICE_EDGE_SURVIVAL_PAPER`)를 상호 참조합니다.
* **첨단 패키징 및 인터포저 레벨의 물리 격리 (Advanced Packaging Containment)** — 단일 칩 레일을 넘어 2.5D/3D 실리콘 인터포저, TSV(실리콘 관통 전극), 마이크로 범프 및 하이브리드 본딩 인터커넥트 구조를 포괄합니다. 패키징 내부 이상 섭란 감지 시 패키지 기판 하단 하드웨어 격리 회로가 0.1ms 이내에 해당 채널의 전원을 물리 차단(Power Gating)하거나 고임피던스(High-Z)로 전환하여 결함 확산을 완화합니다.
* **사이버-물리(Cyber-Physical) 경계면 핸드오프 동기화** — 연산 소프트웨어 계층(Cyber)의 Raft 분산 관제 이관 및 AI 에이전트 신뢰도 투표 결과가 L0 물리 인프라 계층(Physical — CWP 메커니즘, PMIC E-Stop)으로 하드웨어 버스를 통해 직결 전송되며, 논리 상태와 물리 액츄에이터 구동 상태를 실시간 교차 검증하는 프로토콜을 포함합니다.
* **임의 네트워크 수송 레이어 무관성 (Universal Transport Protocol Agnostic)** — 5G/6G/7G, 저궤도(LEO)/정지궤도(GEO) 위성 통신, 양자 전송망, 광 파이버, P2P 메쉬망 및 에어갭(Air-Gap) 망분리 환경 등 전송 매체 종류에 종속되지 않고 단말 내부 오프라인 결정을 최우선 집행하는 구조를 포괄합니다.
* **실행 계층 무관성 (Layer-Agnostic Architecture)** — 하드웨어 계층(마이크로코드, FW, IOMMU/MMU, TL Bridge, 물리 클러치), 시스템 소프트웨어 계층(OS 커널, 드라이버, 하이퍼바이저, 스케줄러, 컨테이너), 메모리/자원 제어 계층, 상위 소프트웨어/AI 계층, 광학/포토닉스 계층(CPO, 광 인터커넥트), 양자 계층(양자 얽힘, 양자 센싱), 미래 물리/논리 매체 계층(테라헤르츠, 분자 소자)을 포괄합니다.
* **제어 토폴로지 무관성 (Topology-Agnostic Control)** — 개별 노드 자율 통제, 팀 내부 자체 통제, 중간 관리자 제어, 중앙 관리 관제(CCS), 수평적 P2P 피어 통제, 다층 트리 및 매트릭스 하이브리드 통제를 포괄합니다.
* **지역 인접 선조치 및 지연 완화 (Localized Proximity Preemptive Action)** — 이상 탐지 시 중앙 제어 지연을 완화하기 위해 가장 가까운 인접 노드가 0.1ms급 1차 국소 격리를 선제 실행한 후 상위 시스템으로 상안 보고하는 구조입니다.
* **예지적 선조치 및 예측 격리 (Predictive Preemptive Action)** — 텔레메트리 누적 데이터, 시계열 동적 패턴 학습, 미세 전압·온도 섭란 감지를 통해 실제 파손 및 오류 발생 전 이상 징후를 감지하여 유휴 블록으로 미리 바이패스하거나 사전 격리를 실행하는 구조를 포함합니다.
* **범용 구조 포괄 원칙** — 구현 스택의 위치(HW/SW/AI/OPTICAL/QUANTUM/FUTURE), 제어 주체 단위(개별/팀/중간관리자/중앙/P2P), 제어 절차 순서, 또는 제어 시점(사후/사전)과 상관없이, 시스템 모듈의 결함 감지 시 이상 구역을 격리하고 무중단 자가치유를 도모하는 모든 구성은 본 규격의 선행기술 범주에 포함됩니다.

---

## 7. 실리보호 (Practical Protection)

* **원안 우선 원칙:** 본 명세서의 법적·기술적 해석은 한국어 원본(`README.ko.md` / `ARCHITECTURE_STRATEGY.md`)을 최우선 기준으로 적용하며, 영문본 및 기타 언어 번역본은 참고용으로만 기능한다.
* **범위 포괄성:** 본 문서에 기술된 조립 방식, TL Bridge 연동, 다층 제어 토폴로지, 첨단 패키징 격리, 사이버-물리 핸드오프, 동적 모듈 탈부착 핫스왑, 인간-AI 시공간 비대칭성 사유 유휴 창 제어, 수 $\mu\text{s}$ 안티탬퍼 및 0.1ms 국소 격리 구성을 포함한 모든 상위 개념은 광범위한 선행기술 선점을 위한 원용 범위로 포괄 적용된다.
* **사업화 내용 분리:** 본 백서 원안에는 Pure Open Source 및 선행기술 개시 내용만을 포함하며, 독자적인 수익 모델 및 사업화 세부 실행안은 별도 기술 문서로 분리 관리한다.
* **구현 유연성 및 시장 맞춤형 확장 선언 (Design-to-Cost Flexibility):** 본 명세서의 하드웨어 구성 및 레이어 구조는 최적 성능을 발휘하는 일 실시예를 예시한 것이다. 실제 양산 및 현장 적용 환경에서는 시장의 수요, 목적, 경제성 및 운용 조건에 따라 특정 모듈의 선택적 생략, 축소, 스케일링 또는 커스텀 최적화가 유연하게 가능하며, 이러한 기능적 변형 및 등가 구현 역시 본 선행기술 공개 범주에 포괄 적용된다.
* **저작권 및 특허 라이선스 이원화:** 본 백서 텍스트 표현물의 저작권에는 **CC BY 4.0**이 적용되며, 개시된 기술 사상 및 방어적 특허 통상실시권 호환성에는 **DPL v1.0 (Defensive Patent License v1.0)**을 이원화하여 포괄 적용한다.

---

## 8. 출처 및 기록 (Sources & Records)

* **소마모아 생태계 저장소 및 학술 식별자 (Ecosystem Repositories & DOIs)**
  * 최상위 범용 생존 아키텍처 마스터 허브 (`smart-system-multi-survival-architecture`) — GitHub: `deundeuni / smart-system-multi-survival-architecture` | (신규 Release 발행 시 Zenodo DOI 자동 발급 예정)
  * 최상위 거점 관문 및 메인 저장소 (`soma-moa`) — GitHub: `deundeuni / soma-moa` | CERN Zenodo DOI: `10.5281/zenodo.22435773` | 관문 도메인: `somamoa.ai.kr`
  * 연산 생존 제어기 (`chiplet-apu-multi-system-survival-architecture`) — GitHub: `deundeuni / chiplet-apu-multi-system-survival-architecture` | CERN Zenodo DOI: `10.5281/zenodo.22374987`
  * 엣지·온디바이스 자율 연산 백서 (`ON_DEVICE_EDGE_SURVIVAL_PAPER`) — GitHub: `deundeuni / ON_DEVICE_EDGE_SURVIVAL_PAPER` | (Release 발행 시 Zenodo DOI 발급 예정)
  * 공간 HMI 게이트웨이 (`POLYLINK-HUD`) — GitHub: `deundeuni / POLYLINK-HUD` | CERN Zenodo DOI: `10.5281/zenodo.22726318`
  * 재난 피난 유도 & 보조 인프라 (`LAST-LIGHT`) — GitHub: `deundeuni / LAST-LIGHT` | CERN Zenodo DOI: `10.5281/zenodo.22373189`
  * 선행 광학 인지 인프라 (`FIRST-LIGHT`) — GitHub: `deundeuni / FIRST-LIGHT` | CERN Zenodo DOI: `10.5281/zenodo.22683225`
  * 단말 착용형 열응력 완화 모듈 (`MAX-LIFE-ICE-BELT`) — GitHub: `deundeuni / MAX-LIFE-ICE-BELT` | CERN Zenodo DOI: `10.5281/zenodo.22373686`
  * CWP 진입 유도 정렬 (`CWP-Entry`) — GitHub: `deundeuni / CWP-Entry` | CERN Zenodo DOI: `10.5281/zenodo.22683234`
  * CWP 배터리 교환 도킹 (`CWP-Battery-Swap`) — GitHub: `deundeuni / CWP-Battery-Swap` | CERN Zenodo DOI: `10.5281/zenodo.22373538`
  * CWP 전자기 클램핑 (`CWP-Clamping-Battery-Swap-System`) — GitHub: `deundeuni / CWP-Clamping-Battery-Swap-System` | CERN Zenodo DOI: `10.5281/zenodo.22373722`
  * CWP 롤링 셀프얼라인 (`CWP-Rolling-Self-Align-Battery-Swap-System`) — GitHub: `deundeuni / CWP-Rolling-Self-Align-Battery-Swap-System` | CERN Zenodo DOI: `10.5281/zenodo.22373704`
* **법적 근거 및 적용 라이선스 규정 (Legal Statutes & Licenses)**
  * 대한민국 특허법 제103조 — 선사용에 의한 통상실시권
  * 미국 특허법 35 U.S.C. §273 — Defense to Infringement Based on Prior Commercial Use
  * 적용 라이선스: CC BY 4.0 & DPL v1.0 (Defensive Patent License v1.0)
  * 기술적 기반 참조 표준: UCIe, CXL, TL-UL 등 모듈러 Interconnect 오픈 표준을 참조한 생존형 확장 규격

---

### Appendix A: Inventorship
* **System Architect & Sole Inventor:** deundeuni
* **Primary Repository:** github.com/soma-moa
* **License:** CC BY 4.0 (Attribution Required) + DPL v1.0

### Appendix B: Version History
* **v3.5:** ON_DEVICE 백서 핵심 5선 이식 통합 개정
* **v3.6:** 4절 누락 청구 항목(무단 이송 차단 회로 및 트라이스테이트/독립 릴레이 물리·논리 격리) 명시적 복원, 12개 생태계 저장소 목록 정리 및 출처 정합 완비 (DOI 미발급 2개 항목 발급 대기 상태 반영)

### Appendix C: AI Assistance Disclosure
* **Original Architecture & Concepts:** deundeuni (Human) - Sole Inventor, 전체 구상 및 의사결정 주체
* **Technical & Legal Drafting Support:** Generic Generative AI Text Refinement & Structuring Tools (범용 생성형 AI 텍스트 정제 및 구조화 도구)

*본 고지는 역할 투명성을 위한 것이며, AI 프롬프트 및 내부 추론 과정은 공개하지 않는다. 모든 최종 결정과 지식재산권(IP) 소유권은 원안자(deundeuni / soma-moa)에게 귀속된다.*
