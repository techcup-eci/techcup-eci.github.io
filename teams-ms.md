# REQUIREMENTS ANALYSIS
**Date:** 03/04/2026 | **Prepared by:** DOSW COMPANY

---

## RF-001 — Create Tournament

| Field | Details |
|-------|---------|
| **Code** | RF-001 |
| **Name** | Create Tournament |
| **Description** | The system must allow the organizer to create a new tournament by entering its basic information |
| **How it works** | The organizer accesses the tournaments module, selects "Create Tournament," fills out the form with the required information, and confirms its creation |
| **Primary user** | Organizer |
| **Prerequisites** | The user must be logged in and have the Organizer role assigned |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Start Date | Tournament start date | Date | Must be equal to or later than the current date | YES |
| End Date | Tournament end date | Date | Must be after the start date | YES |
| Number of teams | Maximum number of participating teams | Integer | Must be greater than 1 | YES |
| Cost per team | Registration fee per team | Integer | Must be greater than or equal to 0 | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of successful creation | Text | Displayed upon completion of creation | YES |
| Tournament created | Tournament visible in the tournament list | Registration | Initial status: "Draft" | IF |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Organizer | Go to the tournament management section | |
| 2 | System | Select the "Create Tournament" option | |
| 3 | Organizer | Select "Create tournament" | |
| 4 | System | A form with the required fields will appear | |
| 5 | Organizer | Fill in the tournament details and confirm | |
| 6 | System | Verify that all required fields are complete and comply with the rules | E1, E2, E3 |
| 7 | System | Create the tournament in "Draft" status and display a confirmation message | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the end date is earlier than or equal to the start date, display an error message and do not allow the user to continue | Return to step 5 |
| E2 | System | If the number of devices is less than or equal to 1, display an error message | Return to step 5 |
| E3 | System | If required fields are missing, highlight the missing fields and display an error message | Return to step 5 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-001 | All tournaments are created in "Draft" status by default |
| RN-002 | The valid statuses for a tournament are: Draft, Active, In Progress, or Completed |

---

## RF-002 — Start Tournament

| Field | Details |
|-------|---------|
| **Code** | RF-002 |
| **Name** | Start Tournament |
| **Description** | The system must allow the organizer to change a tournament's status from "Active" to "In Progress," officially starting the tournament |
| **How it works** | The organizer accesses the tournament, selects "Start Tournament," and confirms the action |
| **Primary actor** | Organizer |
| **Prerequisites** | 1) The user must be logged in with the Organizer role; 2) The tournament must be in Active status; 3) There must be at least 2 teams registered with Approved status |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Selected tournament | Tournament you want to start | Selection | Only tournaments with the "Active" status are displayed | YES |
| Confirmation | Confirmation of action | Button | The organizer must explicitly confirm | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of successful start | Text | Displayed upon completion of the action | YES |
| Confirmation message | — | Registration | — | YES |
| Updated status | The tournament changes to "In Progress" | — | Reflected in the tournament list | — |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Organizer | Go to the tournaments module and select a tournament with the status "Active" | |
| 2 | System | View the tournament information with the "Start Tournament" option | |
| 3 | Organizer | Select "Start Tournament" | |
| 4 | System | Request confirmation of the action | |
| 5 | Organizer | Confirm the start of the tournament | |
| 6 | System | Verify that the tournament has at least 2 approved teams | E1 |
| 7 | System | Change the tournament status to "In Progress" and display a confirmation message | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the tournament does not have at least 2 approved teams, display an error message indicating that it cannot be started | Return to step 2 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-003 | A tournament can only be started if it is in active status |
| RN-004 | A minimum of 2 approved teams is required to start a tournament |

---

## RF-003 — End Tournament

| Field | Details |
|-------|---------|
| **Code** | RF-003 |
| **Name** | End Tournament |
| **Description** | The system must allow the organizer to change the status of a tournament from "In Progress" to "Completed" when the tournament ends |
| **How it works** | The organizer accesses the ongoing tournament, selects "End Tournament," and confirms the action |
| **Primary Actor** | Organizer |
| **Prerequisites** | 1) The user must be authenticated with the Organizer role; 2) The tournament must be in the "In Progress" status |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Selected tournament | Tournament to be finalized | Selection | Only tournaments with the status "In progress" are displayed | YES |
| Confirmation | Confirm action | Button | The organizer must explicitly confirm | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of successful completion | Text | Displayed upon completion of the action | YES |
| Updated status | The tournament changes to "Completed" | Registration | Reflected in the tournament list | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Organizer | Go to the tournaments module and select a tournament with the status "In progress" | |
| 2 | System | View the tournament information and select the "End Tournament" option | |
| 3 | Organizer | Select "End Tournament" | |
| 4 | System | Request confirmation of the action | |
| 5 | Organizer | Confirm the completion of the tournament | |
| 6 | System | Verify that all matches in the final phase have been recorded | E1 |
| 7 | System | Change the tournament status to "Completed" and display a confirmation message | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are matches with no recorded result, display an error message indicating that there are pending matches | Return to step 2 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-005 | A tournament can only be finalized if it is in "In Progress" status |
| RN-006 | A tournament cannot be finalized if there are matches without a recorded result |
| RN-007 | Once finalized, the tournament cannot change status |

