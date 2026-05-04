---
date: '2026-05-03'
draft: false
title: 'AI Agentic Plugins, the infrastructure that turns prompts into an ecosystem'
description: 'How agentic plugins package skills, subagents, workflows, and repo rules into versioned components — improving portability while raising security stakes.'
tags: ['ai', 'agents', 'plugins', 'mcp']
categories: ['ai']
---

> _"Plugins are to agents what extensions were to browsers and what npm was to JavaScript: the silent infrastructure that turns a powerful tool into an ecosystem."_

**TL;DR:** agentic plugins package skills, subagents, workflows, and rules as installable, versioned components — reducing “mega-prompts”, improving portability, and increasing security risk.

The way we interact with LLMs has evolved in short but profound leaps: from free-form chat to Markdown-structured prompts, from those to variable templates, and now to something qualitatively different — versioned, scoped, and distributable components. If you still organize your prompts in loose text folders, this article is about the infrastructure replacing that.

The community has converged on an open standard that transforms prompts and workflows into **installable, version-controlled, and portable components**: AI Agentic Plugins.

---

## The anatomy of an AI Agentic Plugin

A modern plugin is not a single prompt file. It is a cohesive package made up of interlocking gears:

![AI Agent Plugin](/images/AI-Plugins.excalidraw.png)

### Skills — the "how to do it"

Skills are modular instruction packages that teach the agent to perform a specific task. Typically, a skill is a folder containing a `SKILL.md` file (with YAML metadata and Markdown instructions), optionally accompanied by scripts, templates, and reference documents.

The key conceptual shift is **progressive disclosure**: at the start of a session, the agent loads only the name and description of each skill. The full instructions only enter the context when the task demands it. This allows an agent to have _hundreds_ of skills available without inflating token consumption.

### Agents — the "who does it"

Subagents are specialized personas with their own scope, tools, and permissions. Instead of asking a single generalist agent to do everything, a plugin can declare dedicated agents — one for security review, another for database migration, another for creating pull requests — each loaded only when relevant.

### Workflow — the "when and in what order"

Skills and agents in isolation are not enough. What sets a plugin apart from a simple prompt collection is the **workflow**: hooks that fire at lifecycle points (before a tool runs, after a file is written, when a task completes), slash commands that kick off compound flows, and MCP configurations that connect the agent to external systems.

The workflow ties everything together: the right agent is invoked at the right moment, with the right skill, against the right data source.

### Instructions or Rules — the "how to always behave"

Rules are global constraints that the agent respects in every interaction within a repository. Unlike skills, which are loaded on demand, rules enter the context automatically — which requires discipline when writing them.

A repository-level instructions file at the project root (for example `CLAUDE.md`, `AGENTS.md`, or an equivalent — depending on the tool) can define, for example, that functions must not exceed 30 lines, that commits must follow the Conventional Commits standard, or that certain directories are read-only for the agent. Plugins can package and distribute these files alongside their skills and hooks, ensuring the entire team operates under the same constraints — without relying on verbal agreement or manual configuration.

Keep these rules short and global: every byte of permanent instruction consumes tokens on every iteration.

---

## Skill ≠ Plugin: the distinction that matters

It is worth locking in a difference that confuses many people:

- **Skill** is an _architectural pattern_ — a self-contained unit of knowledge.
- **Plugin** is a _distribution mechanism_ — the versioned package that delivers skills, hooks, agents, and configurations that execute tasks to achieve a specific goal.

A skill sitting in `.claude/skills/` solves a personal workflow. A plugin is what you publish to a marketplace so that other teams can, with a single command, get exactly the same behavior you designed.

---

## Why this is a structural shift

Standardization brings four concrete gains:

**Real modularity.** Skills are developed, tested, and maintained independently. Teams specialize in domains without needing to understand the entire agent.

