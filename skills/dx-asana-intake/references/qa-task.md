# QA task

A QA task is release QA: it verifies an implementation against the agreed acceptance criteria before launch. It is the QA stage of a Delivery Workstream, and it is also the one QA subtask a Simple Request may need. IDs come from [config.json](config.json). Field rules are in [field-rules.md](field-rules.md).

Do not use it for a recurring health check. That is a Simple Request, recurring QA variant ([simple-request.md](simple-request.md)).

## Where it goes

| Parent | What to do |
|---|---|
| A Delivery Workstream | The workstream already has its QA stage. Fill that one in. Add a QA task only to a workstream that has none, as a sibling of the Implement stages. |
| A Simple Request that needs separate QA | Add one QA subtask under it. |
| Either, and a QA task already exists | Reuse it. Search the parent's subtasks and the client's open tasks before creating another. |

## Fields

Client and Task Type are the same as the parent. Team is QA. Priority matches the parent unless the user says otherwise. Assignee and section as in field-rules.md. Name the proposed validator in the description. The assignee confirms the validator and agrees the QA date before handoff, and the validator then takes over the task. Leave Estimated Effort, DX Sprint and the due date empty.

## Title

`[PREFIX] QA [deliverable]`. Example: `[RP] QA the assigned office map on service area pages`.

## Description

```html
<body>
<strong>OUTCOME</strong>
Verify the implementation against the agreed acceptance criteria before release.

<strong>CHANGES</strong>
<ul>
<li>Test the URLs and page scope in the review environment, on the relevant devices and browsers.</li>
<li>Check the approved content or design and the affected functionality, including forms, links, phone CTAs and tracking where relevant.</li>
<li>Check regression risks across the affected template or page scope.</li>
<li>For affected forms or integrations, verify that a test lead reaches the actual destination. A success message or hidden fields alone do not establish a pass.</li>
<li>For failures, record expected versus actual behavior, reproduction steps and evidence.</li>
<li><strong>Proposed validator:</strong> name.</li>
<li><strong>Estimated time:</strong> The validator confirms.</li>
<li>Start when the review URL, the scope and the required implementation handoffs are ready.</li>
</ul>
<strong>REFERENCES</strong>
<ul>
<li><strong>Parent acceptance criteria:</strong> parent task link.</li>
<li><strong>Implementation handoff and review URLs:</strong> links.</li>
<li><strong>Approved version:</strong> link.</li>
</ul>
<strong>DONE WHEN</strong>
<ul>
<li>Each applicable acceptance criterion passes, and the tested URLs, environment and evidence are recorded.</li>
<li>No unresolved release-blocking defects remain.</li>
<li>This same task is reused for retesting. In-scope failures reopen the relevant implementation task. Do not nest Dev fixes under QA. Unrelated findings need a separate scope decision.</li>
<li>While waiting for fixes, the blocker, its owner and a follow-up date are recorded. Testing resumes when a new handoff is ready.</li>
<li>A review environment PASS allows launch. Production verification belongs to the Launch and verify step.</li>
</ul>
</body>
```

For an active production incident, also follow the incident policy (`incident_policy_url` in config.json).

## Dependencies

QA waits for the implementation subtasks, never for the parent, because the parent stays open through QA. If Dev and pSEO each have an Implement stage, QA waits for both. Launch and verify waits for QA. Under a Simple Request there is no implementation subtask, so set no dependency. The executor posts the handoff (review URL, scope, testing notes) as a comment on the QA task instead.

## Build QA

For a QA handoff on a full website build, include the URLs, the approved scope, access, and the lead-delivery checks. Source: `build_template.build_qa_source_url` in config.json.

## Retesting

Reuse the same QA task. If a defect is in scope, comment on the implementation task so it reopens, and record the blocker, owner and follow-up date on the QA task. A separate, unrelated issue becomes its own Bug or Incident ([simple-request.md](simple-request.md)).

## Procedure

1. Choose the template (SKILL.md step 1).
2. Pre-flight check and search for an existing task (SKILL.md steps 2 and 3).
3. Collect missing information (SKILL.md step 4).
4. Draft and confirm (SKILL.md step 5).
5. Create the QA task with the parent set, the DX project and Backlog section, the assignee, Team QA, and the Client, Task Type and Priority options.
6. Set dependencies from the rules above.
7. Verify and report (SKILL.md step 7).

## Existing tasks

Follow the "Existing tasks" section in [simple-request.md](simple-request.md).
