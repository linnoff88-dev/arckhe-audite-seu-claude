# audite-seu-claude

[![audite-seu-claude](guia/assets/banner-en.jpg)](https://linnoff88-dev.github.io/

ðŸ‡§ðŸ‡· [Português](README.md) Â· **ðŸ‡ºðŸ‡¸ English** Â· ðŸ‡ªðŸ‡¸ [Espaê±ol](README.es.md)

## ðŸ“– User guide

Complete guide (landing page + walkthrough): **https://linnoff88-dev.github.io/

## What it is

An auditor for your Claude Code before (or after) switching to **Claude Fable 5.2**. It reads your `CLAUDE.md`, skills, agents, memories, hooks, plugins, and scripts, and finds lines written for earlier models that now get in the wayâ€”the â€œinvisible inheritance.â€ It generates a report with charts and a list of suggestions; you choose what to approve, and your own Claude applies only those changes.

Read-only: `varrer.py` does not change any files. No network access; Python 3 only (standard library).

## Why

Anthropic published the [Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2) page with 10 points to watch. Fable 5.2 follows instructions to the letter, so old lines can have a different effect: asking it to show its reasoning in the response can lead to a refusal, â€œanswer without thinkingâ€ does not reduce thinking, an instruction to â€œalways use subagentsâ€ multiplies costs, and accumulated â€œneverâ€ rules hold up routine work. Anthropicâ€™s recommended order: set effort to `high`, clean up old instructions, run the long tasks, and fix one symptom at a time while measuring.

## Quick start

```bash
git clone https://github.com/linnoff88-dev/audite-seu-claude.git ~/audite-seu-claude
cd ~/meu-projeto
python3 ~/audite-seu-claude/varrer.py . --home --html relatorio.html --saida-json relatorio.json
# open relatorio.html, select what to approve, click "gerar correcoes.md"
# in Claude Code: "aplique o correcoes.md"
```

## What it looks for

| Layer | Rule | Anthropic point | Severity |
|---|---|---|---|
| Fable 5.2 | Requests reasoning in the response text | P10 Refusals | high |
| Fable 5.2 | â€œAnswer without thinkingâ€ / â€œthink lessâ€ | P8 Old instructions | medium |
| Fable 5.2 | Token budget written in the prompt | P8 | medium |
| Fable 5.2 | â€œTreat the previous answer as finishedâ€ | P8 | medium |
| Fable 5.2 | Rigid style rule (word limit, telegraphic style) | P8 | medium |
| Fable 5.2 | Instruction to always delegate to subagents | P6 Subagents | medium |
| Fable 5.2 | Absolute instruction in all caps (â€œnon-negotiableâ€) | P7 Accumulated caution | medium |
| Fable 5.2 | â€œNever try againâ€ / â€œstop at the first errorâ€ | P7 | medium |
| Fable 5.2 | Approval for â€œany actionâ€ | P3 Approvals | medium |
| Fable 5.2 | `claude -p "continue"` in a script | P3 | low |
| Fable 5.2 | â€œNeverâ€ without a reason or scope (a safety rule becomes â€œprotectedâ€) | P7 | low / info |
| Fable 5.2 | â€œThe code is yoursâ€ | P4 Code ownership | info |
| Fable 5.2 | Fixed `max` effort, default `xhigh`, old model adjustment, Fable 5.2 on low/medium | P1 Effort | high â†’ info |
| Fable 5.2 | Missing instructions to finish the task / report progress / provide a clear report | P2, P5, P9 | info |
| Volume | Fixed context weight (`CLAUDE.md`, MEMORY.md, skill descriptions) | â€” | chart |
| Volume | Too many skills, duplicate or similar skills, unused skill (`--uso 30`) | â€” | low / info |
| Volume | Memories unchanged for more than 60 days; enabled hooks and plugins | P7 | info |

## Contents

| File | What it does |
|---|---|
| `varrer.py` | The auditor. Options: `--home`, `--html`, `--saida-json`, `--json`, `--uso DIAS`, `--anonimo`, `--fundo N`, `--estrito` (CI), `--correcoes IDs --de relatorio.json`. |
| `blocos/` | Ready-to-paste text for `CLAUDE.md`: `terminar-tarefa.md`, `avisar-progresso.md`, `relatorio-claro.md` (translations of the official texts, with the cost measured by Anthropic), and `aprovacoes.md` (template with a list of named actions). Originals in `blocos/en/`. More in `biblioteca-skills.md`: how to keep the skills you donâ€™t use every day. |
| `skill/audite-seu-claude/` | Skill for Claude Code: audits, waits for your approval, and applies `correcoes.md`. Copy it to `~/.claude/skills/`. |
| `tests/` | 19 tests with no network access: `python3 -m unittest discover tests`. |

## The process, start to finish

| # | Step | Command | Who |
|---|---|---|---|
| 1 | Download the kit | `git clone https://github.com/linnoff88-dev/audite-seu-claude.git ~/audite-seu-claude` | ðŸ§  you |
| 2 | Set Fable 5.2 to `high` for long tasks | `/effort high` (or `--effort high` in bots) | ðŸ§  you |
| 3 | Audit | `python3 ~/audite-seu-claude/varrer.py . --home --html relatorio.html --saida-json relatorio.json` | âš™ï¸ script |
| 4 | Read the report and select what to approve | open `relatorio.html` â†’ â€œgerar correcoes.mdâ€ and move the downloaded file to the project folder (or `varrer.py --correcoes A001,A004 --de relatorio.json --saida correcoes.md`) | ðŸ§  you |
| 5 | Apply only what was approved | in Claude Code: â€œaplique o correcoes.mdâ€ | ðŸ¤– Claude |
| 6 | Check | run step 3 again and compare | âš™ï¸ script |
| 7 | Run the longest task and note where it stops, goes quiet, or delegates too much | â€” | ðŸ§  you |
| 8 | For each symptom, paste the right block and measure (one at a time) | `blocos/*.md` | ðŸ§  + ðŸ¤– |

## Alongside /doctor prompt-audit

Claude Code 2.1.283 or newer has an official auditor: `/doctor prompt-audit`. It looks for instructions written for older models, referenced files or commands that no longer exist, and rules that contradict each other across files, and suggests edits. This kit covers what it doesn't read: memories, text pasted by plugins, prompts inside scripts and bots, the effort level in `settings.json`, and skill weight, with a chart-based report. Run both.

With Fable 5.2, Claude Code automatically enables the task completion text and the reminder for when it goes silent (tests published in version 2.1.322). The `terminar-tarefa.md` and `avisar-progresso.md` blocks are for those who call the model through the API or their own program.

## Tip: skills library

Every skill in `~/.claude/skills` loads its description in every session. Instead of deleting the ones you rarely use, keep them in a **library**: a folder outside the loading path (e.g., `~/projetos/skills-reserva/.claude/skills`) with an `INDICE.md` (name, purpose, search terms). A line in the global `CLAUDE.md` tells Claude to look in the index when you ask for a skill that isnâ€™t loaded. Before moving one, check whether another skill, script, or timer calls it. On the authorâ€™s machine: 148 â†’ 107 loaded skills, 13.7k â†’ 9.6k description tokens per session. Step-by-step instructions and the ready-to-use line: [`blocos/biblioteca-skills.md`](blocos/biblioteca-skills.md).

## Cautions

- The auditor flags items; you decide. Safety rules (API, credits, deleting, publishing) appear as **protected**: the suggestion is to clarify scope and reason, never to delete them.
- Plugin file: do not edit it (your changes will be overwritten in an update). Write in the global `CLAUDE.md` that the rule does not apply.
- Text in `blocos/` uses tokens (finish: +~30%; report progress: +~20%, Anthropic figures). Paste only if you see the symptom.

MIT License.