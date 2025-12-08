# 3. Class diagram

이 장은 '협업의 민족' 시스템을 다양한 관점에서 바라본 Class diagram(이하 CD)과 각각에 대한 설명을 기술한다.

## 3.1. DB class diagram

* 서버의 구조를 파악하기 위해 DB의 관점에서 본 CD를 작성했다.
  * ER diagram을 먼저 작성한 후, 이를 CD로 변환시켰다.
* 변수의 이름은 소문자로 시작하며 단어의 구분은 대문자로 한다.

![[그림 3-1] DB class diagram](./image/DBclassdiagram.png)
[그림 3-1] DB class diagram

---

#### User

**Class Description**: 사용자의 정보를 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | userPk | Long | private | 사용자의 고유 식별자(PK) |
| | password | String | private | 암호화된 비밀번호 |
| | name | String | private | 사용자 이름 |
| | email | String | private | 사용자 이메일(Unique) |
| | phone | String | private | 전화번호 |
| | field | String | private | 직무/전공 분야 |
| | isDeleted | Boolean | private | 탈퇴 여부(Soft delete) |
| | deletedAt | LocalDateTime | private | 탈퇴 처리 시각 |

---

#### Projects

**Class Description**: 프로젝트 정보를 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | projectPk | Long | private | 프로젝트 식별자(PK) |
| | name | String | private | 프로젝트명 |
| | joinCode | String | private | 프로젝트 참여 코드 |
| | owner | User | private | 프로젝트 관리자(User FK) |
| | projectUsers | List<ProjectUser> | private | 프로젝트-사용자 참여 관계 목록 |
| | calendarEvents | List<CalendarEvent> | private | 프로젝트 일정 목록 |
| | posts | List<Post> | private | 게시글 목록 |
| | timePolls | List<TimePoll> | private | 시간 조율표 목록 |

---

#### ProjectUser

**Class Description**: 사용자와 프로젝트 간 참여 관계 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | projectUserPk | Long | private | 식별자(PK) |
| | project | Project | private | 소속 프로젝트 (N:1) |
| | user | User | private | 참여 사용자 (N:1) |
| | status | ProjectStatus | private | 참여 상태(PENDING / APPROVED) |
| | role | ProjectRole | private | 프로젝트 내 역할(OWNER / MEMBER) |

---

#### Post

**Class Description**: 프로젝트에서 작성되는 게시글 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | postPk | Long | private | 게시글 식별자(PK) |
| | project | Project | private | 소속 프로젝트(FK) |
| | user | User | private | 작성자(FK) |
| | postNumber | Long | private | 프로젝트 내 게시글 번호 |
| | title | String | private | 게시글 제목 |
| | content | String | private | 게시글 본문 |
| | isNotice | Boolean | private | 공지 여부 |
| | hasVoting | Boolean | private | 투표 포함 여부 |
| | vote | Vote | private | 게시글에 포함된 투표(1:1 관계) |

---

#### Votes

**Class Description**: 게시글에 포함된 투표 기능 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | id | Long | private | 투표 식별자(PK) |
| | post | Post | private | 소속 게시글(FK, 1:1)수 |
| | title | String | private | 투표 제목 |
| | startTime | LocalDateTime | private | 투표 시작 시간 |
| | endTime | LocalDateTime | private | 투표 종료 시간 |
| | allowMultipleChoices | Boolean | private | 복수 선택 허용 여부 |
| | isAnonymous | Boolean | private | 익명 투표 여부 |
| | voteOptions | List<VoteOption> | private | 선택지 목록 |

---

#### VoteOption 

**Class Description**: 투표 선택지 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | id | Long | private | 선택지 식별자(PK) |
| | content | String | private | 선택지 내용 |
| | count | Integer | private | 해당 선택지 득표수 |
| | vote | Vote | private | 소속 투표(FK) |
| | voteRecords | List<VoteRecord> | private | 사용자별 투표 기록 |

---

#### VoteRecord 

**Class Description**: 사용자의 투표 기록을 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | responsePk | Long | private | 투표 기록 식별자(PK) |
| | voteOption | VoteOption | private | 사용자가 선택한 투표 항목 |
| | user | User | private | 투표한 사용자(FK) |

---

#### CalendarEvents

**Class Description**: 프로젝트 내에서 관리되는 일정 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | eventPk | Long | private | 일정 식별자(PK) |
| | project | Project | private | 소속 프로젝트(FK) |
| | createUser | User | private | 일정 생성자(FK) |
| | participants | Set<User> | private | 일정 참여자 목록(N:M) |
| | title | String | private | 일정명 |
| | color | String | private | 일정 색상 태그 |
| | startTime | LocalDateTime | private | 시작 시간 |
| | endTime | LocalDateTime | private | 종료 시간 |
| | description | String | private | 일정 상세 설명 |

---

#### TimePoll

