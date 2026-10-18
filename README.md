# ha-korea-devices

한국 가정에서 많이 쓰는 기기를 Home Assistant(HA)에 연결한 기록입니다. 같은 기기를 쓰는 분이 설정값을 그대로 가져다 쓰고, 제가 겪은 시행착오는 건너뛸 수 있게 정리했습니다.

## 기기 목록

| 기기 | 연동 방식 | 문서 |
|---|---|---|
| 에어메이드 AMH-9902UV 가습기 (2026년형) | Tuya, tuya-local로 로컬 제어 | [tuya/airmade-amh9902uv](tuya/airmade-amh9902uv/README.md) |
| 삼성 에어컨(스탠드형·벽걸이형), 세탁기, 건조기 | SmartThings 공식 통합 | [smartthings](smartthings/README.md) |

코콤 월패드(KHN-V100JA) 연동은 별도 레포 [jelitz/kocom-wallpad](https://github.com/jelitz/kocom-wallpad)에 있습니다.

## 테스트 환경

- Home Assistant 2026.10 (Docker 컨테이너)
- tuya-local 2026.9.2 (HACS)
- 확인한 날짜: 2026년 10월

다른 버전에서는 화면 이름이나 엔티티 이름이 다를 수 있습니다.

## 대시보드 예시

[examples/dashboard.yaml](examples/dashboard.yaml)에 가습기를 카드 하나로 묶은 예시와 에어컨·세탁실 카드가 있습니다. 엔티티 ID는 각자 환경에 맞게 바꿔 쓰세요.

## 라이선스

MIT
