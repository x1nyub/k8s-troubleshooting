# 🚨 트러블슈팅 미션 16: Service DNS가 개별 Pod IP를 반환하지 않는 문제

## 📝 상황 설명
`disco` 네임스페이스에 3개의 파드로 구성된 분산 애플리케이션(`discovery-app`)이 배포되었습니다. 이 애플리케이션 노드들은 클러스터링을 구성하기 위해 DNS 조회를 통해 피어(Peer) 파드들의 개별 IP 주소를 파악해야 합니다. 
그러나 `discovery-svc` Service를 통해 DNS를 조회하면, 3개의 파드 IP가 아닌 단일 IP 하나만 반환되어 클러스터링이 실패하고 있습니다.

## 🎯 목표
1. 테스트 파드 내에서 `nslookup` 명령어를 사용하여 현재 Service가 단일 가상 IP(ClusterIP)를 반환하는 현상을 확인합니다.
2. 로드밸런싱을 수행하는 **일반 Service**와, DNS 레코드에 개별 파드 IP를 직접 반환하는 **Headless Service**의 아키텍처 차이를 이해합니다.
3. K8s Service 스펙을 Headless 모드로 전환하여 장애를 해결합니다.

## ✅ 성공 판정 기준 (중요!)
미션의 최종 성공 여부는 다음 DNS 질의 결과를 통해 확인합니다.
* **해결 전:** `nslookup discovery-svc` 실행 시 Address 항목에 IP가 **1개**만 반환됩니다.
* **해결 후:** 설정을 수정하고 동일한 명령어를 실행했을 때, 현재 실행 중인 `discovery-app` 파드들의 IP 주소 **3개**가 모두 Address 목록에 출력되어야 합니다.

## 🚀 실행 방법
1. **네임스페이스 생성:** `kubectl create ns disco`
2. **장애 상황 배포:** `helm install mission-16 .`
3. **상태 모니터링:** `kubectl get pod,svc -n disco`
   - `discovery-app` 파드 3개와 `dns-tester` 파드가 모두 `Running` 상태인지 확인합니다.
4. **원인 분석:** - DNS 테스트용 파드에 접속하여 네임서버를 질의합니다.
   - `kubectl exec -it dns-tester -n disco -- nslookup discovery-svc`
   - 단일 IP만 반환되는 것을 확인합니다.
5. **문제 해결 및 재배포:** - `values.yaml` 파일의 설정을 수정하여 Service를 Headless 모드로 전환합니다.
   - `helm upgrade mission-16 .` 명령어로 변경 사항을 적용한 뒤 `nslookup`을 다시 실행하여 3개의 IP가 반환되는지 확인합니다.
