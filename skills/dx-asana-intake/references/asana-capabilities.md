# Asana capabilities

The skill describes actions by capability. Map each one to the Asana tool this assistant has. Tool names in parentheses are examples from the Asana connector the skill was written against; other connectors will name them differently.

| Capability | What it must do | Example tool (Asana connector) |
|---|---|---|
| Read a task | Return name, notes, assignee, memberships, custom fields, parent, dependencies, subtasks and comments on request | get_task |
| Search tasks | Filter by text, project, completion status, with a result limit | search_tasks |
| Read project fields | Return custom field names, GIDs and enum options for a project | get_project |
| Create tasks | Create up to 3 tasks per call, with project, section, assignee, parent, HTML notes and custom fields | create_tasks |
| Update tasks | Change fields, add or remove dependencies, and set the parent | update_tasks |
| Add to project | Add an existing task to a project and section without removing its other projects | add_projects |
| Comment | Add a comment (rich text, with fewer tags than task notes) | add_comment |

Avoid preview or dry-run variants of these tools, such as `*_preview` names. Create and update the real task.

## Connector rules

- **Custom fields format.** Some connectors take custom fields as a JSON string on create and as an object on update. If a call is rejected for the type, switch to the other format. Always send option GIDs.
- **DX membership.** Custom fields apply only to tasks that are members of the DX project. For a new task, send project and section in the same create call. For an existing task, add it to the project first, then set the fields in a second call.
- **Subtasks.** A subtask joins the DX board only if it is created with the DX project and section. Parent membership does not carry over. To create a subtask of a subtask, set the parent to that subtask's GID.
- **HTML.** `html_notes` must be well-formed XML with one `<body>` root. Allowed tags: strong, em, u, s, code, ol, ul, li, a, blockquote, pre, h1, h2, hr, img, table, tr, td. Do not use `<br>`. Comments allow fewer tags: no h1, h2, hr or img.
- **Comments are permanent.** Comments may not be editable or deletable through the connector. If one is wrong, post a new comment that says it replaces the earlier one.
- **Dependencies.** Set them after the tasks exist, using real GIDs. "Waits for" means the task depends on the other.
- **Batching and retries.** Create at most 3 tasks per call. An error or timeout can still have created tasks. Before retrying, read the parent's subtasks or search the project and retry only what is missing. A blind retry creates duplicates.
- **Limits.** The connector may be unable to change project fields, native templates, saved views, recurrence, the project overview or automation rules. If the work needs one of these, say so in the report and leave it to a person.

## Example create payload

One task: client EcoForce, Team Dev, Task Type Content Update, Priority Medium. IDs come from [config.json](config.json).

```json
{
  "name": "[EF] Replace the homepage hero image",
  "assignee": "1213804605282484",
  "project_id": "1219828023221466",
  "section_id": "1219828023221491",
  "html_notes": "<body>...</body>",
  "custom_fields": "{\"1213128780071580\":\"1215492738937221\",\"1219827917639837\":\"1219827917639840\",\"1219828023230235\":\"1219828023230239\",\"1219828023230218\":\"1219828023230221\"}"
}
```

## Verify after writing

Read each task back and check: name, assignee, DX project and section membership, custom field values, parent, and dependencies. Compare with what you intended. Fix any mismatch before reporting.
