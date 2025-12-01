# 협업의 민족
## Software Design Specification

![로고 이미지](./image/로고.png) 

## TEAM MEMBER
* 22213492 | 강재호 | kikihottong@gmail.com
* 22321627 | 김가영 | 22321627@yu.ac.kr
* 22321620 | 류승현 | 22321620@yu.ac.kr
* 22321658 | 박한비 | phbengel@yu.ac.kr
* 22011728 | 손승우 | thstmddn321@yu.ac.kr
* 22112114 | 신현우 | 22112114@yu.ac.kr
* 22321615 | 전준환 | hwan3009@yu.ac.kr

---

## Revision history

| Revision date | Version # | Description | Author |
| :--- | :--- | :--- | :--- |
| 10/26/2025 | 1.0.0 | 보고서의 전반적인 틀 및 Introduce 작성 | 박한비 |
| 10/27/2025 | 1.1.0 | Use Case Diagram 추가 | 박한비 |
| 10/28/2025 | 1.1.1 | Use Case Description 작성(1) | 박한비 |
| 10/29/2025 | 1.1.2 | Use Case Description 작성(2) | 박한비 |
| 10/31/2025 | 1.1.3 | Use Case Description 작성(3) | 박한비 |
| 11/01/2025 | 1.1.4 | Use Case Description 작성(4) | 박한비 |
| 11/03/2025 | 1.1.5 | Use Case Description 수정(5) | 박한비 |
| 11/03/2025 | 1.2.0 | UI prototype 작성 | 박한비 |
| 11/04/2025 | 1.3.0 | 구현 요구사항 작성 | 박한비 |
| 11/05/2025 | 1.4.0 | DB Class Diagram 보고서 통합 | 박한비 |
| 11/06/2025 | 1.4.1 | 김가영 피드백 기반 전반적인 보고서 수정 | 박한비 |
| 11/07/2025 | 1.5.0 | Sequence Diagram 및 State Machine Diagram, Glossary, References 추가 | 박한비 |
| 11/08/2025 | 1.5.1 | 보고서 전체적인 단어 수정 | 박한비 |
| 11/09/2025 | 1.6.0 | Function class diagram 추가 | 박한비 
| 11/25/2025 | 1.6.1 | Use case description 작성(5) 및 수정, Use case diagram 수정 | 박한비 |
| 11/26/2025 | 1.6.2 | Glossary, UI prototype 수정 | 박한비 |
| 11/27/2025 | 1.6.3 | UI prototype 이미지 변경 및 내용 삭제 | 박한비 |
| 11/30/2025 | 1.6.4 | Class diagram 및 Sequence diagram 수정 | 박한비 |
| 12/01/2025 | 1.7.0 | 시간조율 class diagram, sequence diagram 추가 | 박한비 |

---

## Contents
1.  Introduction
2.  Use case analysis
3.  Class diagram
4.  Sequence diagram
5.  State machine diagram
6.  User interface prototype
7.  Implementation requirements
8.  Glossary
9.  References

---

## Authors for each section
* Introduction – 박한비
* Use case analysis – 박한비
* Class diagram
  * DB class diagram - 강재호, 손승우, 전준환
  * Function class diagram
    * User class diagram - 손승우
    * Project class diagram - 강재호
    * Calendar class diagram - 강재호
    * 시간조율 class diagram – 전준환
* Sequence diagram
  * User sequence diagram – 김가영, 손승우
  * Project sequence diagram – 류승현, 강재호
  * Calendar sequence diagram – 류승현, 강재호
  * Community sequence diagram – 신현우, 전준환
  * 시간조율 sequence diagram – 전준환
* State machine diagram - 류승현, 박한비, 신현우
* User interface prototype – 박한비
* Implementation requirements - 박한비
* Glossary – 류승현, 박한비, 전준환
* References - 박한비

---

## 1. Introduction
 현대 사회는 개인의 역량뿐만 아니라, 팀 단위의 목표 달성을 협업 능력을 점점 더 중요하게 요구하고 있다. 본 프로젝트는 협업의 핵심 과제로 일정 관리와 소통을 규정하며, 이 둘은 결과물의 품질에 직접적인 영향을 미친다. 실제로 전문적인 일정 관리 도구가 아닌 카카오톡 투표 같은 것을 사용하여 회의 일정을 정하려고 할 때, 일정 조율에 어려움이 존재하는 것을 경험하였다. 시간 단위로 투표를 만들기에는 번거로우며, 일 단위로 투표를 진행하여도 투표 종료 후 구체적인 시간을 조율하기에는 힘들었다. 이때 보조도구로서 when2meet을 사용하더라도 UI가 불편하여 반복 활용성이 낮다는 지적이 있었다. 또 다른 팀원은 팀 일정을 개인 캘린더에 저장하는 것을 깜빡하여 일정 충돌이나 누락으로 프로젝트 진행에 차질을 빚는 경험을 공유하였다.

 본 프로젝트는 이러한 실사용 맥락의 불편을 해소하기 위해 웹 기반 협업 관리 도구 '협업의 민족'을 제안한다. '협업의 민족'은 설치 없이 접근 가능한 웹 환경과 누구나 빠르게 익힐 수 있는 직관적 UI를 기반으로 한다. 팀의 모든 일정을 한 곳에 모으고 개인 일정을 자동으로 동기화여 일정 누락 및 충돌을 줄이며, 동시에 팀원 간의 시간 조율과 일정 확정 단순화에 초점을 맞추었다. 더불어 협업에 필수적인 의사소통 창구로서 게시판을 제공하여 아이디어 공유와 자료 교환, 피드백 순환을 한 플랫폼 안에서 처리할 수 있도록 설계하였다.
 
 이를 위해 '협업의 민족'은 다음과 같은 핵심 기능들을 제공한다. 첫째, '마이페이지' 및 '달력' 기능은 팀원 개인의 일정을 공유하고 팀 전체의 스케줄을 통합 관리한다. 둘째, 별도의 '시간조율' 기능으로 프로젝트 참여자들 간의 가용 시간 교집합을 계산하여 회의 시간 후보 수집부터 확정까지의 과정을 효율화한다. 셋째, '게시판' 기능은 아이디어 공유, 자료 교환, 실시간 피드백 등 원활한 의사소통을 위한 창구 역할을 수행한다.
 
'협업의 민족'은 이러한 통합적인 기능 제공을 통해 사용자에게 효율적인 일정 조율, 원활한 의사소통이라는 보다 편한 협업 환경을 제공하고자 한다. 본보고서는 현재 페이지의 '협업의 민족'의 개발 배경 및 목적을 시작으로, 각 단계별 Diagram 및 관련 설명과 UI Prototype에 대해 순차적으로 기술할 것이다.
