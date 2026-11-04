---
name: dx-asana-intake
description: Create or update DX Operations tasks in Asana for website work. Routes a request to a Simple Request (including bug, incident and recurring QA variants), a Delivery Workstream with its stages, or a release QA task, then drafts, confirms and creates it with the right fields, dependencies and verification. Use when someone asks to log, file, intake or update a DX website request, bug, incident, workstream or QA task in Asana.
---

# DX Asana intake

Create or update one task (or one parent with its stages) in the DX Operations project in Asana. Works with any AI assistant that has Asana tools, through capability names rather than one vendor's tool names.

## Before you start

1. Read [config.json](references/config.json). It holds every project, section, assignee, custom field, option, client and template ID. Never guess an ID.
2. Read [field rules](references/field-rules.md). It applies to every task.
3. Read [Asana capabilities](references/asana-capabilities.md) to map the capability names below to the tools this assistant has.
4. Load only the template reference you need (step 1 below).

## Step 1: choose the template

Pick one, then follow only that reference.

| The request is | Use | Reference |
|---|---|---|
| One bounded change or check, one owner, verified by a self-check, one reviewer or one QA pass | Simple Request | [simple-request.md](references/simple-request.md) |
| A bug or an incident, including P1 | Simple Request, incident variant | [simple-request.md](references/simple-request.md) |
| A recurring health check | Simple Request, recurring QA variant | [simple-request.md](references/simple-request.md) |
| Work that passes between teams: scope, then content and/or design, then client approval, then build, QA and launch. Two dependent builds (for example Dev first, then pSEO) also count | Delivery Workstream | [delivery-workstream.md](references/delivery-workstream.md) |
| Release QA for a Simple Request or a Delivery Workstream | QA task | [qa-task.md](references/qa-task.md) |
| A new site, or a major build phase of one | Not covered here. Tell the user to create it from the Website Build Project Template in Asana (see the Delivery Workstream reference, "Full website builds") | [delivery-workstream.md](references/delivery-workstream.md) |
| The work already exists as a task in Asana | Do not create a new task. Update it and add it to DX (see "Existing tasks" in the reference you chose) | Section "Existing tasks" in the chosen reference |

Decide in this order:

1. Search Asana first (step 3). If the task exists, use "Existing tasks".
2. A new site, or a major build phase of one, is a full website build. Do not use these templates.
3. If the work needs sign-off on content or design before anything is built, or one team must hand a piece to another before it can be built or released, use Delivery Workstream.
4. Otherwise use Simple Request.
5. If you cannot choose between 3 and 4, ask: "Does anyone besides the person doing the work need to deliver something first, or approve something, before this can be built or released?" A yes means Delivery Workstream.

Simple Request and Delivery Workstream are formats. They do not replace Task Type. QA is a Team, never a Task Type.

## Step 2: live lookup and pre-flight check

Run once per session, before creating or changing anything. Follow `live_lookup` in [config.json](references/config.json).

1. Read the DX project (`project.gid`) with its custom field settings and enum options, including the section list.
2. For each field in `custom_fields`, find it by its exact `name` (Team, Task Type, Client, Priority, Estimated Effort, Sprint). For each option, find it by its exact name. Use the live GIDs for every write.
3. Find the Backlog section by exact name. Do not use the TEMPLATES section.
4. If a field or option name is missing, stop and tell the user. Do not guess a GID.
5. If a live GID differs from config.json, use the live value and tell the user config.json needs updating.
6. If the live read fails, stop and tell the user. Only if the user explicitly agrees, use the GIDs in config.json, and say in the report that they were not checked live.
7. If the user names a client not in `clients`, check the live Client options before saying it does not exist.

## Step 3: search for an existing task

Search by the main keywords, the client prefix or any Gmail thread ID, open tasks only, limit 10. Search the DX project too. Do not rely on a section filter: it has been ignored. Check each result's project and section membership yourself, and count only tasks in DX. If a match exists, use "Existing tasks" in the chosen reference and do not create anything.

## Step 4: collect what is missing

You need: client, the exact change or check, affected URLs, acceptance criteria, and who checks the result. Ask every missing question in one message.

## Step 5: draft, then confirm

Show a short draft in the chat: title, template, fields, description. For a workstream, list every stage with its Team and what it waits for. Wait for the user to confirm, unless they told you to create it now.

## Step 6: create

Create the task (or parent, then stages) with the capabilities in [asana-capabilities.md](references/asana-capabilities.md). Every new task gets:

- Assignee and section from `assignee` and `project.backlog_section_gid` in config.json.
- Project `project.gid`.
- Client, Team, Task Type and Priority with option GIDs.
- No Estimated Effort, Sprint or due date unless the user gave the value.

Create in small batches and check for duplicates after each batch.

## Step 7: verify and report

Read each task back and compare it with what you intended. Fix any mismatch before reporting. Then report, in this order:

1. Task links and the template used, with one sentence on why.
2. Fields you set.
3. Fields you left empty on purpose (Estimated Effort, Sprint, due date) and why.
4. Anything you assumed (a proposed Priority, a guessed Team, a missing prefix), so the user can correct it.
5. Dependencies you set, or could not set.
6. A reminder that the creator follows up until the assignee confirms the receiving owner.

## Do not

- Do not create anything before the draft is confirmed, unless the user told you to create it now.
- Do not create a duplicate. Search first.
- Do not create tasks in any other project, and do not add new tasks to other team backlogs. Existing tasks keep their current projects.
- Do not edit, complete or assign the source template tasks.
- Do not set Estimated Effort, Sprint or the due date unless the user gave the value.
- Do not put Dev corrections under a QA task.
- Do not leave bracket placeholders in a created task. Ask for the missing detail first.
- Do not write em dashes, en dashes, emojis or filler into task text.
