# 🚨 트러블슈팅 미션 17: Custom Resource 생성 시 API 그룹을 찾을 수 없는 문제

## 📝 상황 설명
Kubernetes API를 확장하여 `Widget`이라는 커스텀 리소스를 사내 표준으로 사용하려 합니다. 클러스터 관리자가 CRD를 성공적으로 등록했다고 전달했으나, 개발자가 `Widget` 인스턴스를 생성하려고 하면 다음과 같은 에러가 발생하며 배포가 거부됩니다.

> `error: resource mapping not found for name: "sample-widget" ... no matches for kind "Widget" in version "mycompany.io/v1"`

## 🎯 목표
1. `kubectl get crd` 명령어를 통해 `widgets.mycompany.io` CRD가 실제로 존재하는지 확인합니다.
2. CRD의 상세 스펙(`kubectl get crd widgets.mycompany.io -o yaml`)을 분석하여, 버전(`v1`)의 속성 중 API 서버 노출을 막고 있는 설정을 찾아냅니다.
3. 설정을 수정하여 커스텀 리소스가 정상적으로 생성되도록 조치합니다.

## ✅ 성공 판정 기준
* `kubectl apply -f sample-widget.yaml` 명령어가 에러 없이 성공(created)해야 합니다.
* `kubectl get widgets -n extensions` 명령어를 실행했을 때 `sample-widget`이 정상적으로 조회되어야 합니다.

## 🚀 실행 방법
1. **네임스페이스 생성:** `kubectl create ns extensions`
2. **CRD(장애 상황) 배포:** `helm install mission-17 .`
3. **장애 재현:** `kubectl apply -f sample-widget.yaml`
   - 에러 메시지(`no matches for kind "Widget"`)를 확인합니다.
