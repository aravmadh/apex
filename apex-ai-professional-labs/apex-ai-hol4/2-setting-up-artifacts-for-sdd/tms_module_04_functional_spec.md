# Talent Acquisition Portal - APEXlang Comparison Functional Specification

> Application Name: Talent Acquisition Portal - APEXlang Comparison
> Document Type: Business Functional Specification
> Version: 1.0

---

## 1. Purpose

The Talent Acquisition Portal is a simple recruiting application for managing job requisitions, candidates, interviews, and offers.

This version is intended to be generated from a functional specification using APEXlang. It should be lightweight, clear, and focused on the core recruiting process.

---

## 2. Users

| User | What They Need |
|---|---|
| Recruiter | Track candidates, schedule interviews, and manage offers. |
| Hiring Manager | Review requisitions, candidates, interview outcomes, and offer status. |
| HR Administrator | Review the full recruiting pipeline and maintain recruiting data. |

---

## 3. Main Pages

| Page | Purpose | Main Table |
|---|---|---|
| Home | Show recruiting summary metrics and charts. | Multiple tables |
| Job Requisitions | Create and manage hiring requests. | `TMS_JOB_REQUISITIONS` |
| Jobs | View job roles and salary ranges. | `TMS_JOBS` |
| Departments | View departments linked to requisitions and jobs. | `TMS_DEPARTMENTS` |
| Candidate Pipeline | Track candidates by requisition and hiring stage. | `TMS_CANDIDATES` |
| Interview Schedule | View and maintain candidate interview stages. | `TMS_INTERVIEW_STAGES` |
| Offers | Create and manage candidate offers. | `TMS_OFFERS` |

---

## 4. Core Recruiting Flow

```text
Hiring need is created
Job requisition is opened
Candidate is added to the requisition
Candidate moves through screening and interview stages
Interview feedback is recorded
Offer is created
Offer is accepted, rejected, expired, or withdrawn
```

---

## 5. Business Rules

### 5.1 Job Requisitions

- A requisition must have a job, department, requested by user, headcount, status, and open date.
- Headcount must be greater than zero.
- Close date cannot be earlier than open date.
- Requisition status must be Draft, Pending Approval, Open, Filled, Cancelled, or Closed.
- Open requisitions should be easy to identify.

### 5.2 Jobs And Departments

- A job must have a title and department.
- Maximum salary should not be less than minimum salary.
- A department must have a name.
- Department names should be unique.

### 5.3 Candidates

- A candidate must belong to one requisition.
- Candidate first name, last name, and email are required.
- The same candidate email should not be duplicated for the same requisition.
- Candidate stage must be Applied, Screening, Interview, Offer, Hired, or Rejected.
- AI score, when entered, must be between 0 and 10.
- Candidates in Offer or Hired stage should be easy to find.

### 5.4 Interviews

- An interview must belong to a candidate and requisition.
- Interview stage name is required.
- Interviewer should reference an employee when available.
- Scheduled interviews should show the scheduled date clearly.
- Interview score, when entered, must be between 0 and 5.
- Interview outcome must be Scheduled, Proceed, Hold, Rejected, No Show, Completed, or Cancelled.
- Feedback notes are internal recruiting information.

### 5.5 Offers

- An offer must belong to a candidate and requisition.
- Offered salary must be greater than zero.
- Offer status must be Draft, Pending Approval, Sent, Accepted, Rejected, Expired, or Withdrawn.
- Accepted offers should be easy to identify.
- Expired and withdrawn offers should remain visible for history.

---

## 6. Home Dashboard

The Home page should be read-only and show a quick recruiting summary.

Summary cards:

- Open requisitions.
- Active candidates.
- Candidates in interview stage.
- Offers sent or pending approval.

Charts:

- Candidates by current stage.
- Requisitions by status.
- Offers by status.
- Candidates by source.

---

## 7. Reporting Needs

The application should provide simple reports for:

- Requisitions by status.
- Candidate pipeline by stage.
- Candidate list by requisition.
- Upcoming interviews.
- Interview outcomes.
- Offers by status.
- Accepted offers.

Reports should allow filtering, sorting, and drill-down to record details.

---

## 8. Security Expectations

- Recruiters can manage requisitions, candidates, interviews, and offers.
- Hiring managers can review requisitions, candidates, interviews, and offers.
- HR administrators can view and maintain all recruiting data.
- Interview feedback and offer salary information should be shown only to authorized users.

---

## 9. Success Criteria

The application is successful when:

- Users can see open requisitions and active candidates.
- Recruiters can add and update candidate records.
- Interviews can be scheduled and reviewed.
- Offers can be created and tracked.
- The Home page gives a useful recruiting snapshot.
- Reports make it easy to understand the hiring pipeline.