---

## RF-004 — Check Tournament

| Field | Details |
|-------|---------|
| **Code** | RF-004 |
| **Name** | Check Tournament |
| **Description** | The system must allow any authenticated user to view information about an existing tournament |
| **How it works** | The user accesses the tournaments module, views the list of available tournaments, and selects one to view its details |
| **Primary actor** | All actors |
| **Prerequisites** | The user must be logged in to the system |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Selected tournament | Tournament for which you want to view information | Selection | All existing tournaments are displayed | YES |

### Output Data (Departure Details)

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Start Date | Tournament start date | Date | — | YES |
| End Date | Tournament end date | Date | — | YES |
| Number of teams | Maximum number of teams | Integer | — | YES |
| Cost per unit | Registration fee | Decimal number | — | YES |
| Status | Current tournament status | Text | Draft, Active, In Progress, or Completed | YES |
| Registered teams | List of teams registered for the tournament | List | Visible only if the tournament has teams | NO |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | User | Access the tournaments module | |
| 2 | System | Display the list of available tournaments | E1 |
| 3 | User | Select a tournament | |
| 4 | System | Display detailed information about the selected tournament | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are no registered tournaments, display an informational message indicating that no tournaments are available | End of flow |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-008 | All authenticated users can view tournaments regardless of their role |
| RN-009 | Tournaments in "Draft" status are only visible to the Organizer and the Administrator |

---

## RF-005 — Register as a Player

| Field | Details |
|-------|---------|
| **Code** | RF-005 |
| **Name** | Register as a Player |
| **Description** | The system must allow students, graduates, faculty, administrative staff, and family members to register on the platform as users |
| **How it works** | The user accesses the registration page, selects their affiliation type, enters their personal information, and confirms the registration |
| **Primary user** | Student, Graduate, Faculty Member, Administrative Staff, Family Member |
| **Prerequisites** | The user must not already have an account in the system |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Relationship Type | Relationship to the School | Selector | Student, Graduate, Faculty, Administrative Staff, Family Member | YES |
| Email | — | Text | Institutional email (@escuelaing.edu.co) for students, graduates, teachers, and admins; Gmail for family members | YES |
| Full Name | User's first and last name | Text | Minimum 3 characters | YES |
| ID | — | Numeric | No other user may have the same username | YES |
| Academic program | Current semester | Selector | Must be Systems Engineering, AI, Cybersecurity, Statistics, Master's in Information Management, Computer Science, or Data Science. Applies only to students, graduates, and faculty | COND |
| Semester | User's age | Numeric | Applies only to students. Value between 1 and 10 | COND |
| Age | — | Numeric | Must be greater than 0 | YES |
| Gender | User's gender | Selector | Cannot be "therian" | YES |
| Password | — | Text (password) | Minimum 8 characters; must include uppercase letters, lowercase letters, and a number | IF |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Registration successful | Text | Displayed upon completion of registration | YES |
| Account created | User registered in the system | Registration | The user can log in | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | User | Go to the registration page | |
| 2 | System | Display the registration form | |
| 3 | User | Select relationship type | |
| 4 | System | Adjust the form based on the selected type (show or hide conditional fields) | |
| 5 | User | Fill in all required fields and confirm | |
| 6 | System | Validate that all required fields are complete and comply with the rules | E1, E2, E3, E4 |
| 7 | System | Create the user account and display a confirmation message | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the email address does not match the relationship type, display an error message | Return 5 |
| E2 | System | If a user with the same email address or ID already exists, display an error message indicating they are already registered | Return 5 |
| E3 | System | If the password does not meet security requirements, an error message is displayed | Return 5 |
| E4 | System | If required fields are missing, highlight the missing fields and display an error message | Return 5 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-010 | Students, graduates, faculty, and administrative staff must register using their institutional email address @escuelaing.edu.co |
| RN-011 | Family members must register with a personal Gmail email address |
| RN-012 | No two users may have the same ID or email address |
| RN-013 | The academic program and semester fields apply only depending on the type of affiliation |

---

## RF-006 — Create Sports Profile

