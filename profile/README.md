# Omni Card
수많은 카드, 수많은 혜택으로 인해 선택의 어려움을 겪는 사용자들을 위해, <br>
하나의 카드에서 소비자 구매내역에 맞추어 혜택이 변경되도록 하는, <br>
AI 기반 맞춤 카드 혜택 플랫폼, **Omni Card** 입니다.

### [🔗 www.omnicard.shop](https://www.omnicard.shop/)
![옴니카드 포트폴리오용 배너](https://github.com/user-attachments/assets/f2b49b03-660d-46b3-820b-d53bea90b3a9)

<br>
<br>
<br>

## :memo: Table of Contents
- [프로젝트 개요](#프로젝트-개요)
- [프로젝트 Repository](#프로젝트-repository)
  - [1. FrontEnd](#1-frontend)
  - [2. BackEnd](#2-backend)
  - [3. DevOps](#3-devops)
- [Frontend](#-frontend)
  - [1. 기술스택](#1-기술스택)
  - [2. 개발 중점사항](#2-개발-중점사항)
- [Backend](#-backend)
  - [1. 기술스택](#1-기술스택-1)
  - [2. erd](#2-erd)
  - [3. 개발 중점사항](#3-개발-중점사항)
- [DevOps](#-devops)
  - [1. Architecture](#1-architecture)
  - [2. 설계 개요](#2-설계-개요)

<br>
<br>
<br>

# 프로젝트 개요
(진행중...)
## Information Architecture
![FE_IA](https://github.com/user-attachments/assets/bb3d3e41-e5a4-44f0-ab7c-a5a972a5d93b)

<br>
<br>
<br>

# 프로젝트 Repository

#### 아직 개발중이기에, 아래 레포지토리의 README.md는 작성이 되어있지 않습니다. 

### 1. FrontEnd

| 이름 | 설명 |
|:---|:---|
| [Omni-FE](https://github.com/2025-Gachon-capstone/Omni-FE) | UI 및 API 통신을 위한 React 레포지토리 |

### 2. BackEnd

| 이름 | 설명 |
|:---|:---|
| [Omni-BE-Gateway](https://github.com/2025-Gachon-capstone/Omni-BE-Gateway) | 라우팅 및 필터를 위한 Spring Gateway 레포지토리 |
| [Omni-BE-User](https://github.com/2025-Gachon-capstone/Omni-BE-User) | 로그인, 로그아웃 등 유저 관련 API Spring 레포지토리 |
| [Omni-BE-Sponsor](https://github.com/2025-Gachon-capstone/Omni-BE-Sponsor) | 스폰서 관련 API Spring 레포지토리 |
| [Omni-BE-Payment](https://github.com/2025-Gachon-capstone/Omni-BE-Payment) | 결제 관련 API Spring 레포지토리 |
| [Omni-BE-Card](https://github.com/2025-Gachon-capstone/Omni-BE-Card) | 카드 관련 API Spring 레포지토리 |
| [Omni-BE-File](https://github.com/2025-Gachon-capstone/Omni-BE-File) | 이미지 저장을 위한 API Spring 레포지토리 |
| [Omni-BE-AI](https://github.com/2025-Gachon-capstone/Omni-BE-AI) | AI 모델 서빙 및 AI 관련 API Flask 레포지토리 |

### 3. DevOps

| 이름(Private) | 설명 |
|:---|:---|
| Omni-Manifest | 애플리케이션 배포를 위한 리소스 정의 GitOps 레포지토리 |
| Omni-Manifest-Tool | Grafana, Prometheus 등 툴을 위한 GitOps 레포지토리 |

<br>
<br>
<br>

# 🌎 Frontend
##  1. 기술스택 
![FE_TechStack](https://github.com/user-attachments/assets/09f8ad86-ea83-4d32-b566-f40027098e42)


## 2. 개발 중점사항
<details>
  <summary> 🗂️ 2-1. FSD 아키텍처 설계</summary>

<hr>

  - 프로젝트 구조를 **기능 중심(Feature-Based)** 으로 설계하여 폴더 및 모듈의 역할을 명확히 분리
  - `app`, `features`, `pages`, `shared`, `widgets` 등으로 구조화해 **확장성 있는 코드베이스 구축**

    ```
      📦src
      ┣ 📂app           // 앱 초기화, 전역 설정
      ┃ ┗ 📂routes
      ┣ 📂features      // 유저 액션 단위 기능
      ┃ ┗ 📂user (예시)
      ┃ ┃ ┗ 📂payment
      ┃ ┃ ┃ ┣ 📂api      // 기능 api
      ┃ ┃ ┃ ┣ 📂model    // 비즈니스 로직
      ┃ ┃ ┃ ┣ 📂type     // 타입 정의
      ┃ ┃ ┃ ┗ 📂ui       // UI 컴포넌트
      ┣ 📂pages          // 실제 라우팅되는 페이지
      ┣ 📂shared         // 공통 UI 컴포넌트, 유틸, 스타일 등
      ┗ 📂widgets        // 페이지 내 조합 단위 UI 블록
    ```
  - 팀원 간 작업 충돌을 줄이고, 기능 단위로 개발이 가능하도록 구성
  - 공통 UI 요소는 `shared`, 사용자 행동 단위 기능은 `features`로 분리하여 **코드 탐색성과 재사용성**을 높임

</details>

<details>
  <summary> ✨ 2-2. Table 컴포넌트 모듈화</summary>

  <hr>

  - Omni-Card 플랫폼 내 다양한 페이지에 반복되는 내역 화면(`Table UI`)을 재사용 가능한 컴포넌트로 분리하여 구현
  - **컬럼 데이터 정의 기반의 동적 렌더링 구조**로, 텍스트뿐 아니라 버튼, 스타일 박스 등의 컴포넌트도 셀에 입력값으로 렌더링 가능
  - 각 컬럼에 **선택적 클릭 이벤트 핸들러**를 연결할 수 있어 사용자 인터랙션 처리에 유연함
  - 결제 내역, 혜택 리스트, 유저 관리 등 여러 페이지에서 동일한 테이블 레이아웃을 사용하면서도 **간단한 커스터마이징만으로 재사용 가능**, 중복 코드 제거 및 유지보수성 향상
</details>

<details>
  <summary> 💳 2-3. Toss 위젯을 활용한 결제 기능 구현</summary>

  <hr>

  - Toss Payments 위젯을 활용해 **가상의 쇼핑몰 결제 시스템 구축**
  - 결제 과정 중 생성되는 **주문 ID, 결제 금액 등 주요 정보를 `Zustand`로 전역 상태로 관리**
  - 결제 완료 시 주문 정보 생성 → 결제 인증 완료 피드백 표시까지의 **엔드 투 엔드 흐름을 구현**
</details>


<br>
<br>
<br>

# 🌱 Backend
## 1. 기술스택
![image](https://github.com/user-attachments/assets/6a5a476c-95d9-482e-8342-14f4b0d705e4)

## 2. ERD
![image](https://github.com/user-attachments/assets/8a52389f-17bb-420c-8778-d595a91d3b09)

## 3. 개발 중점사항
<details>
  <summary> 🧱 3-1. MSA 기반 서비스 분리</summary>

  <hr>

  - **도메인 기반 설계(DDD)** 원칙에 따라 서비스 단위로 기능을 분리하여, 각각의 독립된 애플리케이션으로 구성
  - 각 서비스는 **완전히 독립된 Git 저장소(Repository)** 로 관리
  - 서비스 간 결합도를 낮춰 **유지보수성과 확장성**을 확보

    ```
        📦 gateway-service // API Gateway
        📦 user-service    // 사용자 관리
        📦 card-service    // 카드 등록/조회
        📦 payment-service // 결제 처리
        📦 sponsor-service // 스폰서 혜택 관리
        📦 file-service // 이미지 파일 관리
        📦 ai-service // AI 모델 관리
    ```
  - 각 서비스는 **완전한 자율성과 독립성**을 가지며, 장애 전파 방지 및 수평 확장에 유리한 구조로 운영 가능

</details>

<details>
  <summary> 🔁 3-2. OpenFeign을 활용한 내부 통신</summary>

  <hr>

  - 서비스 간 통신은 `OpenFeign`을 기반으로 인터페이스 선언만으로 구현
  - 각 서비스 간의 의존성은 **HTTP 기반 통신에 국한**되며, 코드 수준의 결합은 없음
  - 통신 중 발생 가능한 예외에 대해 `@FeignClient`의 `fallback` 처리를 통해 **서비스 안정성 확보**
  - Gateway를 통한 경로 분기 및 라우팅 처리로 **외부 요청의 진입점 통제**
</details>

<details>
  <summary> 🔐 3-3. Gateway 기반 인증 처리</summary>

  <hr>

  - JWT 기반 인증을 도입하고, 인증 필터는 `API Gateway`에서 전처리
  - 클라이언트는 Access/Refresh Token을 통해 인증을 진행하며, 각 마이크로서비스에서는 인증 완료된 사용자 정보만 활용
  - 인증 처리 로직은 각 서비스로 공유되지 않으며, **Gateway 레벨에서만 집중 관리**하여 **보안성과 일관성 확보**
</details>

<br>
<br>
<br>

# 🚀 DevOps
## 1. Architecture
<img width="1217" alt="Image" src="https://github.com/user-attachments/assets/69b6022d-b674-481d-92e1-df4b86cae892" />

- 2개의 환경으로 구성
  
  1. `Dev` : React, Spring, Flask, Grafana 등 `GKE 기반 애플리케이션 환경`
     
  2. `DB` : 3개의 도커 컨테이너 DB + GCS(사용자 프로필 사진용)가 존재하는 `Storage 환경`

<br>
<br>
<br>

## 2. 설계 개요

<details> 
  <summary>☁️ 2-1. Cloud Infrastructure</summary>

<hr>

1. 🧾 Terraform 모듈 기반 GCP 멀티 환경 구성
   - 운영 비용 절감을 위해, 총 `3개의 GCP 계정` 사용. - ✨ 각각 300달러 제공 ✨
   - `AI`, `Dev`, `GCSDB` 3개의 환경으로 구분. <br>

     => 직접 구축한 [terraform-google-multi-env](https://github.com/steamedEggMaster/terraform-google-multi-env) 을 통해 효율적으로 멀티 환경 관리.

<br>

2. 👬 IAM Workload Identity
   - k8s의 KSA(Kubernetes Service Account)를 GSA(GCP Service Account)와 매핑하여, <br>
     ✨ 별도의 `Json 파일없이 내부적인 인증`을 수행 ✨ 함으로써 안정성 및 편의성 증가. <br>

     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; => 애플리케이션에서 KSA만 지정하면 리소스 접근 가능.
     
   - 계정 간 매핑도 가능하여, 다른 계정의 리소스(GCS 등)에 `내부적 인증으로 접근 가능한 엄청난 장점` 존재!

<br>

3. 📡 NLB + Ingress 네트워크 구성
   - GCP 프리티어 계정 `최대 IP 할당 개수 8개`를 넘지 않기 위해, <br>
     단일 접속 지점, 네트워크 비용 효율성, 리소스 관리 효율성을 위해, <br>
     `GCP Network LoadBalancer(4 Layer) + Ingress Controller` 조합 사용.

   - NLB는 `TCP 및 IP 기반 Ingress Controller로의 로드밸런싱` 수행. <br>
     Ingress Controller는 `TLS 종료 및 경로, 헤더 기반으로 Service로의 라우팅` 수행. <br>
     Service는 `Pod로 로드 밸런싱` 수행.

     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; => `총 2번의 로드밸런싱`으로 서버 안정성 극대화‼️

<br>

4. 🛤️ GCP Vertex AI 워크로드
   - 🚫 단기간에 AI 워크로드를 구축하는 것은 `경험 및 지식 부족`으로 불가❗️ <br>

     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; => GCP에서 제공하는 `서버리스 AI 워크로드 서비스` 사용.
   
   - GCS와의 연동을 통해 AI 훈련 결과 저장 및 GKE 플라스크 서버가 AI 모델 서빙

<br>

5. 🖥️ DB 서버용 VM Instance
   - 현재 우리 프로젝트에서 `MySQL`, `Neo4j`, `Milvus` 총 3개의 DB가 필요. <br>
     MySQL은 GCP에서 관리형 서비스로 `제공 O`, 나머지는 `제공 X`. <br>
     최대한 관리형으로 알아보고자 했지만, <br>
     `비용적인 측면`, `필요 리소스 부적합` 등 이유로 인해 직접 구축 필요. <br>

     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; => VM에서 Docker 컨테이너로 띄우고, 마운트를 통해 중요 정보들을 저장하자. <br>
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; => 이후 마이그레이션 시, scp 명령어를 통해 데이터 이전.

</details>

<details> 
  <summary>☸️ 2-2. Kubernetes</summary>

<hr>

1. ⚙️ CI/CD 파이프라인
   - Github 친화적인 `Github Actions`로 CI, <br>
     Kubernetes 친화적인 `ArgoCD`로 CD 수행.

     ```
     1. 코드 빌드 및 이미지 빌드
     2. GCP 계정 연결 및 GCR(Google Container Registry)에 저장
     3. sed 명령어를 통해, GitOps를 위한 Manifest 레포지토리에 이미지 태그 변경 후 저장
     4. ArgoCD 접속 후 Refresh 및 Sync로 롤링 배포 수행.
     ```
     
   - 사용 템플릿을 자세히 적어 팀원 간 `의사소통 효율성 및 기술 이해도 증대`. <br>

     - [사용 가이드 노션 바로가기](https://smoggy-twister-652.notion.site/1db9ac17facf801f951adcc5fa9c901f?pvs=4) - 깃허브용

<br>

2. ✉️ k8s 내부 서버 간 통신을 `Service Name`(`내부 FQDN`)을 통해 수행.
   - Service 리소스는 Pod 집합에 대한 `단일 진입점 제공` 및 `로드 밸런싱 기능` 제공.
   - ✨ 경험한 최고의 장점 ✨ <br>
     : `도메인 기반 연결`이기에 애플리케이션 환경변수 변경이 필요없음. -> `마이그레이션 및 운영 효율성` 극대화‼️.

<br>

3. 📊 모니터링 환경
   - `Grafana + Loki + Prometheus` 오픈소스 모니터링 조합을 사용하여, <br>
     비용 절감 및 쿠버네티스 환경의 리소스 관리 안정성, <br>
     여러 장소에서 개발을 수행해야 하는 `팀원들이 참여 가능한 모니터링 환경` 구성.

   - 현재 구성중인 `Custom Dashboard` (지속적인 업데이트 중)
     ```
     📁 Backend
       └─ 📁 Service Overview - 전체 서비스 골든 지표
            └─ 1. RPS
            └─ 2. p99 Response Time (ms)
            └─ 3. 4xx 에러 비율
            └─ 4. 5xx 에러 비율
            └─ 5. Live Threads
    	    └─ 6. JVM CPU Usage
    	    └─ 7. JVM Heap Memory Usage
            └─ 8. JVM Buffer Poll Usage
            └─ 9. GC Preasure
            └─ 10. File Descriptos Usage
     ```

<br>

4. 🔄 GitOps 전략
   - 대부분의 k8s 리소스들은 `GitOps 전략`을 통해 관리하여, <br>
     `운영 안정성` 및 `자동화`에 포커싱.

     - `Deployment, Service, Secret Manifest`를 통한 k8s 리소스 생성
     - `App of Apps + Helm Chart`를 통한 kube-prometheus-stack과 loki-stack 배포
     - `ConfigMap` 리소스를 통한 Grafana Dashboard 선언형 구축
   
   - 폴더 구조
     ```
     Omni-Manifest/
     └── dev
         ├── Omni-BE-AI/
         │   ├── deployment.yaml  # Deployment + Service 리소스 설정
         │   └── secret.yaml      # Container 환경변수 설정값
         ├── Omni-BE-Gateway/
         │   ├── deployment.yaml
         │   └── secret.yaml
         ├── ...
         └── Omni-FE
             ├── deployment.yaml
             └── secret.yaml
     ```
     ```
     Omni-Manifest-Tool/
     └── dev
         ├── Omni-Grafana/
         │   ├── kube-prometheus-stack-charts.yaml    # Grafana + Prometheus
         │   ├── loki-stack-charts.yaml               # Loki + Promtail
         │   ├── Omni-Grafana-Certificate.yaml
         │   └── Omni-Grafana-Ingress.yaml
         └── Omni-Grafana-Dashboards/
             ├── Omni-User-Dashboard.yaml
             └── 계속 추가중...
     ```

<br>

5. 🔐 HTTPS 자동화
   - `Cert-Manager + Ingress Nginx`를 사용하여, <br>
     `HTTPS 적용` 및 `인증서 관리`를 `자동화`.

     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; => Terraform을 이용하여 GKE 생성 시 함께 생성되도록 설정.

</details>

<details>
  <summary>📚 2-3. Micro Service</summary>

<hr>

1. 📄 MSA 구조 설계
   - 앞단의 `Spring Gateway`, 뒷단을 `Spring 마이크로 서비스`들로 구성하여 MSA 구축.

   - 뒷단의 서버에서 로그인을 통해 `JWT Token 생성` 및 `반환`. <br>
     Gateway에서 `Filter`를 통해 `Token 유효성 검증`.
  
   - 브라우저 <-> Gateway <-> MSA 이기에, <br>
     Gateway에서만 `CORS 설정`을 하여 Allow Origin 수행.
  
<br>

2. 🧷 Swagger UI 통합
   - 마이크로 서비스들의 Swagger가 흩어져 있기에, `각각을 직접 보기 힘들다`는 단점 존재.
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; => Gateway에서 `Swagger UI를 통합`하여 보여주자‼️

   - 과정
     1. Gateway 서버의 UI 접속
     2. 마이크로서비스들의 API Docs 경로로 요청을 하는데,
        이때 호스트는 Gateway 접속 도메인으로 설정되어 요청된다.

        - Gateway에서 도메인에 대한 CORS 설정 필요
        - Gateway로 요청이 들어온 것이라 판단하기에, 라우팅 설정 필요
          ```
          globalcors:
            cors-configurations:
              '[/**]':
                allowedOrigins:
                  - ${HTTPS_GATEWAY_URL}
                
          - id: user-service
            uri: ${USER_SERVER_ADDRESS}
            predicates:
              - Path=/user/**

          springdoc:
            swagger-ui:
              urls[0]:
                name: Omni-BE-User
                url: ${HTTPS_GATEWAY_URL}/user/v3/api-docs
          ```
    3. 접속하여 마이크로 서비스별 Swagger 확인.
</details>

<br>
<br>
<br>

# Team

<div align="center">

|**Yoon SeongMun**|**Park YoungJun**|**Ha BeomSu**|**Park ChanYoung**|**Oh SuBin**|
|:-:|:-:|:-:|:-:|:-:|
|<img src="https://avatars.githubusercontent.com/u/137385615?v=4" width="150" height="150"/>|<img src="https://avatars.githubusercontent.com/u/57661519?v=4" width="150" height="150"/>|<img src="https://avatars.githubusercontent.com/u/174731707?v=4" width="150" height="150"/>|<img src="https://avatars.githubusercontent.com/u/149857288?v=4" width="150" height="150"/>|<img src="https://avatars.githubusercontent.com/u/91336314?v=4" width="150" height="150"/>|
| PM/AI/Frontend | AI | Backend     | DevOps        |Frontend/Design|
|[@loading1031](https://github.com/loading1031)|[@illuminateP](https://github.com/illuminateP)|[@Habeomsu](https://github.com/Habeomsu)|[@steamedEggMaster](https://github.com/steamedEggMaster)|[@odukong](https://github.com/odukong)|

</div>
