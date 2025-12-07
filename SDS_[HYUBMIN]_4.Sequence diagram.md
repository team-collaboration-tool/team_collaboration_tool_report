# 4. Sequence diagram

이 장은 시스템 내 각 기능의 동작 흐름을 표현한 Sequence diagram(이하 SD)과 그 설명을 제공한다. SD는 사용자의 요청이 시스템 내부를 통해 처리되는 과정을 시간의 흐름에 따라 시각적으로 나타낸 것으로, 각 기능별로 Controller, Service, Repository 간의 메시지 교환 관계를 중심으로 구성된다.<br>
각 SD는 특정 use case와 대응되며, use case의 scenario를 기반으로 작성되었다. 따라서 사용자와 시스템 간 상호작용 과정에서 발생하는 main success 및 extension scenario를 함께 표현한다. 이를 통해 시스템의 처리 절차를 명확히 이해하고, 각 기능이 수행되는 구체적인 단계를 분석할 수 있다.

## 4.1. User sequence diagram

### 회원가입

![[그림 4-1] 회원가입 Sequence diagram](./image/회원가입.png)
[그림 4-1] 회원가입 Sequence diagram

[그림 4-1]은 사용자가 '협업의 민족'에 회원가입을 하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #1>의 경우이다.<br>
사용자가 회원가입 화면에서 필수정보를 입력하고 회원가입 버튼을 클릭할 때 기능이 시작된다. 사용자가 회원가입 요청을 보내면, 시스템은 순차적으로 이메일 중복 여부를 검증하고, 비밀번호를 암호화하여 DB에 새로운 사용자를 저장한다.

### 로그인

![[그림 4-2] 로그인 Sequence diagram](./image/로그인.png)
[그림 4-2] 로그인 Sequence diagram

[그림 4-2]는 사용자가 '협업의 민족'에 로그인을 하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #2>의 경우이다.<br>
사용자가 로그인 화면에서 이메일과 비밀번호를 입력하고 로그인 버튼을 클릭할 때 기능이 시작된다. 사용자가 로그인 요청을 보내면, 시스템은 순차적으로 사용자의 신원을 검증하고, 정상적으로 인증된 사용자에게만 JWT 토큰을 발급한다.

### 개인정보 확인

![[그림 4-3] 개인정보 확인 Sequence diagram](./image/개인정보_확인.png)

[그림 4-3] 개인정보 확인 Sequence diagram

[그림 4-3]은 사용자가 자신의 개인정보를 확인하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #23>의 경우이다.<br>
사용자가 로그인된 상태에서 설정 페이지로 넘어갈 때 기능이 시작된다. 사용자가 개인정보 조회 요청을 보내면, 시스템은 JWT 토큰을 통해 사용자를 인증하고, 토큰에 포함된 이메일을 기반으로 해당 사용자의 정보를 조회한다. 조회 결과는 이름(name), 이메일(email), 전화번호(phone), 분야(field)로 구성된 사용자 정보로 반환되며, 클라이언트 화면에 현재 개인정보들이 표시된다.

### 개인정보 변경

![[그림 4-4] 개인정보 변경 Sequence diagram](./image/개인정보_변경.png)
[그림 4-4] 개인정보 변경 Sequence diagram

[그림 4-4]는 사용자가 비밀번호를 제외한 개인정보를 수정하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #4>의 경우이다.<br>
사용자가 설정 화면에서 이름이나 분야(전공/직무) 혹은 전화번호를 수정하고 서버에 api 요청을 보낼 때 기능이 시작된다. 사용자가 정보 수정 요청을 보내면, 시스템은 JWT 토큰을 통해 사용자를 인증하고, 해당 이메일의 사용자 정보를 찾아 수정된 값을 반영한다.

### 비밀번호 변경

![[그림 4-5] 비밀번호 변경 Sequence diagram](./image/4-5.png)
[그림 4-5] 비밀번호 변경 Sequence diagram

[그림 4-5]는 사용자가 개인정보 중 비밀번호를 변경하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #4>의 경우이다.<br>
사용자가 설정 화면에서 현재 비밀번호와 새 비밀번호, 새 비밀번호 확인을 입력하고 저장 버튼을 클릭할 때 기능이 시작된다. 사용자가 비밀번호 변경 요청을 보내면, 시스템은 JWT 토큰을 통해 사용자를 인증하고, 현재 비밀번호가 일치할 경우, 새 비밀번호와 새 비밀번호 확인까지 일치하면 비밀번호를 변경할 수 있다.

