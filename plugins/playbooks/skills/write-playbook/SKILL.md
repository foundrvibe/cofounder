---
name: write-playbook
description: >-
  Turns a job a person already knows how to do into a new playbook: the five
  files and the catalog entry, validated. Use when the user asks to create,
  write, add, or author a playbook, or to turn their own process into one. The
  cofounder skill offers it when no ready playbook fits a request.
---

# Write a playbook

A playbook is a job someone already knows how to do, written down once. You interview that person, write the five files and the catalog entry, and validate them. The method is theirs. You only give it a shape that the run-playbook skill can follow.

This file is the only link a user needs to send. Fetch the other files yourself.

Base URL: `https://raw.githubusercontent.com/foundrvibe/playbooks-for-agents/main/`

| File | URL |
|---|---|
| Fill rules | `https://raw.githubusercontent.com/foundrvibe/playbooks-for-agents/main/rules/fill.md` |
| Run a playbook | `https://raw.githubusercontent.com/foundrvibe/playbooks-for-agents/main/skills/run-playbook/SKILL.md` |
| Catalog | `https://raw.githubusercontent.com/foundrvibe/playbooks-for-agents/main/catalog.json` |
| Starter files | `https://raw.githubusercontent.com/foundrvibe/playbooks-for-agents/main/playbooks/_starter/<file>` |
| Shared research guides | `https://raw.githubusercontent.com/foundrvibe/playbooks-for-agents/main/research/<name>.md` |

Read the fill rules first. Every run of the new playbook must be able to obey them. Then read the starter files, and the five files of the ready playbook in the catalog that is closest to this job. Copy its conventions, not its content.

## 1. Interview

Read what the user already gave you. Then ask the questions below that are still open, in one message, and wait. Do not draft files in that reply.

1. What is the job, and who do you do it for?
2. What does the finished result look like? Paste one you made, with private details removed, or list its sections in order.
3. What do you need from the client that only they know? What numbers, notes, or files do they hand you?
4. What do you look up yourself, and where?
5. What do you work out or decide from all that? If the job ends in a decision, what are the possible outcomes, and what is the rule for each?
6. What are the steps, in the order you do them? What do beginners get wrong? What do you never do?
7. How would someone ask for this? Give three to six phrases.
8. Does the place matter: country, region, or city?

"I don't know" is an answer. Do not ask again. Where the expert has no rule, propose one, and list it after the files as your proposal for them to confirm. Do not present your proposal as their method. A rule may also come from a public source the expert names; link it in `research.md`.

## 2. Name it

- `id`: lowercase kebab-case, not already in the catalog. It is also the folder name.
- `category`: a category id from the catalog. Create a category only when none fits.
- `version`: `1.0.0`.
- `title` and `description`: the title is two or three words. The description says what the agent checks and what comes back, in two or three sentences.

## 3. Write the five files

The folder is `playbooks/<category>/<id>/`. It holds exactly these five files.

### template.md

This is the result. Its sections are the expert's sections, in the expert's order.

- Start with `# {{field}}`, then a two-column table of the key facts, ending with `**Language**` and `**Research**` rows.
- A single value is `{{field}}`. A list is a table with one `{{#each field}}` block per row, closed by `{{/each}}`.
- Comparable items go in a table. When a table has two or more numbers that share a unit, add a `{{chart:slot}}` under it.
- Add a `{{diagram:slot}}` only when the result describes a process.
- End with `## Assumptions` and `## Sources`, each as a table.
- No missing-info section and no questions. The starter has a `## Missing info` section; drop it, as the ready playbooks do.
- Write no digits in the fixed text. The check fails on any number in the document that is not in the fill.

### schema.json

Every token in `template.md` is a field here, and every field sets these keys:

| Key | Value |
|---|---|
| `type` | `string`, `number`, or `array` of objects. An object in an array sets `additionalProperties: false` and `required`. |
| `description` | The rule for filling the field. The expert's judgment goes here and in the steps. |
| `x-source` | `user`: only the user knows it. `user_data`: the user's numbers, notes, lists, or files. `research`: looked up on a public page and cited. `derived`: worked out from the other fields. |
| `x-ask` | The question, for `user` and `user_data`. `Leave this to the agent.` for the other two. |
| `x-unknown` | `_Unknown — not provided_` for `user` and `user_data`. `Research this field and cite a source. Do not leave it blank.` for `research`. `Derive this field from the context and sources. Do not leave it blank.` for `derived`. |

