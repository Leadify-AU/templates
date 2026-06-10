# LeadExec Storage Management and Test Lead Cleanup SOP

**Version:** 1.0
**Last updated:** May 27, 2026
**Owner:** Christian Lara, Data Management Lead
**Co-owner:** Oliver Leonor, Engineering
**Approver:** Audrey Jean-Louis
**Review cadence:** Quarterly, or after any storage-limit incident

---

## 1. Why this process exists

LeadExec is hosted by ClickPoint on a database with a fixed size quota.

On April 7, 2026, Leadify encountered a LeadExec storage issue where new test lead submissions failed with:

"The database 'LeadExec_6679' has reached its size quota."

The short-term fix is to keep the storage upgrade in place, but storage upgrades are not a substitute for good operational hygiene.

This SOP defines how Leadify identifies, reviews, exports/logs, and deletes test leads safely so test records do not keep accumulating in LeadExec.

This first version focuses only on test lead cleanup. Larger items such as automation, external archiving, or API-based deletion are listed as future options and are not part of v1.

## 2. What counts as a test lead

A test lead is any LeadExec record created for development, QA, smoke testing, integration testing, or deploy verification. It was never intended to be contacted as a real customer.

A lead may be treated as a test lead if one or more strong indicators are present:

- Email uses the `@leadifytest.com` domain
- Email starts with `test`, `qa`, or `dev`
- First name is `Test` or starts with `Test`
- Last name contains a known test marker such as `TestPoor`, `TestGood`, `LeadExecCheck`, or a lander/scenario name
- Phone number uses an obvious test pattern such as `0400000001`, `0400000002`, or similar
- The lead was submitted during a known QA/development window
- The lead came from an internal smoke test or curl test

Do not delete a lead if:

- It looks like a real customer
- It has a real-looking email and phone number with no clear test marker
- It has a contact attempt, note, sold/delivered status, or manual status change
- It was delivered to a buyer/client
- There is any doubt about whether it is a test

Rule: If unclear, keep the lead and escalate for review.

## 3. Test lead naming convention going forward

All future LeadExec test leads should follow a consistent naming pattern.

### Email

Use the `@leadifytest.com` domain.

Examples:

- `testpoor@leadifytest.com`
- `testgood@leadifytest.com`
- `qa-assetfinance@leadifytest.com`
- `deploy-check@leadifytest.com`

### First name

Use `Test` as the first name or as a prefix.

Examples:

- `Test`
- `TestPoor`
- `TestGood`
- `TestQA`

### Last name

Use the lander, vertical, or scenario being tested.

Examples:

- `SimpleLoanFinder`
- `AssetFinanceTDT`
- `ConveyancingFlow`
- `LeadExecCheck`

### Phone

Use an obvious test phone pattern.

Examples:

- `0400000001`
- `0400000002`
- `0400000003`

This convention applies to:

- Engineering curl tests
- QA smoke tests through live forms
- CRM integration tests
- Post-deploy checks
- LeadExec source or campaign validation

## 4. Monthly cleanup schedule

**Frequency:** Once per calendar month
**Suggested timing:** First business day of each month
**Executor:** Christian Lara
**Reviewer:** Oliver Leonor
**Approver:** Audrey Jean-Louis, when required by the approval rules below

The monthly cleanup should not be skipped for more than one month.

If LeadExec storage utilisation becomes a concern before the next scheduled cleanup, Christian or Audrey can request an ad-hoc cleanup review.

## 5. Steps to search/filter possible test leads

[CONFIRM WITH CHRISTIAN]

Proposed starting filter logic:

1. Open LeadExec.
2. Navigate to the lead search or lead management area.
3. Search for leads where email contains `leadifytest.com`.
4. Search for leads where first name contains or starts with `Test`.
5. Search for leads where last name contains known test markers.
6. Apply a date range that excludes the current active testing window where possible.
7. Export the results before deleting anything.

Recommended saved view name:

`Test Lead Cleanup Filter`

Christian to confirm:

