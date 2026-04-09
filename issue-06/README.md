# 🚨 트러블슈팅 미션 6: 파드는 Running인데 접속이 안 됩니다!

## 📝 상황 설명
웹 애플리케이션을 배포한 뒤 `Service`를 통해 접근하려 하지만 응답이 없습니다. 
`kubectl get pods`를 확인해 보니 상태는 `Running`이지만, `READY` 열이 `0/1`로 표시되고 있습니다.

## 🎯 목표
1. 파드가 `Running` 상태임에도 불구하고 왜 `READY` 상태가 되지 않는지 원인을 분석하세요.
2. 설정을 수정하여 파드가 트래픽을 받을 수 있는 상태(`READY 1/1`)로 만드세요.

## 🚀 실행 방법
1. **장애 배포:** `helm install mission-6 .`
2. **상태 확인:** `kubectl get pods` 및 `kubectl describe pod web-ready`
3. **해결 후 재배포:** `helm upgrade mission-6 .` (또는 삭제 후 재배포)
