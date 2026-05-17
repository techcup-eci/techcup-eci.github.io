# Software Requirements Specification (SRS)
## Project: TechCup Fútbol — Tournament Management Platform
**Company:** CVDS COMPANY  
**Date of Document:** 03/04/2026  
**Document Format:** Markdown Conversion (.md)

---

## 1. Abbreviations & General Rules
| Abbreviation | Meaning |
| :--- | :--- |
| **RF** | Functional Requirement (Requisito Funcional) |
| **RNF** | Non-Functional Requirement (Requisito No Funcional) |
| **RN** | Business Rule (Regla de Negocio) |

### General Tournament Statuses (RN-002):
The valid statuses for a tournament are: **Draft**, **Active**, **In Progress**, or **Completed**.

---

## 2. Functional Requirements Detailed Specification

### [RF-001] Create Tournament
* **Description:** The system must allow the organizer to create a new tournament by entering its basic information.
* **How it works:** The organizer accesses the tournaments module, selects "Create Tournament," fills out the form with the required information, and confirms its creation.
* **Primary User:** Organizer
* **Prerequisites:** The user must be logged in and have the Organizer role assigned.

#### Input Data
| Name | Description | Field Type | Rules / Application | Required |
| :--- | :--- | :--- | :--- | :--- |
| **Start Date** | Tournament start date | Date | Must be equal to or later than the current date | **YES** |
| **End Date** | Tournament end date | Date | Must be after the start date | **YES** |
| **Number of Teams** | Maximum number of participating teams | Integer | Must be greater than 1 | **YES** |
| **Cost per Team** | Registration fee per team | Integer | Must be greater than or equal to 0 | **YES** |

#### Output Data
| Name | Description | Field Type | Rules / Application | Required |
| :--- | :--- | :--- | :--- | :--- |
| **Confirmation Message** | Tournament created successfully | Text | Displayed upon completion of creation | Yes |
| **Initial Status** | Set to "Draft" | Text | Automatic assignment | Yes |

#### Basic Flow
1. **Organizer:** Goes to the tournament management section.
2. **Organizer:** Selects the "Create Tournament" option.
3. **System:** Displays a form with the required fields.
4. **Organizer:** Fills in the tournament details and confirms.
5. **System:** Verify that all required fields are complete and comply with the rules.
6. **System:** Creates the tournament in **Draft** status and displays a confirmation message.

#### Alternative Flows
* **E1 - Invalid Dates:** If the end date is earlier than or equal to the start date, the system displays an error message and does not allow the user to continue. *(Return to step 4)*
* **E2 - Invalid Team Count:** If the number of teams is less than or equal to 1, the system displays an error message. *(Return to step 4)*
* **E3 - Missing Fields:** If required fields are missing, the system highlights the missing fields and displays an error message. *(Return to step 4)*

#### Business Rules
* **RN-001:** All tournaments are created in "Draft" status by default.
* **RN-002:** The valid statuses for a tournament are: Draft, Active, In Progress, or Completed.

---

### [RF-002] Start Tournament
* **Description:** The system must allow the organizer to change a tournament's status from "Active" to "In Progress," officially starting the tournament.
* **How it works:** The organizer accesses the tournament, selects "Start Tournament," and confirms the action.
* **Primary Actor:** Organizer
* **Prerequisites:** 1. The user must be logged in with the Organizer role.
  2. The tournament must be in **Active** status.
  3. There must be at least 2 teams registered with **Approved** status.

#### Input Data
| Name | Description | Field Type | Rules / Application | Required |
| :--- | :--- | :--- | :--- | :--- |
| **Selected Tournament** | Tournament you want to start | Selection | Only tournaments with "Active" status are displayed | **YES** |
| **Confirmation** | Confirmation of action | Button | The organizer must explicitly confirm | **YES** |

#### Output Data
| Name | Description | Field Type | Rules / Application | Required |
| :--- | :--- | :--- | :--- | :--- |
| **Confirmation Message** | Confirmation of successful start | Text | Displayed upon completion of the action | **YES** |

#### Basic Flow
1. **Organizer:** Goes to the tournaments module and selects a tournament with the status "Active".
2. **System:** Views the tournament information with the "Start Tournament" option available.
3. **Organizer:** Selects "Start Tournament".
4. **System:** Requests explicit confirmation of the action.
5. **Organizer:** Confirms the start of the tournament.
6. **System:** Verifies that the tournament has at least 2 approved teams.
7. **System:** Changes the tournament status to **In Progress** and displays a confirmation message.

#### Alternative Flows
* **E1 - Insufficient Teams:** If the tournament does not have at least 2 approved teams, display an error message indicating that it cannot be started. *(Return to step 2)*

#### Business Rules
* **RN-003:** A tournament can only be started if it is in active status.
* **RN-004:** A minimum of 2 approved teams is required to start a tournament.

---

### [RF-003] End Tournament
* **Description:** The system must allow the organizer to change the status of a tournament from "In Progress" to "Completed" when the tournament ends.
* **How it works:** The organizer accesses the ongoing tournament, selects "End Tournament," and confirms the action.
* **Primary Actor:** Organizer
* **Prerequisites:**
  1. The user must be authenticated with the Organizer role.
  2. The tournament must be in the **In Progress** status.

