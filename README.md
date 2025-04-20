# Omni Card
### [www.omnicard.shop](https://www.omnicard.shop/)

<br>
<br>
<br>

## :memo: Table of Contents
- [프로젝트 개요](#프로젝트-개요)
- [프로젝트 Repository](#프로젝트-repository)
  - [1. FrontEnd](#1-frontend)
  - [2. BackEnd](#2-backend)
  - [3. AI](#3-ai)
  - [4. DevOps](#4-devops)
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
### 1. FrontEnd
1. [Omni-FE](https://github.com/2025-Gachon-capstone/Omni-FE)

### 2. BackEnd
1. [Omni-BE-Gateway](https://github.com/2025-Gachon-capstone/Omni-BE-Gateway)
2. [Omni-BE-User](https://github.com/2025-Gachon-capstone/Omni-BE-User)
3. [Omni-BE-Sponsor](https://github.com/2025-Gachon-capstone/Omni-BE-Sponsor)
4. [Omni-BE-Payment](https://github.com/2025-Gachon-capstone/Omni-BE-Payment)
5. [Omni-BE-Card](https://github.com/2025-Gachon-capstone/Omni-BE-Card)
6. [Omni-BE-File](https://github.com/2025-Gachon-capstone/Omni-BE-File)

### 3. AI
1. [Omni-BE-AI](https://github.com/2025-Gachon-capstone/Omni-BE-AI)

### 4. DevOps
1. Omni-Manifest
2. Omni-Manifest-Tool

<br>
<br>
<br>

# 🚀 DevOps
### 1. Architecture
<img width="1217" alt="Image" src="https://github.com/user-attachments/assets/97345605-4f4c-447d-8de8-c7cafc50571e" />

<br>
<br>
<br>

### 2. 설계 개요
1. ☁️ GCP 계정 분리 및 Terraform 모듈
   - 운영 비용 절감을 위해, 총 `3개의 GCP 계정`을 사용했습니다. - 각각 300달러 제공
   - `AI`, `Dev`, `GCSDB` 3개의 계정이자 환경으로 구분했으며, <br>
     직접 구축한 [terraform-google-multi-env](https://github.com/steamedEggMaster/terraform-google-multi-env) 을 통해 효율적으로 멀티 환경을 관리합니다.


2. 🌐 네트워크 구성
   - GCP 프리티어 계정 `최대 IP 할당 개수 8개`를 넘지 않기 위해, <br>
     `단일 접속 지점`, `네트워크 비용 효율성`, `리소스 관리 효율성`을 위해 <br>
     `Ingress Controller + Network LoadBalancer` 조합을 사용하였습니다.


3. 🧭 MSA 구조 설계
   - Spring Gateway를 앞단에 두고, 서버들을 분리하여 MSA 구조를 구성하였습니다.
   
   - Service 리소스로부터 생성된 `내부 Domain Name`을 사용하여 라우팅을 수행하였으며, <br>
     이로 인해, Gateway와 뒷단의 서버들 사이 `로드 밸런싱`이 수행됩니다.

4. ⚙️ CI/CD 파이프라인
   - Github 친화적인 `Github Actions`로 CI를, <br>
     Kubernetes 친화적인 `ArgoCD`로 CD를 수행하였습니다.
     
   - 사용 템플릿을 자세히 적어 팀원 간 `의사소통 효율성을 높혔습니다`. <br>

     - [사용 가이드 노션 바로가기](https://smoggy-twister-652.notion.site/1db9ac17facf801f951adcc5fa9c901f?pvs=4) - 깃허브용


5. 🔐 Workload Identity
   - GCP의 `Workload Identity` 기능을 통해, <br>
     별도의 Json 파일 사용 없이, `KSA(K8s Service Account)`가 <br>
     `IAM Service Account`를 통해, `여러 계정의 리소스에 접근 가능`하도록 설정하였습니다.
    

6. 🧠 Serverless Vertex AI
   - AI 서버를 직접 운영하는 대신, <br>
     Serverless 환경인 `Vertex AI`를 사용하여 <br>
     관리 부담을 줄이고, `빠른 훈련 및 배포`가 가능하도록 했습니다.


7. 📊 모니터링 환경
   - `Grafana + Loki + Prometheus` 오픈소스 모니터링 조합을 사용하여, <br>
     비용 절감 및 쿠버네티스 환경의 리소스 관리 안정성,
     여러 장소에서 개발을 수행해야 하는 팀원들이 참여 가능한 모니터링 환경을 구성하였습니다.
   
   - `팀의 운영에 도움이 되는 정보`들을 확인 가능하도록, <br>
     `Custom Dashboard`를 `구축`하기 위해 노력하고 있습니다.

   - 목표 대시보드 구성(업데이트 중)
     ```
     📁 core-backend ## 백엔드 중심
        └─ 📊 user-service
             └─ 1. 전체 요청 수 대비 에러 비율 5xx
             └─ 2. 전체 요청 수 대비 에러 비율 4xx
             └─ 3. 느린 URI Top 5
             └─ 4. 4xx 에러 비율 높은 URI Top 5
             └─ 5. 5xx 에러 비율 높은 URI Top 5
       	     └─ 6. URI별 성공률
      	     └─ 7. RPS
      	     └─ 8. HTTP 상태 코드별 RPS
      	     └─ 9. 분당 요청 수
        └─ 📊 payment-service
        └─ 📊 sponsor-service
        └─ 📊 file-service
        └─ 📊 card-service
        └─ 📊 shared-overview
             ├─ 🔍 전체 서비스의 로그인/회원가입 트렌드
             └─ 🔍 전체 서비스의 에러 비율 비교
      
      📁 core-infra ## 데브옵스 중심
        └─ 📊 node-exporter를 통해 메모리, CPU 관리
        └─ 📊 pod & container 개수 확인
        └─ 📊 JVM + GC + 메모리
     ```
   

8. 🔄 GitOps 전략
   - 대부분의 k8s 리소스들은 `GitOps 전략`을 통해 관리하여, <br>
     `운영 안정성` 및 `자동화`에 집중하고 있습니다.

     - `Deployment, Service, Secret Manifest`를 통한 WAS 배포
     - `App of Apps + Helm Chart`를 통한 kube-prometheus-stack과 loki-stack 배포
     - `ConfigMap` 리소스를 통한 Grafana Dashboard 구축
   
   - 폴더 구조
     ```
     Omni-Manifest/
     └── dev
         ├── Omni-BE-AI/
         │   ├── deployment.yaml
         │   └── secret.yaml
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
         │   ├── kube-prometheus-stack-charts.yaml
         │   ├── loki-stack-charts.yaml
         │   ├── Omni-Grafana-Certificate.yaml
         │   └── Omni-Grafana-Ingress.yaml
         └── Omni-Grafana-Dashboards/
             ├── Omni-User-Dashboard.yaml
             └── 계속 추가중...
     ```

9. 🔐 HTTPS 자동화
   - `Cert-Manager + Ingress Nginx`를 사용하여, <br>
     `HTTPS 적용` 및 `인증서 관리`를 `자동화`하였습니다.
