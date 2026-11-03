# Field rules for every task

Values come from [config.json](config.json). Use option GIDs, never display names.

| Field | Rule |
|---|---|
| Assignee | `assignee` in config.json, always, for new tasks and every subtask. Name the proposed executor in the description (Owner line). The assignee reassigns after confirming the receiving owner. |
| Section | `project.backlog_section_gid`, always. |
| Client | Take it from `clients`. If a prefix is shared or unclear, ask. See `client_rules`. Never guess. |
| Task Type | Incident = active service disruption or risk. Bug = behavior fails the agreed expectation. Enhancement / New Request = new capability or improvement. Content Update = bounded copy or asset change. Routine Maintenance = preventive or recurring upkeep or check. |
| Team | The team that executes the work. Dev: WordPress, ACF, forms, plugins, hosting, integrations, code. SEO/pSEO: copy, schema, internal links, Airtable, Whalesync, programmatic pages. Design: UX, visual design, images. QA: testing and verification. Account Management: client follow-up, approvals, scope. |
| Priority | Use what the requester states. A P1 incident is Critical. If nothing is stated, ask. If the user cannot say, set Medium and mark it as proposed in the report. |
| Estimated Effort | Leave empty unless the requester gives an estimate. The executor confirms hours before work starts. Map a stated estimate to the nearest range. Never invent hours. |
| Estimated time (native, after the pending change) | Fill only when the requester gives hours and minutes. Never convert effort ranges into hours. |
| DX Sprint | Leave empty at intake. Set it only when the requester names a sprint that exists in the field. |
| Due date | Leave empty at intake. Put any requested deadline in CHANGES. The date is confirmed when the task moves to To Do. |
| Followers | None by default. |

**Estimates and feedback.** The initial estimate covers execution, the assignee's own checks and expected feedback within the agreed scope. Normal feedback is handled on the same task inside that estimate. If the forecast goes over the estimate or threatens a committed date, the executor flags the reason and the revised forecast to the assignee early, and Account Management communicates any agreed date change. An overrun is not automatically new scope.

**Writing rules.** Plain sentences. No em dashes, en dashes, emojis, filler, or bracket placeholders left in the text. Quote client text exactly when it is evidence. Use exact URLs.

**HTML body.** Task descriptions use `html_notes` with one `<body>` root. Escape `&`, `<` and `>` in anything pasted from an email or page. See [asana-capabilities.md](asana-capabilities.md) for allowed tags.
