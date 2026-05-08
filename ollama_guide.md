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

Ollama exposes an OpenAI-compatible API on `http://localhost:11434`, so any tool that supports OpenAI can point to it locally.

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

> **RTX 2050/3050 note:** 7B parameter models work well. 14B models will be slow and may partially offload to CPU. Avoid 32B+ entirely.

---

## Realistic Expectations for RTX 2050 (4 GB VRAM)

Knowing what works and what doesn't saves a lot of frustration.

**Works well:**
- Coding assistance and autocomplete
- Small and medium models (≤ 7B)
- Offline chat and Q&A
- Lightweight reasoning tasks

**Struggles or fails:**
- 14B+ models (slow, partial CPU offload)
- Autonomous multi-step agents
- Large context windows (> 8K tokens)
- Multiple models loaded simultaneously
- Heavy multimodal (vision) tasks

---

## Understanding Quantization

When you pull a model from Ollama, it's almost always a quantized version — a compressed form of the original weights that uses less VRAM with minimal quality loss.

| Format | Quality | VRAM Usage | Best for |
|---|---|---|---|
| `q4_K_M` | Good | Low | Laptop GPUs (RTX 2050/3050) |
| `q5_K_M` | Better | Medium | 6–8 GB VRAM |
| `q8_0` | High | High | 8–12 GB VRAM |
| `fp16` | Full | Very high | Workstation GPUs only |

Ollama defaults to `q4_K_M` for most models, which is the right choice for 4 GB VRAM. You can specify a quantization explicitly:

```powershell
ollama pull llama3:8b-instruct-q5_K_M
```

---

## Step 1 — Install Ollama

**Option A — GUI Installer (recommended)**

Download from: https://ollama.com/download  
Run the `.exe` and follow the wizard. Ollama installs as a background Windows service.

**Option B — PowerShell one-liner**

```powershell
irm https://ollama.com/install.ps1 | iex
```

**Verify installation:**

```powershell
ollama --version
# Expected: ollama version is 0.x.x
```

---

## Step 2 — Update NVIDIA Drivers

Outdated drivers are the #1 cause of GPU detection failures.

```powershell
nvidia-smi
```

If this command fails or shows a driver older than 525.x, update at:  
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
# Should show: driver version, CUDA version, GPU memory
```

---

## Step 3 — Install Models

Models are large files (2–8 GB each). Pull only what you need.

```powershell
# Best for coding tasks
ollama pull qwen2.5-coder:7b

# Fast, lightweight general assistant (~2 GB)
ollama pull phi3

# Strong general-purpose chat
ollama pull llama3:8b

# Reasoning and logic tasks
ollama pull deepseek-r1:8b
```

**Model recommendations by use case:**

| Use Case | Model | VRAM needed |
|---|---|---|
| Coding / autocomplete | `qwen2.5-coder:7b` | ~4.5 GB |
| Quick chat / testing | `phi3` | ~2 GB |
| General assistant | `llama3:8b` | ~5 GB |
| Reasoning / math | `deepseek-r1:8b` | ~5 GB |

**Models to avoid on 4 GB VRAM:**
- Any `70b` or `32b` model
- `deepseek-r1` full version (use `:8b` instead)
- `mixtral:8x22b`

**Check what's installed:**

```powershell
ollama list
```

---

## Ollama Model Storage Location

Models are stored here on Windows:

```
C:\Users\<YOUR_USERNAME>\.ollama\models
```

If you're running low on disk space, you can inspect folder sizes here and delete unused models with `ollama rm`.

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

Ollama runs a background server process. Killing it frees VRAM, RAM, and reduces battery drain.

**Stop both Ollama processes:**

```powershell
taskkill /F /IM "ollama.exe"
taskkill /F /IM "ollama app.exe"
```

**Verify it stopped:**

```powershell
tasklist | findstr ollama
# No output = fully stopped
```

> **Important — Windows auto-restart behavior:**  
> Running `ollama list`, opening Continue.dev in VS Code, or loading Open WebUI in the browser will automatically restart the Ollama server in the background. This is by design, but it means a simple `taskkill` isn't truly "permanent" — tools that depend on Ollama will wake it back up the moment you use them.

**Start Ollama again manually:**

```powershell
ollama serve
# or just run any model:
ollama run qwen2.5-coder:7b
```

---

## Step 7 — Monitoring GPU Usage

While a model is running, open a second PowerShell window:

```powershell
# One-time snapshot
nvidia-smi

