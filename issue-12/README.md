# 🚨 트러블슈팅 미션 12: 설정 파일을 넣었더니 Nginx가 죽어요!

## 📝 상황 설명
`prod` 네임스페이스에 배포된 Nginx 웹 서버에 `custom.conf` 설정을 추가하기 위해 ConfigMap을 만들고 마운트했습니다. Helm 배포는 성공적으로 끝났지만, 파드가 `CrashLoopBackOff` 상태에 빠져 무한 재시작을 반복하고 있습니다.

## 🎯 목표
1. 파드의 로그(`kubectl logs`)를 확인하여 Nginx가 어떤 파일을 찾지 못해 종료되었는지 확인하세요.
2. 디렉터리에 ConfigMap을 통째로 마운트할 때 기존 컨테이너 내부의 파일들이 지워지는 **볼륨 섀도잉(Volume Shadowing)** 현상을 이해하세요.
3. 기존 파일을 보존하면서 단일 파일만 안전하게 주입하는 **`subPath`** 옵션을 사용하여 문제를 해결하세요.

## 🚀 실행 방법
1. **장애 배포:** `helm install mission-12 .`
2. **원인 분석:** `kubectl get pod -n prod`로 상태 확인 후, `kubectl logs -l app=webapp -n prod` 실행
   - `[emerg] open() "/etc/nginx/nginx.conf" failed` 에러를 확인하세요.
