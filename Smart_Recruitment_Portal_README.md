# Smart Recruitment Portal

## Project Overview

The **Smart Recruitment Portal** is a Salesforce-based recruitment
management system designed to automate the recruitment lifecycle from
job opening creation and candidate registration to application
screening, interview scheduling, feedback, and final selection.

The project addresses the difficulty and time involved in managing
recruitment activities manually. By centralizing candidate, job opening,
application, interview, and feedback information in Salesforce, the
system provides an organized and automated recruitment process.

> **Platform:** Salesforce\
> **Project Type:** Recruitment Management / CRM Automation\
> **Core Technology:** Salesforce Custom Objects, Lookup Relationships,
> Record-Triggered Flows, Email Automation

------------------------------------------------------------------------

## Problem Statement

Managing recruitment manually can be time-consuming and difficult
because candidate information, applications, interviews, feedback, and
hiring decisions may need to be handled separately.

The Smart Recruitment Portal automates these activities using
Salesforce. It maintains recruitment data in a centralized platform and
uses automation to update candidate/application statuses and communicate
important recruitment events through email.

------------------------------------------------------------------------

## Project Objectives

The main objectives of the Smart Recruitment Portal are:

-   Centralize recruitment information in Salesforce.
-   Manage job openings and vacancies.
-   Maintain candidate personal and professional information.
-   Track candidate applications.
-   Automate application submission processing.
-   Automate candidate screening using a scoring mechanism.
-   Schedule interviews for shortlisted candidates.
-   Automatically populate candidate information during interview
    scheduling.
-   Capture interviewer feedback and ratings.
-   Automate final candidate selection.
-   Send email notifications at important stages.
-   Reduce manual status updates and follow-ups.
-   Maintain a record of recruitment decisions and communication.

------------------------------------------------------------------------

# Recruitment Lifecycle

The recruitment process is organized into **8 key steps**:

### 1. Job Opening Creation

The recruiter creates a job opening and sets its status as **Open**.

The job opening contains information such as:

-   Job Title
-   Department
-   Required Skills
-   Experience Required
-   Number of Vacancies
-   Job Description

### 2. Candidate Registration

The candidate submits personal and professional information such as:

-   Name
-   Email
-   Phone Number
-   Education
-   Experience
-   Skills
-   Resume

### 3. Application Submission

The candidate applies for an available job opening.

An Application record is created and the candidate's status is updated
to **Applied** through automation.

### 4. Application Screening

The recruiter reviews the candidate's profile.

The screening automation evaluates:

-   CGPA
-   Qualification
-   Experience

A total score out of 100 is calculated.

The candidate is categorized as:

  Total Score   Result
  ------------- --------------
  70 or above   Shortlisted
  40--69        Under Review
  Below 40      Rejected

### 5. Interview Scheduling

Shortlisted candidates proceed to the interview stage.

An Interview record is created with interview-related information, and
the candidate is notified through email.

### 6. Interview Conducted

The interview is conducted and the Interview record is updated with the
relevant interview status/details.

### 7. Feedback Submission

After the interview, the interviewer/HR submits feedback containing
ratings and comments.

The Feedback record is associated with the Candidate, Interview, and
Application.

### 8. Final Selection

The final decision is based on the feedback.

The supported outcomes are:

-   **Selected → Hired**
-   **Rejected → Rejected**
-   **Hold → Under Review**

------------------------------------------------------------------------

# System Architecture

The Smart Recruitment Portal uses Salesforce as the centralized platform
for the recruitment lifecycle.

The major components are:

``` text
                    SMART RECRUITMENT PORTAL
                              |
                         SALESFORCE
                              |
       -------------------------------------------------
       |             |             |        |          |
   Candidate     Job Opening   Application Interview Feedback
       |             |             |        |          |
       |             |-------------|        |----------|
       |--------------------------|---------|
                              |
                         AUTOMATION
                              |
          -----------------------------------------
          |              |             |          |
      Application     Screening    Interview    Final
      Submission      Automation   Scheduling   Selection
          |              |             |          |
          -----------------------------------------
                              |
                     Email Notifications
                              |
                         Candidate
```

The architecture centralizes candidate data, job openings, applications,
interviews, and feedback within Salesforce.

------------------------------------------------------------------------

# Salesforce Data Model

The project uses **5 core custom objects**.

## 1. Candidate

The Candidate object stores candidate personal and professional
information.

### Fields

-   Candidate Name
-   Email
-   Phone Number
-   Education
-   Experience
-   Skills
-   Resume

------------------------------------------------------------------------

## 2. Job Opening

The Job Opening object stores information about available positions.