| Field | Details |
|-------|---------|
| **Code** | RF-006 |
| **Name** | Create Sports Profile |
| **Description** | The system must allow a registered player to complete their sports profile by indicating their playing positions, jersey number, and photo |
| **How it works** | The player accesses their profile, selects "Complete Sports Profile," enters the sports information, and confirms |
| **Primary User** | Student, Graduate, Professor, Administrative Staff, Family Member |
| **Prerequisites** | The user must be logged in and registered in the system (RF-005 completed) |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Playing positions | Positions the player can play | Multiple selection | Options: Goalkeeper, Defender, Midfielder, Forward. You must select at least one | YES |
| Jersey number | Preferred jersey number | Integer | Value between 1 and 99 | YES |
| Photo | Player photo | Image file | Supported formats: JPG, PNG. Maximum size: 2MB | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of created sports profile | Text | Displayed upon completion of the action | YES |
| Sports profile updated | Sports information visible on the player's profile | Registration | Visible to captains searching for players | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Player | Go to your profile and select "Complete sports profile" | |
| 2 | System | Display the sports profile form | |
| 3 | Player | Select your playing positions | |
| 4 | Player | Enter your preferred jersey number | |
| 5 | Player | Upload your photo | |
| 6 | Player | Confirm the information | |
| 7 | System | Verify that all fields are complete and comply with the rules | E1, E2, E3 |
| 8 | System | Save the athlete profile and display a confirmation message | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If at least one game position is not selected, an error message is displayed | Return to 3 |
| E2 | System | If the jersey number is outside the range of 1–99, display an error message | Return to 4 |
| E3 | System | If the photo is larger than 2MB or is not in JPG/PNG format, display an error message | Return to 5 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-014 | A player may select one or more playing positions |
| RN-015 | The jersey number is a preference; uniqueness is validated within the team at the time of registration |
| RN-016 | The player profile must be complete for the player to be invited to a team |

---

## RF-007 — Mark as Available

| Field | Details |
|-------|---------|
| **Code** | RF-007 |
| **Name** | Mark as Available |
| **Description** | The system must allow a player to indicate that they are available to join a team, enabling captains to find and contact them |
| **How it works** | The player accesses their profile and activates the availability option |
| **Primary user** | Student, Graduate, Professor, Administrative Staff, Family Member |
| **Prerequisites** | 1) The user must be logged in; 2) Must have a complete sports profile (RF-006); 3) Must not belong to a team in the active tournament |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Availability status | Indicator of whether the player is available | Toggle (Yes/No) | Default is "Unavailable" | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Status Change Confirmation | Text | Displayed when availability changes | YES |
| Updated status | The player appears or disappears from captain searches | Registration | Reflected in the player search module (RF-011) | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Player | Access your profile | |
| 2 | System | Display the profile with the availability option | |
| 3 | Player | Enable or disable availability | |
| 4 | System | Validate that the player meets the prerequisites | E1, E2 |
| 5 | System | Update the availability status and display a confirmation message | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the player does not have a complete sports profile, a message appears indicating that they must complete it first | END FLOW |
| E2 | System | If the player already belongs to a team in the active tournament, do not allow them to set their availability and display an informational message | END FLOW |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-017 | A player who already belongs to a team cannot mark themselves as available |
| RN-018 | When joining a team, availability is automatically deactivated |
| RN-019 | Only players with a complete sports profile can activate their availability |

---

## RF-008 — Manage Team Invitations

| Field | Details |
|-------|---------|
| **Code** | RF-008 |
| **Name** | Manage Team Invitations |
| **Description** | The system must allow a player to view invitations received to join a team and accept or decline them |
| **How it works** | The player accesses their invitation inbox, views pending invitations, and selects to accept or decline each one |
| **Primary user** | Student, Graduate, Professor, Administrative Staff, Family Member |
| **Prerequisites** | 1) The user must be logged in; 2) Must have at least one pending invitation |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Selected invitation | Invitation the player wishes to manage | Selection | Only invitations with the "Pending" status are displayed | YES |
| Action | Player's decision regarding the invitation | Button (Accept/Decline) | — | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of action taken | Text | Indicates whether the invitation was accepted or declined | YES |
| Updated status | — | Registration | Accepted: the player joins the team. Declined: the invitation is rejected | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Player | Access your invitation inbox | |
| 2 | System | View the list of pending invitations with team and tournament information | E1 |
| 3 | Player | Select an invitation and choose "Accept" | |
| 4 | System | Verify that the player does not belong to another team in the same tournament | E2 |
| 5 | System | Verify that the team has not reached the maximum of 12 players | E3 |
| 6 | System | Add the player to the team, deactivate their availability, and display a confirmation | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are no pending invitations, display a message indicating that there are no invitations | End of flow |
| E2 | System | If the player already belongs to a team in that tournament, display an error message and do not allow acceptance | Return to 2 |
| E3 | System | If the team already has 12 players, display a message indicating that the team is full | Return to 2 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-020 | A player may only belong to one team per tournament |
| RN-021 | Upon accepting an invitation, the player's availability is automatically deactivated |
| RN-022 | Upon accepting an invitation, all other invitations for the same tournament are automatically declined |
| RN-023 | A team cannot have more than 12 players |

---

## RF-009 — Create Team

