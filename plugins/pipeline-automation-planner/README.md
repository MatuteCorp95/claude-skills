# Pipeline Automation Planner

Plan the automation of a recurring data process (file intake, reconciliations, recurring analyses or reports) before writing any pipeline code.

## What it does

Claude walks a developer through five phases: intake of how the process runs today, a stress test of failure modes, a current-state diagram, a target design split into small independent projects joined by contracts, and a plan document a manager can approve. Every tool choice is weighed on scalability, ROI, currency, security and AI fit, and every estimate shows its basis and confidence. Unknowns that only other people can answer become a ready-to-send open questions list, grouped by who to ask.

## Install

In Claude Code:

```
/plugin install pipeline-automation-planner --marketplace MatuteCorp95/claude-skills
```

On claude.ai or the Claude desktop app: Customize > Plugins > Add > Add marketplace, paste `https://github.com/MatuteCorp95/claude-skills`, then install Pipeline Automation Planner.

## Use it

Describe a data process you want to automate, for example: "Every month we receive vendor files in different layouts, an analyst cleans them in Excel and builds a summary report. It takes several days. Help me plan automating it." Reply OK to move between phases, or "expand <section>" for detail.

## Data

This plugin contains instructions only. It has no scripts or connectors and sends no data anywhere. When web search is available, Claude may look up public pricing and version information for the tools it compares.