### Fields

-   Job Title
-   Department
-   Required Skills
-   Experience Required
-   Number of Vacancies
-   Job Description

------------------------------------------------------------------------

## 3. Application

The Application object represents a candidate's application for a
particular job opening.

### Fields

-   Candidate --- Lookup
-   Job Opening --- Lookup
-   Application Date

The Application connects a Candidate with a Job Opening.

------------------------------------------------------------------------

## 4. Interview

The Interview object stores interview scheduling and interview-related
information.

### Fields

-   Candidate --- Lookup
-   Application --- Lookup
-   Interview Date
-   Interview Time
-   Interviewer
-   Interview Status
-   Meeting Link

The Interview record is associated with the candidate and the
corresponding application.

------------------------------------------------------------------------

## 5. Feedback

The Feedback object stores interviewer/HR feedback after an interview.

### Fields

-   Candidate --- Lookup
-   Interview --- Lookup
-   Application --- Lookup
-   Rating
-   Comments
-   Overall Rating
-   Final Decision

------------------------------------------------------------------------

# Object Relationships

The five custom objects are connected using **Lookup Relationships**.

``` text
                    Candidate
                   /    |     \
                  /     |      \
                 v      v       v
          Application  Interview Feedback
               |
               v
          Job Opening
```

### Relationship Structure

-   **Candidate → Application:** Candidate is associated with
    applications.
-   **Job Opening → Application:** A job opening is associated with
    applications.
-   **Application → Interview:** The application is associated with
    interview records.
-   **Candidate → Interview:** Interview records reference the
    candidate.
-   **Candidate → Feedback:** Feedback records reference the candidate.
-   **Interview → Feedback:** Feedback records reference the interview.
-   **Application → Feedback:** Feedback records reference the
    application.

The Application connects the Candidate to the Job Opening and is also
used in the interview and feedback process.

------------------------------------------------------------------------

# Salesforce Automation

The project contains **4 automated flows**.

  -----------------------------------------------------------------------
  Flow              Purpose           Trigger           Main Outcome
  ----------------- ----------------- ----------------- -----------------
  Flow 1            Application       Application       Candidate status
                    Submission        created           updated to
                                                        Applied +
                                                        confirmation
                                                        email

  Flow 2            Application       Application       Candidate
                    Screening         created/updated   screened and
                                                        application
                                                        status updated

  Flow 3            Interview         Interview created Interview
                    Scheduling                          scheduled +
                                                        candidate
                                                        notified

  Flow 4            Final Selection   Feedback          Candidate
                                      submitted         status/final
                                                        decision
                                                        updated +
                                                        communication
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# Flow 1 --- Application Submission Flow

## Objective

Automate the application submission process when a candidate applies for
a job.

## Flow Configuration

**Flow Type:** Record-Triggered Flow\
**Object:** Application\
**Trigger:** When an Application record is created

## Process

``` text
Candidate applies
       |
       v
Application record created
       |
       v
Record-Triggered Flow starts
       |
       v
Update Candidate Status
       |
       v
Status = Applied
       |
       v
Send confirmation email
       |
       v
Process completed
```

## Key Components

### Trigger

The flow starts when a new Application record is created.

### Update Candidate

The related Candidate record is updated and the candidate status is set
to:

**Applied**

### Email Action

A confirmation email is sent to the candidate containing
application-related information.

### Tracking

The process maintains records of application submissions and status
updates.

## Benefits

-   Eliminates manual status updates.
-   Provides consistent communication.
-   Provides real-time application tracking.
-   Reduces human error.

------------------------------------------------------------------------

# Flow 2 --- Application Screening Flow

## Objective

The Application Screening Flow automates candidate evaluation by
calculating a total score based on the candidate's qualifications.

The flow evaluates:

-   CGPA
-   Qualification
-   Experience

## Flow Configuration

**Flow Type:** Record-Triggered Flow\
**Object:** Application\
**Trigger:** Application record creation/update as configured for the
screening process

## Screening Process

``` text
Application Created/Updated
          |
          v
Retrieve Candidate
          |
          v
Retrieve Job Opening
          |
          v
Evaluate CGPA
       0–40
          |
          v
Evaluate Qualification
       0–30
          |
          v
Evaluate Experience
       0–30
          |
          v
Calculate Total Score
       /     |      \
      /      |       \
   >=70    40–69     <40
     |        |        |
     v        v        v
Shortlisted Under     Rejected
          Review
```

## Scoring Structure

  Evaluation Area     Maximum Score
  ----------------- ---------------
  CGPA                           40
  Qualification                  30
  Experience                     30
  **Total**                 **100**

