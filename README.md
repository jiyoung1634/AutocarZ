# AutocarZ 프로젝트 종합 가이드 (25.07.21 기준)

---

## 📋 프로젝트 개요

**고라니 전용 YOLO 모델**을 활용한 라즈베리파이 기반 실시간 객체 탐지 시스템

### 🎯 프로젝트 목적

- **실시간 객체 인식**: 카메라로 고라니 등 위험 동물 감지
- **웹 UI 제공**: 브라우저에서 실시간 영상/상태 확인
- **안전 시스템**: 위험 상황을 빠르게 알림
- **플랫폼 최적화**: 라즈베리파이와 노트북 환경 자동 최적화

### 🔧 주요 기술

- **Python**: 전체 프로그램 언어
- **Flask**: 웹서버 (브라우저에서 실시간 영상 제공)
- **YOLOv8**: AI 객체 인식 (고라니 전용 모델)
- **OpenCV**: 이미지/영상 처리 (얼굴 탐지 + YOLO 보조)
- **라즈베리파이/USB 웹캠**: 실제 하드웨어

### 🆕 최신 기능 (25.07.21)

- **🦌 고라니 전용 YOLO 모델**: AIHUB 데이터셋 기반 고라니 특화 인식
- **🔄 플랫폼별 자동 최적화**: 라즈베리파이(성능 모드) vs 노트북(고품질 모드)
- **🎯 이중 검출 시스템**: YOLO(파란색) + OpenCV(빨간색) 동시 검출
- **⏱️ 검출 결과 유지**: 검출된 객체 박스 3-5초간 화면에 유지
- **🇰🇷 한글 라벨 지원**: "고라니" 등 한글 클래스명 표시
- **🐛 디버그 로깅**: 검출된 클래스 ID, 개수, 신뢰도 실시간 출력

---

## 📁 폴더 구조 (2025년 기준, 실제 깃트리 반영)

```
/home/pi/autocarz/
├── src/
│   ├── main.py                  # 🚀 전체 시스템 실행(Flask 웹서버 진입점)
│   ├── camera/
│   │   └── camera_manager.py    # 📸 카메라 제어, 프레임 생성, YOLO/OpenCV 통합
│   ├── detection/
│   │   ├── yolo_detector.py     # 🤖 YOLO 객체 인식
│   │   └── opencv_detector.py   # 🤖 OpenCV 객체 인식 (얼굴 탐지)
│   ├── routes/
│   │   ├── main_routes.py       # 🌐 메인/비디오피드 라우트
│   │   ├── settings_routes.py   # ⚙️ 설정 변경 라우트
│   │   └── status_routes.py     # 📊 시스템 상태 라우트
│   └── utils/
│       └── settings_manager.py  # 🔧 설정 파일 관리
├── templates/
│   └── index.html               # 📄 웹 UI 템플릿
├── static/
│   ├── css/style.css            # 🎨 웹 스타일
│   └── js/script.js             # 🖱️ 웹 동작(JS)
├── models/
│   ├── best.pt                  # 🧠 YOLO 학습 모델
│   ├── data.yaml                # 📋 YOLO 클래스 정보 (고라니 등)
│   ├── args.yaml                # ⚙️ YOLO 훈련 설정
│   └── haarcascades/            # 🎭 OpenCV 얼굴 탐지 모델
│       └── haarcascade_frontalface_default.xml
└── requirements.txt             # 📦 필요한 라이브러리 목록
```

---

## 🚀 설치 및 실행 가이드

### 1️⃣ 시스템 요구사항

- **하드웨어**: 라즈베리파이 4 (4GB RAM 이상 권장), 라즈베리파이 카메라 모듈 또는 USB 웹캠, MicroSD 카드 (32GB 이상)
- **소프트웨어**: Debian GNU/Linux 12 (bookworm), Python 3.13.5

### 2️⃣ 설치 및 실행

