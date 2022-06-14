---
finished date: 2021-06-26
tags:
  - IoT
  - OpenCV
  - RPI
  - Arduino
  - Firebase
  - App_Inventor
  - Python
  - team_project
---
# smart_sink
아두이노, 라즈베리파이, Firebase의 실시간 통신을 이용한 스마트 홈 시스템

- 사용자가 스스로 사용 환경을 조절할 수 있도록 구현, 맞춤형 서비스
- 원하는 용도에 맞춘 물 온도 값 설정
- 사용자의 세안액 및 물비누 제공량 설정을 통해 기존 일정량만 제공되는 방식 개선
- 세면대 배수구 막힘 관리
- 세안액, 물비누, 배수 막힘 관리 용액 부족 방지(앱 알림)
- 자외선 살균 기능 자동 동작

## system architecture

<p align="center">
  <img src="/images/system_architecture.png" alt="system architecture">
</p>

## HW
1.	실제 사이즈 세면대 구현  
시연 시 실제 사용하는 것과 같은 환경을 조성하기 위해 성인 남녀 기준 적정 높이인 1m로 세면대 높이를 설정한다. 물 온도 조절 기능 등을 구현하기 위해 일정 크기 이상의 물 탱크를 배치해야 하는 것 또한 실제 사이즈에 가까운 세면대를 구현한 요인 중 하나이다. 하드웨어 뼈대는 각종 디지털 디바이스, 액체 탱크 밑 용기의 부피와 하중을 견딜 수 있는 다용도 수납장으로 한다. 
2.	외관 디자인  
깔끔한 외관을 위해 포맥스와 하드보드지로 다용도 수납장의 겉면을 감싸 욕실 내 벽을 구현한다. 세면대 시연을 위해 실제 물을 사용하기 때문에 방수가 되는 재료를 사용하는 것이 필수적이라 판단하였다.

<p align="center">
  <img src="/images/sink_front.jpg" alt="sink front"><br/>
  세면대 정면    
</p>
<p align="center">
  <img src="/images/sink_back.jpg" alt="sink back"><br/>
  세면대 후면
</p>


3.	탱크 및 호스  
각종 용액을 담는 탱크 및 통은 구멍 뚫기 등 가공이 편리한 플라스틱 용기로 구성한다. 호스는 가공이 쉽고 시각적으로 액체의 이동을 확인하기 쉬운 투명 실리콘 호스로 선택하였다.

<p align="center">
  <img src="/images/inside sink.png" alt="inside sink idea"><br/>
  세면대 내부 아이디어
</p>

<p align="center">
  <img src="/images/sink_inside.jpg" alt="inside sink"><br/>
  실제 세면대 내부
</p>

<p align="center">
  <img src="/images/sink_tank.jpg" alt="sink tank"><br/>
  세면대 탱크
</p>


4.	수도꼭지 및 세면대  
세면대의 수도꼭지는 각각 다른 용액(물, 세안제/물비누, 막힘 방지 용액)을 위한 실리콘 호스 여러 개가 한 수도꼭지에서 나오는 것처럼 구현하기 위해 3D 모델링으로 직접 디자인하여 사용한다. 또한 세면대는 가공이 편리한 플라스틱 대야로 이뤄졌으며 배수를 위해 구멍을 뚫는 등의 가공을 하였다. 세면대 배수구에 사용되는 누수 방지 부품 또한 3D 모델링으로 제작하였다. 

## recognition
### face
- using Haar feature
- get pictures and apply GaussianBlur and recognize LBPHFaceRecognizer
- use CascadeClassifier to find out face in a picture
- train model with LBP Recognizer
### gesture
- using Haar feature
- using Fist, Palm open source model

