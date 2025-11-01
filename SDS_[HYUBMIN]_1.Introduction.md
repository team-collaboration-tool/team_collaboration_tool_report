# 협업의 민족
## Software Design Specification

![로고 이미지](./image/로고.png) 

## TEAM MEMBER
* 22213492 | 강재호 | 
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
* Class diagram – 
* Sequence diagram – 
* State machine diagram – 
* User interface prototype – 박한비
* Implementation requirements - 
* Glossary – 박한비
* References - 

---

## 1. Introduction
현대 사회는 개인의 역량뿐만 아니라, 공동의 목표를 달성하기 위한 팀원 간의 협업 능력을 점점 더 중요하게 요구하고 있다. 학생들의 팀 프로젝트에서부터 직장 내 부서 간의 공동 업무 수행에 이르기까지, 다양한 형태의 협업은 이제 일상적인 과제가 되었다. 그러나 협업 과정에서 구성원 간의 일정을 조율하고, 원활하게 의사소통하며, 프로젝트의 전체적인 진행 상황을 명확하게 공유하는 것은 여전히 복잡하고 어려운 문제로 남아있다. 이러한 문제점은 종종 프로젝트의 지연, 자원의 낭비, 그리고 최종 결과물의 품질 저하로 이어지곤 한다.

본 프로젝트는 이러한 협업 과정의 어려움을 체계적으로 해결하고, 팀 단위의 공동 작업을 보다 효율적으로 수행할 수 있도록 돕는 웹 기반 협업 관리 도구 '협업의 민족'의 개발에 관해 기술한다. '협업의 민족'은 팀 프로젝트를 수행하는 학생들과 협업이 필요한 직장인들을 주요 대상으로 하며, 이들의 작업 효율성을 극대화하는 것을 궁극적인 목적으로 한다. 이를 위해 별도의 설치가 필요 없는 웹 기반 환경을 통해 접근성을 높이고, 누구나 쉽게 사용할 수 있는 직관적인 UI를 제공하는 데 중점을 두었다.

이러한 목적을 달성하기 위해 '협업의 민족'은 다음과 같은 핵심 기능들을 제공한다.
첫째, '대시보드' 및 '달력' 기능은 팀원 개인의 일정을 공유하고 팀 전체의 스케줄을 통합 관리한다.
둘째, 별도의 '시간 조율' 기능으로 프로젝트 참여자들 간의 효율적인 시간 관리를 지원한다.
셋째, '게시판' 기능은 아이디어 공유, 자료 교환, 실시간 피드백 등 원활한 의사소통을 위한 창구 역할을 수행한다.

'협업의 민족'은 이러한 통합적인 기능 제공을 통해 사용자에게 효율적인 일정 조율, 원활한 의사소통이라는 보다 편한 협업 환경을 제공하고자 한다. 본보고서는 현재 페이지의 '협업의 민족'의 개발 배경 및 목적을 시작으로, 각 단계별 Diagram 및 관련 설명과 UI Prototype에 대해 순차적으로 기술할 것이다.