- Exact LeadExec navigation path
- Whether filters support OR logic
- Whether email, first name, last name, phone, source, and date can be filtered together
- Whether bulk selection and bulk deletion are available
- Whether export can include Lead ID, created date, source, status, and delivery fields

## 6. Review process before deletion

Before any deletion:

1. Export the filtered results from LeadExec.
2. Open the export in Excel or Google Sheets.
3. Sort by created date, oldest first.
4. Review each row for false positives.
5. Flag any uncertain rows as `Keep?`.
6. Exclude uncertain rows from deletion.
7. Summarise the proposed cleanup in the monthly cleanup log.

Review checks:

- Does the lead clearly use `@leadifytest.com`?
- Does the name clearly indicate a test?
- Does the phone number look like a test number?
- Was it created during a known QA/development window?
- Does it have any buyer delivery, notes, status changes, or real engagement?
- Could it be a real customer?

If there is doubt, do not delete.

## 7. Export / cleanup log before deletion

Before any lead is deleted:

1. Export the full filtered list from LeadExec.
2. Rename the file using this format: `leadexec-test-lead-cleanup-YYYY-MM-DD.csv`
3. Upload the file to the agreed shared folder.

[CONFIRM WITH CHRISTIAN / AUDREY: final Google Drive folder location]

Suggested location:

`Google Drive > Leadify > Engineering > LeadExec Cleanup Logs`

The export should include as many of these fields as LeadExec allows:

- Lead ID
- Created date
- First name
- Last name
- Email
- Phone
- Source
- Campaign
- Status
- Delivery/sold status
- Notes or disposition fields, if available

The CSV is the audit record. Do not delete it after cleanup.

## 8. Approval and escalation rules

### Standard approval path

For small, clearly marked cleanup batches:

- Christian identifies and exports the test leads
- Oliver reviews the export
- Christian deletes only the approved test leads
- Cleanup is logged in the monthly cleanup table

### Audrey approval required

Ask Audrey for approval before deletion if:

- More than 100 leads are proposed for deletion
- Any lead looks uncertain
- More than 5 rows are flagged for review
- Any lead appears to have been contacted, delivered, or sold
- Any LeadExec error appears during export or deletion
- The cleanup is related to a storage warning or quota incident

### Stop and escalate

Do not proceed if:

- A possible real customer is found in the filtered results
- The filter results look unexpectedly large
- LeadExec returns errors during deletion
- The team is unsure whether a lead has been delivered to a buyer
- ClickPoint or LeadExec storage warnings are active

Rule: Deletion should be conservative. Keeping extra test leads is better than deleting a real lead.

## 9. Monthly cleanup log

| Month | Date executed | Filter result count | Reviewed by | Approved by | Deleted count | CSV link | Notes |
|-------|---------------|---------------------|-------------|-------------|---------------|----------|-------|
| May 2026 | | | | | | | SOP created, first cleanup pending Christian confirmation |

## 10. Future improvement options

These are future options only. They are not part of this v1 SOP.

- Automated export pipeline to archive test leads before deletion
- Scheduled storage monitoring alerts
- API-based cleanup if LeadExec supports safe deletion endpoints
- Leadify-API test lead flagging at submission time
- Dashboard/report showing test lead volume per month
- Automated monthly reminder with a cleanup checklist

Any automation or external archiving work requires a separate task and explicit approval from Audrey.

## 11. Open questions for Christian

- What is the exact LeadExec UI path for searching leads?
- Which filters are available for email, name, phone, source, and date?
- Can filters use OR logic?
- Is bulk delete available?
- Is CSV/Excel export available before deletion?
- What fields can the export include?
- Has Christian performed cleanup before?
- Is there a preferred Drive folder for cleanup logs?
- Are there any lead statuses that should never be deleted?

## 12. Acceptance criteria

This SOP is complete when:

- Christian confirms the LeadExec search/filter and export process
- The cleanup log location is confirmed
- The review and approval rules are accepted by Audrey
- The SOP is linked in the Notion task
- Audrey confirms the SOP is ready for use
