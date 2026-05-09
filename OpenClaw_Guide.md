# OpenClaw: The Complete Guide

*The open-source personal AI assistant and autonomous agent framework.*

## 1. Introduction

**OpenClaw** (formerly known as *Clawdbot*, *Moltbot*, and *Clawd*) is a wildly popular, free, and open-source autonomous AI agent created by Austrian developer Peter Steinberger.

Unlike traditional "on-demand" AI chatbots that wait for a prompt, answer, and stop, OpenClaw operates as a long-running autonomous agent (a "claw"). It runs continuously in the background, checking task lists on a heartbeat, managing files, accessing the web, and communicating with you proactively through everyday messaging apps.

### Core Philosophy

* **True Local Sovereignty:** OpenClaw runs on your own hardware (Mac, Windows, or Linux). Your context, personal preferences, and conversation history stay private.
* **Ubiquitous Interface:** There is no new UI to learn. You interact with OpenClaw via WhatsApp, Telegram, Discord, Signal, Slack, or iMessage.
* **Proactive & Persistent:** It runs 24/7 background tasks, cron jobs, and webhooks. It remembers past context and bridges unrelated conversations into actionable workflows.

---

## 2. Installation & Setup

OpenClaw operates as a long-running Node.js service. You can run it entirely on your own machine or use a managed provider.

### Option A: Local Self-Hosting (Recommended)

You will need Node.js and Git installed on your machine.

1. **Install via CLI:**
```bash
npm install -g openclaw-cli

```


2. **Initialize your environment:**
```bash
openclaw init

```


3. **Start the background service:**

```bash
   openclaw start

```

### Option B: Managed Hosting (e.g., KiloClaw)

If you want to avoid managing 24/7 uptime on your own hardware, services like Kilo.ai offer managed OpenClaw instances.

1. Sign up at a provider like Kilo.ai.
2. Name your agent and provision the instance.
3. Provide an API token for your preferred chat channel (e.g., a Telegram bot token from BotFather).

---

## 3. Configuration

OpenClaw is configured via a global `~/.openclaw/openclaw.json` file or `.env` variables in your workspace.

### Connecting AI Models

OpenClaw is model-agnostic. You can connect it to commercial APIs (Claude, OpenAI, DeepSeek) or run it entirely locally using tools like Ollama.

**Example: Connecting a local Ollama model (e.g., Qwen3-4B or QwQ-32B)**
Add this to your `openclaw.json`:

```json
"providers": {
  "ollama-local": {
    "baseUrl": "http://localhost:11434/v1",
    "api": "openai-completions",
    "models": [{
      "id": "QwQ-32B",
      "name": "Local QwQ Model",
      "contextWindow": 32768
    }]
  }
}

```

*Tip: Test your model connection via the CLI by running `openclaw models test QwQ-32B`.*

### Connecting Chat Interfaces

* **Discord / Slack:** Add your app bot tokens to the `.env` file.
* **Telegram:** Generate a token via BotFather and add `TELEGRAM_BOT_TOKEN=your_token`.
* **WhatsApp:** Install community bridge integrations (e.g., `wechat-publisher` or WhatsApp bridges).

---

## 4. AgentSkills & Capabilities

OpenClaw acts as an "agentic harness." Its true power comes from **AgentSkills**—directories containing a `SKILL.md` file with metadata and tool instructions. There are over 100+ preconfigured bundles in the community registry.

### Key Capabilities:

* **Full System Access:** In a secure sandbox (or full OS access, if granted), OpenClaw can read/write files, run shell commands, and execute scripts.
* **Web & Browser Control:** OpenClaw can navigate websites, scrape data, and fill out forms on your behalf using built-in browser integration.
* **API & Workflow Gluing:** Control smart home devices (Home Assistant, Philips Hue), pull health metrics (WHOOP), or manage media (Spotify, Sonos).

---

## 5. Practical Workflows

Because you interact with OpenClaw through messaging apps, you converse with it as if it were a coworker at a terminal.

### 1. The Autonomous Developer

Connect OpenClaw to GitHub using a fine-grained Personal Access Token (PAT).

> **You (via Telegram):** "Read through my `PraisonAI` repository. Find the outstanding bug regarding memory leaks, write a fix, and open a Pull Request. Do not merge it; just notify me here when it's ready."

### 2. Cross-Platform Task Syncing

If you use multiple machines (e.g., Windows at work, Mac at home), you can sync your `~/.openclaw` directory via iCloud or OneDrive. OpenClaw's intelligent path-adapter plugins resolve path differences automatically.

> **You (on Mac):** `openclaw resume last`
> *(OpenClaw immediately picks up the data-cleaning task you started on your Windows PC three hours prior.)*

### 3. Always-On Monitoring

> **You (via Discord):** "Monitor the webhook for my app's production errors. If an alert comes in while I'm asleep, assess the stack trace. If it's a known timeout issue, restart the server. If it's new, write an incident report to my Obsidian vault."

---

## 6. Security and Enterprise Deployment

While giving an AI full shell access is powerful, it carries risks.

* **The Golden Rule:** Never allow OpenClaw to merge code to your `main` branch or execute destructive commands without human-in-the-loop approval.
* **Sandbox by Default:** Run OpenClaw in its restricted file-system sandbox before giving it global user access.
* **Enterprise Use:** For corporate environments, NVIDIA offers **NemoClaw**, a hardened reference implementation of OpenClaw. It packages the agent with the NVIDIA OpenShell secure runtime and Nemotron local models, ensuring sensitive data (patient records, financials) never leaves the corporate perimeter.

