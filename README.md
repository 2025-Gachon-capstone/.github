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

##### Cloud
1. Terraform 모듈 기반 GCP 멀티 환경 구축
   - 운영 비용 절감을 위해, 총 `3개의 GCP 계정` 사용. - 각각 300달러 제공
   - `AI`, `Dev`, `GCSDB` 3개의 환경으로 구분. <br>

     => 직접 구축한 [terraform-google-multi-env](https://github.com/steamedEggMaster/terraform-google-multi-env) 을 통해 효율적으로 멀티 환경 관리.

2. IAM Workload Identity
   - k8s의 `KSA(Kubernetes Service Account)`를 `GSA(GCP Service Account)`와 매핑하여, <br>
     ✨ 별도의 Json 파일없이 `내부적인 인증`을 수행 ✨ 함으로써 안정성 및 편의성 증가. <br>

     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; => 애플리케이션에서 KSA만 지정하면 리소스 접근 가능.
     
   - `계정 간 매핑`도 가능하여, 다른 계정의 리소스(GCS 등)에 `내부적 인증으로 접근 가능한 엄청난 장점` 존재!
  
3. 🌐 네트워크 구성
   - GCP 프리티어 계정 `최대 IP 할당 개수 8개`를 넘지 않기 위해, <br>
     `단일 접속 지점`, `네트워크 비용 효율성`, `리소스 관리 효율성`을 위해 <br>
     `Ingress Controller + GCP Network LoadBalancer(4 Layer)` 조합 사용.

4. GCP Vertex AI 워크로드
   - 단기간에 AI 워크로드를 구축하는 것은 `경험 및 지식 부족으로 불가`. <br>
     => GCP에서 제공하는 `서버리스 AI 워크로드 서비스` 사용.
   - GCS와의 연동을 통해 AI 훈련 결과 저장 및 GKE 플라스크 서버가 AI 모델 서빙

##### Kubernetes
1. ⚙️ CI/CD 파이프라인
   - Github 친화적인 `Github Actions`로 CI를, <br>
     Kubernetes 친화적인 `ArgoCD`로 CD 수행.

     ```
     1. 코드 빌드 및 이미지 빌드
     2. GCP 계정 연결 및 GCR(Google Container Registry)에 저장
     3. sed 명령어를 통해, GitOps를 위한 Manifest 레포지토리에 이미지 태그 변경 후 저장
     4. ArgoCD 접속 후 Refresh 및 Sync로 롤링 배포 수행.
     ```
     
   - 사용 템플릿을 자세히 적어 팀원 간 `의사소통 효율성 및 기술 이해도 증대`. <br>

     - [사용 가이드 노션 바로가기](https://smoggy-twister-652.notion.site/1db9ac17facf801f951adcc5fa9c901f?pvs=4) - 깃허브용

1. k8s 내부 서버 간 통신을 `Service Name`(내부 FQDN)을 통해 수행.
   - Service 리소스는 Pod 집합에 대한 `단일 진입점 제공` 및 `로드 밸런싱 기능` 제공.
   - 경험한 최고의 장점
     : `도메인 기반 연결`이기에 애플리케이션 환경변수 변경이 필요없음. -> `마이그레이션 및 운영 효율성 극대화`됨.

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
     => Terraform을 이용하여 GKE 생성 시 함께 생성되도록 설정.
##### Infrastructure
3. 🧭 MSA 구조 설계
   - 앞단의 Spring Gateway, 뒷단의 서버 분리하여 MSA 구조 구성.
   
   - Service 리소스로부터 생성된 `내부 Domain Name` 기반 라우팅 수행 <br>
     => Gateway와 뒷단 서버들 사이 `로드 밸런싱` 수행.

  