```bash
# 1. 프로젝트 파일 클론 (다운로드)
git clone https://github.com/jiyoung1634/AutocarZ.git
cd AutocarZ
git checkout raspberrypi  # 라즈베리파이 전용 브랜치로 변경

# 2. Python 가상환경 설정
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip

# 3. 필수 라이브러리 설치
sudo apt update
sudo apt install -y build-essential python3-dev libjpeg-dev
pip install -r requirements.txt

# 4. AI 모델 파일 준비
mkdir -p models  # models 폴더가 없다면 생성
# [중요] best.pt 파일을 models/ 폴더에 넣어주세요. (별도 다운로드 또는 SCP로 전송)

# [중요] 고라니 전용 YOLO 모델 준비
# 방법 1: AIHUB 데이터셋으로 훈련된 모델 사용
# models/best.pt (고라니 전용 모델)
# models/data.yaml (클래스 정보: ['고라니'])

# 방법 2: 기존 COCO 모델 사용 (다른 동물도 인식 가능)
# wget https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8n.pt -O models/best.pt

# 5. YOLO 클래스 정보 파일 확인
# models/data.yaml 파일이 있는지 확인
cat models/data.yaml
# 예상 출력: names: ['고라니']

# 6. OpenCV 얼굴 탐지 모델 추가
mkdir -p models/haarcascades
# OpenCV 내장 cascade 파일 사용(pip) 또는 수동 다운로드

# 7. (PiCamera 사용 시) 카메라 활성화
sudo raspi-config
# Interface Options > Camera > Enable 선택 후 재부팅

# 8. 시스템 실행
cd src
python3 main.py
```

### 3️⃣ 웹 스트리밍 접속

- 브라우저에서 접속: `http://[라즈베리파이_IP]:5000/`
- IP 확인: `hostname -I`

---

## 🔄 모델 교체 가이드

### 🎯 새로운 YOLO 모델 적용하기

```bash
# 1. 기존 모델 백업
cp models/best.pt models/best_backup_$(date +%Y%m%d).pt

# 2. 새 모델로 교체
cp /path/to/new_model.pt models/best.pt

# 3. 새 모델의 클래스 정보 교체
cp /path/to/new_data.yaml models/data.yaml

# 4. 모델 정보 확인
python -c "from ultralytics import YOLO; model = YOLO('models/best.pt'); print('클래스:', model.names)"

# 5. 시스템 재시작
python src/main.py
```

### 📊 모델별 특징

| 모델 타입 | 클래스 수 | 용도 | 정확도 |
|-----------|-----------|------|--------|
| **고라니 전용** | 1개 | 고라니 특화 인식 | 높음 |
| **COCO 일반** | 80개 | 다양한 객체 인식 | 보통 |
| **멀티 클래스** | 2-10개 | 특정 동물들 | 높음 |

### 🔧 플랫폼별 최적화 설정

```python
# 라즈베리파이 환경 (자동 감지)
- 해상도: 640x480
- FPS: 15
- 검출 간격: 3프레임마다

# 노트북/PC 환경 (자동 감지)  
- 해상도: 1280x720
- FPS: 30
- 검출 간격: 매 프레임
```

---

## 🔄 전체 동작 흐름

1. **main.py 실행** → Flask 서버 시작
2. **웹브라우저 접속** → index.html UI 표시
3. **카메라에서 프레임 캡처** → camera_manager.py
4. **YOLO/OpenCV로 객체 인식** → detection/
5. **결과를 웹 UI로 실시간 스트리밍** → main_routes.py + index.html
6. **설정/상태 변경/확인** → settings_routes.py, status_routes.py, settings_manager.py

---

## 🟦 각 폴더/파일의 역할

