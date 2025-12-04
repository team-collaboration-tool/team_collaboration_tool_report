## 3. Class diagram

이 장은 '협업의 민족' 시스템을 다양한 관점에서 바라본 Class diagram(이하 CD)과 각각에 대한 설명을 기술한다.

### 3.1. DB class diagram

* 서버의 구조를 파악하기 위해 DB의 관점에서 본 CD를 작성했다.
  * ER diagram을 먼저 작성한 후, 이를 CD로 변환시켰다.
* 변수의 이름은 소문자로 시작하며 단어의 구분은 언더바(_)로 한다.

![[그림 3-1] DB class diagram](./image/DBclassdiagram.png)
[그림 3-1] DB class diagram

---

#### User

**Class Description**: 사용자의 정보를 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | id | varchar | private | 사용자를 구분하기 위한 고유의 변수 |
| Attributes | password | varchar | private | 사용자의 비밀번호를 나타내는 변수 |
| Attributes | name | varchar | private | 사용자의 이름을 나타내는 변수 |
| Attributes | email | varchar | private | 사용자의 이메일 주소를 나타내는 변수 |
| Attributes | phone | varchar | private | 사용자의 전화번호를 나타내는 변수 |
| Attributes | field | varchar | private | 사용자의 분야(직무/전공)를 나타내는 변수 |

---

#### Projects

**Class Description**: 각 프로젝트들의 정보를 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | id | bigint | private | 프로젝트를 구분하기 위한 고유의 변수 |
| Attributes | project_name | varchar | private | 각 프로젝트명을 나타내는 변수 |
| Attributes | project_owner_user_pk | bigint | private | 프로젝트 관리자를 나타내는 변수(users.id 참조) |
| Attributes | Invite_code | varchar | private | 각 프로젝트의 참여코드를 나타내는 변수 |

---

#### Project_user

**Class Description**: 사용자와 프로젝트 간의 소속 관계를 나타내는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | id | bigint | private | 관계를 구분하기 위한 고유의 변수 |
| Attributes | project_pk | bigint | private | 연결된 프로젝트(projects.id 참조) |
| Attributes | user_pk | bigint | private | 연결된 사용자(users.id 참조) |
| Attributes | status | varchar | private | 사용자의 프로젝트 내 상태를 나타내는 변수(프로젝트 멤버, 승인 대기 멤버) |

---

#### Calendar_events

**Class Description**: 프로젝트의 일정 정보를 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | event_pk | bigint | private | 일정을 구분하기 위한 고유의 변수 |
| Attributes | project_pk | bigint | private | 어떤 프로젝트의 일정인지 구분하는 변수 |
| Attributes | user_pk | bigint | private | 일정을 생성한 사용자를 구분하는 변수 |
| Attributes | title | varchar | private | 일정의 이름을 저장하는 변수 |
| Attributes | start_time | timestamp | private | 일정 시작 시간을 나타내는 변수 |
| Attributes | end_time | timestamp | private | 일정 종료 시간을 나타내는 변수 |
| Attributes | description | text | private | 일정 상세 정보를 나타내는 변수 |

---

#### Event_participants

**Class Description**: 일정 참가자 정보를 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | participants_pk | bigint | private | 참가 기록의 고유 식별자 |
| Attributes | event_pk | bigint | private | 참가한 일정을 구분하기 위한 변수 |
| Attributes | user_pk | bigint | private | 어떤 사용자가 참가하는지 구분하는 변수 |

---

#### Posts

**Class Description**: 각 프로젝트별 게시글 정보를 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | post_pk | bigint | private | 게시글을 구분하기 위한 고유의 변수 |
| Attributes | project_pk | bigint | private | 연결된 프로젝트가 어떤 것인지 구분하는 변수 |
| Attributes | user_pk | bigint | private | 게시글 작성자를 구분하기 위한 변수 |
| Attributes | content | text | private | 게시글 본문 내용을 나타내는 변수 |
| Attributes | created_at | timestamp | private | 게시글 생성 시간을 나타내는 변수 |
| Attributes | updated_at | timestamp | private | 게시글 수정 시간을 나타내는 변수 |
| Attributes | Is_notice | boolean | private | 공지사항 유무를 나타내는 변수 |
| Attributes | has_voting | boolean | private | 투표 기능이 포함되어 있는지 구분하는 변수 |

