# rokey_motion_cv

- 생성 : 2026.01.27 화
- 목적 : 실무 프로젝트
- 기간 : 260127-260209
- 최종 업데이트 : 260130

# 🏗️ AI Co-Cooking Robot System
**LLM 기반 음성 인식 및 실시간 트래킹을 활용한 주방 보조 로봇 시스템**

## 1. 개요 (Summary)
본 프로젝트는 사용자의 음성 명령을 LLM(Large Language Model)으로 해석하여 최적의 요리 레시피를 제안하고, 로봇팔이 실시간으로 사용자를 트래킹하며 요리 도구 전달 및 조리 보조(시즈닝 등)를 수행하는 지능형 협동 로봇 시스템입니다. 사용자와 로봇 간의 원활한 상호작용과 정밀한 매니퓰레이션 기술의 결합을 목표로 합니다.

---

## 2. 하드웨어 구성 (Hardware Configuration)
* **Robot Arm:** Doosan Robotics M0609
* **Gripper:** 2-Finger Parallel Gripper
* **Sensor:** * Depth Camera (Intel RealSense D435i 등) - 사용자 및 사물 인식용
  * Microphone Array - 음성 명령 수집용
* **Controller:** PC (Ubuntu 22.04, ROS2 Humble)

---

## 3. 시스템 아키텍처 (System Architecture)
시스템은 Perception, Decision, Action의 유기적인 순환 구조로 설계되었습니다.

* **Perception:** Vision 기반 사용자 스켈레톤 트래킹 및 Whisper/Google STT 기반 음성 인식.
* **Decision:** LangChain 및 OpenAI를 활용한 사용자 의도 파악 및 레시피 생성.
* **Action:** ROS2 MoveIt2를 통한 충돌 방지 경로 계획 및 DRL(Doosan Robot Language) 기반 모션 제어.

---

## 4. 핵심 워크플로 (Key Workflow)

### [Phase 1] 사용자 인식 및 인터랙션
* **사용자 인식:** 카메라를 통해 사용자의 위치를 실시간으로 추적하며 이동합니다.
* **요구사항 수렴:** 사용자가 요리 메뉴 추천이나 추천 요구 시 LLM이 의도를 분석합니다.
* **인식 전달:** 로봇은 인식 여부와 요청 사항 확인 메시지를 사용자에게 피드백합니다.

### [Phase 2] 조리 중 보조 수행
* **사용자 트래킹:** 조리 중인 사용자의 움직임을 지속적으로 추적합니다.
* **음성 기반 조작:** "소금 뿌려줘"와 같은 요청 시 냄비 위치로 이동하여 시즈닝 동작을 수행합니다.
* **도구 전달:** 필요한 시점에 요리 도구를 픽앤플레이스(Pick-and-Place)하여 사용자에게 전달합니다.

### [Phase 3] 작업 종료 및 정리
* **정리:** 요리가 종료되면 로봇팔이 도구나 용기를 싱크대(볼) 영역으로 안전하게 이동시킵니다.
* **재시작:** 작업 완료 후 다시 사용자 트래킹 모드로 복귀하여 다음 명령을 대기합니다.

---

## 5. 의존성 (Dependencies)
`requirements.txt`에 포함될 주요 라이브러리 목록입니다.

# ROS2 Framework
rclpy

# AI & NLP
openai
langchain

# Computer Vision & Math
opencv-python
pyrealsense2
numpy

---

## 6. 간단 사용설명(Quick Start)
# 워크스페이스 빌드
cd ~/ros2_ws
colcon build --packages-select co_cooking_robot
source install/setup.bash
# 시스템 설명
ros2 launch co_cooking_robot cooking_main.launch.py