## Screening Decision

### Score ≥ 70

Candidate is **Shortlisted**.

### Score 40--69

Candidate is placed **Under Review**.

### Score \< 40

Candidate is **Rejected**.

## Actions Performed

After calculating the total score:

1.  Application status is updated.
2.  Candidate status is updated.
3.  Candidate is categorized as Shortlisted, Under Review, or Rejected.
4.  An appropriate email notification is sent.
5.  Screening information is maintained for future reference.

## Benefits

-   Automates candidate screening.
-   Provides consistent evaluation.
-   Automatically updates candidate/application status.
-   Sends timely candidate notifications.
-   Reduces manual follow-ups.
-   Reduces human error.
-   Maintains screening decisions and communication records.
-   Improves recruitment efficiency.

------------------------------------------------------------------------

# Flow 3 --- Interview Scheduling Flow

## Objective

Automate the interview scheduling process for shortlisted candidates.

When an Interview record is created, the flow handles the
interview-related automation and candidate notification.

## Process

``` text
Shortlisted Candidate
        |
        v
Interview Record Created
        |
        v
Retrieve Related Application
        |
        v
Retrieve Candidate
        |
        v
Populate Candidate Information
        |
        v
Update Interview Details
        |
        v
Send Interview Email
        |
        v
Interview Scheduled
```

## Candidate Information Auto-Population

The interview scheduling flow is used to automatically populate the
candidate name in the Interview object while scheduling the interview.

## Interview Information

The Interview record contains information such as:

-   Candidate
-   Application
-   Interview Date
-   Interview Time
-   Interviewer
-   Interview Status
-   Meeting Link

## Candidate Notification

After interview scheduling, the candidate receives an email containing
the interview-related information.

------------------------------------------------------------------------

# Flow 4 --- Final Selection Flow

## Objective

Automate the final hiring decision after the interview feedback is
submitted.

## Process

``` text
Interview Completed
        |
        v
HR/Interviewer submits Feedback
        |
        v
Feedback Rating Updated
        |
        v
Evaluate Overall Rating
        |
        v
Determine Final Decision
        |
        v
Update Candidate Status
        |
        v
Send Final Notification
```

After the interview is completed, HR provides feedback. The overall
rating is updated based on the feedback, and the final decision is
determined according to the configured rating criteria.

The project documentation shows that when the configured selection
condition is satisfied, the candidate is selected and receives an email
notification.

The final recruitment outcomes are:

-   Selected → Hired
-   Rejected → Rejected
-   Hold → Under Review

------------------------------------------------------------------------

# Email Automation

Email notifications are used throughout the recruitment lifecycle.

The project demonstrates email communication for:

### Application Submission

Candidate receives confirmation that the application has been submitted.

### Application Screening

Candidate receives notification about the screening result.

### Interview Scheduling

Candidate receives interview date, time, interviewer, and
meeting-related information.

### Final Selection

Candidate receives communication about the final recruitment decision.

This provides timely and consistent communication between the
recruitment team and candidates.

------------------------------------------------------------------------

# End-to-End Process

The complete Smart Recruitment Portal process can be represented as:

``` text
                 JOB OPENING
                     |
                     v
              Candidate Registration
                     |
                     v
              Application Submission
                     |
                     v
            Application Submission Flow
                     |
                     v
              Candidate = Applied
                     |
                     v
             Application Screening
                     |
          -------------------------
          |           |           |
          v           v           v
     Shortlisted  Under Review  Rejected
          |
          v
    Interview Scheduling
          |
          v
     Interview Conducted
          |
          v
      Feedback Submitted
          |
          v
     Overall Rating Evaluated
          |
          v
      Final Selection
       /      |       \
      /       |        \
 Selected    Hold    Rejected
    |          |         |
    v          v         v
  Hired   Under Review Rejected
```

------------------------------------------------------------------------

# Sample Business Scenario

Consider a candidate applying for a Salesforce-related job opening.

### Step 1 --- Job Opening

A recruiter creates a job opening with the required skills, experience,
department, number of vacancies, and job description.

### Step 2 --- Candidate Registration

The candidate's personal and professional information is stored in the
Candidate object.

### Step 3 --- Application

The candidate applies for the job. An Application record is created.

The Application Submission Flow automatically updates the candidate's
status to **Applied** and sends a confirmation email.

### Step 4 --- Screening

The Application Screening Flow retrieves the candidate and job opening
information.

The flow evaluates CGPA, qualification, and experience and calculates a
score out of 100.

If the score is 70 or above, the candidate is shortlisted.

### Step 5 --- Interview

