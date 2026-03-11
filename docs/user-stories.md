# User Stories

There are the different user stories for the TECHCUP Football Platform. Each user story is linked with at least one functional or non-functional requirement. 

---

## Traceability table

| User Story | Related Requirement(s) | Microservice |
| --- | --- | --- |
| US-01 | FR-01 | Identity |
| US-02 | FR-02 | Identity |
| US-03 | FR-03 | Users & Players |
| US-04 | FR-04 | Users & Players |
| US-05 | FR-08, FR-09 | Teams |
| US-06 | FR-05, FR-06 | Teams |
| US-07 | FR-07 | Teams |
| US-08 | FR-08 | Teams |
| US-09 | FR-10 | Tournament |
| US-10 | FR-11 | Tournament |
| US-11 | FR-12, FR-13 | Tournament |
| US-12 | FR-13 | Tournament  |
| US-13 | FR-14 | Tournament  |
| US-14 | FR-15 | Tournament  |
| US-15 | FR-12, FR-13, FR-14, FR-22 | Tournament |
| US-16 | FR-16 | Competition |
| US-17 | FR-17 | Competition |
| US-18 | FR-18 | Competition |
| US-19 | FR-19 | Competition |
| US-20 | FR-20, FR-21 | Competition |
| US-21 | FR-22 | Competition |
| US-22 | NFR-01 | Identity |
| US-23 | NFR-02 | Identity |

---

## US-01 - Register account

| Field | Description |
| --- | --- |
| **ID** | US-01 |
| **Title** | Register account |
| **Description** | *AS a person who wants to participate in TECHCUP Football I WANT to create an account using my institutional email (if I am a student, professor, graduate, or administrative staff) or my personal email (if I am a family member) SO THAT I can access the platform and participate in the tournament process.* |
| **Priority** | High |
| **Priority explanation** | *Account registration is the entry point to the platform and is required before accessing any other functionality.* |
| **Related requirement(s)** | FR-01 User registration |
| **Requirement explanation** | *Registration corresponds directly to the requirement that allows users to create an account with the necessary personal and institutional information.* |
| **Acceptance criteria** | - The system accepts institutional email for students, professors, graduates, and administrative staff. - The system accepts personal email for family members. - Required fields include: full name, email, password, relationship with the institution, academic program, date of birth, and identification. - The user can select the role of player or guest at registration time. - The user account is created with Active status by default. - Referees cannot be self-registered; they are created by the organizer. |

---

## US-02 - Log in to the platform

| Field | Description |
| --- | --- |
| **ID** | US-02 |
| **Title** | Log in to the platform |
| **Description** | *AS a registered user I WANT to log in securely to the platform using my email and password SO THAT I can access my profile and use the system functionalities according to my role.* |
| **Priority** | High |
| **Priority explanation** | *Authenticated access is necessary to protect user information and enable role-based interaction with the system.* |
| **Related requirement(s)** | FR-02 User authentication |
| **Requirement explanation** | *Authentication validates credentials, generates a JWT token, and grants access according to the user's role.* |
| **Acceptance criteria** | - The user can log in using their registered email and password. - The system validates credentials and generates a JWT token upon success. - The session has an expiration time. - Passwords are stored encrypted. - On invalid credentials, the system displays an appropriate error message. |

---

## US-03 - Create sports profile

| Field | Description |
| --- | --- |
| **ID** | US-03 |
| **Title** | Create sports profile |
| **Description** | *AS a player I WANT to create and complete my sports profile with my playing position, jersey number, and photo SO THAT captains can identify me and evaluate whether I fit their team needs.* |
| **Priority** | High |
| **Priority explanation** | *The sports profile is essential for team formation and player identification within the tournament.* |
| **Related requirement(s)** | FR-03 Sports profile management |
| **Requirement explanation** | *Profile management allows players to store their sports-related information, which is used by captains during recruitment.* |
| **Acceptance criteria** | - The player can create a sports profile with: playing position (goalkeeper, defender, midfielder, forward), jersey number, and photo. - The player can update the sports profile only when not currently assigned to a team. - The system does not allow deletion of a sports profile. |

---

## US-04 - Set availability as a player