**Class Description**: 프로젝트 내 시간조율표 기능 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | pollPk | Long | private | 조율표 식별자(PK) |
| | project | Project | private | 소속 프로젝트(FK) |
| | creator | User | private | 생성자(FK) |
| | title | String | private | 조율표 제목 |
| | startDate | LocalDate | private | 조율표 시작 날짜 |
| | endDate | LocalDate | private | 조율표 종료 날짜 |
| | startTimeOfDay | LocalTime | private | 하루 시작 시간 |
| | endTimeOfDay | LocalTime | private | 하루 종료 시간 |
| | responses | List<TimeResponse> | private | 응답 목록 |

---

#### TimeResponse

**Class Description**: 시간조율표에서 사용자가 제출한 가능 시간 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | responsePk | Long | private | 응답 식별자(PK) |
| | poll | TimePoll | private | 소속 조율표(FK) |
| | user | User | private | 응답 사용자(FK) |
| | startTimeUtc | LocalDateTime | private | 가능한 시작 시간(KST 기준 변수명만 UTC) |
| | endTimeUtc | LocalDateTime | private | 가능한 종료 시간(KST 기준 변수명만 UTC) |

---
---

## 3.2. Function class diagram

본 절에서는 시스템의 주요 기능별 클래스 구조와 역할을 시각적으로 나타낸 Function class diagram을 제시한다. 각 기능 모듈 내에서 클래스 간의 관계와 책임을 명확히 파악하기 위해 작성되었으며, Controller, Service, Domain, DTO 등 계층 간 상호작용을 중심으로 설계되었다.<br>
각 Function class diagram은 해당 모듈이 담당하는 주요 역할을 기반으로 작성되었으며, 각 클래스의 속성(Attributes)과 연산(Operations)을 함께 기술하여 클래스의 내부 구조를 한 눈에 확인할 수 있다.<br>
이를 통해 시스템 전체의 아키키텍처를 이해하고, 기능 간의 의존 관계 및 데이터 흐름을 한 눈에 확인할 수 있다.

* 변수의 이름은 소문자로 시작하며 단어의 구분은 대문자로 한다.

### 3.2.1. User class diagram

![[그림 3-2] User class diagram](./image/UserCD.png)
[그림 3-2] User class diagram

---

#### User

**Class Description:** 사용자의 정보를 저장하고 DB 테이블 users와 매핑되는 엔티티 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | userPk | Long | private | 사용자 고유 식별자 (PK) |
| | password | String | private | 암호화된 비밀번호 |
| | name | String | private | 사용자 이름 |
| | email | String | private | 사용자 이메일 (UK) |
| | phone | String | private | 사용자 전화번호 |
| | field | String | private | 사용자 분야 (선택사항) |
| | isDeleted | Boolean | private | 탈퇴 여부 (기본값: false) |
| | deletedAt | LocalDateTime | private | 탈퇴 시각 |
| | ownedProjects | List<Project> | private | 소유한 프로젝트 목록 (1:N) |
| | projectUsers | List<ProjectUser> | private | 참여 중인 프로젝트 관계 (1:N) |
| | posts | List<Post> | private | 작성한 게시글 목록 (1:N) |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | User | password : String,<br>name : String,<br>email : String,<br>phone : String,<br>field : String | 생성자 | 새 사용자 객체를 생성하는 생성자 (isDeleted는 false로 초기화) |

---

#### UserController

**Class Description:** 사용자 관련 기능(회원가입, 로그인, 정보 수정, 비밀번호 변경, 개인정보 확인, 회원탈퇴)을 담당하는 컨트롤러 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | userService | UserService | private | 사용자 관련 비즈니스 로직을 수행하는 서비스 class |
| | passwordEncoder | BCryptPasswordEncoder | private | 비밀번호 암호화를 위한 인코더 객체 |
| | jwtTokenProvider | JwtTokenProvider | private | JWT 토큰 발급 및 검증 유틸 클래스 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | signup | request : UserSignupRequestDTO | ResponseEntity<User> | 회원가입 요청을 처리하는 메서드 |
| | checkEmailDuplicate | email : String | ResponseEntity<Map> | 이메일 중복 확인 메서드 |
| | login | request : LoginRequestDTO | ResponseEntity<Map> | 사용자 로그인 처리 및 JWT 토큰 발급 |
| | getMyInfo | userEmail : String | ResponseEntity<UserInfoResponse> | 로그인된 사용자의 개인정보를 반환 (JWT 인증) |
| | updateUser | userEmail : String,<br>request : UserUpdateRequest | ResponseEntity<Void> | 사용자 정보를 수정하는 메서드 |
| | verifyPassword | userEmail : String,<br>request : PasswordVerifyRequest | ResponseEntity<Map> | 비밀번호 확인 메서드 |
| | updatePassword | userEmail : String,<br>request : UserPasswordUpdateRequest | ResponseEntity<Void> | 비밀번호를 변경하는 메서드 |
| | deleteUser | userEmail : String,<br>request : PasswordVerifyRequest | ResponseEntity<Void> | 회원 탈퇴 처리 (Soft Delete) |