**Context efficiency.** Loading everything into a single mega-prompt is the anti-pattern of 2024. [Chroma's research](https://www.trychroma.com/research/context-rot) testing 18 frontier models shows that every model exhibits performance degradation as context grows — a phenomenon the team called *context rot*. On the other side, modular loading systems yield measurable gains: the CodeAgents framework achieved a [55–87% reduction in input tokens](https://arxiv.org/abs/2507.03254) by decomposing tasks across specialized agents with reusable sub-routines. Plugins feed directly into this equation: the *progressive disclosure* mechanism — confirmed by [Anthropic's engineering documentation](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) — ensures skills only enter context when the task demands it, while auxiliary files consume *zero tokens* until needed.

**Coherent distribution.** A "serverless deploy on AWS" workflow can involve dozens of rules, three subagents, and four MCP connections. As a plugin, it becomes `/plugin install aws-serverless` and you're done.

**Portability across tools.** The same `SKILL.md` runs on competing agents — Claude Code, Cursor, Codex, Gemini CLI — as long as they follow the open standard. The skill you write today won't die if your team migrates to a different tool tomorrow.

The combination of these four gains has a multiplying effect: an agent that triggers the right subagent (modularity), loads only the necessary skills (context efficiency), uses a versioned and auditable package (distribution) in a portable format (portability) completes the same task in fewer iterations and with fewer tokens than a generalist agent loaded with all instructions at once.

---

## The current ecosystem

The timeline is short and dense. The trend consolidating in 2026 points toward rapid convergence: platforms such as Anthropic, Vercel, OpenAI, Microsoft, and Google are moving toward a shared standard for distributing skills and plugins — the equivalent of npm for agents. As of this post, each tool still operates with its own native format, but the pressure for interoperability is already visible in how the standards are drawing closer together.

### Agentic tools that support plugins (what “support” means here)

In this post, “support plugins” means there is some installable/distributable mechanism (store, registry, repository, or command) that adds capabilities to the agent — such as skills/commands, integrations, and/or workflows — even if the exact format varies across platforms.

Below is the list of tools that support plugins as of the date of this post.

 1. **Claude Code** (Anthropic)
 2. **Cursor**
 3. **OpenAI Codex CLI**
 4. **Gemini CLI** (Google)
 5. **GitHub Copilot**

---

## Security considerations when installing third-party plugins and skills

Standardization is not synonymous with security. Plugins inherit the **full permissions** of the agent they extend — access to your file system, API keys, credentials, and the ability to execute shell commands. Recent audits have found critical issues (malware, prompt injection, exposed secrets) in a non-trivial fraction of skills published on open marketplaces.

Best practices for teams adopting plugins:

- **Prefer official sources** (Anthropic, known vendors, verified repositories).
- **Read the `SKILL.md`** before installing — malicious instructions can hide inside long text.
- **Limit scope and permissions** when the agent supports it.
- **Treat the catalog as a dependency**: pin versions, control updates, run a security scanner in CI.

---

## Where to learn more

Here are some links for those who want to experiment and get hands-on.

* [Cursor's official plugin store](https://cursor.com/marketplace)
* [Gemini CLI plugin store](https://geminicli.com/extensions/)
* [Some of my plugins on GitHub](https://github.com/ArquitetoMovel/markv-ai-plugins)
* [Plugins for GitHub Copilot CLI and VS Code](https://github.com/github/awesome-copilot/blob/main/docs/README.plugins.md)
* [Plugins for Claude and Claude Code](https://claude.com/plugins)

It is worth noting that an AI Agentic Plugin from one store or platform can be easily ported to another.

### Research and technical references

* [Context Rot – Chroma Research (18 frontier models)](https://www.trychroma.com/research/context-rot)
* [Equipping Agents for the Real World with Agent Skills – Anthropic Engineering](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
* [Effective Context Engineering for AI Agents – Anthropic Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
* [CodeAgents: Token-Efficient Multi-Agent Reasoning – ArXiv](https://arxiv.org/abs/2507.03254)
* [Stop Wasting Your Tokens: Efficient Runtime Multi-Agent Systems – ArXiv](https://arxiv.org/abs/2510.26585)
