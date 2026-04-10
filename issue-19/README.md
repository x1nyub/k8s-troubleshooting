# 🚨 트러블슈팅 미션 19: StorageClass 프로비저너 부재로 인한 PVC Pending 문제

## 📝 상황 설명
`storage` 네임스페이스에 애플리케이션 데이터를 저장하기 위해 PVC(`expand-pvc`)와 Pod를 배포했습니다. 스토리지 클래스 이름은 클러스터에 존재하는 이름으로 정확하게 지정했으나, PVC가 무한정 `Pending` 상태에 머물러 있어 Pod 역시 실행되지 못하고 있습니다.

## 🎯 목표
1. `kubectl get sc` 명령어를 통해 대상 스토리지 클래스의 상세 정보(`PROVISIONER` 항목)를 확인합니다.
2. 동적 프로비저닝(Dynamic Provisioning)이 지원되지 않는 환경에서 K8s 스토리지 바인딩 원리를 이해합니다.
3. 관리자로서 직접 `PersistentVolume(PV)` 리소스를 생성하여 PVC가 정상적으로 볼륨을 할당받을 수 있도록 조치합니다.

## 💡 해결 힌트 (중요)
* 특정 StorageClass의 PROVISIONER가 **`kubernetes.io/no-provisioner`**로 설정되어 있다면, 해당 클러스터는 디스크(볼륨)를 자동으로 생성해 주지 않습니다.
* 이러한 환경에서는 인프라 엔지니어가 직접 PVC 요구 조건(용량, 접근 모드 등)에 맞는 **`PersistentVolume (PV)` 객체를 yaml 파일로 작성하여 클러스터에 수동으로 배포**해야 합니다. (참고: PV는 특정 네임스페이스에 종속되지 않는 클러스터 전역 자원입니다.)

## ✅ 성공 판정 기준
* **상태 확인:** `kubectl get pv,pvc,pod -n storage` 실행 시 다음 상태가 모두 확인되어야 합니다.
  1. 수동으로 생성한 PV가 리스트에 존재할 것.
  2. PVC 상태가 `Pending`에서 **`Bound`**로 변경될 것.
  3. Pod 상태가 **`Running`** 상태로 올라올 것.

## 🚀 실행 방법
1. **네임스페이스 생성:** `kubectl create ns storage`
2. **장애 상황 배포:** `helm install mission-19 .`
3. **현상 분석:** - `kubectl get pvc,pod -n storage`를 통해 대기(Pending) 상태를 확인합니다.
   - `kubectl get sc`를 실행하여 현재 사용 중인 StorageClass의 자동 생성 지원 여부를 확인합니다.
