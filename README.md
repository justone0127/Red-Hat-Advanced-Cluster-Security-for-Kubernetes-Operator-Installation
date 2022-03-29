## Red Hat Advanced Cluster Security for Kubernetes v3.69

### 1. Red Hat Advanced Cluster Security for Kubernetes architecture

Red Hat Advanced Cluster for Security for Kubernetes는 다음 구성요소가 포함되어 있습니다.

- Centralized components

- Per-cluster components

- Per-node component

  ![01_components](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/01_components.png)

![02_architecture](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/02_architecture.png)



Red Hat Advanced Cluster for Kubernetes는 OpenShift Container Platform에 Operator형태로 Container-set으로 설치되며 여기에는 여러 구성 요소가 포함됩니다. 이러한 구성 요소를 다음과 같이 분류할 수 있습니다.

- Centralized components

- Per-cluster components

- Per-node component

  

### 1.1 Centralized components

Centralized components는 한 번만 배포하고 동일한 설치를 사용하여 여러 개의 개별 클러스터를 모니터링할 수 있습니다. Red Hat Advanced Cluster Security for Kubernetes에는 다음과 같은 Centralized components가 포함됩니다.

- Central :  Central은 Red Hat Advanced Cluster for Security for Kubernetes의 주요 구성요소이며 Kubernetes 배포로 설치됩니다. 데이터 지속성, API 상호 작용 및 사용자 인터페이스(포털) 액세스를 처리합니다. 동일한 Central 인스턴스를 사용하여 여러 OpenShift Container Platform 또는 Kubernetes 클러스터를 보호할 수 있습니다.
- Scanner : Red Hat Advanced Cluster for Security for Kubernetes에는 Scanner라는 이미지 취약성 스캔 구성요소가 포함되어 있습니다. 모든 이미지 레이어를 분석하여 CVE(Common Vulnerabilities and Exposures) 목록에서 알려진 취약점을 확인합니다. Scanner는 또한 패키지 관리자가 설치한 패키지의 취약점과 여러 프로그래밍 언어에 대한 종속성을 식별합니다.


### 1.2 Per-cluster components

모니터링하려는 각 클러스터에 클러스터별 구성 요소를 배포합니다. Red Hat Advanced Cluster Security for Kubernetes에는 다음과 같은 클러스터별 구성요소가 포함됩니다.

- Sensor : Red Hat Advanced Cluster for Security for Kubernetes는 Sensor 구성 요소를 사용하여 Kubernetes 및 OpenShift Container Platform 클러스터를 모니터링합니다. 정책 감지 및 시행을 위해 OpenShift Container Platform 또는 Kubernetes API 서버와 상호 작용을 처리하고 Collector와 조정합니다.
- Admission controller : Admission controller는 Red Hat Advanced Cluster for Security for Kubernetes의 보안 정책을 위반하는 워크로드를 생성하는 것을 방지합니다.



### 1.3 Per-node components

모니터링하려는 모든 노드에 노드 별 구성 요소를 배포합니다. Red Hat Advanced Cluster Security for Kubernetes에는 다음과 같은 클러스터별 구성요소가 포함됩니다.

- Collector : Collector는 클러스터 노드의 컨테이너 활동을 분석하고 모니터링합니다. 컨테이너 런타임 및 네트워크 활동에 대한 정보를 수집하여 수집된 데이터를 Sensor로 보냅니다.



### 2. Installation activities

Red Hat Advanced Cluster Security for Kubernetes는 다음 방법으로 설치 할 수 있습니다.

- Installing by using an Operator :  Red Hat Advanced Cluster Security Operator를 사용하여 OpenShift Container Platform에 Red Hat Advanced Cluster Security를 설치 할 수 있습니다.
- Installing quickly using Helm charts : Helm Chart를 사용하여 Red Hat Advanced Cluster Security를 설치 할 수 있습니다.
- Installing quickly by using the `roxctl` CLI : `roxctl` CLI를 사용하여 Red Hat Advanced Cluster Security를 설치 할 수 있습니다. 

### 2.1 roxctl CLI Install

아래 내용을 참고하여 Linux 환경에 `roxctl` CLI를 설치할 수 있습니다.