---

## 4.2. Project sequence diagram

### 프로젝트 생성

![[그림 4-6] 프로젝트 생성 Sequence diagram](./image/4-6.png)
[그림 4-6] 프로젝트 생성 Sequence diagram

[그림 4-6]은 사용자가 새 프로젝트를 생성하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #6>의 경우이다.<br>
사용자가 ProjectCreateRequest와 함께 생성 요청을 보내면, 시스템 JwtAuthenticationFilter를 통해 ownerEmail을 확인하고, ProjectServices는 이 이메일로 User 엔티티를 조회한다. 그 후, Service는 Project 엔티티를 생성하며, 엔티티는 UUID를 이용해 고유한 참여코드를 자동 생성한다. 동시에 ProjectUser 엔티티도 OWNER 역할과 APPROVED 상태로 함께 생성되어 DB에 저장된다.

### 프로젝트 접근

![[그림 4-7] 프로젝트 접근 Sequence diagram](./image/4-7.png)
[그림 4-7] 프로젝트 접근 Sequence diagram

[그림 4-7]은 사용자가 특정 프로젝트에 접근하여 정보를 확인하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #7>의 경우이다.<br>
사용자가 projectId와 함께 프로젝트 조회 요청을 보내면, 시스템은 userEmail과 projectId를 ProjectService로 전달한다. Service는 Project와 User 엔티티를 각각 조회한 후, ProjectUserRepository를 통해 두 엔티티의 관계(ProjectUser)를 확인하고 status가 'APPROVED'인지 검증한다. 권한이 확인되면, Service는 myRole과 myStatus가 포함된 ProjectResponseDTO를 생성하여 반환한다.

### 프로젝트명 수정

![[그림 4-8] 프로젝트명 수정 Sequence diagram](./image/4-8.png)
[그림 4-8] 프로젝트명 수정 Sequence diagram

[그림 4-8]은 사용자(프로젝트 관리자)가 프로젝트명을 수정하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #8>의 경우이다.<br>
사용자가 수정할 데이터(ProjectCreateRequest)와 projectId를 보내면, 시스템은 requesterEmail과 projectId를 ProjectService로 전달한다. Service는 findProjectById로 Project 엔티티를 조회한 뒤, 조회된 Project의 owner.email과 requesterEmail을 비교하여 권한 확인을 수행한다. 권한이 확인되면, project.update()를 호출하여 엔티티를 변경하고, 변경된 내용이 DB에 저장된다.

### 프로젝트 코드 확인

![[그림 4-9] 프로젝트 코드 확인 Sequence diagram](./image/4-9.png)
[그림 4-9] 프로젝트 코드 확인 Sequence diagram

[그림 4-9]는 사용자가 프로젝트 참여코드를 확인하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #9>의 경우이다.<br>
사용자가 projectId와 함께 요청을 보내면, 시스템은 requesterEmail을 ProjectService로 전달한다. Service는 findProjectById로 Project를, UserService로 User를 조회한 뒤, ProjectUserRepository를 통해 ProjectUser 엔티티를 조회하고 status가 'APPROVED'인지 검증한다. 모든 권한 확인을 통과하면, Service는 Project 엔티티의 joinCode를 포함한 ProjectCodeResponse를 생성하여 반환한다.

### 프로젝트 참여

![[그림 4-10] 프로젝트 참여  Sequence diagram](./image/4-10.png)
[그림 4-10] 프로젝트 참여 Sequence diagram

[그림 4-10]은 사용자가 특정 프로젝트에 참여를 요청하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #12>의 경우이다.<br>
사용자가 joinCode와 함께 요청을 보내면, ProjectService는 projectRepository.fi ndByJoinCode로 Project를 찾고 projectUserRepository.existsByProjectAndUser로 중복을 확인한다. 모든 검증을 통과하면, ProjectUser 엔티티가 PENDING 상태와 MEMBER 역할로 생성되어 DB에 저장된다.

### 승인 대기 멤버 확인

