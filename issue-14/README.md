# 🚨 트러블슈팅 미션 14: 애플리케이션 초기화 지연으로 인한 Pod 지속 재시작 문제

## 📝 상황 설명
신규 배포된 웹 애플리케이션(`slow-boot-app`)은 초기 구동(DB 커넥션, 캐시 로딩 등)에 약 20초가 소요되도록 설계되었습니다. 
그러나 해당 Pod 배포 시, 애플리케이션이 정상적으로 기동되기 전에 지속적으로 재시작(Restart)이 발생하며 최종적으로 `CrashLoopBackOff` 상태로 전환되는 장애 현상이 보고되었습니다.

## 🎯 목표
1. Pod 이벤트 로그(`kubectl describe pod`)를 분석하여 컨테이너가 비정상 종료(Killed)되는 정확한 원인을 규명합니다.
2. 현재 설정된 `Liveness Probe`의 파라미터(`initialDelaySeconds`, `periodSeconds`, `failureThreshold`)를 분석하여, 시스템이 컨테이너를 강제 종료하기까지의 임계 시간을 계산합니다.
3. 초기 구동 시간이 긴 애플리케이션의 안정적인 배포를 위해 **`Startup Probe`**를 도입하고 장애를 해결합니다.

## 🚀 실행 방법
1. **네임스페이스 생성:** `kubectl create ns apps`
2. **장애 상황 배포:** `helm install mission-14 .`
3. **상태 모니터링:** `kubectl get pod -n apps -w` 
   - 약 20초 경과 시점에서 애플리케이션 기동이 완료되기 전 Pod가 강제 종료 및 재시작되는 현상을 확인합니다.
4. **원인 분석:** 재시작 발생 후 아래 명령어를 통해 상세 이벤트를 확인합니다.
   - `kubectl describe pod -l app=slow-boot -n apps`
   - 하단 Events에서 `Liveness probe failed` 및 `Killing container with id...` 메시지를 확인하여 Liveness Probe에 의한 강제 종료임을 인지합니다.