- 최신 버전의 `roxctl` CLI 다운로드 (root 권한으로 실행합니다)

  ```bash
  wget https://mirror.openshift.com/pub/rhacs/assets/3.69.0/bin/Linux/roxctl -O /usr/local/bin/roxctl
  ```

- 실행 권한 부여

  ```bash
  chmod +x /usr/local/bin/roxctl
  ```

- `PATH` 확인

  ```bash
  echo $PATH
  ```

- 설치한 `roxctl` 버전 확인

  ```bash
  $ roxctl version
  3.69.0
  ```

### 2.2 Installing by using an Operator

**2.2.1 Red Hat Advanced Cluster Security for Kubernetes Operator**

- Red Hat Advanced Cluster Security for Kubernetes Operator에는 다음 두 가지 사용자 지정 리소스가 포함되어 있습니다.
  - `Central` : Central Resource는 다음 서비스의 논리적 그룹입니다.
    - Central
    - Scanner
  - `SecuredCluster` : SecuredCluster Resource는 다음 서비스의 논리적 그룹입니다.
    - Sensor 
    - Collector 
    - Admission Controller

**2.2.2 Installing Red Hat Advanced Cluster Security for Kubernetes Operator**

- OpenShift Console 접속 > Operators > OperatorHub 선택

- 검색창에 **Advanced Cluster Security** 검색 

  ![03_acs_operator](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/03_acs_operator.png)

- **Red Hat Advanced Cluster Security for Kubernetes Operator** 선택 > 상세 정보 확인 

  ![04_acs_operator_detail](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/04_acs_operator_detail.png)

  - 설치 모드의 기본값을 **클러스터의 모든 네임스페이스**로 유지
  - **설치된 네임스페이스** 필드에 대해 Operator를 설치할 특정 네임스페이스를 선택, **Red Hat은 rhacs-operators** 네임스페이스에 Kubernetes Operator용 Red Hat Advanced Cluster Security를 설치할 것을 권장
  - **승인 전략**에 대해 자동 또는 수동 업데이트를 선택
    - 자동 업데이트를 선택하면 새 버전의 Operator를 사용할 수 있을 때 OLM(Operator Lifecycle Manager)이 실행 중인 Operator 인스턴스를 자동으로 업그레이드합니다.
    - 수동 업데이트를 선택하면 최신 버전의 Operator를 사용할 수 있을 때 OLM에서 업데이트 요청을 생성합니다. 그런 다음 클러스터 관리자는 해당 업데이트 요청을 수도응로 승인하여 Operator를 새 버전으로 업데이트해야 합니다.

- Install 클릭

  ![06_acs_operator_install](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/06_acs_operator_install.png)

- 확인

  - 설치가 완료되면 **Operator > 설치된 운영자로 이동 > 설치 확인**

    ![05_acs_installed_operators](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/05_acs_installed_operators.png)



### 2.3 Verifying Central Installation

Red Hat Advanced Cluster Security for Kubernetes의 주용 구성요소는 Central입니다. `Central`은 사용자 지정 리소스를 사용하여 OpenShift Container Platform에 `Central`을 설치할 수 있습니다. `Central`은 한 번만 배포하고 동일한 `Central` 설치를 사용하여 여러 개의 개별 클러스터를 모니터링 할 수 있습니다.

- OpenShift Container Platform 4.6 이상을 사용해야 합니다.



**2.3.1 Procedure**

- 콘솔 접속 >  `stackrox`라는 새로운 프로젝트 생성

  ![07_creating_new_project](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/07_creating_new_project.png)

- **Operators > Installed Operators > Advanced Cluster Security for Kubernetes > Central 생성**

  ![08_creating_centrals](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/08_creating_centrals.png)

- 설치 확인

  - **Operators > Installed Operators > Centrals 목록 > stackrox-central-services > 오른쪽에 Admin Credentials Info에서 명령어를 복사하여 패스워드 확인**

    ```bash
    oc -n stackrox get secret central-htpasswd -o go-template='{{index .data "password" | base64decode}}'
    ```

    또는 

    **Workloads > Secrets >  central-htpasswd  확인**
    ![14_stackrox_secret](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/14_stackrox_secret.png)