| Field | Details |
|-------|---------|
| **Code** | RF-009 |
| **Name** | Create Team |
| **Description** | The system must allow a user with the Captain role to create a team by specifying the team name and uniform colors |
| **How it works** | The captain accesses the teams module, selects "Create Team," enters the name and uniform colors, and confirms the creation |
| **Primary user** | Captain |
| **Prerequisites** | 1) The user must be logged in with the Captain role (assigned by the tournament organizer); 2) The captain must not have created any other team |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Team name | Team ID Name | Text | Maximum 50 characters. No other team may have the same name | YES |
| Primary uniform color | Primary uniform color | Color picker | — | YES |
| Secondary uniform color | Secondary uniform color | Color picker | Must be different from the primary color | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of successful creation | Text | Displayed upon completion of creation | YES |
| Team created | Team visible in the team list | Registration | The captain is assigned as the creator and first member of the team | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Captain | Go to the teams module and select "Create Team" | |
| 2 | System | Verify that the user has the Captain role | |
| 3 | System | Verify that the captain does not have another team created | |
| 4 | System | Display the team creation form (name and colors) | |
| 5 | Captain | Enter the team name, select the uniform colors, and confirm | |
| 6 | System | Validate the entered data | |
| 7 | System | Create the team, assign the captain as the creator and first member, log the action in the audit trail, and display a confirmation | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the user does not have the Captain role, display an error message | End of flow |
| E2 | System | If the captain already has a team created, display a message indicating that they cannot create another one | End of flow |
| E3 | System | If a team with the same name already exists, display an error message | Return to step 5 |
| E4 | System | If the primary and secondary colors are the same, display an error message | Return to step 5 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-001 | Only a user with the Captain role (assigned by the organizer) can create a team |
| RN-002 | The team must have a minimum of 7 and a maximum of 12 players to register for a tournament |
| RN-003 | A player cannot belong to two teams at the same time |
| RN-004 | A team cannot have more than one player with the same jersey number |
| RN-005 | More than half of the members must be from Systems Engineering, AI, Cybersecurity, or Statistics |
| RN-006 | The captain automatically becomes the first member of the team upon its creation |
| RN-007 | The creation action must be recorded in the audit log |

---

## RF-010 — Manage Team Players

| Field | Details |
|-------|---------|
| **Code** | RF-010 |
| **Name** | Manage Team Players |
| **Description** | The system must allow the captain to manage team members, including accepting or rejecting player requests to join the team and removing players from the team |
| **How it works** | The captain accesses their team management, views pending join requests, accepts or rejects them, and can remove existing players from the team |
| **Primary User** | Captain |
| **Prerequisites** | 1) The user must be authenticated with the Captain role; 2) The captain must have a team created |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Link Request | Request sent by a player to join the team | Selection | Only pending requests are displayed | YES |
| Action on request | Captain's decision on the request | Button (Accept/Reject) | — | YES |
| Player to be removed | Player to be removed from the team | Selection | Only current team players are displayed (except the captain) | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of action taken | Text | Indicates whether the request was accepted, rejected, or a player was removed | YES |
| Updated member list | Updated list of team players | List | Reflects the changes made | YES |
| Updated request status | The application status changes to Accepted or Rejected | Record | Upon acceptance, the player is added to the team. Upon rejection, the request is discarded | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Captain | Access your team management | |
| 2 | System | View the list of pending team invitation requests and current team members | |
| 3 | Captain | Select a request and choose "Accept" | |
| 4 | System | Verify that the team has not reached the maximum of 12 players | |
| 5 | System | Verify that the player does not belong to another team | |
| 6 | System | Verify that the player's jersey number is not duplicated on the team | |
| 7 | System | Add the player to the team, log the action in the audit trail, and display a confirmation | |
| 8 | System | Update the team member list | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are no pending requests, display a message indicating that there are no requests to process | End of flow |
| E2 | System | If the team already has 12 players, display a message indicating that the team is full | Return to step 2 |
| E3 | System | If the player already belongs to another team, display an error message | Return to step 2 |
| E4 | System | If the player's jersey number already exists on the team, display a warning message | Return to step 2 |
| E5 | Captain | If the captain selects "Reject" for the request, the system discards the request and displays a confirmation | Return to step 2 |
| E6 | System | If the captain selects to remove a player and there is an Active or In Progress tournament, display an error message indicating that the player cannot be removed | Return to step 2 |
| E7 | System | If there is no Active or In Progress tournament, remove the player from the team, log the action in the audit log, and display a confirmation | Return to step 2 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-008 | Only the captain can accept or reject requests to join their team |
| RN-009 | A team cannot have more than 12 players |
| RN-010 | A player cannot belong to two teams simultaneously |
| RN-011 | A player cannot be removed from the team if there is an Active or In-Progress tournament |
| RN-012 | A team cannot have more than one player with the same jersey number |
| RN-013 | A player can only submit one team request at a time (validated by user support - Ferrari) |
| RN-014 | All player management actions must be recorded in the audit log |

---

## RF-011 — Update Equipment

| Field | Details |
|-------|---------|
| **Code** | RF-011 |
| **Name** | Update Equipment |
| **Description** | The system must allow the captain to update their team's name, provided the team is not registered in a tournament with an Active or In Progress status |
| **How it works** | The captain accesses their team management, selects "Update Team," changes the name, and confirms the changes |
| **Primary user** | Captain |
| **Prerequisites** | 1) The user must be logged in with the Captain role; 2) The captain must have a team created; 3) The team must not be registered in a tournament with an Active or In Progress status |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Team Name | New name for the team | Text | Maximum 50 characters. No other team may have the same name | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Update successful | Text | Displayed upon completion of the update | YES |
| Team updated | The team name has been updated in the system | Registration | The change is visible to all users | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Captain | Go to your team management page and select "Update Team" | |
| 2 | System | Verify that the team is not registered in an Active or In Progress tournament | |
| 3 | System | Display the form with the team's current name | |
| 4 | System | The captain sees the current name | |
| 5 | Captain | Change the team name and confirm the changes | |
| 6 | System | Validate the entered data | |
| 7 | System | Updates the team name, logs the action in the audit trail, and displays a confirmation | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the team is registered in a tournament with a status of Active or In Progress, display an error message indicating that it cannot be updated | End of flow |
| E2 | System | If another team with the same name already exists, an error message is displayed | Return to step 5 |
| E3 | System | If the name is empty or exceeds 50 characters, an error message is displayed | Return to step 5 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-015 | You can only update the team name if it is not registered in an Active or In Progress tournament |
| RN-016 | There cannot be two teams with the same name |
| RN-017 | The update must be recorded in the audit log |

