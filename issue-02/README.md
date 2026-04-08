# 🚨 트러블슈팅 미션 2: 프론트엔드 파드가 준비되지 않아요!

## 📝 상황 설명
새로운 버전의 웹 애플리케이션(`web-frontend`) 파드를 배포하기 위해 매니페스트를 적용했지만, 
파드가 `Running` 상태로 넘어가지 못하고 계속 대기 중입니다. 
`kubectl get pods`를 확인해 보니 상태가 `ImagePullBackOff`로 표시됩니다.

## 🎯 목표
1. 파드가 배포되지 않고 이미지를 당겨오지 못하는 근본적인 원인을 분석하세요.
2. Helm 차트의 설정값을 수정하여 파드를 `Running` 상태로 만드세요.

## 🚀 실행 방법
1. **이전 미션 정리:** `helm uninstall mission-1` (아직 지우지 않았다면)
2. **장애 배포:** `helm install mission-2 .`
3. **상태 확인:** `kubectl get pods` 
4. **원인 분석 후 수정 및 재배포:** `helm upgrade mission-2 .`
