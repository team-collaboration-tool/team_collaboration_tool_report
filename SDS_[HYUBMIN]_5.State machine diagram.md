# 5. State machine diagram

 이 장은 시스템의 State machine diagram(이하 SMD)을 그리고 설명한다. [그림 5-1]은 프로젝트 관리자 기준으로 그린 diagram이다.

![[그림 5-1] State Machine Diagram](./image/StateMachineDiagram.png)
[그림 5-1] State Machine Diagram

각 state는 웹사이트가 사용자에게 어떤 화면을 보여주고 있는지에 대한 상태를 나타낸다. 따라서 state의 이름은 코드 상의 Fragment 클래스들과 1:1 대응된다. 본 SMD에는 화면상에서 크게 구분되는 5개의 복합 상태가 존재한다. 사용자는 상단 메뉴바의 버튼 클릭을 통해 시스템을 다른 복합 상태로 전환할 수 있다. 메뉴바를 통한 다른 복합 상태로의 전이는 각 state 내부의 종료점(end point)과 연결된 state에서만 가능하다.