- **네트워킹 > 라우트 > central 라우트를 통해 RHACS 콘솔 접속**

  ![09_rhacs_console](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/09_rhacs_console.png)



### 2.4 Generating Init Bundle 

보안 클러스터를 생성하기 위해서는 init Bundle을 생성해야 합니다. 보안 클러스터는 이 번들을 사용하여 Central에 인증합니다. roxctl CLI를 사용하거나 RHACS 포털에서 init bundle을 생성할 수 있습니다.

**2.4.1 Generating an init bundle by using the RHACS portal**

- RHACS 포털 접속 > Platform Configuration > Integration > Authentication Tokens  항목 > Cluster Init Bundle 선택 > Generate bundle

  ![10_cluster_init_buldle](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/10_cluster_init_buldle.png)

  ![11_generate_bulde](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/11_generate_bulde.png)

  ![12_generate_bundle_02](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/12_generate_bundle_02.png)

  생성된 Cluster Init Bundle 파일을 다운로드 받습니다. 

  **Download Kubernetes secretes file 선택**

**2.4.2 Creating resources by using the init bundle**

- 위에서 저장한 secret 파일을 저장합니다.  

   파일 이름 : init-bundle.yaml

  ```yaml
  cat <<EOF>> init-bundle.yaml 
  
  # This is a StackRox cluster init bundle.
  # This bundle can be used for setting up any number of StackRox secured clusters.
  # NOTE: This file contains secret data and needs to be handled and stored accordingly.
  #
  #   name:      "test"
  #   createdAt: 2022-03-28T05:20:51.409973868Z
  #   expiresAt: 2023-03-28T05:21:00Z
  #   id:        7f615b16-8fc0-481f-8626-2ecf3d7a6604
  #
  
  apiVersion: v1
  kind: Secret
  metadata:
    annotations:
      init-bundle.stackrox.io/created-at: "2022-03-28T05:20:51.409973868Z"
      init-bundle.stackrox.io/expires-at: "2023-03-28T05:21:00Z"
      init-bundle.stackrox.io/id: 7f615b16-8fc0-481f-8626-2ecf3d7a6604
      init-bundle.stackrox.io/name: test
    creationTimestamp: null
    name: collector-tls
  stringData:
    ca.pem: |
      -----BEGIN CERTIFICATE-----
      MIIB0jCCAXigAwIBAgIUaaNC7BfGjgPPVTkJmONIz+LKGS0wCgYIKoZIzj0EAwIw
      RzEnMCUGA1UEAxMeU3RhY2tSb3ggQ2VydGlmaWNhdGUgQXV0aG9yaXR5MRwwGgYD
      VQQFExM3NzAyNjE3NDg3NDkzOTEwMTQzMB4XDTIyMDMyODA1MDUwMFoXDTI3MDMy
      NzA1MDUwMFowRzEnMCUGA1UEAxMeU3RhY2tSb3ggQ2VydGlmaWNhdGUgQXV0aG9y
      aXR5MRwwGgYDVQQFExM3NzAyNjE3NDg3NDkzOTEwMTQzMFkwEwYHKoZIzj0CAQYI
      KoZIzj0DAQcDQgAEPDwUNTw/61c2Al3zXsslgKGtNMQ4pq2WxiEBkvo35Ap2D+EC
      Xnh18z622ldTsbGVR123WH84ZX7Hj0hb8wjBG6NCMEAwDgYDVR0PAQH/BAQDAgEG
      MA8GA1UdEwEB/wQFMAMBAf8wHQYDVR0OBBYEFGyVu2vYWIxe85Xa7QL8zAHnUikh
      MAoGCCqGSM49BAMCA0gAMEUCIGI3VZtdZ19+jGQ/Q1Ixigcc/l5V79VPah4GTJqc
      DqgXAiEAuU/p4RD35gJ2PykdYdRckMbTTT0CWqa2d8tgVELP9dc=
      -----END CERTIFICATE-----
    collector-cert.pem: |
      -----BEGIN CERTIFICATE-----
      MIICozCCAkmgAwIBAgIJANlqAZ3wbvdUMAoGCCqGSM49BAMCMEcxJzAlBgNVBAMT
      HlN0YWNrUm94IENlcnRpZmljYXRlIEF1dGhvcml0eTEcMBoGA1UEBRMTNzcwMjYx
      NzQ4NzQ5MzkxMDE0MzAeFw0yMjAzMjgwNDIxMDBaFw0yMzAzMjgwNTIxMDBaMIGs
      MS0wKwYDVQQKEyQ3ZjYxNWIxNi04ZmMwLTQ4MWYtODYyNi0yZWNmM2Q3YTY2MDQx
      GjAYBgNVBAsMEUNPTExFQ1RPUl9TRVJWSUNFMUAwPgYDVQQDDDdDT0xMRUNUT1Jf
      U0VSVklDRTogMDAwMDAwMDAtMDAwMC0wMDAwLTAwMDAtMDAwMDAwMDAwMDAwMR0w
      GwYDVQQFExQxNTY2NjMzNjAzMTYxNjk4OTAxMjBZMBMGByqGSM49AgEGCCqGSM49
      AwEHA0IABIZAjtdVeVu4kMd8Da3Ii4++RprwsXtBbOUR3uVNevgYQqS6dhCOiQrk
      IkYQqmnHiVDZ4WcATnQ2rmwKGLXeTKWjgbcwgbQwDgYDVR0PAQH/BAQDAgWgMB0G
      A1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAMBgNVHRMBAf8EAjAAMB0GA1Ud
      DgQWBBTqwypZ8QgbdxzH4NcPcYI6PINgjTAfBgNVHSMEGDAWgBRslbtr2FiMXvOV
      2u0C/MwB51IpITA1BgNVHREELjAsghJjb2xsZWN0b3Iuc3RhY2tyb3iCFmNvbGxl
      Y3Rvci5zdGFja3JveC5zdmMwCgYIKoZIzj0EAwIDSAAwRQIhAOM4DCBNlh+rlKN0
      0M6tqP/vAkmWAHum5koIILbkDAkpAiB/iPQSBOnhis3/COzZfUoxTSuCH8Zz9viw
      s/zVFg0Q5Q==
      -----END CERTIFICATE-----
    collector-key.pem: |
      -----BEGIN EC PRIVATE KEY-----
      MHcCAQEEIJr1xkkLRHi5wbGZTdcSRwJR9cnO300bIFr54IV5KOAxoAoGCCqGSM49
      AwEHoUQDQgAEhkCO11V5W7iQx3wNrciLj75GmvCxe0Fs5RHe5U16+BhCpLp2EI6J
      CuQiRhCqaceJUNnhZwBOdDaubAoYtd5MpQ==
      -----END EC PRIVATE KEY-----
  ---
  apiVersion: v1
  kind: Secret
  metadata:
    annotations:
      init-bundle.stackrox.io/created-at: "2022-03-28T05:20:51.409973868Z"
      init-bundle.stackrox.io/expires-at: "2023-03-28T05:21:00Z"
      init-bundle.stackrox.io/id: 7f615b16-8fc0-481f-8626-2ecf3d7a6604
      init-bundle.stackrox.io/name: test
    creationTimestamp: null
    name: sensor-tls
  stringData:
    ca.pem: |
      -----BEGIN CERTIFICATE-----
      MIIB0jCCAXigAwIBAgIUaaNC7BfGjgPPVTkJmONIz+LKGS0wCgYIKoZIzj0EAwIw
      RzEnMCUGA1UEAxMeU3RhY2tSb3ggQ2VydGlmaWNhdGUgQXV0aG9yaXR5MRwwGgYD
      VQQFExM3NzAyNjE3NDg3NDkzOTEwMTQzMB4XDTIyMDMyODA1MDUwMFoXDTI3MDMy
      NzA1MDUwMFowRzEnMCUGA1UEAxMeU3RhY2tSb3ggQ2VydGlmaWNhdGUgQXV0aG9y
      aXR5MRwwGgYDVQQFExM3NzAyNjE3NDg3NDkzOTEwMTQzMFkwEwYHKoZIzj0CAQYI
      KoZIzj0DAQcDQgAEPDwUNTw/61c2Al3zXsslgKGtNMQ4pq2WxiEBkvo35Ap2D+EC
      Xnh18z622ldTsbGVR123WH84ZX7Hj0hb8wjBG6NCMEAwDgYDVR0PAQH/BAQDAgEG
      MA8GA1UdEwEB/wQFMAMBAf8wHQYDVR0OBBYEFGyVu2vYWIxe85Xa7QL8zAHnUikh
      MAoGCCqGSM49BAMCA0gAMEUCIGI3VZtdZ19+jGQ/Q1Ixigcc/l5V79VPah4GTJqc
      DqgXAiEAuU/p4RD35gJ2PykdYdRckMbTTT0CWqa2d8tgVELP9dc=
      -----END CERTIFICATE-----
    sensor-cert.pem: |
      -----BEGIN CERTIFICATE-----
      MIICtDCCAlmgAwIBAgIJAIA2rV67Nh78MAoGCCqGSM49BAMCMEcxJzAlBgNVBAMT
      HlN0YWNrUm94IENlcnRpZmljYXRlIEF1dGhvcml0eTEcMBoGA1UEBRMTNzcwMjYx
      NzQ4NzQ5MzkxMDE0MzAeFw0yMjAzMjgwNDIxMDBaFw0yMzAzMjgwNTIxMDBaMIGl
      MS0wKwYDVQQKEyQ3ZjYxNWIxNi04ZmMwLTQ4MWYtODYyNi0yZWNmM2Q3YTY2MDQx
      FzAVBgNVBAsMDlNFTlNPUl9TRVJWSUNFMT0wOwYDVQQDDDRTRU5TT1JfU0VSVklD
      RTogMDAwMDAwMDAtMDAwMC0wMDAwLTAwMDAtMDAwMDAwMDAwMDAwMRwwGgYDVQQF
      ExM5MjM4NzYyMzA3OTc2NTY4NTcyMFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE
      Jk8v4AMJgxIh6JIp4+hpVYBTr4w6mV2lDrQcU5Vj7YHFVP6SlUTCfAhuJWbYPZHn
      v+SElt0C1Bs/2lJEkpIHxqOBzjCByzAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYw
      FAYIKwYBBQUHAwEGCCsGAQUFBwMCMAwGA1UdEwEB/wQCMAAwHQYDVR0OBBYEFMNT
      fkTb3KHfGjDBOp+uuRFoiWJGMB8GA1UdIwQYMBaAFGyVu2vYWIxe85Xa7QL8zAHn
      UikhMEwGA1UdEQRFMEOCD3NlbnNvci5zdGFja3JveIITc2Vuc29yLnN0YWNrcm94
      LnN2Y4Ibc2Vuc29yLXdlYmhvb2suc3RhY2tyb3guc3ZjMAoGCCqGSM49BAMCA0kA
      MEYCIQDT9O2un7GOhMKXWI/6WIpn+vWv8G8rHlKnjH86Udj3QAIhAKnLT5H6ZBnz
      61rGRGiOQbtlLnJelsBPGwh3TvHeMJJQ
      -----END CERTIFICATE-----
    sensor-key.pem: |
      -----BEGIN EC PRIVATE KEY-----
      MHcCAQEEIJkNWzRUzGBzneP87GUrvzVEuIrjwvsAZBz27gRhsFsxoAoGCCqGSM49
      AwEHoUQDQgAEJk8v4AMJgxIh6JIp4+hpVYBTr4w6mV2lDrQcU5Vj7YHFVP6SlUTC
      fAhuJWbYPZHnv+SElt0C1Bs/2lJEkpIHxg==
      -----END EC PRIVATE KEY-----
  ---
  apiVersion: v1
  kind: Secret
  metadata:
    annotations:
      init-bundle.stackrox.io/created-at: "2022-03-28T05:20:51.409973868Z"
      init-bundle.stackrox.io/expires-at: "2023-03-28T05:21:00Z"
      init-bundle.stackrox.io/id: 7f615b16-8fc0-481f-8626-2ecf3d7a6604
      init-bundle.stackrox.io/name: test
    creationTimestamp: null
    name: admission-control-tls
  stringData:
    admission-control-cert.pem: |
      -----BEGIN CERTIFICATE-----
      MIICwjCCAmegAwIBAgIIZCID/U1OLYkwCgYIKoZIzj0EAwIwRzEnMCUGA1UEAxMe
      U3RhY2tSb3ggQ2VydGlmaWNhdGUgQXV0aG9yaXR5MRwwGgYDVQQFExM3NzAyNjE3
      NDg3NDkzOTEwMTQzMB4XDTIyMDMyODA0MjEwMFoXDTIzMDMyODA1MjEwMFowgbsx
      LTArBgNVBAoTJDdmNjE1YjE2LThmYzAtNDgxZi04NjI2LTJlY2YzZDdhNjYwNDEi
      MCAGA1UECwwZQURNSVNTSU9OX0NPTlRST0xfU0VSVklDRTFIMEYGA1UEAww/QURN
      SVNTSU9OX0NPTlRST0xfU0VSVklDRTogMDAwMDAwMDAtMDAwMC0wMDAwLTAwMDAt
      MDAwMDAwMDAwMDAwMRwwGgYDVQQFExM3MjE1MzMzOTM5NDU5NTM0MjE3MFkwEwYH
      KoZIzj0CAQYIKoZIzj0DAQcDQgAEkd1CoFut/0auZspCj8E6hdjiSgqTtcp96RY1
      5H2XVsRJ4CHPsu5vyYp52jpm9gMczGy8rfA80YmkMQcb7wCGPKOBxzCBxDAOBgNV
      HQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsGAQUFBwMCMAwGA1Ud
      EwEB/wQCMAAwHQYDVR0OBBYEFGM+o/vpWdAxnP4yeulTbyKUiXfIMB8GA1UdIwQY
      MBaAFGyVu2vYWIxe85Xa7QL8zAHnUikhMEUGA1UdEQQ+MDyCGmFkbWlzc2lvbi1j
      b250cm9sLnN0YWNrcm94gh5hZG1pc3Npb24tY29udHJvbC5zdGFja3JveC5zdmMw
      CgYIKoZIzj0EAwIDSQAwRgIhAJ0fGydvIP2zzvR5B5qXyeCfXazT4Msq3cZEz/SG
      +P6CAiEAp/qUYpIh35duoH0baMoCo9Y0nUvCuAtl1+gaSPeep/w=
      -----END CERTIFICATE-----
    admission-control-key.pem: |
      -----BEGIN EC PRIVATE KEY-----
      MHcCAQEEIG/TdFdgAVenAHSdTISK+p8yUmC3SkeI7U74LokGnAawoAoGCCqGSM49
      AwEHoUQDQgAEkd1CoFut/0auZspCj8E6hdjiSgqTtcp96RY15H2XVsRJ4CHPsu5v
      yYp52jpm9gMczGy8rfA80YmkMQcb7wCGPA==
      -----END EC PRIVATE KEY-----
    ca.pem: |
      -----BEGIN CERTIFICATE-----
      MIIB0jCCAXigAwIBAgIUaaNC7BfGjgPPVTkJmONIz+LKGS0wCgYIKoZIzj0EAwIw
      RzEnMCUGA1UEAxMeU3RhY2tSb3ggQ2VydGlmaWNhdGUgQXV0aG9yaXR5MRwwGgYD
      VQQFExM3NzAyNjE3NDg3NDkzOTEwMTQzMB4XDTIyMDMyODA1MDUwMFoXDTI3MDMy
      NzA1MDUwMFowRzEnMCUGA1UEAxMeU3RhY2tSb3ggQ2VydGlmaWNhdGUgQXV0aG9y
      aXR5MRwwGgYDVQQFExM3NzAyNjE3NDg3NDkzOTEwMTQzMFkwEwYHKoZIzj0CAQYI
      KoZIzj0DAQcDQgAEPDwUNTw/61c2Al3zXsslgKGtNMQ4pq2WxiEBkvo35Ap2D+EC
      Xnh18z622ldTsbGVR123WH84ZX7Hj0hb8wjBG6NCMEAwDgYDVR0PAQH/BAQDAgEG
      MA8GA1UdEwEB/wQFMAMBAf8wHQYDVR0OBBYEFGyVu2vYWIxe85Xa7QL8zAHnUikh
      MAoGCCqGSM49BAMCA0gAMEUCIGI3VZtdZ19+jGQ/Q1Ixigcc/l5V79VPah4GTJqc
      DqgXAiEAuU/p4RD35gJ2PykdYdRckMbTTT0CWqa2d8tgVELP9dc=
      -----END CERTIFICATE-----
  EOF
  ```

