# Nemesis Red — Getting Started

Super simple steps. Each one is a self-contained mini-guide you can follow (or film) in a minute or two. You bring a Kali box and an AI key (or run fully offline). Nemesis Red does the rest.

---

## 1. Install and run (2 minutes)

1. Install **Docker** (Desktop on Mac/Windows, Engine on Linux).
2. Pull the image:
   ```bash
   docker pull ghcr.io/eobi/nemesis-red:latest
   ```
3. Run it, pointed at your Kali and your AI key:
   ```bash
   docker run --rm -p 8000:8000 \
     -e KALI_HOST=<your-kali-ip> -e KALI_USERNAME=<kali-user> -e KALI_PASSWORD=<kali-password> \
     -e ANTHROPIC_API_KEY=<your-key> \
     ghcr.io/eobi/nemesis-red:latest
   ```
   `KALI_USERNAME` is optional — it defaults to `kali`. Set it only if your box uses a different user (like `root`).

   **Example** (with real-looking values — just swap in yours):
   ```bash
   docker run --rm -p 8000:8000 \
     -e KALI_HOST=192.168.1.50 -e KALI_USERNAME=kali -e KALI_PASSWORD=kali \
     -e ANTHROPIC_API_KEY=sk-ant-api03-Xa9k...your-real-key \
     ghcr.io/eobi/nemesis-red:latest
   ```
4. Open **http://127.0.0.1:8000** and create your account.

That's it. You're in, and already connected to your Kali.

---

## 2. Connect your Kali and install the tools

1. You need a **Kali box** reachable over SSH (a VM, a spare host, anything). Stock Kali works.
2. It auto-connects from the `KALI_HOST` you passed. To add or switch, use **Settings → Connections**.
3. Open the **Install** page → pick your Kali → **Install**.
4. Watch it provision the full arsenal (290+ tools). Already-installed tools are skipped, so it's safe to re-run any time.

> Works on Kali out of the box (Debian-based hosts too, for the tools in their repos).

---

## 3. Pick your AI — cloud, or fully offline

Whatever you choose is used everywhere: Copilot, Vulnerability Assessment, and Pentest.

**Cloud (Claude or GPT):** add one of these env vars to your `docker run`, then pick it in **Settings → LLM**.
```bash
-e ANTHROPIC_API_KEY=<your-key>    # Claude
-e OPENAI_API_KEY=<your-key>       # OpenAI
```
**Example** (full command, Claude):
```bash
docker run --rm -p 8000:8000 \
  -e KALI_HOST=192.168.1.50 -e KALI_USERNAME=kali -e KALI_PASSWORD=kali \
  -e ANTHROPIC_API_KEY=sk-ant-api03-Xa9k...your-real-key \
  ghcr.io/eobi/nemesis-red:latest
```

**Fully offline (any local model, zero cost):**
1. Install [Ollama](https://ollama.com), pull a model: `ollama pull qwen2.5-coder:7b` (or `ollama pull llama3`, `mistral`, `deepseek-coder`...).
2. Add `-e OLLAMA_HOST=<your-ollama-url>` to your `docker run`.
3. Pick your model in **Settings → LLM** — anything you've pulled shows up automatically. Nothing leaves your machine.

**Example** (full command, fully offline — no cloud key at all):
```bash
docker run --rm -p 8000:8000 \
  -e KALI_HOST=192.168.1.50 -e KALI_USERNAME=kali -e KALI_PASSWORD=kali \
  -e OLLAMA_HOST=http://host.docker.internal:11434 \
  ghcr.io/eobi/nemesis-red:latest
```

---

## 4. AI Copilot — run a tool, get your next move

1. Open the **console**.
2. Run any tool in a session (e.g. `nmap -sV <target>`).
   **Example:** `nmap -sV 192.168.1.50`
3. Click the **✨ Intellicense** button on that session.
4. Read: **Found** → **Why it matters** → **Next steps** — then click **Run** on a suggested command.

The AI reads your raw output and hands you the next commands. You stay in control.

---

## 5. Autonomous Vulnerability Assessment

1. Open **Vulnerability Assessment**.
2. Enter a **web URL** or a **network/CIDR**, pick a depth (Quick → Exhaustive).
   **Examples:** `https://example.com` · `192.168.1.0/24` · `10.0.0.5`
3. Click **Launch**.
4. Watch the live reasoning graph build and findings pour in.
5. Click **Download Report (PDF)** and **Download Remediation (PDF)**.

Hundreds of CVE-backed findings per run. No babysitting.

---

## 6. Autonomous Pentest (Beta)

1. From a completed assessment, open **Pentest**.
2. Choose your envelope: **safe** (no destructive commands) and **harm** (stop at first win, or keep going).
3. Click **Launch Pentest**.
4. Watch exploits fire, and check **Captured Loot** — real credentials, hashes, and shells, with the full exploit chain.
5. **Download Report (PDF)** for proof you can hand off.

Not a summary. Actual compromise, proven.

---

## 7. Schedule recurring scans (Business / Enterprise)

1. Open **Scheduled Tasks**.
2. Click **New scheduled task** → set the target and depth.
3. Choose **One-time** (pick a date/time) or **Repeating** (a cron preset: hourly, daily 3am, weekly...).
4. Click **Create**.

It runs itself on schedule. Set your domains once, get continuous coverage.

---

## That's the whole product

Point it at your Kali, pick your AI, and let it work: recon, assessment, exploitation, and scheduled coverage — all local, all yours.

- **Sign up / console:** [app.nemesislabs.xyz](https://app.nemesislabs.xyz/register)
- **Image:** `ghcr.io/eobi/nemesis-red:latest`
- **More:** [nemesislabs.xyz](https://nemesislabs.xyz)

> Authorized testing only. Use Nemesis Red only against systems you own or have explicit written permission to assess.