![[그림 4-11] 승인 대기 멤버 확인 Sequence diagram](./image/4-11.png)
[그림 4-11] 승인 대기 멤버 확인 Sequence diagram

[그림 4-11]부터 [그림 4-15]까지는 사용자(프로젝트 관리자)가 프로젝트 멤버를 관리하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #10>의 경우이다. 

[그림 4-11]은 프로젝트 멤버를 관리하기 위해 프로젝트 관리자가 승인 대기 멤버 목록을 확인하는 sequence diagram이다.<br>
사용자가 projectId와 함께 요청을 보내면, 시스템은 requesterEmail을 ProjectService로 전달한다. Service는 findProjectById로 Project 엔티티를 조회한 뒤, owner.email과 requesterEmail을 비교하여 관리자 권한 확인을 수행한다. 권한이 확인되면, projectUserRepository.findByProjectAndStatus를 호출하여 status가 'PENDING'인 ProjectUser 레코드 목록을 조회하고, 이 목록을 ProjectJoinRequestResponse 목록으로 변환하여 반환한다.

### 승인 대기 멤버 승인

![[그림 4-12] 승인 대기 멤버 승인 Sequence diagram](./image/4-12.png)
[그림 4-12] 승인 대기 멤버 승인 Sequence diagram

[그림 4-12]는 프로젝트 관리자가 참여 요청을 보낸 멤버를 승인하는 sequence diagram이다.<br>
프로젝트 관리자가 projectId와 projectUserPk를 보내면, ProjectService는 (관리자 권한을 확인한 뒤) projectUserRepository.findById로 ProjectUser 엔티티를 조회한다. Service는 projectUser.approve() 함수를 호출하여 해당 엔티티의 status를 PENDING에서 APPROVED로 변경하고, 이 변경 사항은 DB에 저장된다.

### 승인 대기 멤버 거절

![[그림 4-13] 승인 대기 멤버 거절 Sequence diagram](./image/4-13.png)
[그림 4-13] 승인 대기 멤버 거절 Sequence diagram

[그림 4-13]은 프로젝트 관리자가 참여 요청을 보낸 멤버를 거절하는 sequence diagram이다.<br>
프로젝트 관리자가 projectId와 projectUserPk를 보내면, ProjectService는 findProjectById로 Project를 조회하고, owner.email과 requesterEmail을 비교하여 권한 확인을 수행한다. 권한이 확인되면 projectUserRepository.findById로 ProjectUser를 조회하고, 여러 검증(프로젝트 일치, PENDING 상태)을 거친 뒤, projectUserRepository.delete(projectUser)를 호출하여 요청을 DB에서 삭제한다.

### 프로젝트 멤버 삭제

![[그림 4-14] 프로젝트 멤버 삭제 Sequence diagram](./image/4-14.png)
[그림 4-14] 프로젝트 멤버 삭제 Sequence diagram

[그림 4-14]는 프로젝트 관리자가 프로젝트에 참여 중인 멤버를 삭제하는 sequence diagram이다.<br>
프로젝트 관리자가 projectId와 projectUserPk를 보내면, ProjectService는 findProject ById로 Project를 조회하고, owner.email과 reque sterEmail을 비교하여 권한 확인을 수행한다. 권한이 확인되면 projectUserRepository.findById로 Projec tUser를 조회하고, 프로젝트 관리자 본인이 아닌지 검증한 뒤, projectUserRepository.del ete(memberToExpel)를 호출하여 멤버를 DB에서 삭제한다.

### 프로젝트 관리자 권한 양도

![[그림 4-15] 프로젝트 관리자 권한 양도 Sequence diagram](./image/4-15.png)
[그림 4-15] 프로젝트 관리자 권한 양도 Sequence diagram