#### Input Data
| Name | Description | Field Type | Rules / Application | Required |
| :--- | :--- | :--- | :--- | :--- |
| **Selected Tournament** | Tournament to be finalized | Selection | Only tournaments with "In Progress" status are displayed | **YES** |
| **Confirmation** | Confirm action | Button | The organizer must explicitly confirm | **YES** |

#### Output Data
| Name | Description | Field Type | Rules / Application | Required |
| :--- | :--- | :--- | :--- | :--- |
| **Confirmation Message** | Confirmation of successful completion | Text | Displayed upon completion of the action | **YES** |

#### Basic Flow
1. **Organizer:** Goes to the tournaments module and selects a tournament with the status "In Progress".
2. **System:** Displays the tournament information and selects the "End Tournament" option.
3. **Organizer:** Selects "End Tournament".
4. **System:** Requests confirmation of the action.
5. **Organizer:** Confirms the completion of the tournament.
6. **System:** Verifies that all matches in the final phase have been recorded.
7. **System:** Changes the tournament status to **Completed** and displays a confirmation message.

#### Alternative Flows
* **E1 - Pending Matches:** If there are matches with no recorded result, display an error message indicating that there are pending matches. *(Return to step 2)*

#### Business Rules
* **RN-005:** A tournament can only be finalized if it is in "In Progress" status.
* **RN-006:** A tournament cannot be finalized if there are matches without a recorded result.
* **RN-007:** Once finalized, the tournament cannot change status.

---

### [RF-004] Check Tournament
* **Description:** The system must allow any authenticated user to view information about an existing tournament.
* **How it works:** The user accesses the tournaments module, views the list of available tournaments, and selects one to view its details.
* **Primary Actor:** All roles (authenticated users)
* **Prerequisites:** The user must be logged in to the system.

#### Input Data
| Name | Description | Field Type | Rules / Application | Required |
| :--- | :--- | :--- | :--- | :--- |
| **Selected Tournament** | Tournament for which you want to view information | Selection | All existing tournaments are displayed | **YES** |

#### Departure Details / Output Data
| Name | Description | Field Type | Rules / Application | Required |
| :--- | :--- | :--- | :--- | :--- |
| **Start Date** | Tournament start date | Date | - | **YES** |
| **End Date** | Tournament end date | Date | - | **YES** |
| **Number of Teams** | Maximum number of teams | Integer | - | **YES** |
| **Registration Fee** | Cost per unit/registration fee | Decimal | - | **YES** |
| **Status** | Current tournament status | Text | Draft, Active, In Progress, or Completed | **YES** |
| **List of Teams** | Registered teams for the tournament | List | Visible only if the tournament has teams | NO |

#### Basic Flow
1. **User:** Accesses the tournaments module.
2. **System:** Displays the list of available tournaments.
3. **User:** Selects a tournament.
4. **System:** Displays detailed information about the selected tournament.

#### Alternative Flows
* **E1 - No Tournaments Available:** If there are no registered tournaments, display an informational message indicating that no tournaments are available. *(End of flow)*

---

### [RF-009] Create Team
* **Key Business Rules:**
  * **RN-011:** Only a user with the Captain role (assigned by the organizer) can create a team.
  * **RN-012:** The team must have a minimum of 7 and a maximum of 12 players to register for a tournament.

---

### [RF-014] Set Up Tournament
* **Key Business Rules:**
  * **RN-046:** Only the organizer can configure the tournament.
  * **RN-047:** Settings can only be modified while the tournament is in "Draft" or "Active" status.

---

### [RF-017] Record Match Result
* **Description:** Allows the assigned Referee or Organizer to record goals, cards, and match outcomes.
* **Constraint:** Results can only be recorded for matches belonging to a tournament that is currently "In Progress".

---

### [RF-019] View Tournament Bracket & Statistics
* **Description:** The system must allow any authenticated user to view the tournament bracket (displaying matchups for quarterfinals, semifinals, finals) and view specific statistical categories.
* **Flow Summary:**
  1. User accesses the tournament statistics or bracket view.
  2. System displays categories (Top Scorers, Match History, Bracket).
  3. User selects a tournament and a category.
  4. System calculates and displays results automatically.

#### Alternative Flows
* **E1 - No Stats Available:** If there are no tournaments with a status of "In Progress" or "Completed", display a message indicating that no statistics are available. *(End of flow)*
* **E2 - No Match Data:** If there are no matches with recorded results for the selected category, display a message indicating that there is no data yet.

#### Business Rules
* **RN-076:** Statistics are calculated automatically based on recorded results.
* **RN-077:** The top scorers list is sorted from highest to lowest number of goals.
* **RN-078:** The match history is sorted chronologically from most recent to oldest.
* **RN-079:** All authenticated users can view the tournament statistics.

---
### Revision History
* **Prepared by:** DOSW COMPANY
* **Approved by:** CVDS COMPANY
* **Status:** Converted perfectly to Markdown from the original specification.
