# 🚨 트러블슈팅 미션 8: 권한이 없어서 조회가 안 돼요!

## 📝 상황 설명
모니터링을 위해 `reader-sa`라는 전용 계정(ServiceAccount)을 만들고 파드를 배포했습니다. 
하지만 파드 내부에서 `kubectl get pods` 명령을 실행하면 `forbidden` 에러가 발생하며 목록을 가져오지 못합니다.

## 🎯 목표
1. RBAC(Role, RoleBinding)의 개념을 이해하고 누락된 설정이 무엇인지 찾으세요.
2. `reader-sa` 계정이 `demo` 네임스페이스의 파드를 조회(`list`, `get`)할 수 있도록 권한을 부여하세요.
3. 파드 로그(`kubectl logs auth-test-pod -n demo`)를 확인하여 파드 목록이 정상 출력되는지 확인하세요.

## 🚀 실행 방법
1. **네임스페이스 생성:** `kubectl create ns demo` (필요 시)
2. **장애 배포:** `helm install mission-08 .`
3. **로그 확인:** `kubectl logs -f auth-test-pod -n demo`
   - `User ... cannot list resource "pods"` 에러를 확인하세요.