[그림 4-15]는 프로젝트 관리자가 다른 멤버에게 관리자 권한을 양도하는 sequence diagram이다.<br>
사용자가 projectId와 newOwnerProjectUserPk(양도받을 멤버의 ProjectUser PK)를 보내면, 시스템은 이 정보와 인증된 currentOwnerEmail을 ProjectService.transferOwners hip으로 전달한다. Service는 findProjectById로 Project를 조회한 뒤, 핵심 권한 확인(인가) 로직으로서 project.getOwner().getEmail()과 currentOwnerEmail을 비교한다. 권한이 확인되면, Service는 projectUserRepository.findById로 양도받을 대상 멤버의 ProjectUser 엔티티를 조회하고, 이 멤버가 APPROVED 상태인지, 그리고 자기 자신에게 양도하는 것은 아닌지 추가적인 검증을 수행한다.<br>
모든 검증을 통과하면, Service는 projectUserRepository.findByProjectAndUser로 현재 프로젝트 관리자의 ProjectUser 엔티티를 찾아, 현재 프로젝트 관리자의 역할은 MEMBER로 강등시키고 대상 멤버의 역할은 OWNER로 승격시킨다. 동시에, Project 엔티티의 owner 필드 자체도 새로운 프로젝트 관리자의 User 객체로 변경한 뒤, 업데이트된 ProjectResponseDTO를 반환한다.

### 프로젝트 삭제

![[그림 4-16] 프로젝트 삭제 Sequence diagram](./image/4-16.png)
[그림 4-16] 프로젝트 삭제 Sequence diagram

[그림 4-16]은 사용자(프로젝트 관리자)가 프로젝트를 삭제하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #11>의 경우이다.<br>
프로젝트 관리자가 설정 페이지에서 '삭제' 버튼을 클릭할 때 기능이 시작된다. 사용자가 projectId와 함께 삭제 요청을 보내면, ProjectService는 findProjectById로 Project 엔티티를 조회한 뒤, owner.email과 requesterEmail을 비교하여 권한 확인을 수행한다. 권한이 확인되면, projectRepository.delete(project)를 호출하여 프로젝트를 DB에서 삭제한다.

### 프로젝트 나가기

![[그림 4-17] 프로젝트 나가기 Sequence diagram](./image/4-17.png)
[그림 4-17] 프로젝트 나가기 Sequence diagram

[그림 4-17]은 사용자(프로젝트 참여자)가 프로젝트를 나가는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #13>의 경우이다.<br>
사용자가 projectId와 projectName을 보내면, ProjectService는 findProjectById로 Project를, UserService로 User를 조회한다. Service는 프로젝트 관리자가 아닌지, 프로젝트명이 일치하는지, ProjectUser가 존재하는지 순차적으로 검증한다. 모든 검증을 통과하면, projectUserRepository.delete(projectUser)를 호출하여 DB에서 해당 관계를 삭제한다.

---

## 4.3. Calendar sequence diagram

### 일정 추가

![[그림 4-18] 일정 추가 Sequence diagram](./image/4-18.png)
[그림 4-18] 일정 추가 Sequence diagram

[그림 4-18]은 사용자(프로젝트 참여자)가 새로운 일정을 추가하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #14>의 경우이다.<br>
사용자가 projectId와 CalendarEventCreateRequest DTO를 보내면, 시스템은 이 정보와 userEmail을 CalendarEventService로 전달한다. Service는 checkProjectMembership을 통해 'APPROVED' 상태의 멤버인지 권한 확인을 수행한다. 권한이 확인되면, Ser vice는 findParticipantsByPks를 호출하여 participantUserPks 목록을 Set 엔티티로 변환하고, 이를 newCalendarEvent 생성자에 전달하여 CalendarEvent 엔티티를 생성한 뒤 calendarEventRepository.save를 호출하여 DB에 저장하고 생성된 CalendarEventResponse를 반환한다.

### 일정 편집

![[그림 4-19] 일정 편집 Sequence diagram](./image/4-19.png)
[그림 4-19] 일정 편집 Sequence diagram

[그림 4-19]는 사용자(프로젝트 참여자)가 등록된 일정을 수정 또는 삭제하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #15>의 경우이다.<br>
사용자가 eventId와 CalendarEventCreateRequest DTO를 보내면, 시스템은 이 정보와 userEmail을 CalendarEventService로 전달한다. Service는 findEventById로 수정할 CalendarEvent 엔티티를 조회한 뒤, 권한 확인 로직으로서 event.getCreateUser().getEmail()(일정 생성자) 또는 event.isParticipant()(참가자) 중 하나라도 userEmail과 일치하는지 비교한다. 권한이 확인되면, findParticipantsByPks로 새로운 참가자 목록을 조회하고 event.update()를 호출하여 엔티티를 변경한 뒤, 변경된 CalendarEventResponse를 반환한다.