# Live refresh every 2 seconds
nvidia-smi -l 2
```

Look for `ollama.exe` in the process list and watch the VRAM column. If VRAM usage is near 0 MB, the model is running on CPU — much slower. This usually means your GPU driver needs updating.

---

## Step 8 — VS Code Integration (Continue.dev)

This gives you a Cursor-like AI coding experience — inline edits, chat, and autocomplete — completely free and offline.

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

> **Note:** Newer versions of Continue may use a graphical model setup wizard instead of manual JSON editing. If you see a GUI on first launch, use it to add an Ollama provider and select `qwen2.5-coder:7b`.

**Key shortcuts:**

| Action | Shortcut |
|---|---|
| Open AI chat sidebar | `Ctrl + L` |
| Inline code edit | `Ctrl + I` |
| Accept autocomplete | `Tab` |

---

## Step 9 — Open WebUI (Browser Interface)

Open WebUI gives you a ChatGPT-style interface for all your local models.

**Prerequisites:** Install Docker Desktop → https://www.docker.com/products/docker-desktop  
Make sure Docker Engine is running (green icon in the system tray) before proceeding.

**Run Open WebUI (CMD):**

```cmd
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

**Run Open WebUI (PowerShell — multi-line for readability):**

```powershell
docker run -d -p 3000:8080 `
  --add-host=host.docker.internal:host-gateway `
  -v open-webui:/app/backend/data `
  --name open-webui `
  --restart always `
  ghcr.io/open-webui/open-webui:main
```

**Verify the container started:**

```powershell
docker ps
# Should show "open-webui" with status "Up"
```

**Open in browser:**

```
http://localhost:3000
```

**What you get:**
- ChatGPT-style UI with local models
- Multi-model switching
- Chat history
- File uploads (PDF, text)
- System prompt customization
- Optional voice input

---

## Troubleshooting

### GPU not detected / CUDA error

```
error: CUDA error: shared object initialization failed
```

Update NVIDIA drivers → reboot → restart Ollama. To temporarily bypass and use CPU:

```powershell
$env:OLLAMA_NO_GPU="1"
ollama run qwen2.5-coder:7b
```

### Not enough memory

```
error: model requires more system memory than available
```

Close Chrome, Discord, games, and overlays. Switch to a smaller model (`phi3` instead of `llama3:8b`).

### `nvidia-smi` command not found

Reinstall NVIDIA drivers using Clean Installation. If that doesn't fix it, add the NVIDIA tools folder to your system PATH manually:

```
C:\Windows\System32\DriverStore\FileRepository\nv_dispi.inf_amd64_<version>\
```

### Ollama server not responding

```powershell
taskkill /F /IM ollama.exe
ollama serve
```

### Model download stuck or corrupted

```powershell
ollama rm <model-name>
ollama pull <model-name>
```

### Docker container won't start

Make sure Docker Desktop is running first, then retry the `docker run` command. If the container already exists from a previous attempt:

```powershell
docker rm open-webui
# then re-run the docker run command
```

---

## Performance Optimization

**Before a session:**
- Close Chrome tabs (~100–200 MB RAM each)
- Disable Discord video/streaming
- Close MSI Center, gaming overlays, and RGB software
- Close unused background applications

**During use:**
- Load one model at a time — Ollama automatically unloads a model from VRAM after 5 minutes of inactivity
- For coding tasks, `qwen2.5-coder` outperforms general models
- For quick one-off questions, `phi3` loads in ~5 seconds vs ~15 for 8B models

**After a session:**

```powershell
taskkill /F /IM ollama.exe
taskkill /F /IM "ollama app.exe"
```

---

## Quick Reference — All Commands

```powershell
# Pull (download) a model
ollama pull MODEL_NAME

# Run interactive chat
ollama run MODEL_NAME

# Run a single prompt (non-interactive)
echo "your prompt" | ollama run MODEL_NAME

# List installed models
ollama list

# Remove a model (frees disk space)
ollama rm MODEL_NAME

# Copy a model under a new name
ollama cp MODEL_NAME NEW_NAME

# Start the Ollama server manually
ollama serve

# Stop Ollama completely
taskkill /F /IM ollama.exe
taskkill /F /IM "ollama app.exe"

# Check GPU, VRAM, and driver info
nvidia-smi

# Live GPU monitor (refreshes every 2 seconds)
nvidia-smi -l 2

# Run in CPU-only mode (no GPU)
$env:OLLAMA_NO_GPU="1"; ollama run MODEL_NAME

# Check Docker containers
docker ps
```

---

## Recommended Stack

```
Ollama                 ←  runtime and model manager
  + qwen2.5-coder:7b   ←  primary coding model
  + phi3               ←  fast lightweight assistant

VS Code + Continue.dev ←  AI-powered editor (Cursor alternative, free)
Open WebUI             ←  browser chat interface
```

This gives you a fully offline, GPU-accelerated AI development environment with zero recurring cost.

---

*Last updated: May 2026*
