# 🚨 트러블슈팅 미션 20: Cross-Namespace Network Policy 트래픽 제어

## 📝 상황 설명
보안 요구사항에 따라 `team-b` 네임스페이스로 인입되는 모든 외부 트래픽을 차단하는 `Default Deny` 정책이 클러스터 관리자에 의해 적용되었습니다. 
하지만 서비스의 정상적인 동작을 위해, 예외적으로 **`team-a` 네임스페이스의 `app=client` 파드가 `team-b` 네임스페이스의 `app=api` 파드에 접근하는 트래픽만은 허용**해야 합니다. 현재는 모든 통신이 타임아웃(Timeout)으로 차단되고 있습니다.

## 🎯 목표
1. NetworkPolicy의 `namespaceSelector`와 `podSelector`를 조합하여 세밀한 Ingress 규칙을 작성하는 방법을 학습합니다.
2. 개발자로서 직접 허용 정책(Allow Policy) yaml 매니페스트를 작성하고 배포하여, 요구사항에 맞는 통신만 개방합니다.

## 💡 해결 힌트 (매우 중요!)
* 쿠버네티스 NetworkPolicy는 화이트리스트(Whitelist) 방식을 따릅니다. Default Deny가 걸려있더라도, 허용하는 규칙을 추가로 배포하면 해당 트래픽만 통과할 수 있습니다.
* `namespaceSelector`를 사용하려면 대상 네임스페이스에 고유한 라벨(Label)이 지정되어 있어야 합니다. (`kubectl get ns --show-labels`로 확인해 보세요.)
* **[🚨 함정 주의]** `curl` 명령어로 호출하는 대상은 `Service`의 포트(8080)이지만, Network Policy는 `Service`가 아닌 **`Pod`의 앞단(Container Port)**을 지키는 방화벽입니다. 방화벽 규칙을 짤 때 어떤 포트를 열어주어야 할지 깊이 고민해 보세요!

## ✅ 성공 판정 기준
* `team-a`의 client 파드에서 `team-b`의 api 파드로의 통신은 **성공(HTTP 200 또는 응답)**해야 합니다.
  * `kubectl exec client -n team-a -- curl -s --max-time 3 api.team-b:8080`
* `team-a`의 client 파드에서 `team-b`의 web 파드로의 통신은 계속 **실패(Timeout)**해야 합니다.
  * `kubectl exec client -n team-a -- curl -s --max-time 3 web.team-b:80`

## 🚀 실행 방법
1. **장애 상황 배포:** `helm install mission-20 .` (네임스페이스 자동 생성됨)
2. **현상 분석:** - `README`의 성공 판정 기준에 있는 `curl` 명령어를 실행하여 현재 모든 통신이 타임아웃으로 실패하는지 확인합니다.
3. **문제 해결:** - 공식 문서를 참고하여, 조건을 만족하는 NetworkPolicy 매니페스트 파일(예: `allow-api.yaml`)을 직접 작성합니다.
   - `kubectl apply -f allow-api.yaml` 명령어로 클러스터에 정책을 추가합니다.
   - 다시 `curl` 테스트를 수행하여 `api` 파드에만 통신이 성공하는지 확인합니다.