### 일정 조회

![[그림 4-20] 일정 조회 Sequence diagram](./image/4-20.png)
[그림 4-20] 일정 조회 Sequence diagram

[그림 4-20]은 사용자(프로젝트 참여자)가 프로젝트의 일정을 조회하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #16>의 경우이다.<br>
사용자가 projectId를 보내면, 시스템은 projectId와 userEmail을 CalendarEventService로 전달한다. Service는 findProjectById와 findByEmail로 Project와 User를 조회한 뒤, checkProjectMembership을 호출하여 요청자가 'APPROVED' 상태의 멤버인지 권한 확인을 수행한다. 권한이 확인되면, calendarEventRepository.findByProject_ProjectPk를 호출하여 DB에서 해당 프로젝트의 모든 일정을 조회하고 List로 변환하여 반환한다.

---

## 4.4. Community sequence diagram

### 게시글 작성

![[그림 4-21] 게시글 작성 Sequence diagram](./image/4-21.png)
[그림 4-21] 게시글 작성 Sequence diagram

[그림 4-21]은 사용자(프로젝트 참여자)가 게시판에서 새 게시글을 작성하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #17>의 경우이다.<br>
 사용자가 게시판에서 “게시글 작성” 버튼을 누르고 제목, 내용, 투표 옵션(선택 사항)을 입력한 뒤 등록 버튼을 클릭하면 기능이 시작된다. 시스템은 먼저 로그인한 사용자 정보를 조회하여 게시글 작성 권한을 확인하고, 사용자가 선택한 프로젝트 정보를 불러온다. 이후 프로젝트 내 게시글 번호(postNumber)를 계산하여 새 게시글 객체를 생성한다.<br>
 만약 사용자가 투표 생성 옵션을 활성화했다면, 시스템은 투표 제목, 선택지 목록, 중복 선택 여부, 익명 여부, 마감 시간 등을 검증한 후 Vote 및 VoteOption 엔티티를 생성하여 게시글과 연결한다.<br>
 모든 정보가 준비되면 시스템은 게시글(Post) 및 필요한 경우 Vote 관련 엔티티를 데이터베이스(PostRepository, VoteRepository)에 저장한다. 저장이 완료되면 시스템은 새로 생성된 게시글 정보를 포함한 PostResponse를 사용자에게 반환한다.

### 게시글 수정

![[그림 4-22] 게시글 수정 Sequence diagram](./image/4-22.png)
[그림 4-22] 게시글 수정 Sequence diagram

[그림 4-22]는 사용자(프로젝트 참여자)가 등록한 게시글을 수정하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #18>의 경우이다.<br>
사용자가 게시글 상세 화면에서 “수정” 버튼을 클릭하여 제목, 내용, 공지 여부(선택 사항)를 변경한 뒤 저장을 누르면 기능이 시작된다. 시스템은 먼저 로그인한 사용자 정보를 조회하고, 수정 대상 게시글의 작성자와 일치하는지 비교하여 수정 권한을 검증한다. 권한이 확인되면 시스템은 사용자가 입력한 제목·내용·공지 여부를 게시글 엔티티에 반영하고, 기존에 연결된 투표 정보가 있는 경우에는 변경하지 않고 그대로 유지한다. 수정이 완료되면 시스템은 갱신된 게시글 정보를 PostResponse 형태로 반환한다.

### 게시글 삭제

![[그림 4-23] 게시글 삭제 Sequence diagram](./image/4-23.png)
[그림 4-23] 게시글 삭제 Sequence diagram

[그림 4-23]은 사용자(프로젝트 참여자)가 등록한 게시글을 삭제하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #18>의 경우이다.<br>
 사용자가 게시글 상세 화면에서 “삭제” 버튼을 클릭해 삭제를 확정하면 기능이 시작된다. 시스템은 먼저 로그인한 사용자 정보를 조회한 뒤, 삭제 대상 게시글의 작성자와 일치하는지 비교하여 삭제 권한을 검증한다. 권한이 확인되면 시스템은 해당 게시글을 데이터베이스에서 삭제한다.<br>
 게시글에 투표 기능이 포함되어 있는 경우, 게시글과 연관된 Vote 엔티티 및 그 하위의 VoteOption, VoteRecord 엔티티도 JPA cascade 규칙에 따라 함께 삭제된다. 또한 삭제된 게시글 뒤에 위치한 게시글들의 postNumber가 하나씩 앞으로 당겨지도록 재정렬 작업이 수행된다. 모든 삭제 작업이 완료되면 시스템은 성공 메시지를 반환하며, 화면은 갱신된 게시글 목록으로 전환된다.

