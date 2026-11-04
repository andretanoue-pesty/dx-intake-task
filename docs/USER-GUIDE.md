# DX Asana intake: user guide

This guide explains how to install the `dx-asana-intake` skill and use it to create or update DX Operations website tasks in Asana.

## What the skill does

You describe a website request in plain language. The assistant:

1. Picks the right template: Simple Request, Delivery Workstream, or release QA task.
2. Checks Asana for an existing task, so nothing gets duplicated.
3. Asks for anything missing, in one message.
4. Shows you a draft: title, template, fields and description.
5. Creates the task only after you confirm.
6. Reads the task back and reports what it set and what it left empty.

It does not set Estimated Effort, Sprint or due dates unless you give those values.

## Before you start

You need:

- An AI assistant that supports agent skills and can load a skill folder (for example Codex, or Claude with skills enabled).
- Asana tools connected to that assistant, with access to the DX Operations project (Website Maintenance & Improvements).
- Access to this GitHub repository (`andretanoue-pesty/dx-intake-task`).

## Install

You don't need to use a terminal or write any code. Pick the path that matches your situation.

### Option A: from the plugin library (once it is published)

1. Open your AI assistant's plugin or library page (on claude.ai, look for **Plugins**).
2. Search for **dx-asana-intake** and click **Install**.
3. If the page says the plugin is waiting for approval, ask your DX owner to approve it.
4. Start a new chat. The skill is now available.

### Option B: for the test group, before it is published

Your DX owner will send you a ZIP file of the skill. Then:

1. Save the file `dx-asana-intake.zip` somewhere you can find it, such as your Downloads folder. Do not unzip it.
2. Upload the ZIP to your assistant's skill upload page, the place where it adds skills. If you can't find it, ask your DX owner where it is. Choose the ZIP file.
3. If the page reports an error, check that you uploaded `dx-asana-intake.zip` and not a different file, then ask your DX owner for a fresh copy.
4. Start a new chat, so the assistant loads the skill.

If you use Codex, you can also ask it to install the skill for you:

```text
Use $skill-installer to install skills/dx-asana-intake from andretanoue-pesty/dx-intake-task.
```

Codex may ask you to sign in to GitHub the first time. Sign in with your own GitHub account, which needs access to this repository.

## Confirm your Asana setup

The skill's first step checks the Asana templates and fields it depends on. You can run that check yourself by asking:

```text
Use $dx-asana-intake to check the Asana setup for DX Operations.
```

You don't need to change anything in your setup. At the start of each session, the skill looks up the current Asana fields by name, so changes to fields and options are picked up automatically. If it reports a missing field or option, tell your DX owner.

## Use it

Ask in plain language. Examples:

```text
Use $dx-asana-intake to file this as a DX request: the EcoForce homepage hero image needs replacing. Owner is Dina.
```

```text
Use $dx-asana-intake to log a bug: the Directions map on Rowland service area pages is broken on mobile.
```

```text
Use $dx-asana-intake to set up the Launch of the office map for all Rowland service area pages. It needs client approval and Dev, then pSEO, then QA.
```

```text
Use $dx-asana-intake to add a QA task for the Rowland office map release.
```

The assistant will then:

- Ask for missing details, such as the client, the exact change, the affected URLs, the acceptance criteria and who checks the result.
- Show you the draft.
- Wait for your confirmation. Reply "yes", "create it", or correct the draft first.

## Which template it picks

| If the request is | The assistant uses |
|---|---|
| One bounded change or check with one owner | Simple Request |
| A bug, or an incident (including P1) | Simple Request, bug or incident variant |
| A recurring health check | Simple Request, recurring QA variant |
| Work that passes between teams: scope, then content or design, then approval, then build, QA and launch | Delivery Workstream |
| Release QA for a Simple Request or a workstream | QA task |
| A new website, or a major build phase of one | Not created by this skill. Create it from the Website Build Project Template in Asana |
| Work that already exists as a task in Asana | Updates the existing task and adds it to DX |

If the assistant can't tell between a Simple Request and a Delivery Workstream, it asks:

> Does anyone besides the person doing the work need to deliver something first, or approve something, before this can be built or released?

A yes means Delivery Workstream.

## What you should expect in the report

After it creates or updates tasks, the assistant reports, in order:

1. The task links and the template used, with one sentence on why.
2. The fields it set.
3. The fields it left empty on purpose (Estimated Effort, Sprint, due date) and why.
4. Anything it assumed, such as a proposed Priority, a guessed Team or a missing client prefix. Correct these if they're wrong.
5. Dependencies it set, or couldn't set.
6. A reminder that the creator follows up until the assignee confirms the receiving owner.

## Tips

- Give the client name or prefix. If the client has no prefix in the list, the assistant asks you before naming the task.
- For work across several clients, say so. The assistant uses the All / Multi Clients option and the `[Multi-Client]` prefix.
- Give Priority if you know it. Otherwise the assistant proposes one and marks it in the report.
- Give estimates only in hours and minutes if you have them. The assistant never converts effort ranges into hours.
- Paste the Gmail thread ID or the client's original email when you have it. The assistant keeps the thread ID in the task.
- If an incident is active, alert the assignee directly as well. Creating the task doesn't start the response.

## Troubleshooting

| Problem | What to do |
|---|---|
| The assistant can't reach Asana | Check that your Asana connection is active and has access to the DX Operations project. |
| "Config is out of date" | A field or option has changed in Asana. Ask the DX owner to update `references/config.json`. |
| A client isn't recognised | The assistant reads the live client options before saying it doesn't exist. If it still can't find the client, check the spelling or ask the DX owner to add it to the config. |
| A task was created twice | Check the DX board for duplicates, and archive the extra one. The skill searches for duplicates before creating, so tell the DX owner what happened. |
| A call timed out and the assistant didn't report back | Ask it to check what was created. It should read the parent's subtasks or search the project before retrying. |

## Maintaining the skill

- Live Asana templates win over the written rules. The skill checks them before writing.
- The IDs in `references/config.json` are dated (verified 2026-10-27). Update that file when DX fields, clients or templates change.
- To use another Asana project, copy `config.json`, replace the values, and keep the same keys.
- Keep changes to the skill in a branch, and open a pull request for review.