---

#### UserRepository

**Class Description:** JPA를 사용하여 DB의 사용자 테이블(users)과 상호작용하는 Repository 인터페이스

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | findByEmail | email : String | Optional<User> | 이메일로 사용자 조회 |
| | existsByEmail | email : String | boolean | 이메일 중복 여부 확인 (탈퇴 계정 포함) |

---

#### UserService

**Class Description:** 회원가입, 사용자 정보 조회 및 수정, 비밀번호 변경, 회원 탈퇴 등 사용자 관련 로직을 담당하는 서비스 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | userRepository | UserRepository | private | 사용자 정보를 관리하는 JPA Repository |
| | passwordEncoder | BCryptPasswordEncoder | private | 비밀번호 암호화용 인코더 객체 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | isEmailExists | email : String | boolean | 이메일 중복 여부 확인 |
| | registerUser | request : UserSignupRequestDTO | User | 회원가입 처리 및 사용자 저장 (비밀번호 암호화) |
| | findByEmail | email : String | User | 이메일로 사용자 조회 (존재하지 않으면 예외 발생) |
| | updateUser | email : String,<br>request : UserUpdateRequest | void | 사용자 정보 수정 (이름, 전화번호, 분야) |
| | verifyPassword | email : String,<br>password : String | boolean | 비밀번호 일치 여부 확인 (본인 인증용) |
| | updatePassword | email : String,<br>request : UserPasswordUpdateRequest | void | 비밀번호 변경 처리 (현재 비밀번호 검증 후 변경) |
| | deleteUser | email : String | void | 회원 탈퇴 처리 (Soft Delete: isDeleted=true, deletedAt 설정) |

---

#### DTO Classes

- UserSignupRequestDTO

**Attributes**
| Name | Type | Constraints |
| --- | --- | --- |
| password | String | @NotBlank,<br> @Pattern: 8~20자 영문/숫자/특수문자 |
| name | String | @NotBlank |
| email | String | @NotBlank,<br>  @Pattern: 이메일 형식 |
| phone | String | @NotBlank,<br>  @Pattern: 11자리 숫자 |
| field | String | 선택사항,<br>  @NotBlank 제거됨 |

- LoginRequestDTO

**Attributes**
| Name | Type | Constraints |
| --- | --- | --- |
| email | String | |
| password | String | |

- UserInfoResponse

**Attributes**
| Name | Type | Constraints |
| --- | --- | --- |
| userPk | Long | |
| name | String | |
| email | String | |
| phone | String | |
| field | String | |

- UserUpdateRequest

**Attributes**
| Name | Type | Constraints |
| --- | --- | --- |
| name | String | 선택사항 |
| phone | String | 선택사항,<br> @Pattern: 11자리 숫자 |
| field | String | 선택사항 |

- UserPasswordUpdateRequest

**Attributes**
| Name | Type | Constraints |
| --- | --- | --- |
| currentPassword | String | @NotBlank |
| newPassword | String | @NotBlank,<br> @Pattern: 8~20자 |

- PasswordVerifyRequest

**Attributes**
| Name | Type | Constraints |
| --- | --- | --- |
| password | String | @NotBlank |

---

#### JwtTokenProvider

**Class Description:** JWT 토큰 생성, 검증, 정보 추출을 담당하는 유틸리티 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | secretKey | String | private | JWT 서명용 비밀키 (환경변수) |
| | expirationTime | long | private | 토큰 만료 시간 (86400000ms = 24시간) |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | generateToken | email : String | String | 이메일 기반 JWT 토큰 생성 |
| | getEmailFromToken | token : String | String | 토큰에서 이메일 추출 |
| | validateToken | token : String | boolean | 토큰 유효성 검증 (만료, 서명 등) |

---

#### JwtAuthenticationFilter

**Class Description:** HTTP 요청의 JWT 토큰을 검증하고 Spring Security에 인증 정보를 설정하는 필터 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | jwtTokenProvider | JwtTokenProvider | private | JWT 토큰 검증 유틸 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | doFilterInternal | request : HttpServletRequest,<br>response : HttpServletResponse,<br>filterChain : FilterChain | void | Authorization 헤더에서 JWT 추출 및 검증 후 SecurityContext에 인증 정보 설정 |

---
---

### 3.2.2. Project class diagram

![[그림 3-3] Project class diagram](./image/ProjectCD.png)
[그림 3-3] Project class diagram

---

#### ProjectController

