# Hand + Tool 실시간 탐지

웹캠 한 화면에서 **hand 모델**(hand)과 **tool 모델**(pliers / screwdriver / spanner, tool_01 병합 후 balldriver / masterwrench 추가)을 동시에 실행합니다.

---

## 🚀 빠른 시작 (Windows)

1. `setup.bat` 더블클릭 — 처음 한 번만 (가상환경 + 라이브러리 설치, 수 분 소요)
2. `run_webcam.bat` 더블클릭 — 웹캠 탐지 시작

카메라가 안 밀리면 `list_cameras.bat`로 번호를 확인한 뒤 `run_webcam.bat --source 1`처럼 실행하세요. 
*Python 3.9~3.12가 필요합니다 (`python.org` 설치 시 "Add Python to PATH" 체크).*

---

## ⌨️ 조작키

| 키 | 동작 | 키 | 동작 |
| :--- | :--- | :--- | :--- |
| `q` / `ESC` | 종료 | `h` | hand 표시 on/off |
| `space` | 일시정지 | `t` | tool 표시 on/off |
| `s` | 화면 저장 (`captures/`) | `+` / `-` | confidence 조절 |
| `r` | 녹화 시작/중지 | `f` | 좌우 반전 |

---

## ⚙️ 자주 쓰는 옵션 (`run_webcam.bat` 뒤에 붙임)

* **`--source 1`**: 다른 카메라 / 영상 파일 (`--source test.mp4`)
* **`--hand-conf 0.6`**: hand 확신도 임계값 (기본 0.5, 모량이 많으면 올림)
* **`--tool-conf 0.4`**: tool 확신도 임계값 (놓치는 게 많으면 내림)
* **`--tool-every 2`**: tool 모델을 2프레임마다 실행 (CPU에서 FPS 향상)
* **`--imgsz 480`**: 추론 해상도 낮춤 (뿌려지지만 정확도 감소)
* **`--out result.mp4 --no-show`**: 화면 없이 곧바로 영상만 저장

---

## 📁 폴더 구조

```text
hand_tool/
├── run_webcam.bat / setup.bat / list_cameras.bat  # 실행 프로그램
├── detect_webcam.py
├── models/
│   ├── hand_best.pt       # hand 모델 (YOLOv8n, AP50 0.95)
│   └── tool_best.pt       # tool 모델 (YOLOv11s, AP50 0.005)
├── datasets/
│   ├── hand/ (images, labels)/(train, val, test) + data.yaml  (1클래스)
│   └── tool/ (images, labels)/(train, val, test) + data.yaml  (5클래스: tool_01 병합 완료)
├── runs/                   # 이전 학습 결과(그래프, results.csv)
├── scripts/train.py        # 재학습 스크립트
├── scripts/merge_tool01.py # tool_01 데이터셋 겹칠 스크립트 (이미 1회 실행됨)
└── scripts/merge_and_train.py  # tool_01 병합(오로연) + 집합 + 학습 올인원 (클래스 5개 유지)