---

## RF-012 — Equipment Audit

| Field | Details |
|-------|---------|
| **Code** | RF-012 |
| **Name** | Equipment Audit |
| **Description** | The system must automatically record the creation, update, and deactivation of equipment for traceability and audit purposes |
| **How it works** | The system automatically records every action performed on the equipment. The administrator can view the audit log to review the history of actions |
| **Primary Actor** | System (automatic), Administrator (query) |
| **Prerequisites** | 1) The audit log is automatically generated whenever an action is performed on devices; 2) To view the log, the user must be authenticated with the Administrator role |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Action performed | Type of action performed on the device | Text | Values: Creation, Update, Deactivation, Add Player, Remove Player | YES |
| User who performed | User who performed the action | Text (automatic) | Automatically captured from the authenticated user | YES |
| Date and time | Time the action was performed | Date/Time (automatic) | Captured automatically from the system | YES |
| Action details | Description of the change made | Text (automatic) | Includes data before and after the change | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Audit log | List of all actions recorded on equipment | List/Table | Shows action, user, date/time, and details. Sorted from newest to oldest | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | System | Detects that an action was performed on a team (creation, update, deactivation, or player management) | |
| 2 | System | Automatically records the action with user, date/time, and details in the audit log | |
| 3 | Administrator | Go to the audit module and select "Team Audit" | |
| 4 | System | Displays the list of recorded actions | |
| 5 | System | Allows filtering by team, action type, user, or date range | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are no audit logs, display a message indicating that no actions have been logged | End of flow |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-018 | All actions related to the creation, updating, and deactivation of teams must be recorded automatically |
| RN-019 | Any action involving the addition or removal of players from the team must be automatically recorded |
| RN-020 | Audit logs cannot be modified or deleted |
| RN-021 | Only the administrator can view the audit log |

---

## RF-013 — Verify Payment

| Field | Details |
|-------|---------|
| **Code** | RF-013 |
| **Name** | Verify Payment |
| **Description** | The system must allow the organizer to review the payment receipts uploaded by the captains and approve or reject the team's registration |
| **How it works** | The organizer accesses the payments module, views the pending receipts, reviews each one, and approves or rejects them |
| **Primary user** | Organizer |
| **Prerequisites** | 1) The user must be logged in with the Organizer role; 2) There must be at least one receipt with a status of "Pending" |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Selected Receipt | Payment receipt to be reviewed | Selection | Only receipts with the status "Pending" are displayed | YES |
| Decision | Organizer's action on the receipt | Button (Approve/Reject) | — | YES |
| Reason for rejection | Reason for rejecting the receipt | Text | Required only if the decision is "Reject" | COND |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of action taken | Text | Indicates whether the payment was approved or rejected | YES |
| Updated status | The payment status changes depending on the decision | Record | Approved: the team is registered. Rejected: the captain can upload a new receipt | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Organizer | Access the payment verification module | |
| 2 | System | Display the list of receipts with the "Pending" status and team information | E1 |
| 3 | Organizer | Select a receipt to review it | |
| 4 | System | Displays the receipt and the associated equipment data | |
| 5 | Organizer | Review the receipt and select "Approve" | |
| 6 | System | Change the payment status to "Approved," register the team in the tournament, and display a confirmation | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are no pending receipts, display a message indicating that there are no payments to review | End of flow |
| E2 | System | Changes the payment status to "Rejected," notifies the captain with the reason, and displays a confirmation. The captain can upload a new receipt | Return to 2 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-041 | Only the organizer can approve or reject payment receipts |
| RN-042 | Upon approving the payment, the team is officially registered in the tournament (status "Approved") |
| RN-043 | When rejecting a payment, a reason must be provided |
| RN-044 | A team with a rejected payment can upload a new receipt |
| RN-045 | Only teams with approved payments may participate in the tournament |

---

## RF-014 — Set Up Tournament