**Class Description:** 사용자 관련 기능(회원가입, 로그인, 정보 수정, 비밀번호 변경, 개인정보 조회)을 담당하는 컨트롤러 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | projectService | ProjectService | private | Project 도메인의 비즈니스 로직을 처리하는 서비스(생성자를 통한 의존성 주입) |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | createProject | @RequestBody ProjectCreateRequest request<br>@AuthenticationPrincipal String email | ResponseEntity<ProjectResponseDTO> | 새 프로젝트를 생성 |
| | getParticipatingProjects | @AuthenticationPrincipal String email | ResponseEntity<List<ProjectResponseDTO>> | 현재 사용자가 참여 중인 프로젝트 목록 조회 |
| | getProject | @PathVariable Long projectid | ResponseEntity<ProjectResponseDTO> | 단일 프로젝트의 상세 정보 조회 |
| | updateProject | @PathVariable Long projectId<br>@RequestBody ProjectCreateRequest request<br>@AuthenticationPrincipal String email | ResponseEntity<ProjectResponseDTO> | 프로젝트명 수정 |
| | leaveProject | @PathVariable Long projectId<br>@RequestBody ProjectCreateRequest request<br>@AuthenticationPrincipal String email | ResponseEntity<void> | 프로젝트 나가기(프로젝트명 확인 필요, 관리자일 경우 불가능) |
| | deleteProject | @PathVariable Long projectId<br>@RequestBody ProjectCreateRequest request<br>@AuthenticationPrincipal String email | ResponseEntity<void> | 프로젝트 삭제 |
| | getProjectJoinCode | @AuthenticationPrincipal String email<br>@PathVariable Long projectId | ResponseEntity<ProjectCodeResponse> | 프로젝트의 고유 참여코드 조회 |
| | requestJoin | @AuthenticationPrincipal String email<br>@RequestParam String joinCode | ResponseEntity<ProjectJoinRequestResponse> | 참여코드를 사용해 프로젝트 참여를 요청(상태:PENDDING) |
| | getPendingRequestList | @AuthenticationPrincipal String email<br>@PathVariable Long projectId | ResponseEntity<List<ProjectJoinRequestRsponse>> | 프로젝트 승인 대기 멤버 목록을 조회(관리자 권한) |
| | approveProject | @PathVariable Long projectId<br>@PathVariable Long projectUserPk<br>@AuthenticationPrincipal String email | ResponseEntity<ProjectResponseDTO> | 프로젝트 참여 요청을 승인(관리자 권한) |
| | rejectProjectJoin | @PathVariable Long projectId<br>@PathVariable Long projectUserPk<br>@AuthenticationPrincipal String email | ResponseEntity<Void> | 프로젝트 참여 요청을 거절(관리자 권한) |
| | expelMember | @PathVariable Long projectId<br>@PathVariable Long projectUserPk<br>@AuthenticationPrincipal String email | ResponseEntity<Void> | 프로젝트 멤버를 삭제(관리자 권한, 자신x) |
| | transferOwnership | @PathVariable Long projectId<br>@PathVariable Long projectUserPk<br>@AuthenticationPrincipal String email | ResponseEntity<Void> | 프로젝트 관리자 권한을 다른 멤버에게 양도(관리자 권한) |

---

#### ProjectService

**Class Description:** 프로젝트(생성, 수정, 삭제), 멤버 관리(참여, 승인, 삭제, 관리자 권한 양도), 권한 검증(관리자, 멤버) 등 프로젝트와 관련된 서비스 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | projectRepository | ProjectRepository | private | Project 엔티티에 대한 DB 작업 처리 |
| | projectUserRepository | ProjectUserRepository | private | ProjectUser(참여 관계) 엔티티에 대한 DB 작업 처리 |
| | userService | UserService | private | User 엔티티 정보를 조회하기 위해 사용 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | findProjectById | Long projectId | Project | ID로 Project 엔티티를 조회 |
| | createProject | ProjectCreateRequest request | ProjectResponseDTO | 새 프로젝트를 생성하고, 요청자를 OWNER 및 APPROVED 상태로 등록 |
| | getParticipatingProject | String userEmail | List<ProjectResponseDTO> | 사용자가 참여 중인(APPROVED 상태) 모든 프로젝트 목록 조회 |
| | getProject | Long projectld<br>String userEmail | ProjectResponseDTO | 단일 프로젝트 정보 조회(APPROVED 상태 확인) |
| | updateProject | Long ProjectId<br>ProjectCreateRequest request<br>String requesterEmail | ProjectRsponseDTO | 프로젝트 수정(OWNER 권한) |
| | leaveProject | Long projectId<br>String userEmail<br>String projectName toConfirm | void | 프로젝트 나가기(요청자가 Member인지, 프로젝트명이 일치하는지 확인) |
| | deleteProject | Long projectId | void | 프로젝트 삭제(요청자가 OWNER인지, 프로젝트명이 일치하는지 확인) |
| | getProjectJoinCode | Long projectId<br>String requesterEmail | ProjectCodeResponse | 프로젝트 참여코드 조회 |
| | requestJoin | String joinCode<br>String requesterEmail | ProjectJoinRequestResponse | 참여코드로 프로젝트 참여 요청(PENDING 상태로 ProjectUser 생성) |
| | rejectProjectJoin | Long projectUserPk<br>String ownerEmail<br>Long projectid | void | projectUserPk에 해당하는 참여 요청을 거절(관리자 권한 확인) |
| | getPendingRequestList | Long projectId | List<ProjectJoinRequestResponse> | PENDING 상태인 승인 대기 멤버 목록 조회 |
| | expelProjectMember | Long projecUserPk<br>String ownerEmail<br>Long projectId | void | projectUserPk에 해당하는 멤버를 삭제 |
| | transferOwnership | Long targetUserPk<br>String currentOwnerEmail | void | 프로젝트 OWNER 권한을 다른 멤버에게 양도 |

