# Ollama + Local LLM Setup Guide (Windows)

**Author:** Lovnish Verma | **Platform:** Windows 11 | **Reference Hardware:** MSI Cyborg 15 A12U (RTX 2050, 4GB VRAM)

---

## What is Ollama?

Ollama is a runtime that lets you download and run Large Language Models (LLMs) entirely on your own machine — no internet connection needed after setup, no API costs, no data leaving your device.

**Why use it over cloud APIs?**

| Feature | Ollama (Local) | Cloud API |
|---|---|---|
| Privacy | ✅ 100% local | ❌ Data sent to server |
| Cost | ✅ Free | ❌ Per-token billing |
| Offline use | ✅ Yes | ❌ No |
| Speed | Depends on GPU | Generally faster |
| Model choice | Limited by VRAM | Unlimited |

Ollama exposes an OpenAI-compatible API on `http://localhost:11434`, so any tool that supports OpenAI can use it locally.

---

## System Requirements

| Component | Minimum | Recommended |
|---|---|---|
| OS | Windows 10 | Windows 11 |
| GPU | NVIDIA (any CUDA-capable) | RTX 3050 / 4060 or better |
| VRAM | 4 GB | 8 GB+ |
| RAM | 8 GB | 16 GB |
| Storage | 10 GB free | 50 GB+ SSD |
| Drivers | CUDA 11.x | CUDA 12.x (latest) |

> **RTX 2050/3050 note:** 7B parameter models work well. 14B models will be slow and may partially run on CPU. Avoid 32B+ entirely.

---

## Step 1 — Install Ollama

**Option A — GUI Installer (recommended for beginners)**

Download from: https://ollama.com/download  
Run the `.exe` and follow the wizard. Ollama installs as a background Windows service.

**Option B — PowerShell one-liner**

```powershell
irm https://ollama.com/install.ps1 | iex
```

**Verify installation:**

```powershell
ollama --version
# Expected output: ollama version is 0.x.x
```

---

## Step 2 — Update NVIDIA Drivers

Outdated drivers are the #1 cause of GPU detection failures.

```powershell
nvidia-smi
```

If this command fails or shows an old driver, update at:  
https://www.nvidia.com/download/index.aspx

**During install, choose:**
- Product: GeForce RTX (Notebook)
- OS: Windows 11
- Driver type: **Studio Driver** (more stable) or Game Ready
- Installation type: **Custom → Clean Installation**

Reboot after installing.

**Confirm GPU is ready:**

```powershell
nvidia-smi
# Should show: driver version, CUDA version, and GPU memory usage
```

---

## Step 3 — Install Models

Models are large files (4–8 GB each). Pull only what you need.

```powershell
# Best for coding tasks
ollama pull qwen2.5-coder:7b

# Fast, lightweight general assistant (~2 GB)
ollama pull phi3

# Strong general-purpose chat
ollama pull llama3:8b

# Reasoning / logic tasks
ollama pull deepseek-r1:8b
```

**Check installed models:**

```powershell
ollama list
```

**Model recommendations by use case:**

| Use Case | Model | VRAM needed |
|---|---|---|
| Coding / autocomplete | `qwen2.5-coder:7b` | ~4.5 GB |
| Quick chat / testing | `phi3` | ~2 GB |
| General assistant | `llama3:8b` | ~5 GB |
| Reasoning / math | `deepseek-r1:8b` | ~5 GB |

**Models to avoid on 4 GB VRAM:**

- `70b` or `32b` parameter models (any family)
- `deepseek-r1` (full version, not the `:8b` variant)
- `mixtral:8x22b`

---

## Step 4 — Run Models

**Start an interactive chat:**

```powershell
ollama run qwen2.5-coder:7b
```

You'll get a `>>>` prompt. Type your message and press Enter.

```
>>> write a Python function to parse JSON from a file
```

**Exit the chat:**

```
/bye
```

or press `Ctrl + C`

**Run a single prompt non-interactively (useful for scripting):**

```powershell
echo "Explain recursion in one paragraph" | ollama run phi3
```

---

## Step 5 — Manage Models

```powershell
# List all installed models
ollama list

# Remove a model (frees disk space)
ollama rm qwen2.5-coder:7b

# Copy a model under a new name
ollama cp llama3:8b llama3-backup
```

---

## Step 6 — Stop Ollama

Ollama runs a background server process. Kill it when not in use to free VRAM and RAM.

```powershell
# Force-stop Ollama
taskkill /F /IM ollama.exe

# Confirm it's stopped (no output = good)
tasklist | findstr ollama
```

---