| Field | Description |
| --- | --- |
| **ID** | US-04 |
| **Title** | Set availability as a player |
| **Description** | *AS a player I WANT to mark myself as available to join a team SO THAT captains can find me and invite me to participate in the tournament.* |
| **Priority** | High |
| **Priority explanation** | *Player availability supports the team-building process and helps captains complete their rosters efficiently.* |
| **Related requirement(s)** | FR-04 Player availability management |
| **Requirement explanation** | *Availability management allows players to indicate their willingness to be recruited and makes them discoverable in captain searches.* |
| **Acceptance criteria** | - The player can mark themselves as available for team recruitment. - Available players appear in captain search results. - A player can only send one active membership request at a time. |

---

## US-05 - Respond to team invitations

| Field | Description |
| --- | --- |
| **ID** | US-05 |
| **Title** | Respond to team invitations |
| **Description** | *AS a player I WANT to accept or reject invitations sent by team captains SO THAT I can decide which team I want to join and keep control over my tournament participation.* |
| **Priority** | High |
| **Priority explanation** | *Players must be able to confirm or decline team invitations in a clear and traceable way.* |
| **Related requirement(s)** | FR-08 Team invitation management, FR-09 Team membership validation |
| **Requirement explanation** | *Invitation management enables captains to recruit players, and membership validation ensures team composition rules are respected.* |
| **Acceptance criteria** | - The player receives and can view pending team invitations. - The player can accept or reject each invitation. - If accepted, the system validates team composition rules (size, jersey number uniqueness, program distribution) before confirming membership. - Once in a team, the player cannot join another team simultaneously. |

---

## US-06 - Create a team

| Field | Description |
| --- | --- |
| **ID** | US-06 |
| **Title** | Create a team |
| **Description** | *AS a captain I WANT to create a team with a name and team colors SO THAT I can organize a group of players and prepare for tournament registration.* |
| **Priority** | High |
| **Priority explanation** | *Team creation is a fundamental function and a prerequisite for tournament participation.* |
| **Related requirement(s)** | FR-05 Team creation, FR-06 Team configuration |
| **Requirement explanation** | *Team creation and configuration allow captains to define the identity and structure of their teams.* |
| **Acceptance criteria** | - The captain can create a team providing a name and colors. - The team name can be updated only when not enrolled in an active or in-progress tournament. - The system enforces: minimum 7 players, maximum 12 players, no duplicate jersey numbers, and more than half the members from Systems, AI, Cybersecurity, or Statistical Engineering programs. - A player cannot belong to more than one team. |

---

## US-07 - Search players

| Field | Description |
| --- | --- |
| **ID** | US-07 |
| **Title** | Search players |
| **Description** | *AS a captain I WANT to search for available players by position, semester, age, gender, name, or identification SO THAT I can find suitable players to complete my team.* |
| **Priority** | High |
| **Priority explanation** | *Effective search criteria allow captains to build balanced and eligible teams.* |
| **Related requirement(s)** | FR-07 Player search |
| **Requirement explanation** | *Player search allows captains to identify suitable participants based on specific filters.* |
| **Acceptance criteria** | - The captain can filter players by: playing position, semester, age, gender, name, and identification number. - The search only returns players marked as available and not already on a team. - The captain can send an invitation directly from the search results. |

---

## US-08 - Invite players to my team

| Field | Description |
| --- | --- |
| **ID** | US-08 |
| **Title** | Invite players to my team |
| **Description** | *AS a captain I WANT to send invitations to available players and manage incoming membership requests SO THAT I can complete my team roster.* |
| **Priority** | High |
| **Priority explanation** | *Team completion depends on the captain's ability to contact and recruit players through the platform.* |
| **Related requirement(s)** | FR-08 Team invitation management |
| **Requirement explanation** | *Invitation management allows captains to recruit players and respond to membership requests directly through the platform.* |
| **Acceptance criteria** | - The captain can send invitations to available players. - The captain can accept or reject incoming membership requests from players. - The system updates the team roster upon acceptance. - The system validates team composition rules upon each addition. |

---

## US-09 - Upload payment proof 

| Field | Description |
| --- | --- |
| **ID** | US-09 |
| **Title** | Upload payment proof |
| **Description** | *AS a captain I WANT to upload the payment receipt for my team's tournament registration SO THAT the organizer can review it and approve my team's participation.* |
| **Priority** | High |
| **Priority explanation** | *Payment proof upload is a mandatory step before a team can be officially approved for tournament participation.* |
| **Related requirement(s)** | FR-10 Payment receipt upload |
| **Requirement explanation** | *Payment receipt upload allows captains to submit proof of payment for tournament registration, which triggers the review process.* |
| **Acceptance criteria** | - The captain can upload a payment receipt image when registering the team for an active tournament. - After upload, the inscription status is set to "Under review". - The system validates the file format before storing the receipt. - The captain can cancel the inscription only while its status is still "Under review". - Payment processing does not occur within the platform. |