-  다음 명령어를 실행하여 리소스를 생성합니다.

  ```bash
  oc create -f init-bundle.yaml -n stackrox
  ```

### 2.5 Installing Secured Cluster Services

`SecuredCluster` 사용자 지정 리소스를 사용하여 클러스터에 보안 클러스터 서비스를 설치 할 수 있습니다. 모니터링하려는 환경의 모든 클러스터에 보안 클러스터 서비스를 설치해야 합니다.

**2.5.1 Process**

- `stackrox` 프로젝트 선택 > Operators > Installed Operators > Red Hat Advanced Security for Kubernetes 선택 > Secured Cluster 선택 > 생성

  ![13_secured_cluster](https://github.com/justone0127/Red-Hat-Cluster-Security-for-Kubernetes-Operator-Installation/blob/main/images/13_secured_cluster.png)



### 3. Verifying installation

설치를 완료한 후에 RHACS  포털로 이동하고 몇 가지 취약한 애플리케이션을 실행하여 보안 평가 및 정책 위한 결과를 평가할 수 있습니다.

- 프로젝트 생성

  ```bash
  oc new-project test
  ```

- 치명적인 취약점이 있는 일부 애플리케이션 시작

  ```bash
  oc run shell --labels=app=shellshock,team=test-team \
    --image=vulnerables/cve-2014-6271 -n test
  ```

  ```bash
  oc run samba --labels=app=rce \
    --image=vulnerables/cve-2017-7494 -n test
  ```

  RHACS는 이러한 배포가 클러스터 제출되는 즉시 보안 위험 및 정책 위반에 대해 자동으로 스캔합니다.



### 4. Operating Red Hat Advanced Cluster Security for Kubernetes

Red Hat Advanced Cluster for Security for Kubernetes를 사용하여 수행할 수 있는 것은 다음과 같습니다.

- `Viewing the dashboard` :  Red Hat Advanced Cluster Security for Kubernetes 실시간 대화식 대시보드에 대한 정보를 찾습니다. 이를 사용하여 모든 호스트, 컨테이너 및 서비스의 주요 지표를 확인 할 수 있습니다.
- `Managing compliance` : CIS, NIST, PCI 및 HIPAA를 비롯한 업계 표준을 기반으로 자동화된 검사를 실행하고 규정 준수를 검증하는 방법을 이해합니다.
- `Managing vulnerabilities` : 개선을 위해 취약성을 식별하고 우선 순위를 지정하는 방법을 배웁니다.
- `Responding to violations` : 정책 위합능ㄹ 확인하고 위반의 실제 원인을 드릴다운하고 시정 조치를 취하는 방법에 대해 알아봅니다.



### 5. Configurating Red Hat Advanced Cluster Security for Kubernetes

- `Adding custom certificates` :  Red Hat Advanced Cluster Security for Kubernetes에서 사용자 지정 TLS 인증서를 사용하는 방법을 알아봅니다. 인증서를 설정한 후 사용자와 API 클라이언트는 인증서 보안 경고를 우회할 필요가 없습니다.
- `Backing up Red Hat Advanced Cluster Security for Kubernetes` : Red Hat Advanced Cluster Security for Kubernetes에 대한 수동 및 자동 데이터 백업을 수행하고 인프라 재해 또는 손상된 데이터의 경우 데이터 복원을 위해 이러한 백업을 사용하는 방법을 알아봅니다.
- `Configuring automatic upgrades for secured clusters` : 각 보안 클러스터에 대한 업그레이드 프로세스를 자동화하여 최신 상태를 유지합니다.

### 6. Integrating with other products

- Integrating with PagerDuty 
- Integrating with Slack 
- Integrating with Sumo Logic 
- Integrating by using the syslog protocol 





참고 URL ) 

https://docs.openshift.com/acs/3.69/welcome/index.html

https://docs.openshift.com/acs/3.69/installing/prerequisites.html#acs-general-requirements_acs-prerequisites (prerequisites)

https://docs.openshift.com/acs/3.69/installing/install-ocp-operator.html#installing-using-an-operator (Operator Installation)