An Interview record is created for the shortlisted candidate.

The interview scheduling automation populates the candidate information
and sends the interview details through email.

### Step 6 --- Feedback

After the interview, HR/interviewer submits the candidate's rating and
comments.

### Step 7 --- Final Decision

The overall rating is evaluated and the final decision is recorded.

### Step 8 --- Hiring

If the candidate is selected, the candidate progresses to **Hired** and
receives the final communication.

------------------------------------------------------------------------

# Key Features

-   Salesforce-based recruitment management.
-   Centralized candidate information.
-   Job opening management.
-   Application tracking.
-   Automated application submission.
-   Automated candidate screening.
-   Score-based candidate classification.
-   Interview scheduling automation.
-   Automatic candidate information population.
-   Interview feedback management.
-   Final candidate selection.
-   Automated email notifications.
-   Recruitment status tracking.
-   Reduced manual processing.

------------------------------------------------------------------------

# Benefits

## For Recruiters

-   Reduces repetitive recruitment tasks.
-   Provides centralized candidate and application information.
-   Simplifies application screening.
-   Makes interview scheduling easier.
-   Improves recruitment tracking.

## For Interviewers

-   Provides structured interview and feedback records.
-   Allows ratings and comments to be captured.
-   Supports final selection decisions.

## For Candidates

-   Receives timely application confirmation.
-   Receives screening updates.
-   Receives interview details.
-   Receives final recruitment communication.

## For the Organization

-   Reduces manual work.
-   Reduces human errors.
-   Improves process consistency.
-   Maintains recruitment records.
-   Improves overall recruitment efficiency.

------------------------------------------------------------------------

# Salesforce Components Used

The project is implemented using Salesforce capabilities documented in
the project:

-   Custom Objects
-   Custom Fields
-   Lookup Relationships
-   Record-Triggered Flows
-   Flow-based Record Updates
-   Email Actions / Email Notifications
-   Salesforce Lightning interface

------------------------------------------------------------------------

# Project Modules

## Module 1 --- Data Model

Includes:

-   Candidate
-   Job Opening
-   Application
-   Interview
-   Feedback
-   Fields
-   Lookup relationships

## Module 2 --- Automation Overview

Includes four automated flows:

1.  Application Submission Flow
2.  Application Screening Flow
3.  Interview Scheduling Flow
4.  Final Selection Flow

## Module 3 --- Application Submission

Handles application creation, candidate status update, and confirmation
email.

## Module 4 --- Application Screening

Calculates the candidate score and determines whether the candidate is
shortlisted, under review, or rejected.

## Module 5 --- Interview Scheduling

Creates and manages interview records, automatically populates candidate
information, and sends interview notifications.

## Module 6 --- Final Selection

Uses interview feedback and overall rating to determine the final
recruitment outcome.

------------------------------------------------------------------------

# Project Workflow Summary

  -----------------------------------------------------------------------------
  Stage             Salesforce Record       Automation        Result
  ----------------- ----------------------- ----------------- -----------------
  Job Opening       Job Opening             ---               Opening available

  Registration      Candidate               ---               Candidate
                                                              registered

  Application       Application             Flow 1            Candidate =
                                                              Applied

  Screening         Application             Flow 2            Shortlisted /
                                                              Under Review /
                                                              Rejected

  Interview         Interview               Flow 3            Interview
                                                              scheduled

  Interview         Interview               ---               Interview
  Completion                                                  completed

  Feedback          Feedback                Flow 4            Final decision
                                                              processed

  Hiring            Candidate/Application   Flow 4            Hired / Under
                                                              Review / Rejected
  -----------------------------------------------------------------------------

------------------------------------------------------------------------

# Project Outcome

The Smart Recruitment Portal demonstrates how Salesforce can be used to
manage and automate a complete recruitment lifecycle.

The solution connects candidate information, job openings, applications,
interviews, and feedback through a centralized Salesforce data model.
Record-triggered flows automate status changes, candidate screening,
interview scheduling, and final selection, while email automation keeps
candidates informed throughout the process.

The result is a more structured recruitment process with reduced manual
work, improved communication, and better tracking of candidate progress.

------------------------------------------------------------------------

# Conclusion

The **Smart Recruitment Portal** provides a centralized Salesforce-based
solution for managing recruitment activities from job opening creation
through final candidate selection.

By combining custom objects, lookup relationships, record-triggered
flows, scoring logic, and email notifications, the project demonstrates
practical Salesforce automation for a real-world recruitment scenario.

The project covers the complete recruitment journey:

**Job Opening → Candidate → Application → Screening → Interview →
Feedback → Final Selection → Hiring**
