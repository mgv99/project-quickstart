# Product Requirements Document: Sales CRM

## 1. Overview

A lightweight CRM application that allows sales reps to manage contacts, track deals through a pipeline, and capture notes -- while giving sales managers visibility into team performance through a dashboard.

## 2. Goals

- Give sales reps a single place to manage prospects and deals.
- Provide pipeline visibility so reps know the status of every deal.
- Preserve call/meeting context through notes attached to contacts and deals.
- Enable managers to monitor team performance at a glance.

## 3. Users & Roles

| Role | Description |
|------|-------------|
| **Sales Rep** | Creates and manages contacts, deals, and notes. Owns their own pipeline. |
| **Sales Manager** | Has read access to all reps' data. Views aggregate dashboard. |

## 4. User Stories & Requirements

### 4.1 Contact Management

> *As a sales rep, I want to create, edit and delete contacts so I can keep my prospect list up to date.*

**Requirements**

| ID | Requirement | Priority |
|----|-------------|----------|
| C-1 | Rep can create a contact with: name (required), email, phone, company, job title | Must |
| C-2 | Rep can edit any field on an existing contact | Must |
| C-3 | Rep can delete a contact (soft-delete; data retained 30 days) | Must |
| C-4 | Deleting a contact with linked deals requires confirmation | Must |
| C-5 | Contact list supports search by name, email, or company | Should |
| C-6 | Contact list supports sorting and pagination | Should |

**Acceptance Criteria**
- A newly created contact appears in the contact list immediately.
- Editing a contact updates all linked deal views.
- Deleting a contact with active deals shows a warning dialog before proceeding.

---

### 4.2 Deal Management

> *As a sales rep, I want to create deals linked to a contact so I can track my pipeline.*

**Requirements**

| ID | Requirement | Priority |
|----|-------------|----------|
| D-1 | Rep can create a deal with: title (required), value, expected close date, linked contact (required) | Must |
| D-2 | Each deal is linked to exactly one contact | Must |
| D-3 | A contact can have multiple deals | Must |
| D-4 | Deal list supports filtering by stage, contact, and date range | Should |

**Acceptance Criteria**
- A deal cannot be created without selecting an existing contact.
- Deals are visible from both the deals list and the linked contact's detail page.

---

### 4.3 Deal Pipeline / Stage Progression

> *As a sales rep, I want to move deals through stages (New, Qualified, Won, Lost) so I can see where each deal stands.*

**Requirements**

| ID | Requirement | Priority |
|----|-------------|----------|
| P-1 | Deals have exactly one stage at any time: **New**, **Qualified**, **Won**, or **Lost** | Must |
| P-2 | Default stage on creation is **New** | Must |
| P-3 | Rep can move a deal to any stage (no enforced linear progression) | Must |
| P-4 | Stage changes are timestamped in an activity log | Should |
| P-5 | Pipeline is viewable as a Kanban board (columns = stages) | Should |
| P-6 | Deals can be moved via drag-and-drop on the Kanban board | Could |

**Acceptance Criteria**
- Changing a deal's stage updates the dashboard totals in real time (or on next refresh).
- The activity log records who changed the stage and when.

---

### 4.4 Notes

> *As a sales rep, I want to add notes to a contact or deal so I don't lose context between calls.*

**Requirements**

| ID | Requirement | Priority |
|----|-------------|----------|
| N-1 | Rep can add a text note to a contact | Must |
| N-2 | Rep can add a text note to a deal | Must |
| N-3 | Notes display in reverse-chronological order | Must |
| N-4 | Notes are timestamped and attributed to the author | Must |
| N-5 | Rep can edit or delete their own notes | Should |

**Acceptance Criteria**
- Notes added to a deal also appear on the linked contact's timeline.
- A note cannot be empty.

---

### 4.5 Manager Dashboard

> *As a sales manager, I want to see a dashboard with total deals by stage so I can track team performance at a glance.*

**Requirements**

| ID | Requirement | Priority |
|----|-------------|----------|
| M-1 | Dashboard shows deal count per stage across all reps | Must |
| M-2 | Dashboard shows total pipeline value per stage | Must |
| M-3 | Dashboard supports filtering by rep | Should |
| M-4 | Dashboard supports filtering by date range | Should |
| M-5 | Data refreshes on page load (no manual refresh needed) | Must |

**Acceptance Criteria**
- Dashboard numbers match the sum of all individual rep pipelines.
- A manager with no direct reports sees an empty dashboard, not an error.

---

## 5. Data Model (Conceptual)

```
Contact
  - id (PK)
  - name
  - email
  - phone
  - company
  - job_title
  - owner_id (FK -> User)
  - created_at
  - updated_at
  - deleted_at (soft delete)

Deal
  - id (PK)
  - title
  - value
  - stage (New | Qualified | Won | Lost)
  - expected_close_date
  - contact_id (FK -> Contact)
  - owner_id (FK -> User)
  - created_at
  - updated_at

Note
  - id (PK)
  - body
  - contact_id (FK -> Contact, nullable)
  - deal_id (FK -> Deal, nullable)
  - author_id (FK -> User)
  - created_at
  - updated_at

User
  - id (PK)
  - name
  - email
  - role (rep | manager)
```

## 6. Out of Scope (v1)

- Email/calendar integration
- Bulk import/export of contacts
- Custom deal stages or fields
- Multi-currency support
- Mobile app (responsive web only)
- Reporting beyond the stage-summary dashboard

## 7. Success Metrics

| Metric | Target |
|--------|--------|
| Contact creation-to-deal time | < 2 minutes |
| Dashboard load time | < 2 seconds |
| Rep adoption (weekly active usage) | > 80% of team within 4 weeks |
| Data accuracy (deals in correct stage) | Spot-check > 95% |

## 8. Open Questions

1. Should deals support multiple contacts (e.g., buyer + champion)?
2. Should managers have write access to rep data, or read-only?
3. Is there a need for role-based row-level security on the database side?
4. Should stage transitions trigger notifications (e.g., deal marked Won)?
