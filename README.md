# Cofounder

Your AI cofounder. A free skill for ChatGPT, Claude, or Cursor. Tell it what you are building. It picks the next step, runs it, and remembers the results for next time. [Start here.](#use-it)

Every step is a playbook. A playbook is a job you already know how to do, written down once. Give that playbook to your agent. The agent does the job the way you would, and respects that pattern on every run.

The cofounder calls two other skills: one [runs a single playbook](#run-one-playbook-directly), and one [writes a new playbook](#write-your-own-playbook) from a job you describe.

```mermaid
flowchart LR
  job[A job you already know how to do] --> playbook[Written down once as a playbook]
  playbook --> give[Give the playbook to your agent]
  give --> result[The agent does the job the way you would]
```

Four files make that work:

- [skills/cofounder/SKILL.md](skills/cofounder/SKILL.md) is the front door: it reads every message, picks the next move, and keeps the founder file.
- [rules/fill.md](rules/fill.md) is the contract for every run: cite sources, label estimates, and do not leave a section blank.
- [skills/run-playbook/SKILL.md](skills/run-playbook/SKILL.md) is the loop: pick the playbook, ask, follow its steps, return the result.
- [catalog.json](catalog.json) is the index. Playbooks sit in three categories: **Validation**, **Planning**, and **Growth**. A request is matched to a ready playbook. A stub is skipped. If two ready playbooks both match, the agent asks which one and names the category.

## Playbooks

| Category | Id | What it returns | Ask for it with |
|---|---|---|---|
| Validation | `idea-validator` | A scorecard on pain, market, alternatives, and customer pull, with one next test. | "Validate my idea" |
| Validation | `customer-discovery` | The best-fit customer, where to recruit, screening, a non-leading interview guide, and an evidence table from your own notes. | "Plan customer interviews" |
| Validation | `market-research` | Market definition, segments, bottom-up TAM, SAM, and SOM with formulas and a range, trends, demand signals, and an attractiveness verdict. | "Do market research" |
| Planning | `business-plan` | A business plan in eight sections with a revenue chart. | "Write a business plan" |
| Planning | `mvp-planner` | First users, the user journey, now, later, and never features, user stories, manual steps, a time-boxed build plan, and launch metrics. | "Plan the MVP" |
| Planning | `financial-model` | Monthly revenue, costs, and cash with a cash chart, runway, break-even, unit economics, scenarios, and reconciliation. | "Build a financial model" |
| Growth | `go-to-market` | Positioning, buyer roles, offer, channel tests with a budget chart, a launch plan, and one primary metric. | "Write a go-to-market plan" |
| Growth | `lead-generation` | A sales hypothesis, qualification and scorecard, lawful lead sources, an outreach sequence, a funnel chart, and the outreach rules for your country. | "Build a lead generation plan" |

They work alone or in order: validate the idea, talk to customers, size the market, plan the MVP, plan the launch, find leads, then model the money. When you already ran one, paste its result into the next request and the agent reuses those facts instead of asking again.

Each playbook asks for a **country** (`market_country`) and a **focus region** (`target_region`): a city, province or state, metro, or the whole country. That scopes research, competitors, events, and outreach. The business plan also has **Location** for where the company is based; **Region** is where the market work focuses.

The methods come from Y Combinator and the sources it points founders to: [How to talk to users](https://www.ycombinator.com/library/JQ-how-to-start-a-startup-talking-to-users) and [The Mom Test](https://www.momtestbook.com/) for interviews, [How to plan an MVP](https://www.ycombinator.com/library/6f-how-to-plan-an-mvp) and [Do Things That Don't Scale](https://paulgraham.com/ds.html) for the first version and first customers, [Enterprise sales for founders](https://www.ycombinator.com/library/LF-enterprise-sales-for-founders) for outreach, and [Default Alive or Default Dead?](https://www.paulgraham.com/aord.html) for the model. The shared guides in [research/](research/) hold the details.

No playbook invents an interview, a contact, a quote, or a figure. Anything not given by you or found on an opened page is labeled `Assumption:`. Lead generation writes messages for you to review and send; it does not send them.

## Use it

Paste this into ChatGPT, Claude, or a Cursor agent chat. One link is enough. The cofounder fetches the other skills, the rules, the catalog, and the playbook files.

```text
Follow https://raw.githubusercontent.com/foundrvibe/cofounder/main/skills/cofounder/SKILL.md

I'm building ZedCut, a bilingual B2B SaaS for countertop fabrication shops in Quebec. What should I do next?
```

Then talk to it as you would to a cofounder. Every message goes to the cofounder first, and it decides what happens:

| You say | The cofounder |
|---|---|
| "What should I do next?" | Says where the company stands, names the riskiest assumption, and recommends one playbook. |
| "Write a business plan" | Checks the evidence, then runs that playbook without asking again what it already knows. |
| "I did five interviews, here are my notes" | Records them, closes the test you committed to, and moves to the next step. |
| "Actually the price is 149, not 199" | Updates the fact and names each document that used the old price. |
| "Make me a pitch deck" | Says no playbook covers it, does the work directly, and offers to turn it into a playbook if you will do it again. |
| "Save" or "done" | Saves the founder file and reminds you of the one open test. |

- **It keeps a founder file**: what is a fact, what is still a guess, the verdict of each playbook you ran, and the one test you said you would do. It saves the file every time it changes.
- **It disagrees from evidence.** It will argue against an MVP plan while the idea check says `not yet`, and it puts money first when the model says `default dead`. It says so once. You decide, and it runs what you ask for.
- **It checks one document against another**, such as a price that differs between the go-to-market plan and the financial model.
- **A guess stays a guess** until a customer acts, your own data shows it, or a page the agent opened shows it.

### Where the founder file lives

- **Claude Code or Cursor:** `founder.md` in your folder, with each document saved beside it. Nothing to do.
- **ChatGPT or Claude chat:** make a Project for the company and put the two lines above in its instructions. When the agent prints the founder file, save it into the project's files. Each new chat in that Project picks it up. Without a project, paste the file at the start of the next chat.

## Run one playbook directly

To skip the cofounder and get one document, use the run-playbook link. It keeps no founder file.

```text
Follow https://raw.githubusercontent.com/foundrvibe/cofounder/main/skills/run-playbook/SKILL.md

Create a business plan for ZedCut, a bilingual B2B SaaS for countertop fabrication shops in Canada.
```

Replace the second line with your own request. If several playbooks match, the agent asks which one and names the category.

To choose the playbook yourself, name its id. The agent uses that playbook and does not match intents. This works with the cofounder link too.

```text
Follow https://raw.githubusercontent.com/foundrvibe/cofounder/main/skills/run-playbook/SKILL.md

Playbook: business-plan
Describe the company or idea here.
```

Use any id from the table above in place of `business-plan`, for example:

```text
Follow https://raw.githubusercontent.com/foundrvibe/cofounder/main/skills/run-playbook/SKILL.md

Playbook: customer-discovery
ZedCut, a bilingual B2B SaaS for countertop fabrication shops in Canada. No interviews yet.
```

A raw URL to `template.md`, `schema.json`, or the playbook folder selects it the same way. Replace `main` with a version tag to pin that playbook.

## Write your own playbook

The eight playbooks here are startup work. The format fits any job you already know how to do. This skill interviews you about the job, writes the five files and the catalog entry, and validates them. The cofounder offers it when you ask for work no playbook covers, or you can call it yourself.

```text
Follow https://raw.githubusercontent.com/foundrvibe/cofounder/main/skills/write-playbook/SKILL.md

Turn how I do a pricing review for small software companies into a playbook.
```

In Claude Code or Cursor, with this repository cloned, the agent writes the files and runs `npm run validate` until it passes. In a chat it returns the files for you to add, and says that validation was not run. The method stays yours: where you gave no rule, the agent lists the one it proposed for you to confirm.

<details>
<summary>Add a playbook by hand</summary>

1. Copy `playbooks/_starter/` to `playbooks/<category>/<id>/`. Set `x-id` in `schema.json` to the new id. Set `title` and `description` in `description.json`, and set `x-title` to that same title.
2. Edit `template.md`, `schema.json`, and `example.json`. `template.md` is the result. Mark each field `x-source` as `user`, `user_data`, `derived`, or `research`.
3. In `research.md`, write `## Steps` in the order a person would do the job. Name every `research` field and link the shared guide as `../../../research/<name>.md`. Add a new shared guide only when none fits.
4. Add a catalog entry with `id`, `name`, `version`, `status`, `category`, `description`, `intents`, and the file paths. `name` is the `description.json` title. `description` is the `description.json` description. `category` must be an id in `categories`, and that category's `templates` list must include the new id. Create a category when none fits. Set `status` to `ready` and fill `intents` when a request should select it. Leave `intents` empty while it is a stub.
5. Run `npm ci && npm run validate`.

No new skill is required. CI fails if two ready playbooks share an intent, if `research.md` misses `## Steps` or a research field, or if the example does not pass the fill check.

</details>

<details>
<summary>Pin a version</summary>

Each playbook version is tagged as `<id>-v<version>`, for example `business-plan-v4.0.0`. Replace `main` in any raw URL with the tag to keep a fixed shape:

`https://raw.githubusercontent.com/foundrvibe/cofounder/business-plan-v4.0.0/playbooks/planning/business-plan/template.md`

A tag pins the rules and skill at that commit too. A breaking schema change gets a new major version.

</details>

<details>
<summary>Check a fill</summary>

Save the JSON the agent returns, and optionally its Markdown, then run:

```text
npm ci
node scripts/check-fill.mjs business-plan fill.json filled.md
```

It fails when the JSON does not match the schema, the Markdown still has an unknown marker or a placeholder, `sources` is empty in `web` mode, or a number in the Markdown is not in the JSON. Without `filled.md`, it renders the JSON with `scripts/render-fill.mjs` first.

</details>

<details>
<summary>Test contexts</summary>

[tests/contexts/](tests/contexts/) holds real contexts, such as `zedcut.json`, plus one `zedcut-<id>.json` per playbook. The `template` field names the playbook. Paste its `context` into each agent, save the fill, and add `--expect tests/contexts/<file>.json` to the check to compare the fields it lists.

</details>

<details>
<summary>What comes back</summary>

The agent returns only the finished result. Sources are inside that result. The field values stay in the agent's check and are not pasted after it.

</details>

<details>
<summary>Charts</summary>

Comparable items go in a table. When that table has two or more numbers that share a unit, a chart of those same numbers sits under the table.

- **Agents that can run Python** (ChatGPT data analysis, Claude code execution, Cursor) draw each chart with [charts/render_chart.py](charts/render_chart.py). The colors and size come from [themes/default.json](themes/default.json).
- **Agents that can't run Python** write a Mermaid chart under the same table.

To draw one yourself:

```text
pip install matplotlib
python charts/render_chart.py --kind mermaid-xy --data '{"seriesName": "Revenue", "x": ["Year 1", "Year 2"], "series": [120000, 260000]}' --currency CAD --language fr --out financials.png
```

</details>
