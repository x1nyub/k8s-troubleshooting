# 🚨 트러블슈팅 미션 15: Kubernetes API 호출 권한 거부 (RBAC)

## 📝 상황 설명
`ops` 네임스페이스에 배치된 `data-loader` Job은 쿠버네티스 API를 호출하여 현재 실행 중인 Pod 목록을 조회해야 합니다. 이를 위해 `ServiceAccount`, `Role`, `RoleBinding`을 모두 구성하여 배포했으나, Job이 정상적으로 완료되지 않고 `Error` 상태로 종료되었습니다.

## 🎯 목표
1. Job 내부 컨테이너의 로그(`kubectl logs`)를 확인하여 API 서버가 반환한 `403 Forbidden` 에러 메시지를 분석합니다.
2. 현재 `loader-role`에 부여된 권한 동사(Verb)를 확인하고, `kubectl get pods` 명령어가 내부적으로 요구하는 권한과 일치하는지 대조합니다.
3. K8s RBAC 체계에서 단일 리소스 조회(`get`)와 리소스 목록 조회(`list`)의 차이점을 명확히 이해하고, 설정 파일을 수정하여 Job을 성공시킵니다.

## ✅ 성공 판정 기준 (중요!)
미션을 완벽히 수행했는지 확인하려면 다음 상태를 달성해야 합니다.
* **상태 변경:** `kubectl get pod -n ops` 실행 시 파드의 상태가 `Error`에서 **`Completed`**로 변경되어야 합니다.
  *(참고: `Job` 리소스는 웹 서버(Deployment)와 달리 일회성 작업을 수행하므로, 작업이 정상적으로 끝나면 `Running`이 아닌 `Completed` 상태로 기분 좋게 종료되는 것이 올바른 동작입니다.)*
* **로그 확인:** 파드의 로그를 조회했을 때 403 Forbidden 에러가 사라지고 K8s 파드 목록이 정상적으로 출력되어야 합니다.

## 🚀 실행 방법
1. **네임스페이스 생성:** `kubectl create ns ops`
2. **장애 상황 배포:** `helm install mission-15 .`
3. **상태 모니터링:** `kubectl get job,pod -n ops`
   - Job이 `0/1` Completions 상태에 머무르며, 생성된 Pod가 `Error` 상태인지 확인합니다.
4. **원인 분석:** - `kubectl logs -l job-name=data-loader -n ops` 명령어로 로그를 확인합니다.
   - *"User ... cannot list resource "pods" in API group "" "* 메시지를 확인합니다.
   - `kubectl get role loader-role -n ops -o yaml`을 통해 현재 권한이 `get`만 있음을 확인합니다.