### 게시글 접근

![[그림 4-24] 게시글 접근 Sequence diagram](./image/4-24.png)
[그림 4-24] 게시글 접근 Sequence diagram

[그림 4-24]는 사용자(프로젝트 참여자)가 특정 게시글의 상세 내용을 조회하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #19>의 경우이다.<br>
 사용자가 게시판 목록에서 게시글을 클릭하면 시스템은 해당 게시글 ID를 이용해 게시글, 작성자, 프로젝트 정보를 함께 조회한다. 이후 로그인된 사용자가 있을 경우, 조회된 게시글의 작성자와 비교하여 사용자가 게시글 작성자인지 여부를 판단한다.<br>
 게시글에 투표 기능이 포함된 경우, 시스템은 로그인된 사용자가 해당 투표에 참여한 이력이 있는지 검사하여 참여 여부(hasVoted)를 설정한다. 또한 투표가 실명 투표인 경우에는 해당 게시글의 투표 선택지별 참여 기록(VoteRecord)을 조회하여 게시글 상세 응답에 포함한다.<br>
 이와 같은 과정이 완료되면 시스템은 게시글 정보, 작성자 여부, 투표 참여 여부, (필요시) 실명 투표 참여자 정보를 포함한 PostResponse를 생성하여 사용자에게 반환한다. 사용자는 이를 통해 게시글 상세 화면을 확인할 수 있으며, 게시글 접근은 이후 수정·삭제 등의 기능 수행을 위한 기본 전제로 사용된다.

### 개시글 검색

![[그림 4-25] 게시글 검색 Sequence diagram](./image/4-25.png)
[그림 4-25] 게시글 검색 Sequence diagram

[그림 4-25]는 사용자(프로젝트 참여자)가 특정 게시글을 검색하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #25>의 경우이다.<br>
 사용자가 게시판 화면에서 프로젝트를 선택한 뒤 검색 유형(제목/작성자)을 지정하여 검색창에 키워드를 입력하고 검색 버튼을 누르면 기능이 시작된다. 시스템은 먼저 선택된 프로젝트 ID로 프로젝트를 조회한 후, 키워드가 비어 있는 경우에는 해당 프로젝트의 모든 게시글 목록을 조회한다. 키워드가 존재하는 경우에는 검색 유형이 “제목”이면 제목 기준으로, “작성자”이면 작성자 이름 기준으로 게시글을 필터링하며, 그 외의 검색 유형이 요청된 경우에는 예외를 발생시킨다.<br>
 조회된 게시글 목록은 페이지 정보와 함께 Page<Post> 형태로 반환되고, 시스템은 각 게시글을 PostResponse DTO로 변환하면서 게시글 번호(postNumber)를 포함한 Page<PostResponse> 응답 객체를 생성한다. 최종적으로 이 검색 결과 페이지가 클라이언트에 전달되며, 브라우저는 사용자에게 검색 조건에 맞는 게시글 목록만을 목록 형태로 표시한다.

### 공지사항 등록/해제 

![[그림 4-26] 공지사항 등록/해제 Sequence diagram](./image/공지사항.png)
[그림 4-26] 공지사항 등록/해제 Sequence diagram

 [그림 4-26]은 사용자(프로젝트 참여자)가 특정 게시글을 공지사항으로 등록 또는 해제하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #17>과 <Use Case #18>에서 수행될 수 있는 일부 흐름에 해당한다.<br>
 사용자가 게시글 작성 화면에서 공지사항 유무 체크박스를 클릭하면 기능이 시작된다. 시스템은 먼저 로그인된 사용자 정보를 조회하며, <Use case #18>의 수정의 경우 추가적으로 해당 게시글의 작성자와 일치하는지 비교하여 공지 설정 권한을 검증한다. 작성자가 아닌 사용자가 요청한 경우에는 권한 예외를 발생시켜 동작을 중단한다.<br>
 권한이 확인되면 시스템은 요청 종류에 따라 게시글 엔티티의 isNotice플래그 값을 true(공지 등록) 또는 false(공지 해제)로 변경하고, 변경된 게시글 정보를 기반으로 PostResponse를 생성하여 클라이언트에 반환한다. 이후 화면에서는 해당 게시글이 공지 목록에 포함되거나(등록), 일반 게시글로 내려가는(해제) 형태로 갱신된다. 

