# 🚍 AutocarZ - 자율주행 중 로드킬 안전 강화 및 자동 신고 서비스

# 🙋‍♂️🙋‍♀️ 팀원 소개

<table>
  <tr>
    <th>김지영</th>
    <th>이유석</th>
    <th>사석훈</th>
    <th>신윤서</th>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/3c347b94-3b95-4d42-82ea-ee20c7dfcc45" width="100"/></td>
    <td><img src="https://github.com/user-attachments/assets/44c93308-1e79-4dc5-8ac0-a2dbd30785e8" width="100"/></td>
    <td><img src="https://github.com/user-attachments/assets/26c18044-d8ab-4b8b-a5a9-fb6ccd8b1071" width="100"/></td>
    <td><img src="https://github.com/user-attachments/assets/9b3cd675-6764-4b3e-bca3-7a356b20843f" width="100"/></td>
  </tr>
  <tr>
    <td>PM<br>FrontEnd</td>
    <td>팀원<br>Data Engineer</td>
    <td>팀원<br>Data Engineer<br>Infra</td>
    <td>팀원<br>AI/ML Engineer</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/jiyoung1634">
        <img src="https://img.shields.io/badge/GitHub-Link-black?logo=github&style=flat"/>
      </a>
    </td>
    <td>
      <a href="https://github.com/LYSS-LGU">
        <img src="https://img.shields.io/badge/GitHub-Link-black?logo=github&style=flat"/>
      </a>
    </td>
    <td>
      <a href="https://github.com/Clear-head">
        <img src="https://img.shields.io/badge/GitHub-Link-black?logo=github&style=flat"/>
      </a>
    </td>
    <td>
      <a href="https://github.com/irisyshin">
        <img src="https://img.shields.io/badge/GitHub-Link-black?logo=github&style=flat"/>
      </a>
    </td>
  </tr>
</table>

-------------------------------------------------------------

# 프로젝트 기획서


## 1. 프로젝트 개요
- 프로젝트 주제 : 도로 위 **야생동물 실시간 인식 및 대응 시스템** : AI기반 자율주행 안전 강화 솔루션 설계
- 문제정의 : 주행 중 도로에 갑자기 나타나는 "야생동물"을 효과적으로 감지하여 사고 예방과 충돌 이후 **신속하게 대응할 수 있는 체계 부재**
- 목표 : **데이터 기반 인사이트 도출 및 시각적 보고**

---------------------------------------------------------------------------------------------