---

#### Time_polls

**Class Description**: 시간조율표 정보를 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | poll_pk | bigint | private | 각 시간조율표 이벤트를 구별하는 변수 |
| Attributes | project_pk | bigint | private | 어떤 프로젝트에 속하는지 구분하는 변수 |
| Attributes | creator_user_pk | bigint | private | 누가 생성한 것인지 구분하는 변수 |
| Attributes | title | varchar | private | 각 시간조율표의 이름을 저장하는 변수 |
| Attributes | start_date | date | private | 시간조율 범위의 시작 날짜를 저장하는 변수 |
| Attributes | end_date | date | private | 시간조율 범위의 마감 날짜를 저장하는 변수 |
| Attributes | start_time_of_day | time | private | 시간조율표의 시작 시간을 저장하는 변수(기본값은 09:00) |
| Attributes | end_time_of_day | time | private | 시간조율표의 종료 시간을 저장하는 변수(기본값은 18:00) |

---

#### Votes

**Class Description**: 게시글 내 투표 정보를 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | vote_pk | bigint | private | 투표를 구분하기 위한 고유의 변수 |
| Attributes | post_pk | bigint | private | 어떤 게시글의 투표인지 구분하는 변수 |
| Attributes | title | varchar | private | 투표 제목을 나타내는 변수 |
| Attributes | start_time | timestamp | private | 투표 시작 시간을 나타내는 변수 |
| Attributes | end_time | timestamp | private | 투표 종료 시간을 나타내는 변수 |
| Attributes | allow_multiple_choices | boolean | private | 복수 선택 여부를 나타내는 변수 |
| Attributes | is_anonymous | boolean | private | 익명 투표 여부를 나타내는 변수 |

---

#### Time_response

**Class Description**: 각 시간조율표에 대한 사용자들의 응답을 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | response_pk | bigint | private | 시간 응답을 구분하기 위한 고유의 변수 |
| Attributes | poll_pk | bigint | private | 어떤 시간조율표에 대한 응답인지 구분하는 변수 |
| Attributes | user_pk | bigint | private | 어떤 사용자가 가능한 시간대인지 구분하는 변수 |
| Attributes | start_time_utc | timestamp | private | 드래그한 시간의 시작 시점을 UCT로 저장 |
| Attributes | end_time_utc | timestamp | private | 드래그한 시간의 종료 시점을 UCT로 저장 |

---

#### Vote_options

**Class Description**: 투표의 선택 항목을 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | option_pk | bigint | private | 투표 선택 항목을 구분하기 위한 고유의 변수 |
| Attributes | vote_pk | bigint | private | 어떤 투표에 속한 항목인지 식별하는 변수 |
| Attributes | content | varchar | private | 투표 선택 항목의 내용을 나타내는 변수 |

---

#### Vote_response

**Class Description**: 각 투표에 대해 사용자가 어떤 항목에 투표했는지 저장하는 class

| 구분 | Name | Type | Visibility | Description |
| :--- | :--- | :--- | :--- | :--- |
| Attributes | response_pk | bigint | private | 투표 응답을 기록하는 변수 |
| Attributes | option_pk | bigint | private | 어떤 항목에 투표했는지 구별하는 변수 |
| Attributes | user_pk | bigint | private | 누가 투표를 했는지 구별하는 변수 |

## 3.2. Function class diagram

본 절에서는 시스템의 주요 기능별 클래스 구조와 역할을 시각적으로 나타낸 Function class diagram을 제시한다. 각 기능 모듈 내에서 클래스 간의 관계와 책임을 명확히 파악하기 위해 작성되었으며, Controller, Service, Domain, DTO 등 계층 간 상호작용을 중심으로 설계되었다.<br>
각 Function class diagram은 해당 모듈이 담당하는 주요 역할을 기반으로 작성되었으며, 각 클래스의 속성(Attributes)과 연산(Operations)을 함께 기술하여 클래스의 내부 구조를 한 눈에 확인할 수 있다.<br>
이를 통해 시스템 전체의 아키키텍처를 이해하고, 기능 간의 의존 관계 및 데이터 흐름을 한 눈에 확인할 수 있다.

* 변수의 이름은 소문자로 시작하며 단어의 구분은 대문자로 한다.

---

### User class diagram