---
---

### 3.2.3. Calendar class diagram

![[그림 3-4] Calendar class diagram](./image/CalendarCD.png)
[그림 3-4] Calendar class diagram

---

#### CalendarController

**Class Description:** 클라이언트가 특정 프로젝트의 일정에 접근하려고 할 때 처리되는 컨트롤러 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | calendarEventService | CalendarEventService | private | CalendarEvent 도메인의 비즈니스 로직을 처리 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | createEvent | @PathVariable Long projectId | ResponseEntity<CalendarEventResponse> | 새 일정을 등록 |
| | getEventsByProject | @PathVariable Long porjectId<br>@AuthenticationPrincipal String email | ResponseEntityList<CalendarEventResponse> | 해당 프로젝트의 전체 일정 조회 |
| | updateEvent | @PathVariable Long porjectId<br>@PathVariable Long eventId<br>@RequestVodyCalendarEventCreateRequest request<br>@AuthenticationPrincipal String email | ResponseEntity<CalendarEventResponse> | 등록된 일정 수정 |
| | deleteEvent | @PathVariable Long porjectId<br>@PathVariable Long eventId<br>@AuthenticationPrincipal String email | ResponseEntity<Void> | 일정 삭제 |

---

#### CalendarService

**Class Description:** 클라이언트가 특정 프로젝트의 일정에 접근하려고 할 때 처리되는 컨트롤러 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | calendarEventService | CalendarEventService | private | CalendarEvent 도메인의 비즈니스 로직을 처리 |
| | projectService | ProjectService | private | 프로젝트 정보를 조회하거나 권한을 확인 |
| | userService | UserService | private | 이메일 기반으로 사용자 정보를 조회 |
| | projectUserRepository | ProjectUserRepository | private | 사용자의 프로젝트 참여 상태를 확인 |
| | userRepository | UserRepository | private | 일정에 참여하는 참가자들의 User 엔티티를 조회 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | createEvent | Long projectId<br>CalendarEventCreateRequest request<br>String userEmail | CalendarEventResponse | 새 일정을 생성하고 저장 |
| | getEventsByProject | @PathVariable Long porjectId<br>@AuthenticationPrincipal String email | List<CalendarEventResponse> | 프로젝트 멤버 확인 후, 해당 프로젝트의 모든 일정 조회 |
| | getEvent | @PathVariable Long porjectId<br>@PathVariable Long eventId<br>@RequestBodyCalendarEventCreateRequest request<br>@AuthenticationPrincipal String email | CalendarEventResponse | 단일 일정 조회 |
| | updateEvent | @PathVariable Long porjectId<br>@PathVariable Long eventId<br>@AuthenticationPrincipal String email | CalendarEventResponse | 일정 수정 |
| | deleteEvent | Long eventid<br>String userEmail | void | 일정 삭제 |
| | findEventById | Long eventId | CalendarEvent | eventId로 CalendarEvent 엔티티를 조회 |
| | checkProjectMembership | Project project<br>User user | void | 사용자가 해당 프로젝트의 승인된 멤버인지 확인 |
| | findParticipantsByPks | List<Long> participantUserPks | Set<User> | 사용자 PK 목록으로 User 엔티티 Set을 조회 |

---
---

### 3.2.4. Community class diagram

![[그림 3-5] Community class diagram](./image/CommunityCD.png)
[그림 3-5] Community class diagram

---

#### PostController

