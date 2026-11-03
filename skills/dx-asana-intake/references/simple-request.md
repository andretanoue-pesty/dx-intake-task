# Simple Request

A Simple Request is one bounded change or check with one owner. Bugs, incidents and recurring QA are variants. IDs come from [config.json](config.json). Field rules are in [field-rules.md](field-rules.md).

## Title

`[PREFIX] Action + specific deliverable`. Start with a verb. Use the client prefix from `clients`. No priority prefix such as P1 or P2, because Priority has its own field.

Examples: `[EF] Replace the homepage hero image`, `[RP] Fix the Directions map on service area pages`.

## Variants

| Variant | Task Type | Team | Extra |
|---|---|---|---|
| Standard change | Enhancement / New Request or Content Update | The executing team | None |
| Bug | Bug | The executing team | Add the "Bug or incident" lines to CHANGES |
| Incident | Incident | The executing team | Add the "Bug or incident" lines and the "Incident only" line. P1 is Critical. Policy: `incident_policy_url` in config.json |
| Recurring QA | Routine Maintenance | QA | Add the "Recurring QA only" lines. A person must set the recurrence on the real task, because the connector cannot |

An incident task does not start the response by itself. Tell the user to alert the assignee directly as well.

## Description

Copy the structure below. Remove lines that do not apply. Do not copy any USE section from the source template.

```html
<body>
<strong>OUTCOME</strong>
What must change or be checked, and why it matters. Two sentences at most.

<strong>CHANGES</strong>
<ul>
<li><strong>Exact change or check, affected URLs and scope boundaries:</strong> details.</li>
<li><strong>Bug or incident:</strong> expected versus actual behavior, reproduction steps, when it was first noticed, how far it extends, current lead or business impact.</li>
<li><strong>Requested deadline:</strong> the date, or None requested. This is a request, not a delivery commitment.</li>
<li><strong>Estimated time:</strong> The executor confirms.</li>
<li><strong>Validation:</strong> self-check, a named reviewer, or separate QA.</li>
<li><strong>Owner:</strong> the person who should do the work, if known.</li>
<li><strong>Incident only:</strong> severity and rationale, incident owner, technical responder, Account Management contact, next update. Severity is separate from Priority.</li>
<li><strong>Recurring QA only:</strong> validator, cadence, next due date, URL, device and check scope.</li>
</ul>
<strong>REFERENCES</strong>
<ul>
<li><strong>Approved copy, design or evidence:</strong> links.</li>
<li><strong>Existing request or workstream:</strong> link, if any.</li>
<li><strong>Gmail thread ID:</strong> only when the request came from a client email.</li>
</ul>
<strong>DONE WHEN</strong>
<ul>
<li>Observable acceptance criteria.</li>
<li>The validator verifies the agreed delivery in the review environment or production, with evidence.</li>
<li>If separate QA is needed, reuse an existing QA task or add one QA subtask (see [qa-task.md](qa-task.md)). The parent stays open through validation.</li>
<li>P1 only: root cause fixed; the original failure no longer reproduces in production; affected forms, calls and navigation and regression checks pass; a named verifier records evidence and an appropriate stability check; Account Management records client notification. A temporary mitigation alone does not close the incident.</li>
<li>Recurring QA only: dated PASS or FAIL evidence for the scoped checks; test leads reach the actual destination; existing findings are linked or one Bug or Incident is created per new issue, each with an owner; urgent incidents are escalated; the next occurrence is confirmed. Completing the check closes that cycle, not its unresolved fixes.</li>
</ul>
</body>
```

If the request came from a client email, add the client's original message to REFERENCES as a blockquote, with sender, date and subject on the first line. Trim the signature and quoted history. Keep the Gmail thread ID. Some downstream tools search for it to avoid duplicates, so never drop it.

## Procedure

1. Choose the template (SKILL.md step 1). If it is not a Simple Request, stop and use the right reference.
2. Pre-flight check and search for an existing task (SKILL.md steps 2 and 3).
3. Collect missing information (SKILL.md step 4).
4. Draft and confirm (SKILL.md step 5).
5. Create the task with the create capability, using `project.gid`, `project.backlog_section_gid`, `assignee`, and the Client, Team, Task Type and Priority options from config.json.
6. Verify and report (SKILL.md step 7).

## Existing tasks

If the work already exists as a task, do not create another. Update it and add it to DX.

1. Read it, including notes and comments.
2. Add it to the DX project and Backlog section without removing its current projects. It keeps its parent, dependencies and discussion.
3. Set the DX fields in a second call, after it is a member.
4. Rewrite the title and description to the structure above. Keep embedded images and attachments exactly as the read returned them. Keep any Gmail thread ID. Move useful history to a comment and keep the original evidence under REFERENCES.
5. Assignee: if the task is unassigned or work has not started, assign the configured assignee. If someone is already working on it, keep that person, say so in the report and let the assignee decide.
6. Leave existing due dates alone. Mention in the report if a date is past or conflicts with the plan.
7. Do not change fields that belong to other projects.
