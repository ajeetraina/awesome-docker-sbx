![stars](https://img.shields.io/github/stars/ajeetraina/awesome-docker-sbx)
![Discord](https://img.shields.io/discord/1020180904129335379)
![Twitter](https://img.shields.io/twitter/follow/collabnix?style=social)


# Awesome Docker Sandboxes (sbx) [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of tools, kits, templates, integrations, and resources for [Docker Sandboxes](https://docs.docker.com/ai/sandboxes/) (`sbx`) — running AI coding agents in isolated microVMs.

<img width="990" height="502" alt="image" src="https://github.com/user-attachments/assets/852c9b6f-1bd8-4110-aa76-e64de3f9b5bc" />


[Docker Sandboxes](https://www.docker.com/products/docker-sandboxes/) is a standalone CLI (`sbx`) that runs AI coding agents like Claude Code, Codex, Gemini CLI, Copilot CLI, OpenCode, and Kiro inside isolated **microVMs** each with its own kernel, Docker daemon, filesystem, and network. The agent can build images, install packages, and modify files in full autonomy without ever touching the host. This list tracks how the community is using, extending, and building on top of `sbx`.


-  🔒 **microVM isolation** 
-  🤖 **Multi-agent support** 
-  🔌 **Kits & templates** 
-  🌐 **Network governance** 
-  🔑 **Credential proxy** 
-  🚀 **One-command setup**

> **Quick start:** `brew install docker/tap/sbx && sbx run claude` - your agent runs in a fully isolated microVM in seconds.


> [!NOTE]
> This is a community-maintained list and is **not affiliated with or endorsed by Docker**. Many entries are early-stage and experimental — most are personal or demo repositories with few or no stars. Inclusion here is **not** an endorsement; check each project's own status, license, and security posture before use. Contributions and corrections welcome (see [Contributing](#contributing)).

## How this list is maintained

This list is **human-curated, with an AI agent assisting discovery.** A [Docker Agent](./workflow/auto-curator-agent/sbx-curator.yaml) (the `sbx` Curator) runs on a schedule via [GitHub Actions](./.github/workflows/sbx-auto-curator.yml). It searches both **GitHub** (repos, kits, templates, via the GitHub search API) and the **wider web** (blog posts, tutorials, articles, and other write-ups, via web search) for new `sbx`-related material, judges each candidate for relevance, maturity, and credential-handling/security posture, and proposes additions to a separate **unreviewed** queue. A human reviews those before anything is promoted into the curated sections above.

The agent is deliberately scoped so it can only append to the unreviewed queue — it never edits the hand-curated entries. Final categorization, wording, and inclusion are human decisions. Contributions from people (issues and PRs) are equally welcome and reviewed the same way.

The curator itself is built with [Docker Agents](https://docs.docker.com/ai/cagent/) and a [Nemotron](https://build.nvidia.com) model — a small, fitting example of the kind of agent tooling this list catalogs.

## Contents

- [Official](#official)
- [Wrappers & CLIs](#wrappers--clis)
- [Kits](#kits)
- [Templates & Images](#templates--images)
- [GUIs & Dashboards](#guis--dashboards)
- [Agent-Specific Setups](#agent-specific-setups)
- [Security & Demos](#security--demos)
- [Guides & Tutorials](#guides--tutorials)
- [Articles & Deep Dives](#articles--deep-dives)
- [Videos & Talks](#videos--talks)
- [Background & Comparisons](#background--comparisons)
- [Videos](#videos)
- [Docker Sandboxes Related Stuff](#docker-sandboxes-related-stuff)
- [Recently discovered (auto-added, unreviewed)](#recently-discovered-auto-added-unreviewed)
- [Contributing](#contributing)

## Official

Resources maintained by Docker.

- [docker/sbx-releases](https://github.com/docker/sbx-releases) — The home for `sbx` releases, installation instructions, and issue tracking. Start here.
- [Docker Sandboxes Documentation](https://docs.docker.com/ai/sandboxes/) — Official docs: get started, usage, agents, customize, architecture, security, CLI reference, and FAQ.
- [docker/sbx-kits-contrib](https://github.com/docker/sbx-kits-contrib) — Community repository for `sbx` kits, maintained under the Docker org. Each top-level directory is a kit with a `spec.yaml`. Enforces verified commit signatures.
- [dockersamples/sbx-quickstart](https://github.com/dockersamples/sbx-quickstart) — Official quickstart walkthrough: mount a workspace, run an agent, work with branches, and explore the network governance panel.
- [Product page: Docker Sandboxes](https://www.docker.com/products/docker-sandboxes/) — Overview, supported agents, and install commands for macOS, Windows, and Linux.

## Wrappers & CLIs

Tools that wrap `sbx` to streamline per-project or per-task setup. A notable pattern: several independent authors have converged on a **declarative config → bootstrapped sandbox** model, suggesting shared appetite for one-command setup above the base CLI.

- [maxkrivich/sbx-toolkit](https://github.com/maxkrivich/sbx-toolkit) — "Dotfiles for sandboxes." Two tools — `sbx-setup` (run once per machine to bake in agent config, tool versions via `mise`, and packages) and `sbx-start` (one command per project, reads `.sbx.toml`). Shareable templates and a machine-level base image model.
- [HenrikPoulsen/sbxgo](https://github.com/HenrikPoulsen/sbxgo) — Single-binary Go CLI focused on **per-repo reproducibility**. Commits `.sbxgo/config.toml` to the repo so the whole team shares one environment; supports kits/templates, drift detection, and sandbox-scoped allow/deny domains. Ships versioned binaries with SHA-256 verification. Agent-agnostic.
- [travisallendotdev/agentbox](https://github.com/travisallendotdev/agentbox) — TypeScript/Bun CLI focused on **per-task bootstrap** for Claude Code. One command (`agentbox up project.yaml`) injects secrets, clones repos, loads skills/plugins/hooks, runs lifecycle phases, and fires the agent with an initial prompt. Build from source.
- [lukehedger/sbox](https://github.com/lukehedger/sbox) — Lightweight shell wrapper for launching **one-off Claude Code sandboxes** with opinionated defaults: branch-mode worktrees, `caffeinate` on macOS, Opus + `--dangerously-skip-permissions`, and `acli`/`bun` baked into a snapshotted template. Note: forwards Anthropic and Atlassian credentials into the sandbox via host env vars rather than the `sbx` credential proxy.
- [thewiw/docker-sbx](https://github.com/thewiw/docker-sbx) — Tooling to install the Docker Sandboxes engine and create/manage sandboxes.
- [acomagu/nix-docker-sbx](https://github.com/acomagu/nix-docker-sbx) — A Nix flake for running Docker Sandboxes on Linux.

## Kits

Declarative YAML artifacts ([kits](https://docs.docker.com/ai/sandboxes/customize/kits/)) that extend a sandbox with tools, skills, credentials, and network rules at runtime. Includes vendor, Docker-team, and community kits.

- [docker/sbx-kits-contrib](https://github.com/docker/sbx-kits-contrib) — The central community kit collection (also under Official). Reference examples for common kit patterns, plus a build-your-own-agent tutorial using the Amp kit.
- [dvdksn/kits-cookbook](https://github.com/dvdksn/kits-cookbook) — A cookbook of example kits for `sbx`, by a Docker docs maintainer. Useful as a pattern reference.
- [dvdksn/sbx-kitty](https://github.com/dvdksn/sbx-kitty) — sbx blueprint for the kitty terminal emulator, by a Docker employee. Good pattern reference for tooling kits.
- [kernel/docker-sbx-kit](https://github.com/kernel/docker-sbx-kit) — Vendor integration from [Kernel](https://www.kernel.sh) (agent browser infrastructure). Mixin kit providing the Kernel CLI, Kernel skills for Claude Code, and proxy-managed Kernel API auth — so `KERNEL_API_KEY` never enters the VM. A clean example of the credential-proxy pattern.
- [HofmeisterAn/docker-sandbox-kit](https://github.com/HofmeisterAn/docker-sandbox-kit) — Minimal kit running the GitHub Copilot agent with .NET SDK 10 pre-installed. The host's `GH_TOKEN` is injected by the sandbox proxy at request time. Loadable directly via `git+https://` reference.
- [shelajev/agy-sbx-kit](https://github.com/shelajev/agy-sbx-kit) — Kit for running Google's Antigravity CLI (`agy`) in an isolated sandbox. By a Docker DevRel.
- [shelajev/vibe-sbx-kit](https://github.com/shelajev/vibe-sbx-kit) — Kit for running Mistral Vibe with `MISTRAL_API_KEY` injected via the host proxy.
- [shelajev/tessl-sbx-kit](https://github.com/shelajev/tessl-sbx-kit) — Kit that installs the Tessl CLI and injects credentials via the proxy.
- [shelajev/little-coder-sbx-kit](https://github.com/shelajev/little-coder-sbx-kit) — Kit for running little-coder with Docker Model Runner.
- [shelajev/sbx-rtk-kit](https://github.com/shelajev/sbx-rtk-kit) — Kit for RTK (Rust Token Killer), which reduces LLM token usage by 60–90% via context compression.
- [shelajev/labspace-demo-sbx-kits-dhi](https://github.com/shelajev/labspace-demo-sbx-kits-dhi) — Labspace wrapper for the sbx kits + Docker Hardened Images demo, featuring a host-backed `ttyd` provider.
- [mikegcoleman/sbx-data-demo](https://github.com/mikegcoleman/sbx-data-demo) — Demo kit that installs a Python data-analysis toolkit, dataset, and demo content into a sandbox.
- [market-dot-dev/docker-sbx-kit](https://github.com/market-dot-dev/docker-sbx-kit) — Kit for Traces.
- [MapuH/sbx-shell-pi](https://github.com/MapuH/sbx-shell-pi) — Kit for running the Pi coding agent in a Docker sandbox.
- [natesilva/sbx-kit-npm-pnpm-hardening](https://github.com/natesilva/sbx-kit-npm-pnpm-hardening) — Mixin kit that hardens npm/pnpm against supply-chain attacks by blocking install scripts and enforcing a 7-day release age gate on packages.
- [protyposis/sbx-kits](https://github.com/protyposis/sbx-kits) — Multi-kit collection: OpenCode with OpenChamber sidecar, GitHub Copilot Enterprise auth, GitLab CLI, OpenCode Go auth, and a traffic-inspection CA (ZScaler) kit.
- [savoisn/sbx-kit-vibe](https://github.com/savoisn/sbx-kit-vibe) — Independent Mistral Vibe sbx kit variant with proxy-managed credential injection.
- [kulla/sbx-kits](https://github.com/kulla/sbx-kits) — Personal kit collection, including an asdf version-manager kit.
- [fgervais/sbx-kit-pyocd](https://github.com/fgervais/sbx-kit-pyocd) — Kit for PyOCD (Open On-Chip Debugger) — useful for embedded/hardware development workflows inside a sandbox.
- [fgervais/sbx-kit-serial-console](https://github.com/fgervais/sbx-kit-serial-console) — Kit providing serial console access inside a sandbox, targeting embedded/hardware use cases.

## Templates & Images

Reusable [template](https://docs.docker.com/ai/sandboxes/customize/templates/) images that bake tools and configuration into a sandbox base image.

- [henrybravo/docker-sandbox-run-copilot](https://github.com/henrybravo/docker-sandbox-run-copilot) — Template for running GitHub Copilot CLI in an isolated environment. One of the most-starred community templates.
- [geut/sbx-shell-pi](https://github.com/geut/sbx-shell-pi) — Custom template image for running Pi inside Docker Sandboxes.
- [nicmeriano/claude-sandbox-template](https://github.com/nicmeriano/claude-sandbox-template) — Custom Claude Code template that brings your global config (skills, statusline, etc.) into the sandbox.
- [holbora/dockbox](https://github.com/holbora/dockbox) — Docker sandbox template with MCP Gateway for Claude Code.
- [VeryEvilHumna/claude-bun-docker-sandbox](https://github.com/VeryEvilHumna/claude-bun-docker-sandbox) — Claude template with the Bun runtime preinstalled.
- [ealeyner/nanoclaw-sbx-template](https://github.com/ealeyner/nanoclaw-sbx-template) — Template that pre-bakes NanoClaw's prerequisites (Node, pnpm, build tools).
- [codingforentrepreneurs/opencode-ollama-sbx-template](https://github.com/codingforentrepreneurs/opencode-ollama-sbx-template) — Template for OpenCode + Ollama.
- [travishathaway/docker-sandbox-claude-code-bedrock](https://github.com/travishathaway/docker-sandbox-claude-code-bedrock) — Image for running Claude Code with Amazon Bedrock inside a sandbox.
- [travishathaway/docker-sandbox-opencode-bedrock](https://github.com/travishathaway/docker-sandbox-opencode-bedrock) — Image for running OpenCode with Amazon Bedrock inside a sandbox.
- [alDuncanson/hermit](https://github.com/alDuncanson/hermit) — Custom template for running OpenClaw via Ollama in an isolated sandbox.
- [pavlov-net/sbx-templates](https://github.com/pavlov-net/sbx-templates) — Custom sandbox templates for `sbx`.
- [erkannt/sbx-templates](https://github.com/erkannt/sbx-templates) — Personal collection of `sbx` templates.
- [vskh-docker-images/sandbox-templates](https://github.com/vskh-docker-images/sandbox-templates) — Custom base images for Docker sandboxes.
- [dlorych/docker-ai-sandbox-templates](https://github.com/dlorych/docker-ai-sandbox-templates) — Common sandbox templates reused across projects.
- [GenAI4RSE/sandbox](https://github.com/GenAI4RSE/sandbox) — Enhanced Docker sandbox templates for running AI agents safely (published to Docker Hub).
- [shaftoe/sbx-template-pi](https://github.com/shaftoe/sbx-template-pi) — Template image for the Pi coding agent.
- [caynev/sbx-pi](https://github.com/caynev/sbx-pi) — Template for Pi.
- [trq/pi-sandbox](https://github.com/trq/pi-sandbox) — Minimal template for running vanilla Pi in the `shell` sandbox environment.

## GUIs & Dashboards

Graphical and desktop tools for managing sandboxes, agents, and worktrees.

- [mdelapenya/biomelab](https://github.com/mdelapenya/biomelab) — Desktop GUI for managing git worktrees and coding agents running in Docker Sandboxes (`sbx`). One sandbox per agent per repo; real-time sandbox lifecycle controls (create/start/stop/remove), worktree cards showing branch, dirty, sync, and PR status, multi-repo dashboard, and agent detection for Claude, Kiro, Copilot, Codex, OpenCode, and Gemini.
- [beachead-dev/beachead](https://github.com/beachead-dev/beachead) — Tauri 2.0 desktop app (Rust/React) for managing Docker Sandbox microVMs. Features: Personas (agent configs), Sessions (terminal windows via xterm.js), Network Policies, and per-persona Memory backed by MCP containers in Docker. SQLite local storage.

## Agent-Specific Setups

Personal and project setups focused on a particular agent or environment.

- [alexdaiii/claude-agent-sbx](https://github.com/alexdaiii/claude-agent-sbx) — Running Claude Code inside Docker Sandboxes with ACP for JetBrains.
- [jbradford/sbx-pi-agent](https://github.com/jbradford/sbx-pi-agent) — A personal setup for running the Pi agent in `sbx`.
- [TheTipTapTyper/little-coder-sbx-setup](https://github.com/TheTipTapTyper/little-coder-sbx-setup) — Setup for running the little-coder AI coding agent with a local LLM.
- [fieryWalrus1002/homelab-pi-sbx](https://github.com/fieryWalrus1002/homelab-pi-sbx) — Getting pi.dev working in `sbx` on Ubuntu 24.04.
- [wluberti/sandbox](https://github.com/wluberti/sandbox) — Project to run agentic agents and harnesses in `sbx`.
- [cuolm/pi-sbx-llamacpp](https://github.com/cuolm/pi-sbx-llamacpp) — Run the Pi coding agent in a Docker Sandbox microVM with a local llama-server as inference backend. Includes architecture diagram, `spec.yaml`, and `models.json` for provider config.
- [wireless25/crush-sandbox](https://github.com/wireless25/crush-sandbox) — Docker sandbox for the Crush CLI (Charmbracelet) with Git worktree support for multi-branch parallel agent work and per-workspace persistent caching.

## Security & Demos

Projects exploring the security boundary, threat models, and isolation guarantees of `sbx` — relevant given the product's core purpose.

- [zickgraf-ai/agentic-press](https://github.com/zickgraf-ai/agentic-press) — Reference architecture for secure agentic AI development: MCP injection filtering, audit logging, and more.
- [kiview/you-gotta-keep-the-dogs-away](https://github.com/kiview/you-gotta-keep-the-dogs-away) — Demo code for the JCon 2026 talk "You Gotta Keep the Dogs Away" — sandboxing a malicious MCP server.
- [sebbmn/secure-hermes-sandbox](https://github.com/sebbmn/secure-hermes-sandbox) — Hermes agent in `sbx` with a web-search proxy sanitizer and Firecrawl.

## Guides & Tutorials

Step-by-step, task-focused walkthroughs.

- [Get started with Docker Sandboxes](https://docs.docker.com/ai/sandboxes/get-started/) — Official first-session guide: install, authenticate, run, branch modes, and cleanup.
- [Customizing sandboxes](https://docs.docker.com/ai/sandboxes/customize/) — How templates and kits work and when to use each.
- [Build your own agent kit](https://docs.docker.com/ai/sandboxes/customize/build-an-agent/) — Walkthrough of building an agent kit (using Amp) from base-image choice to invocation.
- [Run Claude Code in a Docker Sandbox with Docker Model Runner](https://docs.docker.com/guides/claude-code-sandbox-model-runner/) — Point Claude Code at a local model served by Docker Model Runner via `ANTHROPIC_BASE_URL` and a policy rule.
- [Customized templates with Docker sandbox](https://andrewlock.net/running-ai-agents-with-customized-templates-in-docker-sandbox/) — Andrew Lock on building custom templates: adding tools to the default base, or layering `sbx` tooling onto a different base image.

## Articles & Deep Dives

Opinion, analysis, and hands-on reports.

### Docker Official Blog

- [A New Approach for Coding Agent Safety](https://www.docker.com/blog/docker-sandboxes-a-new-approach-for-coding-agent-safety/) — Launch post: the problem with running agents in containers vs. microVMs, the four-layer security model, and the roadmap. (Nov 2025)
- [Highlights from AWS re:Invent: Supercharging Kiro with Docker Sandboxes and MCP Catalog](https://www.docker.com/blog/aws-reinvent-kiro-docker-sandboxes-mcp-catalog/) — How the Kiro agent integrates with `sbx` and the MCP Catalog. By Michael Irwin. (Dec 2025)
- [Docker Sandboxes: Run Claude Code and Other Coding Agents Unsupervised (but Safely)](https://www.docker.com/blog/docker-sandboxes-run-claude-code-and-other-coding-agents-unsupervised-but-safely/) — Product deep-dive: YOLO mode, the credential proxy pattern, network policies, and git branch mode. (Jan 2026)
- [Running NanoClaw in a Docker Shell Sandbox](https://www.docker.com/blog/run-nanoclaw-in-docker-shell-sandboxes/) — Walkthrough of running NanoClaw via `sbx shell`, by Oleg Selajev. (Feb 2026)
- [Run OpenClaw Securely in Docker Sandboxes](https://www.docker.com/blog/run-openclaw-securely-in-docker-sandboxes/) — How to use OpenClaw inside `sbx` with credential isolation. (Feb 2026)
- [Secure Agent Execution with NanoClaw and Docker Sandboxes](https://www.docker.com/blog/nanoclaw-docker-sandboxes-agent-security/) — Deeper look at agent security with NanoClaw; covers template customization and policy rules. (Mar 2026)
- [Building AI Teams: How Docker Sandboxes and Docker Agent Transform Development](https://www.docker.com/blog/building-ai-teams-docker-sandboxes-agent/) — Docker Captains Esteban Maya Cadavid and Marco Franzon on orchestrating multiple coding agents with `sbx` and Docker Agent. (Mar 2026)
- [A Virtual Agent Team at Docker: How the Coding Agent Sandboxes Team Uses a Fleet of Agents to Ship Faster](https://www.docker.com/blog/a-virtual-agent-team-at-docker-how-the-coding-agent-sandboxes-team-uses-a-fleet-of-agents-to-ship-faster/) — Inside look at how the `sbx` team uses their own product to parallelize development across branches. By Manuel de la Peña. (May 2026)
- [Comparing Different Approaches to Sandboxing](https://www.docker.com/blog/comparing-sandboxing-approaches-ai-agents/) — Docker Captain Siri Varma Vegiraju compares container isolation, microVMs, and managed sandbox APIs on trust boundary, cold start, and persistence. (May 2026)
- [Docker AI Governance: Unlock Agent Autonomy, Safely](https://www.docker.com/blog/docker-ai-governance-unlock-agent-autonomy-safely/) — How `sbx` network policies, the credential proxy, and audit logging compose into a governance layer for agentic AI. (May 2026)
- [Coding Agent Horror Stories: The Security Crisis Threatening Developer Infrastructure](https://www.docker.com/blog/ai-coding-agent-horror-stories-security-risks/) — Ajeet Raina on real-world credential leak and supply-chain incidents from running agents without proper isolation. (May 2026)
- [The Untrusted Autonomous Workload: How AI Coding Agents Reshape What Isolation Has to Do](https://www.docker.com/blog/untrusted-autonomous-workload-ai-sandboxes/) — Docker Captain Vladimir Mikhalev on the threat model of agentic workloads and why process-level isolation isn't enough. (May 2026)
- [Coding Agent Horror Stories: The rm -rf ~/ Incident](https://www.docker.com/blog/coding-agent-horror-stories-the-rm-rf-incident/) — Second in Ajeet Raina's horror stories series: file-system destruction when agents run without a safety boundary. (Jun 2026)
- [How to Secure AI Agents: A Practical Overview for Development Teams](https://www.docker.com/blog/how-to-secure-ai-agents/) — Srini Sekaran on the full spectrum of controls: network, credential, filesystem, and process isolation in practice. (Jun 2026)

### Community & Independent

- [Stop Running Agents in Containers. Run Them in MicroVMs with Docker sbx](https://www.ajeetraina.com/stop-running-agents-in-containers-run-them-in-microvms-with-docker-sbx/) — A hands-on tour of the full `sbx` experience: install, auth, shell sandbox, Claude Code sandbox, and port publishing, focused on the container-vs-microVM trust boundary.
- [Docker Sandboxes: Running AI Agents in YOLO Mode, Safely](https://www.msbiro.net/posts/docker-sandboxes-ai-agents/) — Matteo Bisi installs, breaks, fixes, and runs GitHub Copilot CLI inside a sandbox on an M4 MacBook — verifying the security claims hands-on, including real-world policy workarounds.
- [Running AI agents safely in a microVM using Docker sandbox](https://andrewlock.net/running-ai-agents-safely-in-a-microvm-using-docker-sandbox/) — Andrew Lock on network policies, direct vs. branch git modes, and the practical implications of `--dangerously-skip-permissions`.
- [Docker Sandboxes (sbx) Quick Start](https://pageai.pro/blog/docker-sandboxes-sbx-quick-start) — The three workflow patterns that separate "tried it once" from "use it daily," including non-interactive prompt runs.
- [How Safe Is Docker Sandbox? Testing AI Agents with Java](https://rabauer.dev/blog/docker-sbx) — Johannes Rabauer and Kevin Wittek (Docker) put `sbx` through its paces against a deliberately vulnerable Java/Maven project designed to leak credentials — covering credential proxying, Docker-in-Docker (Testcontainers), port forwarding, and local LLMs via Docker Model Runner.

## Videos & Talks

Recorded demos, conference talks, live sessions, and walkthroughs.

- [How Safe Is Docker Sandbox? Testing AI Agents with Java](https://www.youtube.com/watch?v=I-FqemEnUAc) — ~2h live session by Johannes Rabauer and Kevin Wittek (Docker): running a malicious Java/Maven project through `sbx` in YOLO mode, covering credential proxying, Docker-in-Docker (Testcontainers), port forwarding, local LLMs via Docker Model Runner, and `sbx` kits. Apr 2026.

## Background & Comparisons

Context on the architecture and how `sbx` compares to alternatives.

- [Architecture | Docker Docs](https://docs.docker.com/ai/sandboxes/architecture/) — How sandboxes work under the hood: microVM isolation, workspace mounting via filesystem passthrough, storage, networking, and lifecycle.
- [Docker Sandbox: Running AI Agents in Isolated Docker Environments](https://www.morphllm.com/docker-sandbox) — A technical comparison of container isolation vs. `sbx`'s microVM model, the four-layer security model, and how managed sandbox APIs compare on cold start, persistence, and pricing.

## Videos

Demo screencasts, live sessions, and tutorials about Docker Sandboxes.

| Thumbnail | Details |
|:---------:|:--------|
| [![Docker Sandboxes Hands-On Guide](https://img.youtube.com/vi/kNGXuIPXR24/hqdefault.jpg)](https://youtu.be/kNGXuIPXR24) | **[Docker Sandboxes Hands-On Guide – A Safe Space for AI Agents!](https://youtu.be/kNGXuIPXR24)** <br> A practical hands-on tour covering YOLO mode, microVM isolation, and network policy tiers — referenced across community write-ups as a key getting-started resource. |
| [![How Safe Is Docker Sandbox? Testing AI Agents with Java](https://img.youtube.com/vi/I-FqemEnUAc/hqdefault.jpg)](https://www.youtube.com/watch?v=I-FqemEnUAc) | **[How Safe Is Docker Sandbox? Testing AI Agents with Java](https://www.youtube.com/watch?v=I-FqemEnUAc)** <br> ~2-hour live session by Johannes Rabauer featuring Kevin Wittek (Docker). Runs a deliberately vulnerable Java/Maven project through Docker Sandbox to test YOLO mode containment, credential-leak blocking, Docker-in-Docker (Testcontainers), port forwarding, and local LLM setup. _(Apr 30, 2026)_ |
| [![Deploying Pi Coding Agents in Docker Sandboxes](https://img.youtube.com/vi/P7AZ-iDbIoc/hqdefault.jpg)](https://youtube.com/watch?v=P7AZ-iDbIoc) | **[Deploying Pi Coding Agents in Docker Sandboxes](https://youtube.com/watch?v=P7AZ-iDbIoc)** <br> Details how to deploy the Pi coding agent inside a secure isolated microVM using the `sbx` CLI, from the Collabnix / Docker and DevOps channel. _(Jun 10, 2026)_ |

## Docker Sandboxes Related Stuff

Adjacent Docker resources commonly used with Docker Sandboxes.

- [Docker AI docs](https://docs.docker.com/ai/) — Entry point for Docker AI tooling, including Sandboxes, Agent, and related workflows.
- [Docker Agent docs](https://docs.docker.com/ai/cagent/) — Documentation for Docker Agent used in this repository’s auto-curator workflow.

## Recently discovered (auto-added, unreviewed)

- [opscart/docker-sandbox-devops](https://github.com/opscart/docker-sandbox-devops) — Hands-on exploration of Docker Sandboxes — labs, architecture notes, threat model, and DevOps templates for running AI coding agents in isolated microVMs. _Maturity: 0⭐, no releases. Security: proxy. Auto-added 2026-05-25, unreviewed._
- [bouli/sbx-cline](https://github.com/bouli/sbx-cline) — A collection of scripts to use cline in docker sandbox. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-25, unreviewed._
- [shelajev/lapdog-sbx-kit](https://github.com/shelajev/lapdog-sbx-kit) — Docker Sandboxes kit that installs Datadog Lapdog and transparently wraps the sandbox's claude binary so every LLM session is captured locally (and optionally forwarded to Datadog LLM Observability). _Maturity: 0⭐, no releases. Security: proxy. Auto-added 2026-05-25, unreviewed._
- [kantegamartin/sbx-claude](https://github.com/kantegamartin/sbx-claude) — Containerizing Claude with Docker Sandbox. _Maturity: 0⭐, no releases. Security: proxy. Auto-added 2026-05-25, unreviewed._
- [AlekseiKanash/bal-sbx](https://github.com/AlekseiKanash/bal-sbx) — Sandboxing tools for LLM agents. _Maturity: 0⭐, no releases. Security: host-env creds. Auto-added 2026-05-25, unreviewed._

- [ThiagoCarmona/claude-pg-devcontainer](https://github.com/ThiagoCarmona/claude-pg-devcontainer) — Dev Container for Claude: **Claude Code + embedded PostgreSQL + MCPs + dev toolkit**. _Maturity: 0⭐, no releases. Security: host-env creds. Auto-added 2026-06-02, unreviewed._
- [nick22985/sbx](https://github.com/nick22985/sbx) — Sandboxed Docker dev environments with multiple language/runtime presets (npm, bun, rust, java, and more). _Maturity: 0⭐, 5 release(s). Security: n/a. Auto-added 2026-06-02, unreviewed._
- [CauldronDevelopmentLLC/sbx](https://github.com/CauldronDevelopmentLLC/sbx) — A tool for easily sandboxing applications in Linux. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-06-02, unreviewed._
- [SamirSaidani/sbx-claude-kit](https://github.com/SamirSaidani/sbx-claude-kit) — Docker sbx mixin kit for Claude Code: persistent settings.json + PulseAudio tunnel for audio/voice mode. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-30, unreviewed._
- [kevbot-git/sandbox-kits](https://github.com/kevbot-git/sandbox-kits) — A collection of reusable kits for Docker's `sbx`. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-30, unreviewed._
- [la-test/sbx1-upptime](https://github.com/la-test/sbx1-upptime) — Sandbox to test and learn Upptime template. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-30, unreviewed._
- [baby-whales-pod/sbx-claude-code-minio](https://github.com/baby-whales-pod/sbx-claude-code-minio) — Setup for running Claude Code in Docker Sandboxes with MinIO integration. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-30, unreviewed._
- [wmeints/sbx-tooling](https://github.com/wmeints/sbx-tooling) — My personal customizations for Docker Sandboxes. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-30, unreviewed._
- [kevbot-git/sandboxd](https://github.com/kevbot-git/sandboxd) — Sandbox'd: A light wrapper over Docker's sbx, with quality of life improvements and a kits manager. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-30, unreviewed._
- [lucatume/sbc](https://github.com/lucatume/sbc) — Docker sbx sandbox system wrapper aimed at Claude Code. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-30, unreviewed._
- [saharshbhansali/docker-sbx-flake](https://github.com/saharshbhansali/docker-sbx-flake) — A flake that packages docker-sandboxes. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-30, unreviewed._
- [typisttech/sbx-kits](https://github.com/typisttech/sbx-kits) — Collection of reusable kits for Docker Sandboxes. _Maturity: 0⭐, no releases. Security: n/a. Auto-added 2026-05-30, unreviewed._
- [ajeetraina/awesome-docker-sbx](https://github.com/ajeetraina/awesome-docker-sbx) — A curated list of tools, kits, templates, integrations, and resources for Docker Sandboxes (sbx) running AI coding agents in isolated microVMs. _Maturity: 2⭐, no releases. Security: n/a. Auto-added 2026-05-29, unreviewed._

## Contributing

## Contributing

1. Make sure the entry is genuinely related to Docker Sandboxes (`sbx`) — not other sandbox products (e.g. E2B, Blaxel).
2. Add it to the most relevant section, keeping the one-line format: `[name](url) — what it is and why it's useful.`
3. For tools, kits, and templates, note maturity honestly (e.g. experimental, no releases) so readers can judge for themselves.
4. Keep descriptions neutral and factual; this is a reference, not a marketing page.

Open a pull request or an issue with the link and a short description.
