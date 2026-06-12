# QA Manual Testing Workflow

## description: 

Use this skill at the start of every Claude in Chrome thread where QA testing is involved.
Covers two modes: 

1. Exploration — Claude navigates the app to discover features and generate test cases formatted for the test case sheet
2. Test Run — Claude executes already-documented test cases step by step, and produces results formatted for the test steps sheet. 

Overview

Two modes. Confirm with the user which one applies at the start of the thread.

---

Mode 1: Exploration (Discover & Draft Test Cases)

Use when: no test cases exist yet; user wants Claude to explore a module or the full app.

Step 1 — Navigate & Explore

* Ask the user for the app URL and which module (or “full app”) to explore.
* Navigate through every visible feature, page, button, form, and state in that scope.
* Take note of: what each feature does, what inputs it accepts, what outcomes are possible, edge cases visible from the UI (empty states, validations, error messages, limits).

Step 2 — Confirm Feature Inventory

Before writing test cases, present a bulleted feature inventory grouped by module/page. Ask the user to confirm, remove, or add anything before proceeding.

Example:

Module: Organisation
- Create Organisation (form with logo upload, ratings, certifications)
- Edit Organisation
- View Organisation Profile (tabs: Profile, Members, Invitations, Documents, Facilities)
- Member management (invite, add peer, change role, remove)
- Organisation overview stats

Step 3 — Output Test Cases

For each confirmed feature, output a table row per test case in this exact column order, ready to paste into the sheet:

Column 1	Column 2	Column 3	Column 4	Column 5	Column 6	Column 7	Column 8
S. NO	Test Case Status	Test Case Name	Test Case Description	Notes	Owner	Product	Method


Rules:

* S. NO: sequential integer continuing from where the user’s sheet left off (ask if unknown, default to 1)
* Test Case Status: always To be done
* Test Case Name: TC##_Module_Action — see naming rules below
* Test Case Description: one sentence — see description rules below
* Notes: leave blank unless something is ambiguous or worth flagging
* Owner: ask user, default QA Team
* Product: ask user, default to app name observed during exploration
* Method: always Manual

After outputting, ask: “Anything to add, remove, or rename before we move to test steps?”

---

Mode 2: Test Run (Execute Documented Test Cases)

Use when: test cases already exist (pasted from sheet or described by user).

Step 1 — Load Test Cases

Ask the user to paste the test cases from the sheet (S. NO, Test Case Name, Description minimum).

Step 2 — Human-in-the-Loop Execution

For each test case, one at a time:

1. Announce which test case you’re starting.
2. Navigate the app and execute the steps.
3. After each logical step group, pause and report what you observed before moving on.
4. At the end of the test case, present the result rows (see output format below).
5. Ask: “Confirmed? Any corrections before I move to the next one?” — wait for response.

Skipping Test Cases

Skip a test case entirely (do not attempt it) if it requires any of the following:

* File uploads (documents, images, PDFs)
* Email verification or reading inbox content
* OTP or authentication code entry
* Actions that require credentials not provided
* Any operation that cannot be completed through browser navigation alone

When skipping, output one line:

⏭ Skipped: [TC Name] — requires [reason]. Run manually.

Then move to the next test case without waiting for confirmation.

Step 3 — Output Test Steps

For each executed step group, output a table row in this exact column order, ready to paste into the sheet:

Column 1	Column 2	Column 3	Column 4	Column 5	Column 6	Column 7	Column 8	Column 9	Column 10
Test Case	Index	Test Steps	Input	Expected Result	Actual Result	Status	Review Description	Remarks	Jam Link


Column rules:

* Test Case: exact TC name from the test case sheet
* Index: sequential integer per test case, starting at 1
* Test Steps: see step writing rules below
* Input: exact values entered or buttons clicked
* Expected Result: see expected result rules below
* Actual Result: As Expected if matched; otherwise describe the deviation specifically
* Status: Passed, Failed, or To be clarified
* Review Description: see review description rules below; blank if Passed
* Remarks: additional observations, suggestions; blank if none
* Jam Link: paste if user provides one, otherwise blank

Handling Failures

If a step fails:

* Stop that test case.
* Output what you have so far with Status Failed.
* State clearly: what was expected, what happened, at which step.
* Ask: “Do you want to log this and move on, or investigate further?”

---

Naming Rules (Test Case Names)

Format: TC##_Module_Action

* ## is zero-padded number (01, 02, … 23)
* Module: singular, PascalCase (Document, Organization, Review, Expert)
* Action: PascalCase business action, not UI action
* Words separated by underscores

Good: TC01_Document_Upload, TC10_Organization_Create, TC23_Review_Manage_Assigned_Reviewers
Bad: TC01_Documents_ClickUploadButton, TC10_Organisations_Form

---

Description Rules (Test Case Description)

Format: Start with “Verify” or “Check”. Describe the business outcome in one sentence.

* Describe what the user can accomplish, not what they click
* One capability per test case
* No UI actions (no “clicks”, “enters”, “presses”)

Good: Verify Platform Admin can upload a document through the complete workflow.
Good: Verify organization members can be filtered by role and status.
Bad: Verify user clicks Comments, enters text, and presses Save.

---

Step Writing Rules (Test Steps column)

* One action per line, numbered
* Short — verb + target only
* Use actual page names and button labels as they appear in the UI

Good:

1. Navigate to Public Library.
2. Open a document.
3. Click Comments.
4. Add a comment.
5. Click Save.

Bad:

1. Navigate to Public Library and open a document and add a comment and save it.

---

Expected Result Rules

* Describe what should be visible or true after the step
* Include: success/error messages (exact text if visible), redirects, status changes, counts, UI state
* Specific enough for a clear Pass/Fail decision

Good:

* Success toast displayed: "Document Submitted Successfully."
* User redirected to Public Library.
* Document status shown as Submitted.

Bad: It works correctly.

---

Remarks Rules

Use only for: defects, requirement gaps, missing coverage, UX improvements.
Leave blank when Status is Passed with no observations.
Always ask the user to attach a Jam link if reporting a defect.

Good: AI standardization stuck at 48%.
Good: Label should read "Select Changes Type" not "Choose Latest Version".
Good: Add loading indicator during document upload.

---

General Rules

* Never skip human confirmation between test cases in Mode 2 (except skipped cases).
* Never fabricate expected results — derive only from observable UI: labels, messages, validation text. Flag if unsure.
* Exact column order matters — users paste directly into the sheet.
* If the user pastes existing test cases at the start of the thread, infer the mode automatically and proceed without asking.
* If both exploration and test run are needed in one session, complete Mode 1 fully and get sign-off before switching to Mode 2.