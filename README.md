# Raspberry_Pi_LLM_Local
Run a local and free LLM on your Raspberry PI



# Tiny LLM on Raspberry Pi (LAN-only, privacy-friendly)

Run a small local LLM on a Raspberry Pi using **Ollama**.
No cloud keys, no Docker required. Your prompts stay on your Pi.

Tested target: Raspberry Pi OS **64-bit**.

---

## Why this is interesting

- **Privacy**: prompts and responses never leave your network
- **Low power**: a Pi can run small models continuously at low energy cost
- **Offline**: works without internet (after model download)
- **Simple API**: Ollama exposes an OpenAI-compatible endpoint (`/v1/*`) that many apps can call

---

## Hardware recommendations

Minimum:
- Pi 4 / Pi 5
- **8GB RAM recommended** (4GB can run smaller models but is tight)

Storage:
- microSD works, SSD is better (models are large)

---




Install curl:

sudo apt install -y curl

Confirm you’re on 64-bit:

uname -m
# expected: aarch64
2) Install Ollama (native)

Install Ollama:

curl -fsSL https://ollama.com/install.sh | sh

Check the service:

sudo systemctl status ollama --no-pager

If it’s not running:

sudo systemctl enable --now ollama
3) Pull a small model

Tiny model (fast, limited reasoning):

ollama pull tinyllama

Better small model (still small, noticeably more capable):

ollama pull qwen2.5:3b

List installed models:

ollama list
4) Run it in the terminal

Interactive chat:

ollama run tinyllama
# or
ollama run qwen2.5:3b

One-shot prompt:

ollama run qwen2.5:3b "Reply with exactly: pong"
5) Use the HTTP API (local)

Ollama listens on port 11434.

List models (OpenAI-style):

curl -s http://127.0.0.1:11434/v1/models | head

Chat completion (OpenAI-style):

curl -s http://127.0.0.1:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen2.5:3b",
    "messages": [{"role": "user", "content": "Say pong"}],
    "temperature": 0,
    "max_tokens": 20
  }'
6) Allow LAN access (optional)

By default, many setups bind to localhost only.
To allow other devices on your LAN to call your Pi:

Find your Pi IP:

hostname -I

Configure Ollama to listen on all interfaces:

Create a systemd override:

sudo systemctl edit ollama

Add this:

[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"

Restart:

sudo systemctl daemon-reload
sudo systemctl restart ollama

Test from another device on the same LAN:

curl -s http://<PI_IP>:11434/v1/models | head
Security note (LAN-only)

If you bind to 0.0.0.0, anyone on your LAN can access the model.
Do NOT expose port 11434 to the public internet.

7) Monitoring

Check the service logs:

journalctl -u ollama -f

See running models:

ollama ps

Stop / restart:

sudo systemctl restart ollama
Troubleshooting
curl: (7) Failed to connect

Ollama not running or not listening where you think it is:

sudo systemctl status ollama --no-pager
ss -ltnp | grep 11434 || true
Slow responses / hangs

Use a smaller model (tinyllama)

Ensure you have enough RAM (8GB recommended)

Prefer SSD over microSD for fewer stalls
## 1) Prerequisites

Update your Pi:
```bash
sudo apt update && sudo apt upgrade -y
