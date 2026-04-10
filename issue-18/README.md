# 🚨 트러블슈팅 미션 18: StatefulSet Pod 간 DNS 이름 해석 실패

## 📝 상황 설명
`dev` 네임스페이스에 분산 데이터베이스 구성을 위해 `dns-app`을 StatefulSet으로 배포했습니다. 각 Pod는 `dns-app-0.dns-app`과 같은 안정적인 DNS 이름을 사용하여 상호 통신을 시도하지만, 현재 `nslookup` 실행 시 `NXDOMAIN` 에러가 발생하며 호스트를 찾지 못하고 있습니다.

## 🎯 목표
1. StatefulSet 파드가 고유한 네트워크 식별자(FQDN)를 가지기 위한 필수 조건을 이해합니다.
2. 일반 Service(ClusterIP)와 Headless Service의 DNS 레코드 생성 방식 차이를 분석합니다.
3. 설정을 수정하여 각 파드가 고유한 DNS 이름으로 상호 해석이 가능하도록 조치합니다.

## ✅ 성공 판정 기준
* `kubectl exec netshoot -n dev -- nslookup dns-app-0.dns-app` 실행 시, 에러 없이 해당 파드의 실제 IP 주소가 반환되어야 합니다.

## 🚀 실행 방법
1. **장애 배포:** `helm install mission-18 .`
2. **현상 확인:** `kubectl exec -it netshoot -n dev -- nslookup dns-app-0.dns-app`
   - `NXDOMAIN` 에러가 발생하는지 확인합니다.
3. **원인 분석:** `kubectl get svc dns-app -n dev` 명령어로 해당 서비스가 ClusterIP를 가지고 있는지 확인합니다.