## Step 7 — VS Code Integration (Continue.dev)

This gives you a Cursor-like AI coding experience for free.

**Install Continue extension:**

1. Open VS Code → `Ctrl + Shift + X`
2. Search: `Continue`
3. Install **Continue.dev**

**Configure it to use Ollama:**

Press `Ctrl + Shift + P` → search `Continue: Open Config File`

Replace the contents with:

```json
{
  "models": [
    {
      "title": "Qwen Coder (Local)",
      "provider": "ollama",
      "model": "qwen2.5-coder:7b"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Qwen Autocomplete",
    "provider": "ollama",
    "model": "qwen2.5-coder:7b"
  }
}
```

Save the file.

**Key shortcuts:**

| Action | Shortcut |
|---|---|
| Open AI chat | `Ctrl + L` |
| Inline code edit | `Ctrl + I` |
| Accept suggestion | `Tab` |

---

## Step 8 — Open WebUI (Browser Interface)

Open WebUI gives you a ChatGPT-style interface in your browser for all your local models.

**Prerequisites:** Install Docker Desktop → https://www.docker.com/products/docker-desktop

**Run Open WebUI:**

```powershell
docker run -d -p 3000:8080 `
  --add-host=host.docker.internal:host-gateway `
  -v open-webui:/app/backend/data `
  --name open-webui `
  --restart always `
  ghcr.io/open-webui/open-webui:main
```

Open in browser: http://localhost:3000

**What you get:**
- Multi-model switching
- Chat history
- File uploads to models
- System prompt customization
- Voice input (optional)

---

## Monitoring GPU Usage

While a model is running, open a second PowerShell window and run:

```powershell
# One-time snapshot
nvidia-smi

# Live refresh every 2 seconds
nvidia-smi -l 2
```

Look for `ollama.exe` in the process list and watch VRAM usage. If VRAM usage is near 0, the model is running on CPU (slower).

---

## Troubleshooting

### GPU not detected

```
error: CUDA error: shared object initialization failed
```

**Fix:** Update NVIDIA drivers → reboot → restart Ollama. If it persists, temporarily use CPU mode:

```powershell
$env:OLLAMA_NO_GPU="1"
ollama run qwen2.5-coder:7b
```

### Model requires more memory than available

```
error: model requires more system memory than available
```

**Fix:** Close Chrome, Discord, games, and background apps. Switch to a smaller model (`phi3` instead of `llama3:8b`).

### `nvidia-smi` command not found

Your PATH is missing the NVIDIA System Management Interface. Fix: reinstall NVIDIA drivers with a clean install, or manually add `C:\Windows\System32\DriverStore\FileRepository\...` to PATH.

### Ollama server not responding

```powershell
# Restart the server
taskkill /F /IM ollama.exe
ollama serve
```

### Model download stuck or corrupted

```powershell
ollama rm <model-name>
ollama pull <model-name>
```

---

## Performance Optimization

**Before starting a session:**
- Close Chrome tabs (each tab uses ~100–200 MB RAM)
- Disable Discord video/streaming
- Close MSI Center overlays and gaming software
- Close other background applications

**During use:**
- One model at a time — Ollama unloads models from VRAM after 5 minutes of inactivity
- For coding, use `qwen2.5-coder` — it's faster and more accurate than general models on code tasks
- For quick questions, `phi3` loads faster than 8B models

**After your session:**

```powershell
taskkill /F /IM ollama.exe
```

This immediately frees VRAM, RAM, and reduces battery drain.

---

## Quick Reference — All Commands

```powershell
# Install a model
ollama pull MODEL_NAME

# Run interactive chat
ollama run MODEL_NAME

# Run with a single prompt
echo "your prompt" | ollama run MODEL_NAME

# List installed models
ollama list

# Remove a model
ollama rm MODEL_NAME

# Start the server manually
ollama serve

# Stop Ollama completely
taskkill /F /IM ollama.exe

# Check GPU and VRAM
nvidia-smi

# Live GPU monitor (refreshes every 2s)
nvidia-smi -l 2

# CPU-only mode (no GPU)
$env:OLLAMA_NO_GPU="1"; ollama run MODEL_NAME
```

---

## Recommended Stack

```
Ollama  ←  runtime and model manager
  + qwen2.5-coder:7b  ←  primary coding model
  + phi3              ←  fast lightweight assistant

VS Code + Continue.dev  ←  AI-powered editor (like Cursor, but free)
Open WebUI             ←  browser chat interface
```

This setup gives you a fully offline, GPU-accelerated AI assistant with zero recurring cost.

---

*Last updated: May 2026*