## user setting
- 아두이노, 라즈베리파이, Firebase와 앱 인벤터를 사용해 사용자 맞춤형 서비스를 제공하고 사용 환경이 자동으로 조성되는 시스템을 구현하였다. 
1.	스마트 세면대 전용 안드로이드 Application 제작  
앱 인벤터를 사용해 ‘스마트 세면대’ 전용 안드로이드 Application을 제작하여 사용자가 다양한 값을 설정할 수 있도록 하였다. App을 사용하여 사용자별로 기본 모드의 물 온도, 세안 모드의 물 온도, 세안 모드의 세안제 사용 전 물 배출 시간, 세안 모드의 세안제 배출 시간, 세안 모드의 헹구는 물 배출 시간, 손 씻기 모드의 물비누 사용 전 물 배출 시간, 손 씻기 모드의 물 비누 배출 시간, 손 씻기 모드의 헹구는 물 배출 시간을 설정할 수 있다. 또한, App을 사용하여 전체 세면대에 적용되는 살균 UV LED 점등 시간, 배수구 막힘 방지 관리 용액 분사 주기 및 마지막 분사 날짜를 설정할 수 있다. 더불어 원하는 때에 배수구 막힘 관리를 시작할 수 있도록 한다. 
2.	실시간 데이터 베이스 Firebase에 저장  
사용자가 앱에서 지정한 값은 Firebase realtime database에 저장된다. 사용자마다 모드별 물 온도와 세부 설정을 저장한다. 또한 세면대 전체에 적용되는 설정에 대해서도 설정값을 저장한다.
3.	Pi Cam, OpenCV 이용 사용자 및 제스처 인식  
Pi cam과 OpenCV를 통해 스마트홈 구성원의 사진을 학습시켜 얼굴 인식을 구현하고 손을 사용하여 약속한 제스처를 인식하여 라즈베리파이에서 이를 분석하여 사용자를 구별하고 입력된 모드를 확인한다. 모드 인식이 완료되면 라즈베리파이에서 Firebase의 저장된 설정 값을 아두이노로 시리얼 통신을 통해 전송하여 사용자 설정 값에 따라 사용환경을 세팅한다. 이 때, 저장된 사용자가 아닌 경우에는 게스트로 인식하여 게스트에 해당하는 설정 값으로 세팅 된다.
4.	세면대 디스플레이  
사용자는 세면대 디스플레이를 통해 사용자 및 제스처 인식을 진행할 수 있고 설정이 완료되면 GUI를 통해 사용 중인 모드 및 설정 값을 실시간으로 확인할 수 있다.

## temperature control
1.	라즈베리파이, 아두이노 시리얼 통신  
사용자가 ‘스마트 세면대’ 전용 Application에 설정해둔 물 온도 정보를 라즈베리파이에서 Firebase로부터 읽어온다. 라즈베리파이는 시리얼 통신을 통해 이 값을 아두이노로 전송한다.
2.	물 온도 조절  
아두이노에서는 이를 float 형식으로 변환하여 목표 온도로 인식한다. 아두이노는 해당 목표 온도를 만들기 위해 온수와 냉수의 현재 온도를 온도 센서를 통해 파악하고 이를 바탕으로 연산을 진행하여 온수 및 냉수의 워터 펌프의 작동시간을 주기적으로 조절함으로써 그 비율을 조절하고 목표 온도를 갖는 물을 만들 수 있다.

## sewer system
1.	앱 인벤터를 통한 주기 설정 및 배수 관리  
스마트 세면대의 배수구 막힘을 방지하기 위하여 앱 인벤터를 통해 제작한 전용 Application을 통해 사용자가 설정한 주기에 맞춰 혹은 전용 application에서 사용자가 배수구 막힘 방지 기능을 실행했을 때 막힌 배수구를 뚫는 용액이 분사된다. 
2.	경고 문구  
분사 후에는 일정시간 세면대가 동작하지 않으며, 이 때 사용자의 위치를 적외선 센서로 측정하여 사용자 접근으로 파악되면 사용 불가 경고 알림을 시리얼 통신을 통해 라즈베리파이에 전송한다. 라즈베리파이는 해당 정보를 Firebase에 실시간으로 업데이트하고 사용자는 해당 경고 알림을 Application으로 확인함으로써 쾌적하게 세면대를 사용할 수 있다. 또한 디스플레이 화면에서도 경고 문구를 확인할 수 있다.

## remain alarm
1.	잔여량 정보 전송  
세면대 내부의 아두이노 수위 센서를 통해 물비누, 세안액, 배수 막힘 관리 용액의 잔여량을 측정하여 일정 기준보다 적게 남은 경우 잔여량 부족 알림을 시리얼 통신을 통해 라즈베리파이에 전송한다.
2.	잔여량 부족 알람  
라즈베리파이는 해당 정보를 Firebase에 update하여 사용자가 세면대의 상태를 Application을 통해 실시간으로 확인할 수 있도록 한다.

