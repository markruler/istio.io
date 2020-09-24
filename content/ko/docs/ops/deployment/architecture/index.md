---
title: 아키텍처
description: 이스티오의 높은 수준 아키텍처와 설계 목표를 설명한다.
weight: 10
owner: istio/wg-environments-maintainers
test: n/a
---

이스티오 서비스 메시는 논리적으로 **데이터 플레인**과 **컨트롤 플레인**으로
분리된다.

* **데이터 플레인**은 사이드카로 배포된 지능형
  프록시([Envoy](https://www.envoyproxy.io/))의 집합으로 구성된다.
  이러한 프록시는 마이크로서비스 사이에 모든 네트워크 통신을 조정하고 제어한다.
  또한 모든 메시 트래픽에 대한 원격 텔레메트리를 수집하고 보고한다.

* **컨트롤 플레인**은 트래픽을 라우팅하기 위해 프록시를 관리하고 구성한다.

다음 다이어그램은 각 플레인을 구성하는 다양한 구성 요소를 보여준다.

{{< image width="80%"
    link="./arch.svg"
    alt="이스티오 기반 애플리케이션의 전체 아키텍처."
    caption="이스티오 아키텍처"
    >}}

## 구성 요소

이번 섹션에서는 이스티오의 각 핵심 구성 요소에 대한 간략한 개요를 제공한다.

### 엔보이(Envoy)

이스티오는 확장된 버전의
[엔보이](https://envoyproxy.github.io/envoy/) 프록시를 사용한다.
엔보이는 서비스 메시의 모든 서비스에 대한 모든 인바운드와 아웃바운드
트래픽을 조정하기 위해 C++로 개발된 고성능 프록시다.
엔보이 프록시는 데이터 플레인 트래픽과 상호 작용하는
유일한 이스티오 구성 요소다.

엔보이 프록시는 서비스에 사이드카로 배포되며,
예를 들어 다음과 같은 엔보이의 다양한 내장 기능으로 서비스를
논리적으로 확장한다.

* 동적 서비스 디스커버리
* 로드 밸런싱
* TLS 종료
* HTTP/2 및 gRPC 프록시
* 회로 차단기 (Circuit breakers)
* 상태 점검 (Health checks)
* 백분율(%)을 기반으로 트래픽이 분할된 단계별 롤아웃
* 결함 주입 (Fault injection)
* 풍부한 메트릭

이 사이드카 배포는 이스티오가 정책 결정을 시행하고 풍부한 텔레메트리를
추출할 수 있도록 하며, 이것은 전체 메시의 행동에 대한 정보를 제공하기 위해 모니터링
시스템으로 보내질 수 있다.

사이드카 프록시 모델도 코드를 다시 설계하거나 작성할 필요 없이 기존 배포에
이스티오 기능을 추가할 수 있다.

엔보이 프록시가 활성화한 일부 이스티오 기능 및 태스크는 다음과 같다.

* 트래픽 제어 기능: HTTP, gRPC, WebSocket, TCP 트래픽에 대한 풍부한 라우팅 규칙으로
  세밀한 트래픽 제어를 시행한다.

* 네트워크 복원 기능: 재시도, 페일오버, 회로 차단기, 결함 주입을
  설정한다.

* 보안 및 인증 기능: 보안 정책을 시행하고, 구성 API를 통해 정의된 접근 제어 및 속도 제한을
  적용한다.

* 메시 트래픽에 대한 사용자 정의 정책 적용 및 텔레메트리 생성을 허용하는 웹어셈블리(WebAssembly)
  기반 플러그형 확장 모델.

### Istiod

Istiod는 서비스 디스커버리, 구성, 인증서 관리를 제공한다.

Istiod는 트래픽 동작을 제어하는 높은 수준의 라우팅 규칙을 엔보이 별 구성으로 변환하고,
이를 런타임의 사이드카에 전파한다. 파일럿(Pilot)은 플랫폼 별 서비스 디스커버리 메커니즘을
추상화하여 [엔보이 API](https://www.envoyproxy.io/docs/envoy/latest/api/api)를
준수하는 모든 사이드카가 사용할 수 있는 표준 형식으로
통합한다.

이스티오는 쿠버네티스, 컨설 또는 VM과 같은 여러 환경에 대한 탐색을 지원할 수 있다.

이스티오의 [트래픽 관리 API](/ko/docs/concepts/traffic-management/#introducing-istio-traffic-management)를
사용하여 Istiod에게 서비스 메시의 트래픽을
보다 세밀하게 제어하기 위해 엔보이 구성을 개선하도록
명령할 수 있다.

Istiod [보안](/ko/docs/concepts/security/)은 내장 식별 정보와 인증서 관리를
통해 강력한 최종 사용자 및 서비스 간 인증을 가능하게 한다.
서비스 메시에서 암호화되지 않은 트래픽을 업그레이드하기 위해 이스티오를 사용할 수 있다.
이스티오를 사용하여 운영자는 상대적으로 불안정한 3계층이나 4계층 네트워크 식별자보다는
서비스 식별 정보에 기반한 정책을 적용할 수 있다.
또한 [이스티오의 인가 기능](/ko/docs/concepts/security/#authorization)을
사용하여 누가 서비스에 접근할 수 있는지 제어할 수 있다.

Istiod는 인증 기관 (CA, Certificate Authority) 역할을 하며,
데이터 플레인에서 안전한 mTLS 통신이 가능하도록 인증서를 생성한다.