---

## US-10 - Review payment proofs 

| Field | Description |
| --- | --- |
| **ID** | US-10 |
| **Title** | Review payment proofs |
| **Description** | *AS an organizer I WANT to review the payment proofs uploaded by team captains and approve or reject them SO THAT only eligible and verified teams participate in the tournament.* |
| **Priority** | High |
| **Priority explanation** | *Payment approval is a mandatory condition for a team to be officially registered in the tournament.* |
| **Related requirement(s)** | FR-11 Payment validation |
| **Requirement explanation** | *Payment validation allows the organizer to verify submitted receipts and determine team eligibility for the tournament.* |
| **Acceptance criteria** | - The organizer can view a list of inscriptions with "Under review" status. - The organizer can open and inspect each receipt. - The organizer can approve or reject each inscription. - The system updates the inscription status accordingly and notifies the captain. - Possible statuses are: Under review, Approved, Rejected, Cancelled. - Only teams with Approved status can participate in the tournament. |

---

## US-11 - Create tournament 

| Field | Description |
| --- | --- |
| **ID** | US-11 |
| **Title** | Create tournament |
| **Description** | *AS an organizer I WANT to create a tournament with its basic information and upload the regulations and field details SO THAT I can set up the competition structure on the platform.* |
| **Priority** | High |
| **Priority explanation** | *Tournament creation is the foundation for all administrative and operational processes in the platform.* |
| **Related requirement(s)** | FR-12 Tournament creation, FR-13 Tournament configuration |
| **Requirement explanation** | *Tournament creation initializes the competition, and tournament configuration adds the regulations and field information needed before activation.* |
| **Acceptance criteria** | - The organizer can create a tournament providing: start date, end date, registration deadline, number of teams, cost. - The tournament is created with "Draft" status by default. - The organizer can upload the regulations as a PDF document. - The organizer can add fields with: name, image, and description. - The organizer can delete the tournament only while it is in Draft status. |

---

## US-12 - Configure tournament rules and schedule 

| Field | Description |
| --- | --- |
| **ID** | US-12 |
| **Title** | Configure tournament rules and schedule |
| **Description** | *AS an organizer I WANT to update the tournament regulations and field information SO THAT participants have access to clear and up-to-date competition conditions.* |
| **Priority** | High |
| **Priority explanation** | *Proper tournament configuration ensures transparency, order, and consistency throughout the competition.* |
| **Related requirement(s)** | FR-13 Tournament configuration |
| **Requirement explanation** | *Tournament configuration allows organizers to maintain and update the operational parameters and rules of the competition.* |
| **Acceptance criteria** | - The organizer can update the regulations PDF for an existing tournament. - The organizer can add, update, or manage fields (name, image, description). - Changes are immediately visible to all users. - Configuration is available while the tournament is in Draft or Active status. |

---

## US-13 - Manage tournament lifecycle 

| Field | Description |
| --- | --- |
| **ID** | US-13 |
| **Title** | Manage tournament lifecycle |
| **Description** | *AS an organizer I WANT to activate, start, and finalize the tournament at the appropriate times SO THAT the competition progresses through its stages in an orderly and controlled manner.* |
| **Priority** | High |
| **Priority explanation** | *Lifecycle management ensures that each tournament phase (registration, competition, conclusion) occurs under the correct conditions and dates.* |
| **Related requirement(s)** | FR-14 Tournament lifecycle management |
| **Requirement explanation** | *Lifecycle management controls the tournament state transitions that determine what actions are available to each actor at each stage.* |
| **Acceptance criteria** | - The organizer can activate a tournament (Draft → Active) at any time. - The organizer can start a tournament (Active → In Progress) only if the current date equals the tournament start date. - The organizer can finalize a tournament (In Progress → Finished) only if the current date is on or after the tournament end date. - Teams can only register while the tournament is in Active status and before the registration deadline. - Matches take place during In Progress status. |

---

## US-14 - Cancel tournament inscription 

