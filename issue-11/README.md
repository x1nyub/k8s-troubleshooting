# 🚨 트러블슈팅 미션 11: 크론잡이 약속된 시간에 돌지 않습니다!

## 📝 상황 설명
`ops` 네임스페이스의 `cleanup` CronJob이 매분(Every Minute) 실행되어 임시 파일을 정리해야 합니다. 
하지만 배포 후 5분이 지나도 `LAST SCHEDULE` 값이 `<none>`이며, 한 번도 실행된 흔적이 없습니다.

## 🎯 목표
1. 현재 설정된 `schedule: "0 0 1 1 *"`이 구체적으로 언제 실행되는 설정인지 분석하세요.
2. '매분 실행'을 위해 크론 표현식을 어떻게 수정해야 하는지 찾아내세요.
3. 설정을 수정한 후, 실제로 1분 뒤에 Job이 생성되는지 확인하세요.

## 🚀 실행 방법
1. **네임스페이스 생성:** `kubectl create ns ops`
2. **장애 배포:** `helm install mission-11 .`
3. **상태 확인:** `kubectl get cronjob -n ops`
   - `LAST SCHEDULE` 필드가 계속 비어있는지 확인하세요.
4. **해결 후 재배포:** `helm upgrade mission-11 .`