| Field | Details |
|-------|---------|
| **Code** | RF-014 |
| **Name** | Set Up Tournament |
| **Description** | The system must allow the organizer to define the detailed configuration of the tournament, including rules, important dates, schedules, courts, and penalties |
| **How it works** | The organizer accesses the created tournament, selects "Configure Tournament," and fills out the various configuration sections |
| **Primary user** | Organizer |
| **Prerequisites** | 1) The user must be logged in with the Organizer role; 2) The tournament must exist and be in "Draft" or "Active" status |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Regulations | Official tournament rules | Long text | — | YES |
| Important Dates | Key tournament dates (opening ceremony, registration deadline, stages, etc.) | Multiple date selector | Each date must have a name and be within the tournament range | YES |
| Registration Deadline | Deadline for team registration | Date selector | Must be before the tournament start date | YES |
| Match Schedules | Available time slots for scheduling matches | Time selector | — | YES |
| Courts | Fields available for matches | Text | Must include at least one court | YES |
| Penalties | Definition of applicable penalties (cards, ejections, etc.) | Long text | — | NO |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of saved settings | Text | Displayed when saving settings | YES |
| Settings updated | Tournament information updated with settings | Save | Visible to all users viewing the tournament | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Organizer | Go to the tournament and select "Configure Tournament" | |
| 2 | System | Display the configuration form with the available sections | |
| 3 | Organizer | Fill out the tournament rules | |
| 4 | Organizer | Set the important dates and the registration deadline | |
| 5 | Organizer | Enter the match schedules and available courts | |
| 6 | Organizer | Set penalties (optional) and confirm the settings | |
| 7 | System | Verify that all required fields are complete and comply with the rules | E1, E2, E3 |
| 8 | System | Save the settings and display a confirmation message | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the registration deadline is after the tournament start date, display an error message | Return to 4 |
| E2 | System | If any important date is outside the tournament range, display an error message | Return to 4 |
| E3 | System | If at least one court is not registered, display an error message | Return to 5 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-046 | Only the organizer can configure the tournament |
| RN-047 | Settings can only be modified while the tournament is in "Draft" or "Active" status |
| RN-048 | Registration must close before the tournament start date |
| RN-049 | Important dates must fall within the tournament's date range |
| RN-050 | The rules and penalties are visible to all tournament users |

---

## RF-016 — View Rival Lineup

| Field | Details |
|-------|---------|
| **Code** | RF-016 |
| **Name** | View Rival Lineup |
| **Description** | The system must allow captains and players to consult the rival team's lineup for a scheduled match |
| **How it works** | The user accesses the scheduled match and selects "View rival lineup" to visualize the opposing team's formation |
| **Main Actor** | Captain, Player |
| **Prerequisites** | 1) The user must be authenticated; 2) There must be a scheduled match in which their team participates; 3) The rival team must have defined its lineup |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Match | Match for which the rival lineup is to be consulted | Selection | Only scheduled matches for the user's team are displayed | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Rival Starters | List of the 7 starting players of the rival team | List | Shows name, jersey number, and position | YES |
| Rival Reserves | List of substitute players of the rival team | List | Shows name and jersey number | YES |
| Rival Formation | — | Text | Example: 1-2-3-1 | YES |
| Pitch View | Tactical disposition of the rival team | Graphic | Visual representation of the rival formation with players positioned | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | User | Accesses the matches module and selects a scheduled match | |
| 2 | System | Displays match information with the "View rival lineup" option | |
| 3 | User | Selects "View rival lineup" | |
| 4 | System | Displays the complete rival team lineup (starters, reserves, formation, and pitch view) | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the rival team has not yet defined its lineup, displays a message indicating that the lineup is not available | End of flow |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-056 | The rival lineup is only visible once the rival captain has defined it |
| RN-057 | Any player or captain of the team can consult the rival lineup |

---

## RF-017 — Record Match Result

| Field | Details |
|-------|---------|
| **Code** | RF-017 |
| **Name** | Record Match Result |
| **Description** | The system must allow the organizer to record the results of a match, including the score, goal scorers, yellow cards, and red cards |
| **How it works** | The organizer accesses the completed match, selects "Record result," enters the score, goal scorers, and cards, and confirms the entry |
| **Main Actor** | Organizer |
| **Prerequisites** | 1) The user must be authenticated with the Organizer role; 2) There must be a scheduled match that has already been played; 3) The tournament must be in "In Progress" status; 4) The match must not have a previously recorded result |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Match | Match for which the result is recorded | Selection | Only matches that ended in a draw are displayed | YES |
| Home team goals | Number of goals scored by the home team | Integer | Must be greater than or equal to 0 | YES |
| Away team goals | Number of goals scored by the visiting team | Integer | Must be greater than or equal to 0 | YES |
| Goal scorers | Players who scored goals | Multiple selection | Only starting players and substitutes who participated are displayed. You can score more than one goal per player | NO |
| Yellow cards | Players who received a yellow card | Multiple selection | Only players who participated in the match are shown | NO |
| Red cards | Players who received a red card | Multiple selection | Only players who participated in the match are shown | NO |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of successful registration | Text | Displayed upon completing registration | YES |
| Result recorded | Result visible in the match history | Registration | Shows the score, goal scorers, and cards | YES |
| Updated standings | The standings are automatically recalculated | Table | Points, goals scored, goals allowed, and goal difference are updated | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Organizer | Go to the Matches module and select a completed match | |
| 2 | System | View the match information and click "Record Result" | |
| 3 | Organizer | Select "Record Result" | |
| 4 | System | Display the entry form with the participating teams and players | |
| 5 | Organizer | Enter the score (home and away goals) | |
| 6 | Organizer | Select the match scorers (optional) | |
| 7 | Organizer | Select yellow and red cards (optional) | |
| 8 | System | Verify that the score matches the recorded goal scorers | |
| 9 | System | Record the result, update the standings, and display a confirmation | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the number of recorded scorers does not match the score, display a warning message (allows you to continue) | Go back to step 6 |
| E2 | System | If the counter has negative values, display an error message | Go back to step 5 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-058 | Only the organizer can enter match results |
| RN-059 | A match can have only one result entered |
| RN-060 | When a result is entered, the standings are automatically updated |
| RN-061 | A win is worth 3 points, a draw 1 point, and a loss 0 points |
| RN-062 | Red cards may result in penalties according to the tournament rules |