### 투표하기

![[그림 4-27] 투표하기 Sequence diagram](./image/투표.png)
[그림 4-27] 투표하기 Sequence diagram

 [그림 4-27]는 사용자(프로젝트 참여자)가 게시글에 생성된 투표를 하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #19>에서 수행될 수 있는 일부 흐름에 해당한다.<br>
 사용자가 특정 투표 항목을 선택하고 “투표하기” 버튼을 클릭하면 기능이 시작된다. 시스템은 먼저 선택된 옵션 ID를 기반으로 VoteOption 정보를 조회하고, 해당 옵션이 속한 투표(Vote)의 마감 시간이 지나지 않았는지 검증한다. 이어서 로그인된 사용자 정보를 조회하여 투표 참여가 가능한 사용자임을 확인한다.<br>
 단일 선택 투표의 경우, 시스템은 사용자가 이미 해당 투표에 참여했는지 검사하며, 이전 참여 기록이 존재하면 예외를 발생시켜 중복 참여를 차단한다. 중복 선택이 가능한 투표에서는 사용자가 동일한 옵션에 다시 투표하려는 경우에만 예외가 발생한다.<br>
 모든 검증을 통과하면 시스템은 VoteRecord 엔티티를 생성하여 사용자와 선택한 옵션 사이의 투표 기록을 저장하고, 해당 선택지의 득표수를 증가시킨다. 이후 투표가 성공적으로 완료되었다는 응답을 사용자에게 반환하며, 화면은 갱신된 투표 결과를 반영한다.

### 재투표하기

![[그림 4-27] 재투표하기 Sequence diagram](./image/재투표.png)
[그림 4-27] 재투표하기 Sequence diagram

 [그림 4-27]는 사용자(프로젝트 참여자)가 기존에 참여한 투표 결과를 수정하기 위해 재투표를 수행하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #19>에서 수행될 수 있는 일부 흐름에 해당한다.<br>
 사용자가 게시글의 투표 영역에서 기존 선택을 변경하기 위해 “재투표하기” 버튼을 클릭하면 기능이 시작된다. 시스템은 먼저 로그인된 사용자를 조회하고, 사용자가 선택한 특정 옵션 ID 목록을 전달받는다. 이후 투표(Vote)의 기본 정보를 확인하여 투표가 이미 마감되었는지 검증한 뒤, 사용자가 해당 투표에 대해 이전에 남긴 모든 투표 기록(VoteRecord)을 불러온다.<br>
 시스템은 재투표를 위해 다음 두 단계를 순차적으로 수행한다.<br>
- 기존 선택 제거 단계<br>
기존에 사용자가 선택한 옵션 중, 새로 전달된 선택 목록에 포함되지 않은 항목을 탐색한다. 이러한 옵션은 더 이상 선택되지 않은 것으로 판단하여 해당 VoteRecord를 삭제하고, 옵션의 득표수를 감소시킨다.<br>
- 새 선택 등록 단계<br>
기존에는 선택하지 않았으나 새 선택 목록에는 포함된 옵션을 탐색한다. 각 옵션에 대해 새로운 VoteRecord를 생성하고 저장하며, 옵션의 득표수를 증가시킨다.<br>
 이렇게 모든 기존 선택과 새 선택을 완전히 대체하는 방식으로 재투표가 처리된다. 이는 단일 선택/다중 선택 투표 모두에서 동작할 수 있는 구조이며, 중복 선택 투표에서는 여러 개의 선택지가 한 번에 갱신될 수 있다.<br>
 모든 변경이 완료되면 시스템은 투표가 성공적으로 수정되었다는 응답을 사용자에게 전달하고, 화면에는 반영된 투표 결과가 갱신된다.

