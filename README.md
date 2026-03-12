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

## 1) Prerequisites

Update your Pi:
```bash
sudo apt update && sudo apt upgrade -y
