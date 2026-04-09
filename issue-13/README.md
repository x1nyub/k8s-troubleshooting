# 🚨 트러블슈팅 미션 13: ConfigMap을 바꿨는데 앱이 모릅니다! (Hot-reload)

## 📝 상황 설명
운영 환경에서 `app-config`의 설정값을 `debug=false`에서 `debug=true`로 변경했습니다. 하지만 파드의 로그를 보면 여전히 예전 설정(`false`)으로 동작하고 있습니다. 서비스 중단을 막기 위해 파드를 재시작(`delete pod`)하지 않고 새로운 설정을 앱에 실시간으로 반영해야 합니다.

## 🎯 목표
1. 애플리케이션 컨테이너는 시작될 때 1번만 설정을 읽고, 그 후로는 메모리에 올려둔 값을 쓴다는 점을 이해하세요.
2. 쿠버네티스의 `shareProcessNamespace` 기능과 사이드카(Sidecar) 패턴을 학습하세요.
3. 설정 파일을 감시하다가, 파일이 바뀌면 메인 앱에 새로고침 신호(`SIGHUP`)를 보내는 사이드카를 활성화하여 핫 리로드(Hot-reload)를 구현하세요.

## ✅ 성공 판정 기준 (중요!)
미션을 완벽히 수행했는지 확인하려면 다음 두 가지를 동시에 만족해야 합니다:
1. **Logs 변경:** `kubectl logs`를 통해 설정값(`debug=true`)이 반영된 것을 확인해야 합니다.
2. **Age 유지:** `kubectl get pod`를 쳤을 때 파드의 **`AGE`가 초기화되지 않고 그대로 유지**되어야 합니다. (파드가 재기동되었다면 실패입니다.)

## 🚀 실행 방법
1. **장애 배포:** `helm install mission-13 .`
2. **Config 변경:** `values.yaml`에서 `config.debug`를 `"true"`로 변경하고 `helm upgrade mission-13 .` 실행
3. **상태 관찰:** `kubectl logs -l app=hot-reload-app -c app -f`
   - 파일이 업데이트되어도 메인 앱은 계속 `false`를 출력하는 것을 확인하세요.
