# 🚨 트러블슈팅 미션 4: 배치 작업(Job)이 완료되지 않습니다!

## 📝 상황 설명
중요한 데이터 처리를 위해 배치 작업(`fail-job`)을 Job 리소스로 배포했습니다. 
하지만 파드가 생성된 후 얼마 지나지 않아 `Error` 상태로 변하며 종료되고, 
Job은 `COMPLETIONS 0/1` 상태에서 더 이상 진행되지 않습니다.

## 🎯 목표
1. 파드가 왜 `Error` 상태로 종료되었는지 원인을 찾으세요.
2. `backoffLimit` 설정이 Job의 전체 실행 과정에 어떤 영향을 주고 있는지 분석하세요.
3. 코드를 수정하여 Job이 성공적으로 완료(`COMPLETIONS 1/1`)되도록 만드세요.

## 🚀 실행 방법
1. **장애 배포:** `helm install mission-4 .`
2. **상태 확인:** - `kubectl get job -n batch`
   - `kubectl get pod -n batch`
3. **해결 후 재배포:** `helm upgrade mission-4 .`
