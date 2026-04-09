# 🚨 트러블슈팅 미션 9: 스토리지가 할당되지 않습니다! (Pending 현상)

## 📝 상황 설명
데이터 저장을 위해 PVC(PersistentVolumeClaim)와 이를 마운트하는 파드를 배포했습니다. 
하지만 배포 후 한참이 지나도 파드와 PVC가 모두 `Pending` 상태에 머물러 있습니다.

## 🎯 목표
1. 파드와 PVC의 `Events` 로그를 확인하여 왜 `Pending` 상태에 빠졌는지 원인을 분석하세요.
2. 현재 클러스터에 배포된 `StorageClass`의 `provisioner` 설정(`no-provisioner`)이 무엇을 의미하는지 파악하세요.
3. **직접 디스크(PV)를 생성**하여 파드가 `Running` 상태가 되도록 해결하세요.

### 📦 수동 PV(PersistentVolume) 요구 조건
요청된 PVC의 스펙을 만족하도록 아래 조건에 맞춰 `pv.yaml`을 작성해야 합니다.
- **용량 (Capacity):** 1Gi
- **접근 모드 (AccessMode):** ReadWriteOnce
- **스토리지 클래스 (StorageClass):** 배포된 PVC(`data-pvc`)가 요구하는 클래스 이름과 동일하게 설정
- **볼륨 타입:** `hostPath` 사용 (경로: `/mnt/data`)

## 🚀 실행 방법
1. **장애 배포:** `helm install mission-09 .`
2. **상태 확인:** - `kubectl get pod,pvc` (모두 Pending인지 확인)
   - `kubectl describe pvc data-pvc` (이벤트 로그의 `didn't find available persistent volumes` 확인)
3. **해결 진행:** 조건에 맞는 `pv.yaml` 파일을 직접 작성한 뒤 `kubectl apply -f pv.yaml` 명령어로 클러스터에 배포하세요.
4. **최종 점검:** PVC 상태가 `Bound`로 바뀌고, 파드가 `Running`으로 성공적으로 올라오는지 확인하세요!