---

## RF-018 — View Matches (Referee)

| Field | Details |
|-------|---------|
| **Code** | RF-018 |
| **Name** | View Matches (Referee) |
| **Description** | The system must allow the referee to view information about the matches they have been assigned to referee |
| **How it works** | The referee accesses the matches module and views the list of assigned matches with their detailed information |
| **Primary user** | Referee |
| **Prerequisites** | 1) The user must be logged in with the Referee role; 2) There must be at least one match assigned to the referee; 3) The tournament must be in "In Progress" status |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Selected match | Match for which you wish to view information | Selection | Only matches assigned to the referee are displayed | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Date and time | Scheduled date and time of the match | Date/Time | — | YES |
| Field | Venue where the match will take place | Text | — | YES |
| Home team | Name of the home team | Text | — | YES |
| Visiting team | Visiting team name | Text | — | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Referee | Access the assigned matches module | |
| 2 | System | Display the list of matches assigned to the referee, including date, time, field, and teams | |
| 3 | Referee | Select a match to view the details | |
| 4 | System | Displays complete information about the selected match | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If the referee has no matches assigned, a message is displayed indicating that there are no matches to referee | End of flow |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-063 | The referee can only view the matches that have been assigned to them |
| RN-064 | Match information includes the date, time, field, and the two teams playing the match |

---

## RF-019 — See Position Chart

| Field | Details |
|-------|---------|
| **Code** | RF-019 |
| **Name** | See Position Chart |
| **Description** | The system must allow any authenticated user to view the tournament standings, which are automatically calculated based on the recorded results |
| **How it works** | The user accesses the tournament and selects "Standings" to view the updated rankings of all teams |
| **Primary actor** | All actors |
| **Prerequisites** | 1) The user must be logged in; 2) There must be a tournament with a status of "In Progress" or "Completed"; 3) There must be at least one match with a recorded result |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Tournament | Tournament for which you wish to view the standings | Selection | Only tournaments with the status "In Progress" or "Completed" are displayed | YES |

### Output Data

| Name | Description | Field Type | Required |
|------|-------------|------------|----------|
| Team | Team name | Text | YES |
| Games played | Total matches played | Numeric | YES |
| Games won | Total wins | Numeric | YES |
| Ties | Total matches tied | Numeric | YES |
| Games lost | Total matches lost | Numeric | YES |
| Goals scored | Total goals scored | Numeric | YES |
| Goals against | Total goals conceded | Numeric | YES |
| Goal difference | Goals scored minus goals conceded | Numeric | YES |
| Points | Points earned | Numeric | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | User | Go to the tournament and select "Standings" | |
| 2 | System | Display the list of available tournaments | |
| 3 | User | Select a tournament | |
| 4 | System | Calculate and display the standings sorted by points, goal difference, and goals scored | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are no tournaments with a status of "In Progress" or "Completed," display a message indicating that no table is available | End of stream |
| E2 | System | If there are no matches with recorded results, display the table with all values set to zero | — |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-065 | The standings are automatically recalculated every time a result is recorded |
| RN-066 | The tiebreaker criteria are: 1) Points, 2) Goal difference, 3) Goals scored |
| RN-067 | A win earns 3 points, a draw 1 point, and a loss 0 points |
| RN-068 | All authenticated users can view the standings |

---

## RF-020 — Generate Knockout Brackets

