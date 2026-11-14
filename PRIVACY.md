# Privacy policy

Last updated: 14 November 2026

This policy covers the foundrvibe plugin: the `cofounder`, `run-playbook`, and `write-playbook` skills in this repository.

## What the plugin collects

Nothing. The plugin has no server, no accounts, no analytics, and no tracking. The maintainers never receive what you type, the documents it produces, or your founder file.

## Where your data goes

- **Your founder file and documents** stay with you. In a tool with a working folder, they are saved as `founder.md` and `<playbook-id>-<date>.md` in that folder. In a chat, they are printed in the reply for you to keep.
- **Playbook files** are downloaded from this public repository at `raw.githubusercontent.com`. Those requests carry no data about you or your company. GitHub's own privacy statement covers its servers.
- **Web research** runs through the search tool of the AI app you use, under that app's privacy terms. The plugin does not have its own search service.

## Code the plugin may run

To draw charts, the `run-playbook` skill can download `charts/render_chart.py` from this repository and run it in your AI app's own Python tool. To draw diagrams, it can run Mermaid's command-line tool (`npx @mermaid-js/mermaid-cli`) in your terminal. Both run on your side, only when your app allows it, and neither one sends data anywhere. You can read the chart script in [charts/](charts/).

## Changes and contact

Changes to this policy are recorded in this repository's history. For questions, open an issue at https://github.com/foundrvibe/cofounder/issues.
