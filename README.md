# Omni Card
수많은 카드, 수많은 혜택으로 인해 선택의 어려움을 겪는 사용자들을 위해, <br>
하나의 카드에서 소비자 구매내역에 맞추어 혜택이 변경되도록 하는, <br>
AI 기반 맞춤 카드 혜택 플랫폼, **Omni Card** 입니다.

### [www.omnicard.shop](https://www.omnicard.shop/)

<br>
<br>
<br>

## :memo: Table of Contents
- [프로젝트 개요](#프로젝트-개요)
- [프로젝트 Repository](#프로젝트-repository)
  - [1. FrontEnd](#1-frontend)
  - [2. BackEnd](#2-backend)
  - [3. DevOps](#3-devops)
- [DevOps](#-devops)
  - [1. Architecture](#1-architecture)
  - [2. 설계 개요](#2-설계-개요)

<br>
<br>
<br>

# 프로젝트 개요
(진행중...)

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

# 🚀 DevOps
## 1. Architecture
<img width="1217" alt="Image" src="https://github.com/user-attachments/assets/69b6022d-b674-481d-92e1-df4b86cae892" />

- 3개의 환경으로 구성
  
  1. `Dev` : React, Spring, Flask, Grafana 등 `GKE 기반 애플리케이션 환경`
     
  2. `DB` : 3개의 도커 컨테이너 DB + GCS(사용자 프로필 사진용)가 존재하는 `Storage 환경`
     
  3. `AI` : Vertex AI 관리형 워크로드 및 훈련 데이터 저장용 GCS이 존재하는 `AI 훈련 환경`

<br>
<br>
<br>

## 2. 설계 개요

### ☁️ 2-1. Cloud Infrastructure

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

<br>
<br>
<br>

### ☸️ 2-2. Kubernetes

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

<br>
<br>
<br>

### 2-3. 📚 Micro Service

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