**Class Description:** 게시글 관련 기능(게시글 생성, 접근, 수정, 삭제, 공지 등록/해제, 게시글 목록 확인)을 담당하는 컨트롤러 클래스

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | postService | PostService | private | 게시글 비즈니스 로직 처리 서비스 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | createPost | String userEmail<br>String postJson | ResponseEntity<PostResponse> | 게시글 생성 요청 처리 |
| | getPostById | Long PostId<br>String userEmail | ResponseEntity<PostResponse> | 특정 게시글 접근 |
| | updatepost | Long postId<br>PostUpdateRequest | ResponseEntity<PostResponse> | 게시글 수정 |
| | deletepost | Long postId<br>String userEmail | ResponseEntity<Map> | 게시글 삭제 |
| | getAllPosts | Long projectPk<br>String keyword<br>PostSearchType searchType | ResponseEntity<Page<PostResponse>> | 게시글 목록 확인(검색/페이징) |
| | markAsNotice | Long postId<br>String userEmail | ResponseEntity<PostResponse> | 게시글을 공지사항으로 등록 |
| | UnmarkAsNotice | Long postId<br>String userEmail | ResponseEntity<PostResponse> | 공지사항 해제 |
| | getNoticePosts | Long projectPk | ResponseEntity<List<PostResponse>> | 특정 프로젝트의 공지 게시글 목록 확인 |

---

#### VoteController

**Class Description:** 투표 생성, 투표하기, 재투표하기 기능을 제공하는 컨트롤러 클래스

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | voteService | VoteService | private | 투표 관련 비즈니스 로직 처리 서비스 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | createVote | VoteCreateRequest request<br>String userEmail | ResponseEntity<VoteResponse> | 새로운 투표 생성 |
| | castVote | Long optionId<br>String userEmail | ResponseEntity<String> | 투표 시 처음 투표 |
| | reCastVote | Long optionId<br>String userEmail | ResponseEntity<String> | 단일 선택 투표 재투표 |
| | reCastVoteAll | Long voteId<br>List<Long> selectedOptionId<br>String userEmail | ResponseEntity<String> | 중복 선택 투표 재투표 |

---

#### NoticeController

**Class Description:** 프로젝트 내 공지사항 등록 및 목록 확인을 담당하는 컨트롤러 클래스

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | noticeService | NoticeService | private | 공지사항 비즈니스 로직 처리 서비스 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | getNotices | Long projectId<br>String email | ResponseEntity<List<NoticeResponse>> | 프로젝트 공지사항 목록 확인 |
| | createNotice | Long projectId<br>String title<br>String content<br>String email | ResponseEntity<NoticeResponse> | 새로운 공지사항 등록 |

---

#### PostService

**Class Description:** 게시글 생성/수정/삭제/접근 및 투표 연결 로직을 처리하는 서비스 클래스

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | postRepository | PostRepository | private | 게시글 데이터베이스 처리 |
| | userRepository | UserRepository | private | 사용자 조회 |
| | projectRepository | ProjectRepository | private | 프로젝트 조회 |
| | voteRepository | VoteRepository | private | 투표 엔티티 저장 |
| | voteRecordRepository | VoteRecordRepository | private | 투표 기록 조회 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | createPost | PostCreateRequest request<br>String userEmail | PostResponse | 게시글 생성 + 투표 초기화 |
| | getPostById | Long postId<br>String userEmail | PostResponse | 게시글 접근 및 투표 참여 여부 확인 |
| | getPostsByProject | Long projectPk | Page<PostResponse> | 프로젝트별 게시글 목록 확인 |
| | updatePost | Long postId<br>PostUpdateRequest request | PostResponse | 게시글 수정 + 투표 수정 |
| | deletePost | Long postId<br>String userEmail | void | 게시글 삭제 & postNumber 정렬 |
| | markAsNotice | Long postId<br>String userEmail | PostResponse | 게시글 공지 등록 |
| | unmarkAsNotice | Long postId<br>String userEmail | PostResponse | 게시글 공지 해제 |

---

#### VoteService

**Class Description:** 투표 올리기, 투표하기, 재투표하기 등 투표 비즈니스 로직을 수행하는 클래스

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | voteRepository | VoteRepository | private | Vote 엔티티 저장 |
| | voteOptionRepository | VoteOptionRepository | private | VoteOption 조회 |
| | voteRecordRepository | VoteRecordRepository | private | 투표 기록 관리 |
| | userRepository | userRepository | private | 사용자 조회 |
| | postRepository | PostRepository | private | 게시글 조회 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | createVote | VoteCreateRequest request<br>String Email | VoteResponse | 게시글에 투표 생성 |
| | castVote | Long optionId<br>String userEmail | void | 투표 첫 선택 |
| | reCastVote | Long optionId<br>String userEmail | void | 단일 선택 투표 재투표 |
| | reCastVoteAll | Long voteId<br>List<Long> selectedOptionIds<br>String email | void | 중복 선택 전체 재투표 |

---

#### NoticeService

**Class Description:** 프로젝트 공지사항 등록/조회 기능 수행

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | noticeRepository | NoticeRepository | private | 공지사항 조회/저장 |
| | projectService | ProjectService | private | 프로젝트 조회 |
| | userService | UserService | private | 사용자 조회 |
| | projectUserRepository | ProjectUserRepository | private | 프로젝트 참여 상태 확인 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | getNoticeByProject | Long projectId<br>String Email | List <NoticeResponse> | 프로젝트 공지사항 조회 |
| | createNotice | Long projectId<br>String title<br>String content<br>String email | NoticeResponse | 공지사항 등록 |