| Field | Description |
| --- | --- |
| **ID** | US-14 |
| **Title** | Cancel tournament inscription |
| **Description** | *AS a captain I WANT to cancel my team's tournament inscription SO THAT I can withdraw from the competition if needed before the organizer has reviewed our payment.* |
| **Priority** | Medium |
| **Priority explanation** | *Captains need the ability to withdraw from a tournament registration before it is reviewed, to avoid locking teams into a process they can no longer complete.* |
| **Related requirement(s)** | FR-15 Tournament registration cancellation |
| **Requirement explanation** | *Registration cancellation allows captains to withdraw their team from a pending inscription before organizer review.* |
| **Acceptance criteria** | - The captain can cancel the team's inscription only while its status is "Under review". - The system updates the inscription status to "Cancelled". - Cancellation is not possible if the inscription has already been Approved or Rejected. - After cancellation, the team is no longer registered for that tournament. |

---

## US-15 - View tournament information 

| Field | Description |
| --- | --- |
| **ID** | US-15 |
| **Title** | View tournament information |
| **Description** | *AS a user I WANT to consult the tournament's general information including rules, important dates, registered teams, match schedule, and results SO THAT I can stay informed about the competition.* |
| **Priority** | High |
| **Priority explanation** | *Centralized tournament information improves transparency and reduces confusion among all participants.* |
| **Related requirement(s)** | FR-12 Tournament creation, FR-13 Tournament configuration, FR-14 Tournament lifecycle management, FR-22 Tournament statistics consultation |
| **Requirement explanation** | *Tournament creation, configuration, and lifecycle management define the data displayed to users. Statistics consultation provides match history and team performance data.* |
| **Acceptance criteria** | - All users can view: tournament dates, registration deadline, cost, regulations PDF, field information, and current tournament status. - All users can view the list of registered teams once inscriptions are approved. - All users can view match schedules and results when available. - All users can view tournament statistics (top scorers, match history, team results). - The home page displays general information about the current active tournament after login. |

---

## US-16 - Define team lineup

| Field | Description |
| --- | --- |
| **ID** | US-16 |
| **Title** | Define team lineup |
| **Description** | *AS a captain I WANT to select starters, substitutes, and the tactical formation for each match SO THAT my team is properly organized before each game.* |
| **Priority** | High |
| **Priority explanation** | *Lineups are necessary to prepare each match and provide internal visibility for team members.* |
| **Related requirement(s)** | FR-16 Team lineup management |
| **Requirement explanation** | *Team lineup management allows captains to define the players and formation that will participate in a match.* |
| **Acceptance criteria** | - The captain can define starters and substitutes for each scheduled match. - Available formations are: 3-2-1, 2-3-1, 4-1-1, and 1-3-2. The default is 2-3-1. - Lineups are only visible to players and captains belonging to the same team. - The lineup can be set before the match takes place. |

---

## US-17 - View team lineup

| Field | Description |
| --- | --- |
| **ID** | US-17 |
| **Title** | View team lineup |
| **Description** | *AS a player or captain I WANT to consult my team's lineup for an upcoming match SO THAT I know my role and position for the game.* |
| **Priority** | Medium |
| **Priority explanation** | *Players need to know their assigned roles before each match to prepare adequately.* |
| **Related requirement(s)** | FR-17 Opponent lineup consultation |
| **Requirement explanation** | *Lineup consultation allows players and captains to review their own team's registered lineup for a match.* |
| **Acceptance criteria** | - Players and captains can view their own team's lineup for a scheduled match. - The lineup displays starters, substitutes, and formation. - Lineups of opposing teams are not visible to the other team. |

---

## US-18 - Register match results

| Field | Description |
| --- | --- |
| **ID** | US-18 |
| **Title** | Register match results |
| **Description** | *AS an organizer I WANT to register match results including goals, scorers, yellow cards, and red cards SO THAT the system automatically updates the standings and keeps accurate tournament statistics.* |
| **Priority** | High |
| **Priority explanation** | *Match data is required to maintain accurate standings, statistics, and tournament progress.* |
| **Related requirement(s)** | FR-18 Match result registration |
| **Requirement explanation** | *Match result registration allows the system to store outcomes and automatically recalculate tournament standings.* |
| **Acceptance criteria** | - The organizer can register: final score, goals linked to the scorer, yellow cards, and red cards for each match. - The system automatically updates standings after each result is registered. - The organizer can delete a match if teams are disqualified or do not show up. - The organizer can update a match (date, time, field, referee) only if the scheduled date is in the future. |

---

## US-19 - View assigned matches

