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
   - 운영 비용 절감을 위해, 총 `3개의 GCP 계정` 사용. - 각각 300달러 제공
   - `AI`, `Dev`, `GCSDB` 3개의 환경으로 구분 <br>
     => 직접 구축한 [terraform-google-multi-env](https://github.com/steamedEggMaster/terraform-google-multi-env) 을 통해 효율적으로 멀티 환경 관리.


2. 🌐 네트워크 구성
   - GCP 프리티어 계정 `최대 IP 할당 개수 8개`를 넘지 않기 위해, <br>
     `단일 접속 지점`, `네트워크 비용 효율성`, `리소스 관리 효율성`을 위해 <br>
     `Ingress Controller + Network LoadBalancer` 조합 사용.


3. 🧭 MSA 구조 설계
   - 앞단의 Spring Gateway, 뒷단의 서버 분리하여 MSA 구조 구성.
   
   - Service 리소스로부터 생성된 `내부 Domain Name` 기반 라우팅 수행 <br>
     => Gateway와 뒷단 서버들 사이 `로드 밸런싱` 수행.

4. ⚙️ CI/CD 파이프라인
   - Github 친화적인 `Github Actions`로 CI를, <br>
     Kubernetes 친화적인 `ArgoCD`로 CD 수행.
     
   - 사용 템플릿을 자세히 적어 팀원 간 `의사소통 효율성 증대`. <br>

     - [사용 가이드 노션 바로가기](https://smoggy-twister-652.notion.site/1db9ac17facf801f951adcc5fa9c901f?pvs=4) - 깃허브용


5. 🔐 Workload Identity
   - GCP의 `Workload Identity` 기능을 통해, <br>
     별도의 Json 파일 사용 없이, `KSA(K8s Service Account)`가 <br>
     `IAM Service Account`를 통해, `여러 계정의 리소스에 접근 가능`하도록 설정.

   - 현재 프로젝트의 경우 AI 계정의 GCS에, GCSDB 계정의 GCS에 대한 Write 권한을 가진다.
    

6. 🧠 Serverless Vertex AI
   - AI 서버를 직접 운영하는 대신, <br>
     Serverless 환경인 `Vertex AI`를 사용하여 <br>
     관리 부담을 줄이고, `빠른 훈련 및 배포`가 가능하도록 구성.


7. 📊 모니터링 환경
   - `Grafana + Loki + Prometheus` 오픈소스 모니터링 조합을 사용하여, <br>
     비용 절감 및 쿠버네티스 환경의 리소스 관리 안정성, <br>
     여러 장소에서 개발을 수행해야 하는 팀원들이 참여 가능한 모니터링 환경 구성.

   - 현재 구성중인 Custom Dashboard (지속적인 업데이트 중)
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
   

9. 🔄 GitOps 전략
   - 대부분의 k8s 리소스들은 `GitOps 전략`을 통해 관리하여, <br>
     `운영 안정성` 및 `자동화`에 포커싱.

     - `Deployment, Service, Secret Manifest`를 통한 WAS 배포
     - `App of Apps + Helm Chart`를 통한 kube-prometheus-stack과 loki-stack 배포
     - `ConfigMap` 리소스를 통한 Grafana Dashboard 구축
   
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

10. 🔐 HTTPS 자동화
   - `Cert-Manager + Ingress Nginx`를 사용하여, <br>
     `HTTPS 적용` 및 `인증서 관리`를 `자동화`.