---

#### Post (Domain)

**Class Description:** 게시글의 핵심 정보를 저장하는 엔티티

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | postPk | Long | private | 게시글 식별자 |
| | postNumber | Long | private | 프로젝트 내 게시글 순번 |
| | project | Project | private | 게시글이 속한 프로젝트 |
| | user | User | private | 게시글 작성자 |
| | title | String | private | 게시글 제목 |
| | content | String | private | 게시글 내용 |
| | isNotice | Boolean | private | 게시글 공지 등록 여부 |
| | hasVoting | Boolean | private | 게시글 투표 포함 여부 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | update | String title<br>String content<br>Boolean isNotice | - | 게시글 정보 업데이트 |
| | setIsNotice | Boolean isNotice | - | 공지여부 설정 |
| | setVote | Vote vote | - | 게시글에 투표 연결 |

---
---

### 3.2.5. TimeSchedule class diagram

![[그림 3-6] TimeSchedule class diagram](./image/시간조율CD.png)
[그림 3-6] TimeSchedule class diagram

---

#### TimePoll

**Class Description:** 시간조율표의 기본 정보를 저장하고 DB 테이블의 time_poll과 매핑되는 엔티티 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | pollPk | Long | private | 시간조율표 고유 식별자 |
| | project | Project | private | 시간조율표가 속한 프로젝트 |
| | creator | User | private | 시간조율표 생성자 |
| | title | String | private | 시간조율표 제목 |
| | startDate | LocalDate | private | 시간조율표 시작 날짜 |
| | endDate | LocalDate | private | 시간조율표 종료 날짜 |
| | startTimeofDay | LocalDate | private | 하루 중 시작 시간 |
| | endTimeofDay | LocalDate | private | 하루 중 종료 시간 |
| | responses | List<TimeResponse> | private | 사용자들이 제출한 응답 목록 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | TimePoll | Project project, User creator, String title, LocalDate startDate, LocalDate endDate, LocalTime startTimeOfDay, LocalTime endTimeOfDay | 생성자 | 새 시간조율 엔티티를 생성하는 생성자 |

---

#### CreateRequest

**Class Description:** 시간조율을 새로 생성할 때 클라이언트로부터 서버로 전달하는 요청 데이터를 담는 DTO class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | projectId | Long | private | 시간조율이 속한 프로젝트 식별자(PK) |
| | creatorId | Long | private | 시간조율을 생성한 사용자 식별자(PK) |
| | title | String | private | 시간조율 제목 |
| | startDate | LocalDate | private | 시간조율 시작 날짜 |
| | duration | Integer | private | 기준 날짜(startDate)로부터 며칠 동안 조율할지(일 수) |
| | startTimeOfDay | LocalTime | private | 하루 중 조율 시작 시간 |
| | endTimeOfDay | LocalTime | private | 하루 중 조율 종료 시간 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | CreateRequest | projectId: Long, creatorId: Long, title: String, startDate: LocalDate, duration: Integer, startTimeOfDay: LocalTime, endTimeOfDay: LocalTime | 생성자 | 새 시간조율 생성 요청 객체를 생성하는 생성자 |

---

#### PollSummary

**Class Description:** 시간조율 목록 화면에 각 시간조율을 간단히 보여주기 위한 요약 정보 DTO Class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | pollId | Long | private | 시간조율 식별자(PK) |
| | title | String | private | 시간조율 제목 |
| | startDate | LocalDate | private | 시간조율 시작 날짜 |
| | endDate | LocalDate | private | 시간조율 종료 날짜 |
| | duration | Integer | private | 전체 조율 일 수 |
| | startTime | LocalTime | private | 하루 중 조율 시작 시간 |
| | endTime | LocalTime | private | 하루 중 조율 종료 시간 |
| | userCount | Integer | private | 해당 시간조율에 참여한 사용자 수 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | PollSummary | Long pollId, String title, LocalDate startDate, LocalDate endDate, Integer duration, LocalTime startTime, LocalTime endTime, Integer userCount | 생성자 | 목록 조회 응답용 요약 객체 생성자 |

---

#### TimeRange

**Class Description:** 사용자가 가능한 시간 구간을 제출할 때, 한 구간의 시작/끝 시간을 표현하는 내부 DTO Class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | start | String | private | 가능한 시간 구간 시작 시각(ISO 문자열) |
| | end | String | private | 가능한 시간 구간 종료 시각(ISO 문자열) |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | TimeRange | String start, String end | 생성자 | 한 개의 가능 시간 구간을 생성하는 생성자 |

---

#### SubmitRequest