| Field | Details |
|-------|---------|
| **Code** | RF-020 |
| **Name** | Generate Knockout Brackets |
| **Description** | The system must automatically generate the tournament bracket, including randomly assigned opening matches, quarterfinals, semifinals, and the final |
| **How it works** | The organizer accesses the ongoing tournament and selects "Generate Knockout Brackets." The system automatically generates the matchups at random |
| **Primary user** | Organizer |
| **Prerequisites** | 1) The user must be logged in with the Organizer role; 2) The tournament must be in "In Progress" status; 3) The group stage must be complete (all group matches must have a result); 4) No knockout brackets may have been previously generated for the tournament |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Tournament | Tournament for which the brackets are generated | Selection | Only tournaments with the status "In progress" and no generated brackets are displayed | YES |
| Qualified Teams | Teams that advanced to the knockout stage | Automatic | Selected automatically based on the standings | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Confirmation message | Confirmation of successful generation | Text | Displayed once generated | YES |
| Generated keys | Bracket structure with all matchups | Graph/Diagram | Displays the complete tournament bracket | YES |
| Playoff matches | Scheduled matches for the quarterfinals, semifinals, and final | List | Each match has teams, date, and venue to be assigned | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | Organizer | Go to the ongoing tournament and select "Generate knockout brackets" | |
| 2 | System | Verify that all group stage matches have a result | |
| 3 | Organizer | Confirm the generation of brackets | |
| 4 | System | Get the qualified teams based on the standings | |
| 5 | System | Randomly generate the quarterfinal matchups | |
| 6 | System | Create the complete bracket structure (quarterfinals, semifinals, final) | |
| 7 | System | Display the generated brackets and a confirmation message | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are group stage matches with no recorded results, display an error message indicating the pending matches | End of flow |
| E2 | System | If knockout brackets have already been generated for the tournament, display a message indicating that the brackets have already been created | End of flow |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-069 | The matchups for the first round of the knockout stage are generated randomly |
| RN-070 | The brackets include the quarterfinals, semifinals, and final |
| RN-071 | Brackets can only be generated once per tournament |
| RN-072 | Qualified teams are automatically determined by the standings |

---

## RF-021 — Check Knockout Brackets

| Field | Details |
|-------|---------|
| **Code** | RF-021 |
| **Name** | Check Knockout Brackets |
| **Description** | The system must allow any authenticated user to view the tournament bracket, displaying the matchups for the quarterfinals, semifinals, and finals |
| **How it works** | The user accesses the tournament and selects "Knockout Bracket" to view the complete bracket with updated results |
| **Primary actor** | All actors |
| **Prerequisites** | 1) The user must be logged in; 2) There must be a tournament with a status of "In Progress" or "Completed"; 3) The knockout brackets must have been generated previously (RF-020) |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Tournament | Tournament for which you wish to view the knockout brackets | Selection | Only tournaments with generated knockout brackets are displayed | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Bracket | Visual diagram showing all matchups in the tournament | Chart/Diagram | Shows quarterfinals, semifinals, and finals | YES |
| Results by stage | — | Text/Numeric | — | YES |
| Qualified teams | Teams that advanced in each round | Text | The winning teams in each match are highlighted | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | User | Go to the tournament and select "Brackets" | |
| 2 | System | Display the list of tournaments with generated brackets | |
| 3 | User | Select a tournament | |
| 4 | System | Displays the complete bracket with updated matchups and results for each phase | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are no tournaments with generated brackets, display a message indicating that no brackets are available | End of flow |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-073 | All authenticated users can view the playoff brackets |
| RN-074 | The brackets are automatically updated when knockout match results are recorded |
| RN-075 | The winning teams in each matchup are visually highlighted in the bracket |

---

## RF-022 — View Statistics

| Field | Details |
|-------|---------|
| **Code** | RF-022 |
| **Name** | View Statistics |
| **Description** | The system must allow any authenticated user to view tournament statistics, including top scorers, match history, and results by team |
| **How it works** | The user accesses the tournament and selects "Statistics" to view the different available statistical categories |
| **Primary actor** | All actors |
| **Prerequisites** | 1) The user must be logged in; 2) There must be a tournament with a status of "In Progress" or "Completed"; 3) There must be at least one match with a recorded result |

### Input Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Tournament | Tournament for which you wish to view statistics | Selection | Only tournaments with the status "In Progress" or "Completed" are displayed | YES |
| Statistical category | Type of statistics to view | Selector | Options: Top scorers, Match history, Results by team | YES |

### Output Data

| Name | Description | Field Type | Rules / Application | Required |
|------|-------------|------------|---------------------|----------|
| Top scorers | Ranking of players with the most goals scored in the tournament | List/Table | Shows player, team, and number of goals. Sorted from highest to lowest | YES |
| Match history | List of all matches played with their results | List/Table | Shows date, teams, score, goal scorers, and cards | YES |
| Results by team | Summary of results for a specific team in the tournament | List/Table | Displays matches played, won, tied, lost, and team goals | YES |

### Basic Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| 1 | User | Go to the tournament and select "Statistics" | |
| 2 | System | Display the available statistics categories | |
| 3 | User | Select a tournament | |
| 4 | User | Select the statistics category you want to view | |
| 5 | System | Calculates and displays the statistics corresponding to the selected category | |

### Alternative Flow

| Step | Actor | Description | Exceptions |
|------|-------|-------------|------------|
| E1 | System | If there are no tournaments with a status of "In Progress" or "Completed," display a message indicating that no statistics are available | End of flow |
| E2 | System | If there are no matches with recorded results for the selected category, display a message indicating that there is no data yet | Return to step 4 |

### Business Rules

| No. | Description |
|-----|-------------|
| RN-076 | Statistics are calculated automatically based on recorded results |
| RN-077 | The top scorers list is sorted from highest to lowest number of goals |
| RN-078 | The match history is sorted chronologically from most recent to oldest |
| RN-079 | All authenticated users can view the tournament statistics |

---

## Abbreviations

| Abbreviation | Meaning |
|--------------|---------|
| RF | Functional Requirement |
| RNF | Non-Functional Requirement |
| RN / BR | Business Rule |
