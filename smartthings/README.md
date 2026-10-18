# SmartThings 기기를 Home Assistant에 연결하기

삼성 에어컨·세탁기·건조기를 SmartThings 공식 통합으로 HA에 연결하고, 대시보드에 쓸 값을 고르는 방법입니다. 따라 하면 에어컨 센서로 실내 온습도를 표시하고, 세탁기·건조기 상태를 보면서 원격으로 시작·정지할 수 있습니다.

## API 유료화 일정

삼성은 2026년 10월부터 SmartThings API 무료 제공을 단계적으로 끝내고, 개인 사용자용 유료 요금제(월 $4.99)를 도입한다고 발표했습니다. HA의 SmartThings 통합도 이 API를 쓰기 때문에 영향을 받습니다.

- [SmartThings 공지 (2026-06-23)](https://community.smartthings.com/t/a-new-enhanced-smartthings-api-experience/309947)
- [HA SmartThings 통합 문서의 경고](https://www.home-assistant.io/integrations/smartthings/)

유료 요금제를 쓰지 않으려면 SmartThings Station 같은 허브의 Matter 기능으로 기기를 HA에 넘기는 방법을 검토해 볼 수 있습니다. 이 방법은 아직 직접 해 보지 않았습니다.

## 연결 방법

1. HA에서 설정 > 기기 및 서비스 > 통합 추가 > SmartThings를 고릅니다.
2. 삼성 계정으로 로그인하고 권한을 허용합니다.
3. SmartThings 앱에 등록된 기기가 HA에 기기와 엔티티로 생깁니다.

엔티티 ID는 SmartThings의 방 이름과 기기 이름을 로마자로 바꿔서 만듭니다. 예를 들어 "거실 스탠드형 에어컨"은 `climate.geosil_seutaendeuhyeong_eeokeon`이 됩니다.

## 에어컨

에어컨 하나에 기후(climate) 엔티티와 센서 여러 개가 생깁니다.

| 엔티티 | 쓸모 |
|---|---|
| `climate.<이름>` | 켜기·끄기, 냉방·제습·송풍·자동 모드, 희망 온도 |
| `sensor.<이름>_temperature` | 에어컨이 잰 실내 온도 |
| `sensor.<이름>_humidity` | 에어컨이 잰 실내 습도 |
| `sensor.<이름>_power`, `_energy` | 소비 전력, 누적 전력량 |

에어컨은 꺼져 있어도 온도·습도 센서 값을 계속 보냅니다. 그래서 별도 온습도계가 없어도 거실 에어컨 센서를 대시보드 맨 위에 실내 온습도로 띄울 수 있습니다.

## 세탁기와 건조기

| 엔티티 | 쓸모 |
|---|---|
| `sensor.<이름>_machine_state` | 정지·동작·일시정지 |
| `sensor.<이름>_job_state` | 세탁·헹굼·탈수·건조 같은 진행 단계 |
| `sensor.<이름>_completion_time` | 끝나는 예정 시각 |
| `select.<이름>` | 시작(run)·일시정지(pause)·정지(stop) 명령 |
| `binary_sensor.<이름>_remote_control` | 기기의 원격 제어가 켜져 있는지 |
| `select.<이름>_spin_level` 등 | 세탁기의 탈수 세기, 물 온도, 헹굼 횟수, 세제 양 |

### 원격 시작·정지가 안 될 때

삼성 세탁기·건조기는 기기에서 원격 제어(스마트 컨트롤)를 켜 둔 동안에만 밖에서 시작·정지할 수 있습니다. 안전을 위해 삼성이 막아 둔 것이라 HA에서 바꿀 수 없습니다. `binary_sensor.<이름>_remote_control`이 `on`일 때만 `select.<이름>` 명령이 먹습니다.

세탁 코스는 이 통합에서 고를 수 없습니다. 코스는 기기에서 고르고 원격 제어를 켠 다음, HA에서 시작하는 식으로 씁니다.

대시보드에는 원격 제어가 켜져 있을 때만 제어 카드를 보이게 해 두면 헷갈리지 않습니다. [examples/dashboard.yaml](../examples/dashboard.yaml)의 세탁실 부분을 참고하세요.
