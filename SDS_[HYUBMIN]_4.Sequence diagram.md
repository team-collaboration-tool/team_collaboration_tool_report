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

## 4.4. Community sequence diagram

### 게시글 작성

![[그림 4-21] 게시글 작성 Sequence diagram](./image/4-21.png)
[그림 4-21] 게시글 작성 Sequence diagram

[그림 4-21]은 사용자(프로젝트 참여자)가 게시판에서 새 게시글을 작성하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #17>의 경우이다.<br>
사용자가 게시판에서 게시글 작성 버튼을 클릭해 제목과 내용을 입력한 후 등록 버튼을 누를 때 기능이 시작된다. 시스템은 게시판 정보를 기반으로 현재 로그인 사용자를 확인하고, 입력된 내용을 이용해 새 게시글을 생성한다. 생성된 게시글은 DB에 저장되고, 화면은 자동으로 작성한 게시글 페이지로 전환된다.

### 게시글 수정

![[그림 4-22] 게시글 수정 Sequence diagram](./image/4-22.png)
[그림 4-22] 게시글 수정 Sequence diagram

[그림 4-22]는 사용자(프로젝트 참여자)가 등록한 게시글을 수정하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #18>의 경우이다.<br>
사용자가 작성한 게시글 화면에서 수정 버튼을 눌러 내용을 변경하고 저장할 때 기능이 시작된다. 시스템은 로그인된 사용자와 게시글 작성자를 비교해 권한을 검증한 후, 수정된 제목과 내용을 DB에 반영한다. 수정이 완료되면 시스템은 갱신된 게시글 정보를 반환하고, 화면은 수정된 내용으로 갱신된다.

### 게시글 삭제

![[그림 4-23] 게시글 삭제 Sequence diagram](./image/4-23.png)
[그림 4-23] 게시글 삭제 Sequence diagram

[그림 4-23]은 사용자(프로젝트 참여자)가 등록한 게시글을 삭제하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #18>의 경우이다.<br>
사용자가 작성한 게시글 화면에서 삭제 버튼을 클릭해 삭제를 확인할 때 기능이 시작된다. 시스템은 로그인된 사용자와 게시글 작성자를 비교하여 권한을 확인하고, 권한이 유효하면 해당 게시글을 DB에서 삭제한다. 삭제가 완료되면 시스템은 성공 응답을 반환하고, 게시판 목록 화면이 새로 로드된다.

### 게시글 접근

![[그림 4-24] 게시글 접근 Sequence diagram](./image/4-24.png)
[그림 4-24] 게시글 접근 Sequence diagram

[그림 4-24]는 사용자(프로젝트 참여자)가 특정 게시글의 상세 내용을 조회하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #19>의 경우이다.<br>
사용자가 게시판 목록에서 특정 게시글을 클릭하면 기능이 시작된다. 시스템은 선택된 게시글의 ID를 기반으로 작성자와 프로젝트 정보를 함께 조회하고, 게시글의 상세 내용을 사용자에게 표시한다. 게시글 접근 과정은 수정 및 삭제 기능 실행의 기본 전제로 작동한다.

### 개시글 추가

![[그림 4-25] 게시글 추가 Sequence diagram](./image/4-25.png)
[그림 4-25] 게시글 추가 Sequence diagram

[그림 4-25]는 사용자(프로젝트 참여자)가 특정 게시글을 검색하는 use case를 나타내는 sequence diagram이다. use case description에서 <Use Case #25>의 경우이다.<br>
사용자가 게시판 화면의 검색창에 키워드를 입력하고 검색 버튼을 누를 때 기능이 시작된다. 시스템은 제목이나 작성자 조건을 추가해 게시글을 필터링할 수 있다. 검색 결과만을 포함한 목록을 반환하고, 브라우저는 해당 결과를 목록 형태로 표시한다.
