# Delivery Workstream

A Delivery Workstream is one parent task with the stages a coordinated delivery needs, from scope to verified launch. The parent stays open until the final delivery is verified in production. IDs and the stage list come from [config.json](config.json). Field rules are in [field-rules.md](field-rules.md).

## Parent

Title: `[PREFIX] Launch [specific deliverable]`. Example: `[RP] Launch the assigned office map on every service area page`.

Fields: Client, Task Type (the real request type, never "workstream"), Priority, Team. For Team use the team of the coordinating owner. If none is named, use Dev for website deliveries and say so in the report. Assignee and section as in field-rules.md.

```html
<body>
<strong>OUTCOME</strong>
The specific website delivery and why it matters. Two sentences at most.

<strong>CHANGES</strong>
<ul>
<li><strong>Affected pages or URLs:</strong> list.</li>
<li><strong>Included changes:</strong> scope. <strong>Exclusions:</strong> boundaries.</li>
<li><strong>Editing source and delivery constraints:</strong> details.</li>
<li><strong>Coordinating owner:</strong> one person who tracks the final delivery.</li>
<li><strong>Requested deadline:</strong> the date, or None requested. This is a request, not a commitment.</li>
</ul>
<strong>REFERENCES</strong>
<ul>
<li><strong>Client brief or evidence:</strong> link.</li>
<li><strong>Approved content or design:</strong> links.</li>
<li><strong>CRO case or hypothesis:</strong> only if applicable.</li>
<li><strong>Gmail thread ID:</strong> only when the request came from a client email.</li>
</ul>
<strong>DONE WHEN</strong>
<ul>
<li>Observable acceptance criteria.</li>
<li>Required approvals and pre-release QA are recorded.</li>
<li>The delivery is live and the production verifier has verified it, with evidence.</li>
<li>The parent stays open until the complete outcome is verified.</li>
</ul>
</body>
```

Each executor estimates their own stage. Never put the sum of the stages on the parent. Native time rollups already include subtasks.

## Stages

Default: create the parent and every stage in the template, including both client feedback rounds under Design, with the template's dependencies. Do not drop a stage on your own judgment. If a stage looks unnecessary, say so in the draft and let the user decide. If the user drops one, reconnect around it: each task that waited on the dropped stage now waits on whatever that stage waited on.

Read the live template before creating. The stage list in config.json is a dated copy.

1. Search the DX project for the deliverable and keep tasks whose parent is the workstream source template or one of its stages. If the search misses any, read the source template with its subtasks, then each stage.
2. From each template stage, take the wording, the Team and the dependencies. Keep a map from each template stage key to the new task GID as you create them.
3. Name each new stage from its `title` in config.json, replacing [deliverable] with the deliverable.

Stages, in config.json order:

| Key | Stage | Team | Waits for |
|---|---|---|---|
| scope | Confirm scope | Account Management | nothing |
| content | Prepare content | SEO/pSEO | scope |
| design | Design | Design | scope |
| feedback_1 | Client feedback round #1 (child of design) | Design | nothing |
| feedback_2 | Client feedback round #2 (child of design) | Design | nothing |
| approval | Obtain approval | Account Management | content, design |
| implement | Implement | Dev | approval |
| qa | QA (see [qa-task.md](qa-task.md)) | QA | implement |
| launch | Launch and verify | Dev | qa |

Rules for stages:

- Copy the dependencies exactly as the live template has them, whatever this table says. If the template has none for a task, set none.
- Team: use the template's Team. Override only when the user says who will do the work.
- Every stage gets the same Client, Task Type and Priority as the parent, the configured assignee, the Backlog section, and no estimate, due date or Sprint. The feedback rounds get the same.
- The feedback rounds are children of the new Design stage. Create them with parent set to the new Design task's GID.
- Replace placeholders in each stage's wording and delete lines that do not apply. Leave "executor confirms" lines as they are.
- When Dev and pSEO have separate deliverables, create "Implement [deliverable] (Dev)" and "Implement [deliverable] (pSEO)", both waiting on approval, and make QA wait for both. Say in each handoff which one the other waits for. Mention the split in the draft.
- The QA stage is created here, so do not create another. Fill it in following [qa-task.md](qa-task.md).
- Dev corrections never go inside the QA task. In-scope failures reopen the implementation stage.

## Procedure

1. Choose the template (SKILL.md step 1).
2. Pre-flight check and search for an existing task (SKILL.md steps 2 and 3).
3. Collect missing information, including the exact deliverable and who checks the result (SKILL.md step 4).
4. Draft and confirm (SKILL.md step 5). The draft lists every stage with its Team and what it waits for.
5. Create the parent first in the DX project and Backlog, assigned to the configured assignee. Note its GID.
6. Create the stage subtasks in batches of at most 3, in this order: scope, content, design, approval, implement, qa, launch. Each has the parent set, the DX project and Backlog section, the assignee and its fields. After each batch, read the parent's subtasks and confirm there are no duplicates. Record each new GID against its stage key.
7. Create the two feedback rounds with parent set to the new Design task's GID. Read the Design task's subtasks to confirm there are exactly two.
8. Set dependencies in one update call, using the new GIDs that match the template. Then read each stage back and compare with the template.
9. Verify and report (SKILL.md step 7). List every stage with its Team and what it waits for, and say that the dependencies match the template.

## Existing tasks

Follow the "Existing tasks" section in [simple-request.md](simple-request.md).

## Full website builds

Do not use this template for a whole site. Use the Website Build Project Template (`build_template` in config.json). The connector cannot instantiate native templates, so tell the user to create the build from the template in Asana.

When team queues need build tasks, add the same execution and QA tasks to the DX project. Keep their original project, parent and dependencies. Do not copy the whole build into a workstream. On each task added to DX, set Team to the executing team and Task Type to the real request (normally Enhancement / New Request for new build deliverables). The build QA source is `build_qa_source_url` in config.json. A build QA handoff includes the URLs, the approved scope, access, and the lead-delivery checks.

## Example shape

Dev adds a field, pSEO loads data from a spreadsheet into it, QA verifies, then the change goes live. Stages: Confirm scope (skip if the scope is already recorded), Implement (Dev), Implement (pSEO, waits for the Dev field), QA (waits for both), Launch and verify.