---

## 4.4. TimeSchedule sequence diagram

### 시간조율표 생성

![[그림 4-29] 시간조율표 생성 Sequence diagram](./image/4-26.png)
[그림 4-29] 시간조율표 생성 Sequence diagram

[그림 4-29]는 사용자가 새로운 시간조율표를 생성하는 과정을 나타내는 sequence diagram이다. use case description에서 <Use Case #20>의 경우이다.<br>
사용자가 시간조율 생성 화면에서 제목, 날짜, 시간 범위 등을 입력하고 “생성” 버튼을 눌러서 POST /api/time-poll 요청을 전송한다. TimePollController는 이를 받아 createTimePoll()을 호출하며, 서비스는 Project와 User 정보를 조회한 뒤 새로운 TimePoll 엔티티를 생성 및 저장한다.<br>
생성 완료 후 컨트롤러는 최신 목록을 반환하기 위해 즉시 getPollList(projectId)를 다시 호출한다. 서비스는 종료되지 않은(TimePoll.endDate ≥ today) 시간조율표들을 조회한 뒤, 각 항목을 PollSummary로 변환하여 기간(duration)과 응답자 수(userCount)를 계산한다. 최종적으로 컨트롤러는 PollSummary 리스트를 사용자에게 응답으로 전달하며, 사용자는 생성 직후 갱신된 시간조율표 목록을 확인할 수 있다.

### 시간 일정 편집

![[그림 4-30] 시간 일정 편집 Sequence diagram](./image/4-27.png)
[그림 4-30] 시간 일정 편집 Sequence diagram

[그림 4-30]은 사용자가 특정 시간조율에 대해 자신의 가능 시간을 제출하거나 수정하는 과정을 나타낸 sequence diagram이다. use case description에서 <Use Case #21>의 경우이다.<br>
사용자가 가능 시간을 선택하여 POST /api/time-poll/submit요청을 보내면, TimePollController는 이를 TimePollService의 submitResponse()로 전달한다. 서비스는 해당 사용자가 이전에 제출한 응답이 있는지 확인하고, 존재할 경우 기존 응답들을 먼저 삭제한다. 이후 전달받은 availableTimes 정보를 기반으로 새로운 TimeResponse 목록을 생성하여 한 번에 저장한다.<br>
저장이 완료되면 컨트롤러는 최신 히트맵 형태의 시간표를 제공하기 위해 즉시 getPollDetailGrid(pollId, userId)를 다시 호출한다. 서비스는 모든 사용자 응답과 현재 사용자의 응답을 각각 조회하고, 동일한 그리드 생성 로직을 통해 teamGrid와 myGrid를 구성한다. 최종적으로 생성된 DetailResponse가 사용자에게 반환되며, 사용자는 자신의 선택이 바로 반영된 최신 시간표를 확인할 수 있다.

### 시간 일정 확인

![[그림 4-31] 시간 일정 확인 Sequence diagram](./image/4-28.png)

[그림 4-31] 시간 일정 확인 Sequence diagram

[그림 4-31]은 사용자가 특정 시간조율의 상세 정보를 조회하는 과정을 나타내는 sequence diagram이다. use case description에서 <Use Case #22>의 경우이다.<br>
사용자가 GET /api/time-poll/{pollId}?userId=U요청을 전송하면, TimePollController는 이를 받아 서비스의 getPollDetailGrid(pollId, U)를 호출한다. TimePollService는 먼저 TimePollRepository에서 해당 시간 조율표 정보를 조회한 뒤, TimeResponseRepository를 통해 전체 사용자 응답 목록(allResponses)과 현재 사용자의 응답 목록(myResponses)을 각각 가져온다.<br>
조회된 응답 데이터는 동일한 그리드 생성 로직을 사용하여 반복문을 통해 teamGrid와 myGrid로 채워진다. 이후 서비스는 완성된 DetailResponse 객체를 컨트롤러로 반환하고, 컨트롤러는 이를 사용자에게 전달한다. 최종적으로 사용자는 구성된 시간표(teamGrid, myGrid)를 확인하며 팀 전체의 가능 시간과 자신의 가능 시간을 비교해볼 수 있다.