![[그림 3-2] User class diagram](./image/UserCD.png)
[그림 3-2] User class diagram

#### User

**Class Description:** 사용자의 정보를 저장하고 DB 테이블 users와 매핑되는 엔티티 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | userPk | Long | private | 사용자 고유 식별자 (PK) |
| | password | String | private | 암호화된 비밀번호 |
| | name | String | private | 사용자 이름 |
| | email | String | private | 사용자 이메일 |
| | phone | String | private | 사용자 전화번호 |
| | field | String | private | 사용자 분야 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | User | password : String,<br>name : String,<br>email : String,<br>phone : String,<br>field : String | 생성자 | 새 사용자 객체를 생성하는 생성자 |

---

#### UserController

**Class Description:** 사용자 관련 기능(회원가입, 로그인, 정보 수정, 비밀번호 변경, 개인정보 조회)을 담당하는 컨트롤러 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | userService | UserService | private | 사용자 관련 비즈니스 로직을 수행하는 서비스 class |
| | passwordEncoder | BCryptPasswordEncoder | private | 비밀번호 암호화를 위한 인코더 객체 |
| | jwtTokenProvider | JwtTokenProvider | private | JWT 토큰 발급 및 검증 유틸 클래스 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | signup | request : UserSignupRequestDTO | ResponseEntity | 회원가입 요청을 처리하는 메서드 |
| | login | request : Map<String,String> | ResponseEntity | 사용자 로그인 처리 및 JWT 토큰 발급 |
| | getMyInfo | userEmail : String | ResponseEntity | 로그인된 사용자의 개인정보를 반환 |
| | updateUser | userEmail : String, request : UserUpdateRequest | ResponseEntity | 사용자 정보를 수정하는 메서드 |
| | updatePassword | userEmail : String, request : UserPasswordUpdateRequest | ResponseEntity | 비밀번호를 변경하는 메서드 |

---

#### UserRepository

**Class Description:** JPA를 사용하여 DB의 사용자 테이블(users)과 상호작용하는 Repository 인터페이스

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | findByEmail | email : String | Optional | 이메일로 사용자 조회 |
| | existsByEmail | email : String | boolean | 이메일 중복 여부 확인 |

---

#### UserService

**Class Description:** 회원가입, 사용자 정보 조회 및 수정, 비밀번호 변경 등 사용자 관련 로직을 담당하는 서비스 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | userRepository | UserRepository | private | 사용자 정보를 관리하는 JPA Repository |
| | passwordEncoder | BCryptPasswordEncoder | private | 비밀번호 암호화용 인코더 객체 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | registerUser | request : UserSignupRequestDTO | User | 회원가입 처리 및 사용자 저장 |
| | findByEmail | email : String | User | 이메일로 사용자 조회 |
| | updateUser | email : String, <br>request : UserUpdateRequest | void | 사용자 정보 수정 |
| | updatePassword | email : String, <br>request : UserPasswordUpdateRequest | void | 비밀번호 변경 처리 |

---
---

### Project class diagram

![[그림 3-3] Project class diagram](./image/ProjectCD.png)
[그림 3-3] Project class diagram

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

### Calendar class diagram

![[그림 3-4] Calendar class diagram](./image/CalendarCD.png)
[그림 3-4] Calendar class diagram

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

### TimeSchedule class diagram

![[그림 3-6] TimeSchedule class diagram](./image/시간조율CD.png)
[그림 3-6] TimeSchedule class diagram

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
| Operations | TimePoll | Project: Project, creator: User, title: String, startDate: LocalDate, endDate: LocalDate, startTimeOfDay: LocalTime, endTimeOfDay: LocalTime | 생성자 | 새 시간조율 엔티티를 생성하는 생성자 |

---

#### CreateRequest

**Class Description:** 시간조율을 새로 생성할 때 클라이언트로부터 전달받는 요청 데이터를 담는 DTO class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | projectId | Long | private | 시간조율이 속한 프로젝트 식별자(PK) |
| | creatorId | Long | private | 시간조율을 생성한 사용자 식별자(PK) |
| | title | String | private | 시간조율 제목 |
| | startDate | LocalDate | private | 시간조율 시작 날짜 |
| | endDate | LocalDate | private | 시간조율 종료 날짜 (duration을 통해 계산된 값) |
| | duration | Integer | private | 기준 날짜(startDate)로부터 며칠 동안 조율할지(일 수) |
| | startTimeOfDay | LocalTime | private | 하루 중 조율 시작 시간 |
| | endTimeOfDay | LocalTime | private | 하루 중 조율 종료 시간 |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | CreateRequest | projectId: Long, creatorId: Long, title: String, startDate: LocalDate, duration: Integer, startTimeOfDay: LocalTime, endTimeOfDay: LocalTime | 생성자 | 새 시간조율 생성 요청 객체를 생성하는 생성자 |