- Set `x-id`, `x-version`, and `x-title` at the top. `x-id` is the folder name. `x-title` is the `description.json` title. Set `additionalProperties` to `false`.
- Always include `document_language`, `research_mode`, `assumptions`, and `sources`. Copy their definitions from the closest ready playbook. Include `market_country` and `target_region` when the place matters.
- `required` lists every `user` and `user_data` field, plus `document_language`, `research_mode`, and `sources`.
- Ask for little. A field the agent can look up is `research`. A field it can work out is `derived`. Only what is left is a question.
- A number is `type: number` with `x-format` of `currency` or `number`.
- A decision is a `derived` string with an `enum`, and a second field for its reason. The `description` gives the rule for each value. Write the rule so that two people reading the same evidence pick the same value.
- A chart slot is an object field with `x-chart: mermaid-xy`. A diagram slot is a string field with `x-diagram` set to a spec id in `diagrams/`.
- Add an `allOf` rule: when `research_mode` is `web`, `sources` has at least one item, and each researched collection has the minimum number of rows the expert would accept.

### research.md

These are the steps a run follows.

- Open with one paragraph: read this after `schema.json`, what the job is, and the one mistake the expert warned about.
- `## Display`, when the layout needs a rule: cell length, which table gets the chart.
- `## Steps`, numbered, in the expert's order. Step 1 asks for the `user` and `user_data` fields and waits. Each later step names the fields it fills, in backticks, and gives the rule. The decision step spells out the rule for each outcome. The last step lists `assumptions` and `sources`.
- `## Where to look`: one line per `research` field, linked to a shared guide as `../../../research/<name>.md`. The guides are `channels`, `company-facts`, `competitors`, `customer-evidence`, `financial-modeling`, `market-size`, `pricing`, `sales-compliance`, and `source-evaluation`. Add a shared guide only when none fits.
- Every `research` field appears in backticks at least once. Validation fails otherwise.
- Write what the expert never does as a "Do not" in the step where a beginner would do it.

### example.json

A complete, valid fill for a made-up company. It shows the shape.

- Do not copy a client's name or figures from the expert's sample.
- With `research_mode` of `web`, every source is a page you opened while writing it. If you cannot browse, use `offline`, leave `sources` empty, and start each researched value with `Assumption:`.
- Fill every field. Use each `enum` value as written.

### description.json

Only `title` and `description`. The title equals `x-title`.

## 4. Add the catalog entry

In `catalog.json`, add the id to its category's `templates` list, and add an entry to `templates` shaped like the others:

- `id`, `name`, `version`, `status`, `category`, `description`, `intents`, and `files`.
- `name` equals the `description.json` title. `description` equals its description. `version` equals `x-version`.
- `status` is `ready`. `intents` are the expert's phrases in lowercase. No intent may belong to another ready playbook.
- `files` has `template`, `schema`, `example`, `research`, and `description`, each with `path` and `url`. The `url` is the base URL plus the `path`.

## 5. Validate

**If you have the repository and a terminal** (Claude Code, Cursor):

1. Copy `playbooks/_starter/` to the new folder, then write the files.
2. Run `npm ci && npm run validate`. Fix every error and run it again until it passes.
3. Run `node scripts/render-fill.mjs <id> playbooks/<category>/<id>/example.json` and read the output as the client would.

**If you do not,** return the five files and the catalog entry as six code blocks, each under its path. Say in one line that validation was not run, and give the command. Do not say it passes.

## 6. Hand it back

Show the expert the rendered example and ask one question: is this what you would hand to a client? Then list:

- every rule you proposed that they did not give you;
- every step where you guessed the order;
- the command or prompt that runs the new playbook: `Playbook: <id>` under the run-playbook link.

Change the steps and the field descriptions when they correct you. Do not patch the example to hide a weak step.

The first real run is the test. Run the playbook once on a real context with the run-playbook skill. Where the result differs from what the expert would have written, fix `research.md` or a field `description`, raise `x-version` and the catalog `version` together, and validate again.

## 7. Publish

Open a pull request to this repository. In a fork, replace the base URL in `catalog.json`, in `scripts/validate-templates.mjs` (`rawBase`), and in each `SKILL.md`, so that the links point at the fork.
