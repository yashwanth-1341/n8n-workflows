# N8N Workflows

A collection of ready-to-import n8n workflow templates.

## How to Import

1. Download the `.json` file of the workflow you want.
2. Open your n8n instance (cloud or self-hosted).
3. Go to **Workflows → Import from File** and select the JSON file.
4. Open the nodes marked with a warning and select your credentials (for example OpenAI, Slack, Google Sheets or Jira).
5. Replace any `YOUR_...` placeholder values with your own IDs.
6. Click **Execute workflow** to test it, then activate the workflow if it uses a webhook, form or schedule trigger.

Most workflows include sample data and ship with their Slack, Jira, Google Sheets and HTTP nodes disabled, so you can test them with only an OpenAI credential. Enable those nodes when you are ready to connect your own tools.

## Workflows

| Title | Description | Trigger | Link |
|---|---|---|---|
| Customer Feedback Intelligence | Removes duplicates and personal data from incoming feedback, classifies it with an LLM (sentiment, theme, urgency), scores its priority, sends urgent items to Slack, and builds a summary digest. | Webhook / Manual | [Link to Template](Customer%20Feedback%20Intelligence.json) |
| Business Process Issue Triage | Matches reported issues against a known-issue list, uses an LLM to suggest a root cause and next steps, then applies rules to set severity, owner team and SLA. Routes each issue to a Slack alert, a Jira ticket or a self-service reply. | Webhook / Manual | [Link to Template](Business%20Process%20Issue%20Triage.json) |
| Requirement Validation Gate | Scores requirements for quality with an LLM and drafts a user story with acceptance criteria. Then passes each one to Jira, asks a reviewer to decide (the workflow waits for the answer), or drafts clarifying questions for the requester. | Webhook / Manual | [Link to Template](Requirement%20Validation%20Gate.json) |
| Meeting Transcript to Backlog | Reads meeting transcripts from Google Drive. A chain of AI agents extracts requirements, writes user stories and acceptance criteria, proposes a solution and sets a priority. Results are kept in sync between Jira and Google Sheets. | Manual | [Link to Template](Meeting%20Transcript%20to%20Backlog.json) |
| JIRA BOT | The original version of *Meeting Transcript to Backlog*, with its original node names. It works the same way and needs the same setup. | Manual | [Link to Template](JIRA%20BOT.json) |
| Prototype Feedback Loop | Anonymous feedback form. An LLM extracts improvement ideas from each response, which are saved to Google Sheets. Reported blockers trigger a Slack alert. | Form | [Link to Template](Prototype%20Feedback%20Loop.json) |
| Automation Performance and ROI Monitor | Daily check of your n8n executions: success rate, run-time spikes, volume drops, estimated hours saved and LLM cost. Posts a short summary to Slack when something looks wrong. | Schedule / Manual | [Link to Template](Automation%20Performance%20and%20ROI%20Monitor.json) |
| Global Error Handler | Catches failed executions from your other workflows, posts an alert to Slack and logs the error to Google Sheets. Set it under *Settings → Error workflow* in each workflow. | Error Trigger | [Link to Template](Global%20Error%20Handler.json) |

## How Each Workflow Helps

| Workflow | Productivity gain | Value for the organization | How it scales |
|---|---|---|---|
| Customer Feedback Intelligence | Replaces manual reading and tagging of feedback from different channels. Duplicates are counted once. | Urgent feedback from key customers is flagged the same day instead of waiting in an inbox. A recurring digest shows which topics cause the most frustration. Personal data is removed before it reaches the AI. | Accepts single items or batches through one webhook. Large volumes can be processed in batches. |
| Business Process Issue Triage | Fewer tickets bounce between teams, because each issue arrives with a suggested root cause, owner team and next steps. Known how-to questions are answered from the knowledge base and never reach an expert. | Severity and SLA follow written rules, so prioritisation is consistent and auditable. Critical issues are escalated immediately. | The rules sit in one node and can be changed without touching the AI prompt. The knowledge base can grow or move to a vector database. |
| Requirement Validation Gate | Reviewers only spend time on requirements that need judgement. Clear ones reach the backlog with a drafted user story and acceptance criteria. Vague ones go back to the requester with specific questions. | Unclear or risky requirements, such as those involving personal data, are caught before development starts, when changes are cheapest. Every reviewer decision is logged. | All items that need review in one run are batched into a single request. The same quality criteria apply to every requester. |
| Meeting Transcript to Backlog | Turns a meeting transcript into requirements, user stories, Jira tickets, a proposed solution and a priority, with no manual write-up. | Meeting outcomes are no longer lost, and Jira and the tracking sheet stay in sync. | Each AI agent hands structured output to the next, so new steps can be added. Processing only new files is the next improvement. |
| Prototype Feedback Loop | Collects and summarises user feedback automatically. Ideas arrive sorted by category and rough effort, ready for planning. | Decisions on what to improve are based on what users actually report, not on assumptions. Blockers are visible immediately. The form is anonymous, which encourages honest answers. | One form works for any number of prototypes, tools or training sessions. |
| Automation Performance and ROI Monitor | Removes manual checking of execution logs. One short daily note shows what needs attention. | Shows the estimated hours saved and the AI cost of each workflow, which is the evidence needed to decide whether an automation is worth keeping. Problems are spotted before users report them. | Works across any number of workflows. Thresholds and assumptions, such as minutes saved per item, are configurable. |
| Global Error Handler | No time is lost discovering broken automations. | Failures are visible within seconds and logged for follow-up, which keeps trust in the automations high. | One error workflow covers every other workflow. |

**Estimating time saved:** the monitor uses `hours saved = items processed × manual minutes per item ÷ 60`. The default minutes per item are assumptions. Measure how long the task takes today and update them in the *Compute KPIs & Anomalies* node before you report any numbers.

## Setup Notes

- **Meeting Transcript to Backlog** needs Google Drive, Google Docs, Google Sheets, Jira and OpenAI credentials. Your spreadsheet needs two tabs:
  - `requirements` with the columns `Requirement` and `Status`
  - `US & AC` with the columns `Requirement`, `User Story`, `Acceptance Criteria`, `Status`, `Last Modified Date`, `Proposed Solution` and `Priority`
- **Automation Performance and ROI Monitor** uses sample data until you enable the *Fetch n8n Executions* node and add an n8n API key (header `X-N8N-API-KEY`).
- AI node versions change between n8n releases. If a node shows a warning after import, open it and re-select the highlighted field (usually the model).