### 프로젝트 기획 - 서비스 흐름 다이어그램
![image](https://github.com/user-attachments/assets/d6993cc7-a5f4-4266-827c-193cb68da8ef)

--------------------------------------------------------

## 2. 1차 프로젝트 일정 

| 작업 항목                  | 시작 날짜   | 종료 날짜   | 기간(일) |
|---------------------------|------------|------------|---------|
| 프로젝트 정의 및 계획 수립  | 2025-06-25 | 2025-06-25 | 1       |
| 자료 조사 및 벤치마킹       | 2025-06-25 | 2025-06-25 | 1       |
| 아이디어 도출 및 시나리오 작성 | 2025-06-25 | 2025-06-26 | 2       |
| 요구사항 도출 및 WBS 작성   | 2025-06-26 | 2025-06-27 | 2       |
| Prototype 개발 및 검토     | 2025-06-26 | 2025-06-27 | 2       |
| 최종 검토 및 발표 준비     | 2025-06-27 | 2025-06-27 | 1       |
| 1차 프로젝트 발표              | 2025-06-27 | 2025-06-27 | 1       |
 
  --------------------------

# 작업 분할 구조 (WBS)


![alt text](image.png)

  ------------------------------

# 요구사항 정의서

## 1. 기능 요구사항
- [ ] 영상 기반 객체 인식 기능
- [ ] 객체 미인식 시 자동 패스 처리 
- [ ] 한 번 인식된 객체는 화면에서 사라질 때까지 하나의 동일한 객체로 추적
- [ ] 객체 분류 기능
- [ ] 객체 상태 분석 기능 (정지 상태 vs 움직임 상태 분류)
- [ ] 움직임 상태일 경우, 다음 타이밍 기준 재판단 기능
- [ ] 영상 수신 받은 서버는 알림 전송 가능
- [ ] 서버에서 일괄 사용자 알림 발송 기능
- [ ] 객체 인식 위치 확인 기능
- [ ] 인식된 이미지를 DB에 저장 및 향후 학습 데이터로 활용 가능
- [ ] 자주 인식되는 위치 식별 기능

## 2. 비기능 요구사항
- [ ] 객체 인식 시 1초 이내 처리
- [ ] 객체 인식 후 알림까지 5초 이내 수행
- [ ] 객체 인식은 실시간 처리 동작
- [ ] 객체 종류 확장 가능
- [ ] 이벤트 발생 시 비동기적 병렬 처리되어 동시 전송
- [ ] DB 외부 접근 불가, 오직 서버만 가능 (보안성 강화)

----------------------------

# 프로젝트 설계서

## 1. 시스템 아키텍처
![image](https://github.com/user-attachments/assets/fd706b11-a03f-4ab7-99fa-1b5a7d405f64)


## 1-2. 시스템 흐름 구성도

![image](https://github.com/user-attachments/assets/27616406-bb5d-45bf-9abd-92d60cbe5690)

 

## 2. 데이터 설계
- **데이터 흐름**: 원천 데이터 → 전처리 → 분석 → 결과
- **주요 데이터 속성** :
  - 속성 이름: [예: camera_id, value[location, timestamp], animal_image, animal_id 등]
  - 데이터 유형: [비정형 데이터, 구조화/반구조화된 데이터]
  
## 2-1. 네이밍 컨벤션 
```
파스칼 케이스 (PascalCase)형식: 각 단어의 첫 글자가 모두 대문자로 작성
```
```
스네이크 케이스 (snake_case)
형식: 모든 글자를 소문자로 작성하고, 단어는 밑줄(_)로 구분
예시: first_name, total_price, get_user_data
사용처: JavaScript에서는 잘 쓰이지 않지만, 종종 상수에 사용되기도 합니다.
```
```
상수 명명
모든 글자를 대문자로 작성하여 상수라는 점을 강조
"단어 간에 언더스코어(_)"를 넣어 읽기 쉽게 구분
예시 설명
MAX_VALUE: 시스템에서 사용할 최대값이나 특정 기준값
API_URL: 외부 API 호출에 사용하는 고정된 URL
DEFAULT_TIMEOUT: 네트워크 요청이나 기능의 기본 대기 시간을 지정하는 상수
```
## 3. 기술 스택
- 사용 언어 : ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)


### **📊 데이터 분석 및 처리**
- 라이브러리 : ![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white) ![NumPy](https://img.shields.io/badge/Numpy-013243?style=flat&logo=numpy&logoColor=white) ![OpenCV](https://img.shields.io/badge/opencv-%23white.svg?style=flat&logo=opencv&logoColor=white)

### **📈 시각화 도구**
-  ![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat&logo=matplotlib&logoColor=white) ![Seaborn](https://img.shields.io/badge/Seaborn-1E3C72?style=flat) ![Plotly](https://img.shields.io/badge/Plotly-3F4F75?style=flat&logo=plotly&logoColor=white) 

### **🤖 AI / ML**
- 모델 : ![YOLO](https://img.shields.io/badge/YOLO-00FFFF?style=flat&logo=YOLO&logoColor=black) ![ResNet50](https://img.shields.io/badge/ResNet50-FF5959?style=flat)
- 프레임워크 : ![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)
-  머신러닝 : ![Scikit-learn](https://img.shields.io/badge/Scikit--Learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)

### 🗃️ 인프라 & 배포 & 서버
- 클라우드 : ![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white) ![EC2](https://img.shields.io/badge/EC2-FF9900?style=flat&logo=amazon-ec2&logoColor=white)
- 데이터베이스 : <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white" />
- 서버 : ![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=flat&logo=fastapi)

## 4. 예상 문제 및 해결 방안

#### [기상 악화 등 환경 문제로 인한 객체 인식률 감소]
- **문제**: 기상 악화, 블랙박스 화질 저하 등으로 객체 인식률이 낮아질 수 있음
- **해결방안**:
  - 다양한 환경(야간, 안개, 비 등)에서 촬영된 데이터로 학습 강화
  - 고감도 카메라나 열화상 카메라 도입 검토
  - 화질 저하 대응을 위한 전처리 기술 적용

#### [객체 종류 증가로 인한 정확도 저하]
- **문제**:
  - 인식 가능한 객체 종류가 많아질수록 모델 정확도가 낮아질 수 있음
  - 이상치 및 위험 객체에 대한 판단 기준이 불명확하여 오탐지 가능성 존재
- **해결방안**:
  - 클래스 간 학습 데이터의 균형 확보 및 전처리 강화
  - Transfer Learning 또는 앙상블 기법을 통한 분류 정확도 향상
  - 다중 출력(multi-head) 구조 설계로 다양한 객체 처리 가능
  - 명확한 임계값 설정 및 위험 객체군 사전 정의로 이상치 판단 기준 강화

#### [실시간 처리 속도 문제]
- **문제**: 영상 → 분석 → 알림까지 속도가 느릴 수 있음
- **해결방안**:
  - Edge AI 또는 병렬 처리 도입
  - 실시간 스트리밍 최소화, 이벤트 기반 트리거 방식 채택

#### [GPS 위치 정확도 저하]
- **문제**: GPS 좌표가 정확하지 않아 위치 정보에 오류 발생 가능성
- **해결방안**:
  - RTK-GPS 또는 DGPS 등의 정밀 측위 기술 도입
  - 맵 매칭 기반 위치 보정 알고리즘 적용

#### [카메라와 서버 간 연결 불안정]
- **문제**: 통신 오류로 인해 실시간 전송 실패 가능
- **해결방안**:
  - 자동 재연결 기능 구현
  - LTE/5G 네트워크 이중화 적용

#### [프로젝트 기한 내 구현 가능성]
- **문제**: 제한된 시간 내에 모든 기능 구현이 어려울 수 있음
- **해결방안**:
  - 핵심 기능 중심의 MVP 전략 수립
  - 역할 분담 및 일정 관리 툴 적극 활용 (편집됨) 