| Field | Description |
| --- | --- |
| **ID** | US-19 |
| **Title** | View assigned matches |
| **Description** | *AS a referee I WANT to view the matches assigned to me including date, time, field, participating teams, and sanctioned players SO THAT I am properly informed before officiating each game.* |
| **Priority** | High |
| **Priority explanation** | *Referees need access to their assigned match information to support the operational execution of the tournament.* |
| **Related requirement(s)** | FR-19 Match consultation for referees |
| **Requirement explanation** | *Match consultation allows referees to review all relevant details of the games they will officiate, including disciplinary information.* |
| **Acceptance criteria** | - The referee can view only matches assigned to them. - For each match, the system shows: date, time, field, participating teams, and sanctioned players. - The referee cannot modify any match information. |

---

## US-20 - View standings and knockout bracket

| Field | Description |
| --- | --- |
| **ID** | US-20 |
| **Title** | View standings and knockout bracket |
| **Description** | *AS a user I WANT to view the tournament standings table and the knockout bracket SO THAT I can follow the competition progress in real time.* |
| **Priority** | High |
| **Priority explanation** | *Standings and brackets provide transparency and are a key part of the tournament experience for all participants.* |
| **Related requirement(s)** | FR-20 Standings calculation, FR-21 Knockout bracket generation |
| **Requirement explanation** | *Standings calculation automatically updates team statistics after each match. Knockout bracket generation creates and updates the elimination stage structure.* |
| **Acceptance criteria** | - All users can view the standings table showing: matches played, wins, draws, losses, goals for, goals against, goal difference, and points per team. - The standings table updates automatically after each match result is registered. - All users can view the knockout bracket (quarterfinals, semifinals, final). - Initial bracket matchups are generated randomly from approved teams. - The bracket updates automatically as matches are completed. |

---

## US-21 - View tournament statistics

| Field | Description |
| --- | --- |
| **ID** | US-21 |
| **Title** | View tournament statistics |
| **Description** | *AS a user I WANT to consult general tournament statistics such as top scorers, match history, and team performance SO THAT I can follow the competition in detail.* |
| **Priority** | Medium |
| **Priority explanation** | *Tournament statistics provide value to all participants and increase engagement with the platform.* |
| **Related requirement(s)** | FR-22 Tournament statistics consultation |
| **Requirement explanation** | *Statistics consultation allows all users to access aggregated competition data.* |
| **Acceptance criteria** | - All users can consult: top scorers list, match history, and results per team. - Statistics are updated automatically after each match result is registered. - Statistics are accessible without requiring special permissions. |

---

## US-22 - Manage roles and permissions

| Field | Description |
| --- | --- |
| **ID** | US-22 |
| **Title** | Manage roles and permissions |
| **Description** | *AS an administrator I WANT to assign, update, and remove user roles SO THAT each user can only access the functionalities permitted by their role and the platform remains secure.* |
| **Priority** | High |
| **Priority explanation** | *Role-based access control is fundamental to system security and proper functional separation.* |
| **Related requirement(s)** | NFR-01 Role-based access control |
| **Requirement explanation** | *RBAC ensures that each user can only perform actions permitted by their assigned role.* |
| **Acceptance criteria** | - The administrator can assign, update, and remove roles for any user. - Available roles are: guest, player, captain, organizer, referee, and administrator. - The organizer can assign the captain role but cannot escalate any user to administrator. - Referees are created by the organizer, not through self-registration. - Role changes are reflected immediately in system access. |

---

## US-23 - Monitor audit logs

| Field | Description |
| --- | --- |
| **ID** | US-23 |
| **Title** | Monitor audit logs |
| **Description** | *AS an administrator I WANT to review audit logs of system actions SO THAT I can monitor activity, ensure traceability, and detect possible misuse of the platform.* |
| **Priority** | Medium |
| **Priority explanation** | *Audit monitoring supports accountability, security, and administrative control of the platform.* |
| **Related requirement(s)** | NFR-02 Audit logging |
| **Requirement explanation** | *Audit logging records relevant actions across all microservices and makes them available for administrative review.* |
| **Acceptance criteria** | - The system logs: login, logout, user registration, profile updates, team creation/update/inactivation, tournament creation/update/inactivation, inscription changes, and match creation/update/deletion. - Each log entry includes: action type, user, and timestamp. - The administrator can consult and filter audit logs. - Logs are read-only and cannot be modified or deleted by any user. |