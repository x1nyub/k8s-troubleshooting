# 🚨 트러블슈팅 미션 10: 파드가 OOMKilled로 계속 죽어요!

## 📝 상황 설명
`apps` 네임스페이스에 메모리 집약적인 애플리케이션(`oom-demo`)을 배포했습니다. 
하지만 파드가 시작되자마자 곧바로 종료되며, 현재 `CrashLoopBackOff` 상태에 빠져 무한 재시작을 반복하고 있습니다.

## 🎯 목표
1. 파드의 상세 정보(`kubectl describe pod`)를 확인하여, 이전 컨테이너가 종료된 사유(`Last State`)와 종료 코드(`Exit Code`)를 분석하세요.
2. 애플리케이션이 필요로 하는 메모리와 K8s에 설정된 제한(`limits`) 간의 차이를 찾아내세요.
3. `values.yaml`의 리소스 설정을 적절히 수정하여 파드가 안정적으로 `Running` 상태를 유지하도록 만드세요.

## 🚀 실행 방법
1. **네임스페이스 생성:** `kubectl create ns apps` (최초 1회)
2. **장애 배포:** `helm install mission-10 .`
3. **상태 확인:** `kubectl get pod -n apps -w` (파드가 죽고 다시 살아나는 과정을 관찰하세요)
4. **원인 분석:** `kubectl describe pod oom-demo -n apps` 
5. **해결 후 재배포:** `values.yaml` 수정 후 파드를 삭제하고 재설치하거나, `helm upgrade`를 시도해 보세요. (참고: Pod 리소스는 불변이므로 보통 재설치가 필요합니다.)
