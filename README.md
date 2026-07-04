<div align="center">

# Nemesis Red

### Autonomous, LLM-driven offensive security in a single Docker image.

One container orchestrates **290+ Kali tools** over SSH to **your own** Kali box, reasons over the output with **your own** LLM key, and drives the full loop: recon, vulnerability assessment, and live exploitation with proof. Your targets, traffic, and findings never leave your environment.

![Platform](https://img.shields.io/badge/platform-linux%2Famd64%20%7C%20arm64-blue)
![Delivery](https://img.shields.io/badge/delivery-docker-2496ED)
![BYO](https://img.shields.io/badge/BYO-LLM%20key%20%7C%20local%20Ollama-7c5cff)
![License](https://img.shields.io/badge/license-commercial-lightgrey)

[Get started](#quickstart-2-minutes) · [Create an account](https://app.nemesislabs.xyz/register) · [nemesislabs.xyz](https://nemesislabs.xyz)

</div>

![Nemesis Red operator console](assets/01-operator-console.png)

---

## What it is

Nemesis Red is a cyber-operations platform for penetration testers. It runs as an orchestrator: the reasoning happens locally on your machine, the tools run on your Kali box over SSH, and the control plane only handles licensing. You bring the Kali, you bring the LLM key (or point it at a local Ollama), and Nemesis Red does the driving.

Three ways to work, one platform:

- **Copilot console.** A 290-tool arsenal with live multi-session terminals and an AI that reads every command's output, tells you why it matters, and hands you the next commands to run with one click.
- **Autonomous Vulnerability Assessment.** Point it at a target (web URL, CIDR, AD realm, k8s, cloud account, mobile app, LLM API), pick a depth, and it plans and runs a full assessment as a live DAG. Hundreds of findings per run, cross-tool CVE fusion, and a signed PDF report at the end.
- **Autonomous Pentest.** Take the findings from an assessment straight into live exploitation. It selects exploits, fires them, and captures real proof: credentials, hashes, database dumps, and shells.

Everything is gated server-side by a short-lived signed license, so the tool cannot be cracked by editing the image, and every target stays inside your network.

---

## See it work

### AI copilot that thinks alongside you

The Intellicense copilot reads raw tool output and turns it into a plan: what was found, why it matters, and the exact next commands to run. Bring your own frontier model (Claude, GPT) or run it fully local on Ollama.

![AI copilot analyzing scan output](assets/02-ai-copilot.png)

### Autonomous vulnerability assessment as a live DAG

Launch an assessment and watch the reasoning graph build in real time: recon feeds enumeration, enumeration feeds identification, and the LLM fuses tool output into CVE-backed findings. Export a full report and a remediation plan as PDF.

![Autonomous vuln assessor, network target](assets/03-vuln-assessor-network.png)

Works across target types, with evasive routing and a browser stage for web apps:

![Autonomous vuln assessor, web target](assets/04-vuln-assessor-web.png)

### An AI browser agent that logs in for you

Black-box or white-box, the browser agent drives a headed Chromium: it maps the app surface, logs in, and runs authenticated attacks (broken access control, CSRF, XSS, JWT, session), all captured through a proxy.

![AI browser agent](assets/06-ai-browser-agent.png)

### Live exploitation with captured proof

The pentest engine turns findings into action and proves compromise: captured credentials and hashes, database dumps, and opened sessions, with a full exploit chain and audit log.

![Live exploitation with captured loot](assets/05-pentest-exploitation.png)

> The pentest engine is currently in **beta**: actively monitored and improving fast.

---

## Quickstart (2 minutes)

You need three things: a Kali box you control (any VM or host with SSH), an LLM key or a local Ollama, and Docker.

**1. Pull the image**

```bash
docker pull ghcr.io/eobi/nemesis-red:latest
```

Multi-arch: `linux/amd64` and `linux/arm64` (Apple Silicon included).

**2. Run it, pointed at your Kali and your LLM key**

```bash
docker run --rm -p 8000:8000 \
  -e KALI_HOST=<your-kali-ip> -e KALI_PASSWORD=<kali-password> \
  -e OPENAI_API_KEY=<your-llm-key> \
  ghcr.io/eobi/nemesis-red:latest
```

**3. Open the console**

Go to [http://127.0.0.1:8000](http://127.0.0.1:8000). It is already connected to your Kali.

That is the Free tier, running locally with no license key. To unlock Pro, Business, or Enterprise features, add your key:

```bash
  -e RED_LICENSE_KEY=<your-key-from-the-console> \
```

Prefer Claude, or a local model? Swap the LLM env var:

```bash
  -e ANTHROPIC_API_KEY=<your-anthropic-key>     # Claude
  -e OLLAMA_HOST=http://<your-ollama>:11434     # fully local, zero cost
```

Get a license key and manage billing at [app.nemesislabs.xyz](https://app.nemesislabs.xyz/register).

---

## Configuration

Everything is driven by environment variables. There is no file to edit.

| Variable | What it does |
|---|---|
| `KALI_HOST` / `KALI_PORT` / `KALI_USERNAME` / `KALI_PASSWORD` | Your Kali SSH target (defaults: port 22, user `kali`, password auth). Change it any time from the dashboard, or restart with a new host. |
| `KALI_KEY_PATH` | Use an SSH key instead of a password (mount the key and point here). |
| `OPENAI_API_KEY` / `ANTHROPIC_API_KEY` / `OLLAMA_HOST` | Bring your own LLM. Inference runs client-side; you pay your provider directly, or run local Ollama for free. |
| `RED_LICENSE_KEY` | Your activation key. Unlocks your tier. Runs Free if unset. |

Advanced: bind-mount your own `kali_config.ini` at `/app/kali_config.ini` if you prefer a file.

---

## Tiers

| | Free | Pro | Business | Enterprise |
|---|---|---|---|---|
| Vulnerability assessment | 1 target | 10 targets | Unlimited | Unlimited |
| Autonomous pentest | 3-run taste | Full | Full | Full |
| AI copilot | Local model | Frontier | Team-shared | On-prem option |
| Scheduling / continuous | — | Limited | Continuous | Continuous + SLA |
| Reports | Watermarked | All formats | + Compliance, API | + White-label |

Self-serve, monthly or annual, no setup fee. See current pricing and sign up at [nemesislabs.xyz](https://nemesislabs.xyz).

---

## How it stays yours (and safe)

- **Your data never leaves.** Tools run on your Kali, against your targets. Findings and traffic stay in your environment. The cloud only issues a signed license token.
- **Bring your own key.** No token metering. Use your own OpenAI or Anthropic key, or run a local Ollama model at zero cost.
- **Source-protected.** The engine ships compiled, with no readable source and no exploit recipes baked in. Premium content is fetched at runtime and license-gated.
- **Device-bound licensing.** Short-lived, device-fingerprinted tokens with heartbeat and revocation, so credentials cannot be shared.

---

## Requirements

- **Docker** (Desktop or Engine), amd64 or arm64.
- **A Kali box you control**, reachable over SSH. A stock Kali VM works. The engine installs any missing tools it needs.
- **An LLM key** (OpenAI or Anthropic) **or a local Ollama**. For the Free tier a local model is enough.

---

## Links

- **Sign up / console:** [app.nemesislabs.xyz](https://app.nemesislabs.xyz/register)
- **Product site:** [nemesislabs.xyz](https://nemesislabs.xyz)
- **Image:** `ghcr.io/eobi/nemesis-red:latest`

---

<div align="center">

Nemesis Red is a product of **Nemesis Labs**. Authorized testing only: use it only against systems you own or have explicit written permission to assess.

</div>
