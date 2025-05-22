# cs_pjt: Real-Time YOLO + SED Edge Inference System

## 📌 프로젝트 개요

이 프로젝트는 YOLOv5 기반 객체 탐지와 SED(Sound Event Detection)를 병렬로 수행하는 **Edge Inference 시스템**입니다.  
각 디바이스(컨테이너)는 다음 역할을 합니다:

- 📷 YOLO 컨테이너: 실시간 웹캠 스트림을 추론하고 `/stream`으로 전송
- 🎧 SED 컨테이너: 마이크로부터 오디오를 수집, 10초 단위로 `/upload` 전송
- 🌐 sender_api: 두 스트림을 수신하여 `received_data/`에 저장하고, 외부 FastAPI 서버로 전달

이 시스템은 클라우드 없이 독립 실행되며, 도커 기반의 재현 가능한 구조를 제공합니다.

---

## 📂 디렉토리 구조

\`\`\`
cs_pjt/
├── docker-compose.yml               # 전체 오케스트레이션
├── .gitignore
├── README.md
├── yolo/                            # YOLO 추론 코드 및 Dockerfile
│   └── yolo_script.py
├── sed/                             # SED 추론 코드 및 Dockerfile
│   └── sed_script.py
├── sender_api/
│   ├── main.py                      # FastAPI 서버 (/stream, /upload)
│   └── received_data/
│       ├── audio/                   # SED 오디오 저장 (10초 단위)
│       └── video/                   # YOLO 이미지 저장 (실시간 프레임)
\`\`\`

---

## ⚙️ 실행 방법

### 1. Docker Compose 실행

\`\`\`bash
docker-compose up --build
\`\`\`

> YOLO 및 SED 컨테이너가 자동 실행되며, sender_api가 8000 포트에서 수신 대기합니다.

---

## 🔁 데이터 흐름

\`\`\`text
[YOLO container]       ─────┐
                           ├─>  sender_api  ──>  저장 + 외부 전송
[SED container]  (10초 단위) ┘
\`\`\`

- `/stream`: YOLO 이미지 실시간 POST
- `/upload`: 오디오 결과 + 메타데이터 POST

---

## 📄 기타 참고사항

- `.gitignore`를 통해 `sender_api/received_data/{audio,video}` 내부 파일은 Git에 포함되지 않음
- `.gitkeep`로 디렉토리 구조만 유지
- 외부 수신 측 FastAPI 서버는 별도 구성 필요

---

## ✍️ 작성자

- GitHub: [@minsu2698](https://github.com/minsu2698)
- Project by: 김민수