## sterilization
1.	UV등 점등  
세면대 사용 후 쾌적한 환경과 위생을 위해 UV LED를 점등한다. 거리 센서로 사용자와의 거리를 측정하여 사용자가 충분히 멀어지면(80cm 초과) 점등한다.
2.	UV등 소등  
사용자가 사용을 위해 다가오면 (30cm 미만) 소등한다.

## File structure
```
|-- GUI
  |-- icons for GUI
  |-- result.png
  |-- smart_sink_ui.ui
|-- app_inventor
  |-- codes of app inventor as images
|-- face_recognition
  |-- face_dataset.py
  |-- 02_face_training.py
  |-- haarcascades
    |-- files for Haar Cascade
|-- images
  |-- images for this README.md
|-- smart sink 결과 보고서.pdf
|-- smart_sink_final.ipynb
```
- smart_sink_final.ipynb: 라즈베리파이에서 최종적으로 실행될 코드

## role
- 김동원: Firebase 데이터 저장 및 전송, 앱인벤터 알고리즘 개발 및 제어, 라즈베리파이 & Firebase 실시간 통신
- 백정훈: 아두이노 프로그래밍 및 하드웨어 설계, 3D 프린터 모델링, 아두이노 & 라즈베리파이 실시간 통신, 전체업무 조율
- 김나영: 앱인벤터 GUI 설계 및 제작, 회로 설계 및 아두이노 프로그래밍, 하드웨어 설계, 발표영상 편집
- 박희상: OpenCV를 이용한 실시간 얼굴 및 제스처 인식, 하드웨어 제작, SW 시스템 통합

## 한계점
- 파이어베이스, 아두이노, 영상처리와 관련된 전체 코드를 통합하는 과정에서 무한 루프 내에서 OpenCV를 이용한 얼굴 및 제스처 인식, 파이어베이스에서 실시간으로 값을 수신, 아두이노와 시리얼 통신을 통한 값 송수신을 제어하다보니 처리속도가 매우 느림을 발견했다. 
- Thread를 사용해도 동일한 결과가 발생했다. 자료를 찾아보니 python은 내부 정책에 따라서 전역인터프리터 락(GIL)으로 인해서 다중 스레드 환경에서는 싱글 프로세서만 사용하도록 설정되어 있음을 알 수 있었다. 
- 연산 속도를 높이기 위해서 Multiprocessing 방식을 이용하기로 했다. Thread와 달리 프로세스간 변수를 공유하지 않기 때문에 얼굴 및 제스처 인식을 통해서 얻은 결과값을 실시간 데이터로 받아오는 process로 넘겨주기 위해서 queue를 이용했다. 하지만 OpenCV에서 이미지를 인식할 수 없었고, 동시성 제어를 하는 것에서 에러가 발생하는 것임을 깨달을 수 있었다. 
- 동시성 제어를 위해서 세마포어, read write 락 등이 사용되는데 프로젝트의 진행 과정에서 시간 관계상 Multiprocessing으로 처리하는 것을 완료하지 못한 채 마무리했다. 차후 보완 과정에서 Multiprocessing을 사용하여 연산속도를 높이고 동시성 제어 또한 할 수 있다면 더욱 실용적인 스마트 세면대를 구현할 수 있을 것이다.

## reference
- P. S. Foundation, “multiprocessing---프로세스 기반 병렬 처리,” Python Softwaer Foundation, [온라인]. Available: https://python.flowdas.com/library/multiprocessing.html. [액세스: 25 5 2021].
- Paul Viola, Michael Jones, “Rapid Object Detection using a Boosted Cascade of Simple Features”, Cambridge, 2001
- Di Huang, Caifeng Shan, Mohsen Ardabilian, Yunhong Wang, Liming Chen, “Local Binary Patterns and Its Application to Facial Image Analysis”, VOL. 41, NO. 6, Nov, 2011
- MJRoBot, Real-Time Face Recognition: An End-to-End Project, Feb, 23, 2018