---

#### PollSummary

**Class Description:** 시간 조율 목록 화면에 각 시간조율을 간단히 보여주기 위한 요약 정보 DTO Class

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
| Operations | PollSummary | pollId: Long, title: String, startDate: LocalDate, endDate: LocalDate, duration: Integer, startTime: LocalTime, endTime: LocalTime, userCount: Integer | 생성자 | 목록 조회 응답용 요약 객체 생성자 |

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
| Operations | TimeRange | start: String, end: String | 생성자 | 한 개의 가능 시간 구간을 생성하는 생성자 |

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
| Operations | SubmitRequest | pollId: Long, userId: Long, availableTimes: List<TimeRange> | 생성자 | 시간 응답 제출 요청 객체 생성자 |

---

#### DetailResponse

**Class Description:** 단일 시간조율에 대해 상세 화면에서 사용할 전체 시간표(그리드) 정보를 내려주는 응답 DTO class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | pollId | Long | private | 시간조율 식별자(PK) |
| | title | String | private | 시간조율 제목 |
| | gridData | int[][] | private | 날짜/시간 칸마다 몇 명이 가능한지 나타내는 2차원 배열 |
| | dateLabels | List<String> | private | 가로축 날짜 라벨(예: 11/23, 11/24 …) |
| | timeLabels | List<String> | private | 세로축 시간 라벨(예: 09:00, 09:30 …) |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | DetailResponse | pollId: Long, title: String, gridData: int[][], dateLabels: List<String>, timeLabels: List<String> | 생성자 | 시간표 상세 응답 객체를 생성하는 생성자 |

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
| | createPoll | request: CreateRequest | List<PollSummary> 또는 응답 Entity | 새로운 시간조율을 생성하고 최신 목록 반환 |
| | getPollDetail | pollId: Long | DetailResponse | 단일 시간조율의 상세 시간표 정보 조회 |
| | submitTime | request: SubmitRequest | void / 응답 메시지 | 사용자의 가능 시간 정보를 제출 |

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
| Operations | createTimePoll | request: CreateRequest | TimePoll | 새 시간조율을 생성하여 저장 |
| | getPollList | projectId: Long | List<PollSummary> | 프로젝트별 시간조율 목록을 요약 형태로 조회 |
| | submitResponse | request: SubmitRequest | void | 사용자의 가능 시간 응답을 저장(또는 갱신) |
| | getPollDetailGrid | pollId: Long | DetailResponse | 특정 시간조율의 전체 시간표 그리드 데이터를 생성 |
| | fillGrid (private) | grid: int[][], response: TimeResponse, ... | void | 응답 데이터를 기반으로 grid 배열에 인원수를 채워 넣는 내부 헬퍼 메서드 |

---

#### TimeResponse

**Class Description:** 특정 시간조율에 대해 한 사용자가 제출한 가능 시간 구간을 나타내는 엔티티 class

**Attributes**
| 구분 | Name | Type | Visibility | Description |
| --- | --- | --- | --- | --- |
| Attributes | responsePk | Long | private | 시간 응답 고유 식별자(PK) |
| | poll | TimePoll | private | 응답이 속한 시간조율 엔티티 |
| | user | User | private | 응답을 남긴 사용자 엔티티 |
| | startTimeUtc | LocalDateTime | private | 가능한 시간 구간 시작 시각(UTC 기준) |
| | endTimeUtc | LocalDateTime | private | 가능한 시간 구간 종료 시각(UTC 기준) |

**Operations**
| 구분 | Name | Argument | Returns | Description |
| --- | --- | --- | --- | --- |
| Operations | TimeResponse | poll: TimePoll, user: User, startTimeUtc: LocalDateTime, endTimeUtc: LocalDateTime | 생성자 | 한 사용자의 가능 시간 응답 엔티티 생성자 |