- **src/main.py** : Flask 웹서버 실행, 모든 라우트 등록 (이 파일만 실행하면 전체 서비스 동작)
- **src/camera/camera_manager.py** : 카메라(라즈베리파이/USB 웹캠) 제어, 프레임 캡처, YOLO/OpenCV 통합 탐지
- **src/detection/yolo_detector.py** : YOLO v8 기반 객체 인식(고라니 등)
- **src/detection/opencv_detector.py** : OpenCV Haar Cascade 기반 객체 인식 (얼굴 탐지)
- **src/routes/main_routes.py** : 메인 페이지(`/`), 비디오 피드(`/video_feed`) 라우트
- **src/routes/settings_routes.py** : 카메라/AI 설정 변경 라우트(`/update_settings` 등)
- **src/routes/status_routes.py** : 시스템 상태 확인 라우트(`/status`)
- **src/utils/settings_manager.py** : 설정 파일(카메라/AI 등) 로드/저장
- **templates/index.html** : 웹 UI(실시간 스트리밍, 설정, 상태 등 표시)
- **static/css/style.css, static/js/script.js** : UI 스타일/동작 담당
- **models/best.pt** : YOLO 학습된 모델(고라니 등 인식용)
- **models/data.yaml** : YOLO 클래스 정보 (고라니 등 클래스 이름)
- **models/haarcascades/** : OpenCV 얼굴 탐지 모델 파일들

---

## 📦 주요 패키지 분류 및 환경설정 주의사항

### 🤖 AI/머신러닝

- **ultralytics**: YOLO 객체 탐지
- **torch/torchvision**: 딥러닝 프레임워크
- **numpy, scipy**: 과학 연산

### 📷 카메라/이미지

- **picamera2**: 라즈베리파이 카메라 제어
- **opencv-python**: 컴퓨터 비전 (얼굴 탐지 등)
- **pillow**: 이미지 처리

### 🌐 웹 서버

- **Flask**: 웹 프레임워크
- **Jinja2**: 템플릿 엔진

### 🔧 하드웨어 제어

- **pyserial**: 시리얼 통신
- **psutil**: 시스템 모니터링

---

## 📚 참고/기록

- **requirements.txt**: [필수 패키지 및 버전, 환경설정 주의사항이 모두 명시되어 있으니 반드시 참고하세요!]
- **RPI_git_READEME.md**: 라즈베리파이 git md
- **PROJECT_OVERVIEW.md**: 실제 개발/운영/학습 시 한눈에 보는 요약본

---

## ⚠️ 환경설정/호환성 주의사항 (필독!)

- **requirements.txt 파일을 반드시 참고하세요!**

  - 모든 필수 패키지와 권장 버전이 명시되어 있습니다.
  - 설치/환경설정 시 `pip install -r requirements.txt`로 설치하세요.

- **OpenCV, YOLOv8, 라즈베리파이 환경설정이 까다로울 수 있습니다.**

  - numpy, torch, opencv-python 등 라이브러리 버전이 맞지 않으면 에러가 발생할 수 있습니다.
  - 특히 numpy, torch, opencv-python, picamera2는 서로 호환되는 버전이어야 합니다.

- **실제 경험 기반 주의사항**

  - numpy 버전이 너무 높거나 낮으면 OpenCV, YOLO, picamera2와 충돌할 수 있습니다.
  - requirements.txt에 명시된 버전(예: numpy==2.2.6, opencv-python==4.12.0.88 등)을 꼭 지켜주세요.
  - 라즈베리파이 환경에서는 pip로 설치 시 종종 빌드/의존성 에러가 발생할 수 있으니, 에러 메시지를 잘 확인하고 필요시 패키지별 공식 문서/이슈를 참고하세요.

- **OpenCV 디버깅/호환성**

  - 일부 환경(특히 라즈베리파이 최신 OS, Python 3.13.x 등)에서 OpenCV가 정상 동작하지 않을 수 있습니다.
  - 이 경우 opencv-python, numpy, picamera2 버전을 requirements.txt에 맞춰 재설치하거나, 필요시 opencv-python-headless로 대체해보세요.
  - 만약 계속 문제가 발생하면, 공식 깃허브 이슈나 커뮤니티에 환경정보와 에러 메시지를 공유해 도움을 받으세요.

- **YOLOv8, torch, ultralytics**
  - torch, ultralytics, torchvision은 반드시 서로 호환되는 버전으로 설치해야 합니다.
  - requirements.txt에 명시된 버전 외에는 예기치 않은 에러가 발생할 수 있습니다.

---

## 🔧 업데이트 필요

### YOLO 클래스 이름 해결

- **문제**: YOLO 탐지 시 `Class_0`으로만 표시됨
- **원인**: `data.yaml` 파일이 없어서 클래스 이름을 알 수 없음
- **해결**: 훈련 폴더에서 `data.yaml` 파일을 `models/` 폴더로 복사
- **결과**: `Class_0: 고라니`로 정확한 클래스 이름 표시

### OpenCV 얼굴 탐지 개선

- **문제**: OpenCV cascade 파일을 찾을 수 없음
- **원인**: Haar cascade 파일이 설치되지 않음
- **해결**: OpenCV 내장 cascade 경로 자동 탐지 및 사용자 지정 경로 지원
- **결과**: 얼굴 탐지 기능 활성화

### 탐지 결과 시각적 구분

- **YOLO 탐지**: 파란색 박스 + 파란색 라벨 배경
- **OpenCV 얼굴 탐지**: 빨간색 박스 + 빨간색 라벨 배경
- **크기 구분**: OpenCV 박스는 15픽셀 패딩으로 더 크게 표시

### 텍스트 렌더링 개선

- **문제**: 한글 폰트 인코딩 문제로 텍스트 깨짐
- **해결**: 안전한 텍스트 렌더링 함수 추가, ASCII 문자만 사용
- **결과**: 깔끔한 영문 텍스트 표시

### YOLO/OpenCV 이중 탐지 시스템 구현 (25.07.19 추가)

- **목표**: 같은 객체를 YOLO와 OpenCV 두 가지 방법으로 탐지하여 시스템 안정성 확보
- **구현**: OpenCV도 YOLO 모델(`best.pt`)을 사용해서 같은 고라니 객체 탐지
- **시각적 구분**:
  - **YOLO**: 파란색 박스 + `YOLO: 고라니 (신뢰도)`
  - **OpenCV**: 빨간색 박스 + `OpenCV: 고라니 (신뢰도)` (15픽셀 패딩으로 더 크게)
- **장점**: YOLO가 못 잡아도 OpenCV가 잡으면 시스템 작동 확인 가능

### OpenCV cv2.data 오류 해결 (25.07.19 추가)

```python
# 오류 원인: 일부 OpenCV 설치 환경에서 cv2 모듈에 'data' 속성이 없음
# "data" is not a known attribute of module "cv2" 오류 발생

# 해결 방법 1: hasattr 체크 후 사용
if hasattr(cv2, 'data') and hasattr(cv2.data, 'haarcascades'):
    cascade_path = cv2.data.haarcascades + 'haarcascade_frontalface_default.xml'
    cascade = cv2.CascadeClassifier(cascade_path)

# 해결 방법 2: 대안 경로 사용
elif os.path.exists(os.path.join('models', 'haarcascades')):
    cascade_path = os.path.join('models', 'haarcascades', 'haarcascade_frontalface_default.xml')
    cascade = cv2.CascadeClassifier(cascade_path)

# 해결 방법 3: pip로 OpenCV 재설치
pip uninstall opencv-python
pip install opencv-python==4.12.0.88
```

---

## 🛠️ 문제 해결

### 카메라 관련

```bash
# 카메라 상태 확인
vcgencmd get_camera
# 결과: supported=1 detected=1 (정상)

# 카메라 테스트 (라즈베리파이 pi4 camera V2 기준)
libcamera-hello
```

### 패키지 설치 오류

```bash
# 빌드 도구 재설치
sudo apt install -y build-essential python3-dev libjpeg-dev

# simplejpeg 수동 설치
pip install --ignore-installed --no-binary :all: simplejpeg
```

### 웹 서버 접속 불가

```bash
# 방화벽 확인
sudo ufw status

# 포트 5000 열기
sudo ufw allow 5000
```

### YOLO 클래스 이름 문제 (25.07.19 추가)

```bash
# 훈련 폴더에서 data.yaml 복사
cp /path/to/training/data.yaml models/data.yaml

# 또는 수동으로 클래스 이름 확인
python -c "from ultralytics import YOLO; model = YOLO('models/best.pt'); print(model.names)"
```

### OpenCV 얼굴 탐지 문제 (25.07.19 추가)

```bash
# OpenCV 내장 cascade 경로 확인
python -c "import cv2; print(cv2.data.haarcascades)"

# 수동으로 cascade 파일 다운로드
wget https://raw.githubusercontent.com/opencv/opencv/master/data/haarcascades/haarcascade_frontalface_default.xml -O models/haarcascades/haarcascade_frontalface_default.xml
```

### OpenCV cv2.data 오류 해결 (25.07.19 추가)

```python
# 오류 원인: 일부 OpenCV 설치 환경에서 cv2 모듈에 'data' 속성이 없음
# "data" is not a known attribute of module "cv2" 오류 발생

# 해결 방법 1: hasattr 체크 후 사용
if hasattr(cv2, 'data') and hasattr(cv2.data, 'haarcascades'):
    cascade_path = cv2.data.haarcascades + 'haarcascade_frontalface_default.xml'
    cascade = cv2.CascadeClassifier(cascade_path)

# 해결 방법 2: 대안 경로 사용
elif os.path.exists(os.path.join('models', 'haarcascades')):
    cascade_path = os.path.join('models', 'haarcascades', 'haarcascade_frontalface_default.xml')
    cascade = cv2.CascadeClassifier(cascade_path)

# 해결 방법 3: pip로 OpenCV 재설치
pip uninstall opencv-python
pip install opencv-python==4.12.0.88
```

---

## 📤 파일 전송 (SCP)

### 모델 파일 전송

```bash
# Windows에서 라즈베리파이로
scp "C:\path\to\best.pt" pi@192.168.x.x:/home/pi/autocarz/models/
scp "C:\path\to\data.yaml" pi@192.168.x.x:/home/pi/autocarz/models/

# Linux/Mac에서 라즈베리파이로
scp /path/to/best.pt pi@192.168.x.x:/home/pi/autocarz/models/
scp /path/to/data.yaml pi@192.168.x.x:/home/pi/autocarz/models/
```

### 이미지 파일 가져오기

```bash
# 라즈베리파이에서 로컬로
scp pi@192.168.x.x:/home/pi/autocarz/pi_detected_images/* ./downloads/
```

---

## 🎮 카메라 명령어 모음(라즈베리파이 Camera V2 기준)

### 사진 촬영

```bash
# 기본 사진 촬영
libcamera-still -o ~/autocarz/pi_detected_images/test.jpg

# 타임스탬프 포함 사진
libcamera-still -o ~/autocarz/pi_detected_images/photo_$(date +%Y%m%d_%H%M%S).jpg
```

### 동영상 녹화

```bash
# 10초 동영상 녹화
libcamera-vid -t 10000 -o ~/autocarz/pi_detected_images/video_$(date +%Y%m%d_%H%M%S).h264
```

---

## 🟡 Flask 실시간 스트리밍: 프레임 변환 핵심 포인트

- numpy 배열(이미지)은 바로 yield하면 안 됨!
- 반드시 JPEG 등으로 인코딩 → bytes로 변환 → yield

### 📌 핵심 포인트

- `cv2.imencode('.jpg', frame)`
  - numpy 배열을 JPEG로 압축(인코딩)
  - buffer(=이미지 바이트 배열) 반환
- `buffer.tobytes()`
  - numpy 배열을 진짜 bytes로 변환
- `yield (b'--frame...')`
  - Flask가 실시간 스트리밍으로 인식할 수 있는 포맷

### 🛠️ 예시 코드

```python
while True:
    ret, frame = cap.read()
    if not ret:
        continue
    ret, buffer = cv2.imencode('.jpg', frame)
    if not ret:
        continue
    frame_bytes = buffer.tobytes()
    yield (b'--frame\r\n'
           b'Content-Type: image/jpeg\r\n\r\n' + frame_bytes + b'\r\n')
```

---

## ⚠️ 주의사항

### ✅ Git 관리

- models/ 폴더: 용량이 크므로 Git에서 제외
- pi_detected_images/: 감지된 이미지들은 Git에서 제외
- venv/: 가상환경은 Git에서 제외

### ✅ 성능 최적화

- 라즈베리파이 4 권장 (메모리 부족 시 swap 증가)
- 높은 해상도 사용 시 FPS 저하 가능
- 모델 파일 크기에 따른 로딩 시간 고려

### ✅ 템플릿 파일명 변경

- 웹 UI 템플릿 파일이 `templates/index.html` → `templates/yolo_opencv.html`로 변경될 경우
- Flask 라우트 및 코드에서 모두 반영 해야됨 (render_template('yolo_opencv.html'))

### ✅ requirements.txt 통합 관리

- Windows/라즈베리파이 모두 1개 파일로 관리
- 라즈베리파이에서는 `picamera2`만 별도 설치 필요 (sudo apt-get install python3-picamera2)
- pip freeze > requirements.txt로 버전 동기화 가능

### ✅ system_info.txt 활용

- 카메라, Python, YOLO, OpenCV 등 버전 정보 기록 예시:
  - Python, torch, ultralytics, opencv-python, numpy, pillow 등 주요 버전
  - 연결된 카메라 개수/이름 등도 기록

### ✅ .gitignore 실수 방지

- 대용량/민감/개인 설정 파일은 반드시 .gitignore에 추가
- 각 항목별로 "왜 제외하는지" 주석 설명 추가 (실수 방지)

### ✅ 폴더명 변경 안내

- autocarz(복사본 등) → autocarz로 폴더명 변경해도 코드/실행에 영향 없음
- 경로가 하드코딩된 부분 없음 (import, Flask, 모델 로딩 등 모두 상대경로)

### ✅ 실전 git push 체크리스트

- push 전 반드시 백업 (README.md, requirements.txt, .gitignore 등)
- git status로 변경 파일 확인
- .gitignore/requirements.txt/README.md만 신중하게 업데이트
- system_info.txt 최신화 후 커밋 추천

---

## ❓ 자주 묻는 질문 (FAQ)

- **Q. 어떤 파일을 실행해야 하나요?**
  - A. `src/main.py`만 실행하면 전체 서비스가 동작합니다.

- **Q. 웹캠으로도 되나요?**
  - A. 네, USB 웹캠도 지원합니다. 라즈베리파이와 노트북 모두에서 자동으로 최적화됩니다.

- **Q. 고라니가 다른 동물로 잘못 인식돼요!**
  - A. 고라니 전용 모델(`models/best.pt`)을 사용하고 있는지 확인하세요. COCO 모델은 고라니를 인식하지 못합니다.

- **Q. 모델을 바꾸고 싶어요!**
  - A. `models/best.pt`와 `models/data.yaml`만 교체하면 됩니다. 코드 수정 불필요!

- **Q. 검출 결과가 너무 빨리 사라져요!**
  - A. 검출된 객체 박스는 3-5초간 유지됩니다. 더 오래 유지하려면 `camera_manager.py`의 `result_keep_time` 값을 조정하세요.

- **Q. 라즈베리파이에서 성능이 느려요!**
  - A. 자동으로 성능 최적화 모드(640x480, 15fps)로 설정됩니다. 더 빠르게 하려면 해상도를 낮추세요.

- **Q. CSS가 적용 안 돼요!**
  - A. 반드시 Flask 서버(5000포트)로 접속해야 합니다. HTML에서 Live Server로는 안 됩니다.

- **Q. 기능별 코드는 어디에 있나요?**
  - A.
    - `camera_manager.py` (카메라 + YOLO/OpenCV 통합)
    - `yolo_detector.py` (YOLO 객체 인식)
    - `opencv_detector.py` (OpenCV 보조 인식)
    - `main.py` (Flask 웹서버)
    - `routes/` 폴더 (웹 API)

---

> **이 문서는 AutocarZ 프로젝트를 쉽게 이해하고, 기록/학습/운영에 활용할 수 있도록 작성되었습니다.**