**Class Description:** 특정 시간조율에 대해 사용자가 자신의 가능 시간을 제출할 때 사용하는 DTO Class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | pollId | Long | private | 시간조율 식별자(PK) |
| | userId | Long | private | 응답을 제출하는 사용자 식별자(PK) |
| | availableTimes | List<TimeRange> | private | 사용자가 선택한 여러 가능 시간 구간 목록 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | SubmitRequest | Long pollId, Long userId,  List<TimeRange>availableTimes | 생성자 | 시간 응답 제출 요청 객체 생성자 |

---

#### DetailResponse

**Class Description:** 특정 시간조율표 상세 조회 시 반환되는 DTO, 팀 전체 히트맵(teamGrid)과 사용자의 개인 히트맵(myGrid)을 모두 포함

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | pollId | Long | private | 시간조율 식별자(PK) |
| | title | String | private | 시간조율 제목 |
| | teamGrid | int[][] | private | 전체 사용자 응답 히트맵 |
| | myGrid | Int[][] | private | 현재 사용자만의 응답 히트맵 |
| | dateLabels | List<String> | private | 가로축 날짜 라벨(예: 11/23, 11/24 …) |
| | timeLabels | List<String> | private | 세로축 시간 라벨(예: 09:00, 09:30 …) |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | DetailResponse | Long PollId, String title, int[][] teamGrid, int[][] myGrid, List<String> dateLabels, List<String> timeLabels | 생성자 | 시간표 상세 응답 객체를 생성하는 생성자 |

---

#### TimePollController

**Class Description:** 시간조율 관련 HTTP 요청을 처리하고, 클라이언트와 TimePollService 사이의 인터페이스 역할을 하는 REST 컨트롤러 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | timePollService | TimePollService | private | 비즈니스 로직을 처리하는 서비스 객체 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | getAllPolls | projectId: Long | List<PollSummary> | 특정 프로젝트의 전체 시간조율 목록 조회 |
| | createPoll | TimePollDto.CreateRequest request | ResponseEntity<?> | 새로운 시간조율을 생성하고 최신 목록 반환 |
| | getPollDetail | Long pollId, Long userId | ResponseEntity<DetailResponse> | 단일 시간조율의 상세 시간표 정보 조회 |
| | submitTime | TimePollDto.SubmitRequest request | ResponseEntity<DetailResponse> | 사용자의 가능 시간 정보를 제출 |

---

#### TimePollService

**Class Description:** 시간조율 생성, 목록 조회, 응답 저장, 상세 시간표 생성 등 시간조율 비즈니스 로직을 담당하는 Service

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | timePollRepository | TimePollRepository | private | 시간조율 엔티티에 대한 DB 접근 객체 |
| | timeResponseRepository | TimeResponseRepository | private | 시간 응답 엔티티에 대한 DB 접근 객체 |
| | projectRepository | ProjectRepository | private | 프로젝트 엔티티에 대한 DB 접근 객체 |
| | userRepository | UserRepository | private | 사용자 엔티티에 대한 DB 접근 객체 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | createTimePoll | TimePollDto.CreateRequest request | void | 새 시간조율을 생성하여 저장 |
| | getPollList | Long projectId | List<PollSummary> | 프로젝트별 시간조율 목록을 요약 형태로 조회 |
| | submitResponse | TimePollDto.SubmitRequest request | void | 사용자의 가능 시간 응답을 저장(또는 갱신) |
| | getPollDetailGrid | Long pollId, Long userId | DetailResponse | 특정 시간조율의 전체 시간표 그리드 데이터를 생성 |
| | fillGrid (private) | int[][] grid, TimeResponse response, LocalDateTime pollStartDateTime, LocalTime dayStartTime, int slotsPerDay | void | 주어진 TimeResponse의 시간 범위를 30분 단위 슬롯으로 나누어, 해당 구간에 해당하는 grid 셀의 값을 1씩 증가시키는 내부 헬퍼 메서드 |

---

#### TimeResponse

**Class Description:** 특정 시간조율에 대해 한 사용자가 제출한 가능 시간 구간을 나타내는 엔티티 class 변수명이 UTC로 되어 있지만 실제 코드에서는 KST로 구현

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | responsePk | Long | private | 시간 응답 고유 식별자(PK) |
| | poll | TimePoll | private | 응답이 속한 시간조율 엔티티 |
| | user | User | private | 응답을 남긴 사용자 엔티티 |
| | startTimeUtc | LocalDateTime | private | 가능한 시간 구간 시작 시각(KST 기준) |
| | endTimeUtc | LocalDateTime | private | 가능한 시간 구간 종료 시각(KST 기준) |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | TimeResponse | TimePoll poll, User user, LocalDateTime startTimeUtc, LocalDateTime endTimeUtc | 생성자 | 한 사용자의 가능 시간 응답 엔티티 생성자 |
