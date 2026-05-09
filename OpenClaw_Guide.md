# Running an AI Agent on 4GB VRAM: My WhatsApp-Controlled Local Assistant

1. The Problem: Cloud AI is expensive, private AI is hard
2. My Stack: Ollama + OpenClaw + Qwen2.5-3B + RTX 2050
3. The Hard Parts: VRAM limits, Unicode errors, browser automation
4. The Solutions: ASCII-safe scripts, direct URL parsing, debounce locks
5. Results: Latency, VRAM usage, task success rate
6. Code: GitHub repo with reproducible setup
7. What's Next: Playlist tools, GPU monitoring, voice triggers


# OpenClaw + AI Agent Setup Guide (Windows)

**Author:** Lovnish Verma | **Platform:** Windows 11 | **Reference Hardware:** MSI Cyborg 15 A12U (RTX 2050, 4GB VRAM)

---

## What is OpenClaw?

OpenClaw is an agentic framework that sits on top of Ollama. It transforms a "chat bot" into an "assistant" by giving the LLM access to your local system, tools, and messaging channels.

**Why use it with Ollama?**

| Feature | Ollama Alone | OpenClaw + Ollama |
| --- | --- | --- |
| Interface | Terminal / Basic Web | WhatsApp, TUI, & Dashboard |
| File Access | ❌ None | ✅ Read/Write local files |
| System Access | ❌ None | ✅ Execute Shell/PowerShell |
| Connectivity | ❌ Local only | ✅ Remote control via WhatsApp |
| Memory | ❌ Per-session | ✅ Persistent Identity & Long-term memory |

---

## Realistic Expectations for RTX 2050 (4 GB VRAM)

OpenClaw adds a small overhead for "thinking" (tool-calling logic). On a **4GB** card, you must be frugal.

**Works best:**

* **Qwen2.5-3B:** The "sweet spot" for speed and logic.
* **8K Context Window:** High enough for coding, low enough to avoid VRAM swapping.
* **WhatsApp Bridge:** Instant replies for system monitoring.

**Struggles:**

* **Tool-calling on < 7B models:** Small models (**3B**) often hallucinate tool names.
* **Simultaneous Dashboards:** Keeping the Web UI and WhatsApp active concurrently can push VRAM to **3.9/4.0 GB**.

---

## Step 1 — Installation

OpenClaw is a Node.js-based application. Ensure **Node.js (v20+)** is installed.

```powershell
# Install OpenClaw globally
npm install -g @openclaw/cli

# Initialize your profile (this creates C:\Users\<User>\.openclaw)
openclaw onboard

```

---

## Step 2 — Optimization for 4GB VRAM

By default, OpenClaw may try to use large context windows. We must cap this to prevent "slowness" on the RTX 2050.

**Edit `C:\Users\<User>\.openclaw\openclaw.json`:**

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "ollama/qwen2.5:3b"
      },
      "models": {
        "ollama/qwen2.5:3b": {}
      },
      "workspace": "C:\\Users\\princ"
    }
  },
  "auth": {
    "profiles": {
      "ollama:default": {
        "mode": "api_key",
        "provider": "ollama"
      }
    }
  },
  "gateway": {
    "auth": {
      "mode": "token",
      "token": "d47e91b0a9ec4170fd45e04b741185fda1e39c4e255ae023"
    },
    "bind": "loopback",
    "controlUi": {
      "allowInsecureAuth": true
    },
    "mode": "local",
    "nodes": {
      "allowCommands": [
        "python",
        "pip"
      ],
      "denyCommands": [
        "camera.snap",
        "camera.clip",
        "screen.record"
      ]
    },
    "port": 18789,
    "tailscale": {
      "mode": "off",
      "resetOnExit": false
    }
  },
  "meta": {
    "lastTouchedAt": "2026-05-09T01:46:00.000Z",
    "lastTouchedVersion": "2026.5.7"
  },
  "models": {
    "mode": "merge",
    "providers": {
      "ollama": {
        "api": "ollama",
        "apiKey": "ollama-local",
        "baseUrl": "http://127.0.0.1:11434",
        "models": [
          {
            "id": "qwen2.5:3b",
            "name": "qwen2.5:3b",
            "contextWindow": 16384,
            "compat": {
              "supportsTools": true,
              "supportsUsageInStreaming": true
            },
            "input": [
              "text"
            ],
            "cost": {
              "cacheRead": 0,
              "cacheWrite": 0,
              "input": 0,
              "output": 0
            }
          }
        ]
      }
    }
  },
  "plugins": {
    "entries": {
      "ollama": {
        "enabled": true
      },
      "whatsapp": {
        "enabled": true
      }
    }
  },
  "session": {
    "dmScope": "per-channel-peer"
  },
  "tools": {
    "profile": "coding",
    "web": {
      "search": {
        "enabled": false,
        "provider": "ollama"
      }
    }
  },
  "wizard": {
    "lastRunAt": "2026-05-08T17:36:52.827Z",
    "lastRunCommand": "onboard",
    "lastRunMode": "local",
    "lastRunVersion": "2026.5.7"
  },
  "channels": {
    "whatsapp": {
      "enabled": true,
      "selfChatMode": true,
      "dmPolicy": "allowlist",
      "allowFrom": [
        "918894869371"
      ],
      "accounts": {
        "default": {
          "name": "lovnish whatsapp"
        }
      }
    }
  },
  "bindings": [
    {
      "agentId": "main",
      "match": {
        "channel": "whatsapp",
        "accountId": "default"
      }
    }
  ]
}
```

---

## Step 3 — WhatsApp Remote Control

One of OpenClaw's strongest features is the WhatsApp bridge.

1. **Whitelist your number** in `openclaw.json`:
```json
"channels": {
  "whatsapp": {
    "enabled": true,
    "allowFrom": ["91XXXXXXXXXX"]
  }
}

```


2. **Start the Gateway:**
```powershell
openclaw gateway restart

```


3. **Scan the QR Code:**
A QR code will appear in your terminal. Scan it with WhatsApp (Linked Devices) to authorize your local agent.

### 🔄 How to Re-link WhatsApp

If your session expires or you are logged out, you need to trigger a fresh login to see the QR code again:

```powershell
# Force a new login session for WhatsApp
openclaw channels login --channel whatsapp

```

Scan the resulting QR code with your phone. Once the terminal says **"✅ Linked!"**, restart the gateway to resume operations.

---

## Step 4 — Service Management

OpenClaw usually runs as a background service. Use these commands to start, stop, or refresh the system.

| Command | Action |
| --- | --- |
| `openclaw gateway start` | Launches the gateway and connects all channels (WhatsApp/Web). |
| `openclaw gateway stop` | Shuts down the gateway and frees up your **RTX 2050** VRAM. |
| `openclaw gateway restart` | Quickly stops and starts the gateway (required after editing `openclaw.json`). |
| `openclaw status` | Check if the gateway is running and verify channel health. |

---

## Step 5 — Operational Commands

Manage your agent's "brain" and monitor its performance.

| Command | Action |
| --- | --- |
| `openclaw gateway logs --follow` | Watch the agent "think" and view inbound WhatsApp logs. |
| `openclaw reset --scope sessions` | Clear "brain fog" if the agent gets confused. |
| `/new` | (Inside any chat) Starts a fresh session with **0 tokens**. |

---

## Troubleshooting

### Agent is "Silent" on WhatsApp

* **Check Status:** `openclaw status` should show WhatsApp as `OK`.
* **Session Full:** If tokens show **100%**, run `openclaw reset --scope sessions`.
* **Token Overload:** If your session hits **8.2k/8.2k**, send `/new` in the WhatsApp chat.

### GPU Utilization shows 0%

* Task Manager defaults to "3D" view which often misses AI compute.
* **Fix:** Change the graph in Task Manager to **Cuda** or **Compute_0** to see the **3B** model active.

---

## Recommended "Frugal" Stack

```text
Engine:     Ollama (qwen2.5:3b)
Agent:      OpenClaw (v2026.5.7)
Bridge:     WhatsApp (Self-Chat Mode)
Hardware:   RTX 2050 (Locked to 8k Context)

```

---

**What's Next:** Exploring adding a custom Python-based tool to this config, or is the current "Frugal" chat setup exactly what you need for now?


**Example:**

<img width="842" height="447" alt="image" src="https://github.com/user-attachments/assets/6b850f2b-626f-4f27-9e54-15f675c6319f" />



**Script: play_song.py**

```
import webbrowser
import sys

# Get the search query from WhatsApp input
song = " ".join(sys.argv[1:]) if len(sys.argv) > 1 else "lofi hip hop"
url = f"https://www.youtube.com/results?search_query={song}"

print(f"Opening YouTube for: {song}")
webbrowser.open(url)
```


```
Execute shell: python "C:\Users\princ\scripts\play_song.py" "Sad English Songs"
```

## **OR This Way (Better)**

**Script: play_song.py**

```
import sys
import re
import urllib.parse
import webbrowser


def get_direct_url(query: str) -> str:
    # If user sends video ID or full URL, use it directly
    if re.match(r'^[a-zA-Z0-9_-]{11}$', query.strip()):
        return f"https://www.youtube.com/watch?v={query.strip()}&autoplay=1"
    if 'youtube.com/watch?v=' in query:
        return query + '&autoplay=1' if '?' in query else query.replace('watch?v=', 'watch?v=') + '&autoplay=1'
    # Fallback: search (user clicks manually)
    return f"https://www.youtube.com/results?search_query={urllib.parse.quote(query)}"


song = " ".join(sys.argv[1:]) if len(sys.argv) > 1 else "lofi hip hop"
url = get_direct_url(song)
webbrowser.open(url)

```

```
Run The shell command `python "C:\Users\princ\scripts\play_song.py" "https://www.youtube.com/watch?v=YHRvDo8rUoQ"`
```

# Set Volume volume_50.py

```
from pycaw.pycaw import AudioUtilities, IAudioEndpointVolume
from comtypes import CLSCTX_ALL
from ctypes import POINTER, cast
speakers = AudioUtilities.GetSpeakers()
interface = speakers.Activate(IAudioEndpointVolume._iid_, CLSCTX_ALL, None)
volume = cast(interface, POINTER(IAudioEndpointVolume))
volume.SetMasterVolumeLevelScalar(0.5, None)
```

```
python "C:\Users\princ\scripts\volume_50.py"
```

# Set Volume volume_max.py

```
from pycaw.pycaw import AudioUtilities, IAudioEndpointVolume
from comtypes import CLSCTX_ALL
from ctypes import POINTER, cast
speakers = AudioUtilities.GetSpeakers()
interface = speakers.Activate(IAudioEndpointVolume._iid_, CLSCTX_ALL, None)
volume = cast(interface, POINTER(IAudioEndpointVolume))
volume.SetMasterVolumeLevelScalar(1.0, None)
```

```
python "C:\Users\princ\scripts\volume_max.py"
```

# send_mail.py

```
import smtplib
from email.mime.text import MIMEText
import sys

# Expected format: python send_mail.py "to@email.com" "Subject" "Message body"

if len(sys.argv) < 4:
    print("Usage: python send_mail.py <to_email> <subject> <body>")
    sys.exit(1)

to_email = sys.argv[1]
subject = sys.argv[2]
body = sys.argv[3]

GMAIL_USER = "technicalboyprince@gmail.com"
GMAIL_APP_PASSWORD = "xxxx xxxx xxxx xxxx"

msg = MIMEText(body)
msg['Subject'] = subject
msg['From'] = GMAIL_USER
msg['To'] = to_email

try:
    with smtplib.SMTP_SSL('smtp.gmail.com', 465) as server:
        server.login(GMAIL_USER, GMAIL_APP_PASSWORD)
        server.send_message(msg)
    print(f"Email sent successfully to {to_email}")
except Exception as e:
    print(f"Failed to send email: {e}")

```

```
Execute this exact shell command locally as standard user. Do not use elevated mode. python C:\Users\princ\scripts\send_mail.py "princelv84@gmail.com" "Dashboard Ready" "The DTE Punjab dashboard is ready..."
```

<img width="747" height="307" alt="image" src="https://github.com/user-attachments/assets/d5af6507-a48a-460f-8ac1-44850afa1e75" />


<img width="1547" height="366" alt="image" src="https://github.com/user-attachments/assets/836e4a6b-5965-4906-b321-8603c823b4d7" />


# snap_screen.py

```
import pyautogui
import sys
import os
from datetime import datetime


def take_snap():
    path = r"C:\Users\princ\Pictures\Screenshots"
    filename = f"snap_{datetime.now().strftime('%H%M%S')}.png"
    fullpath = os.path.join(path, filename)

    pyautogui.screenshot(fullpath)
    print(f"Screenshot saved: {fullpath}")


if __name__ == "__main__":
    take_snap()
    sys.stdout.flush()
    sys.exit(0)
```

```
python "C:\Users\princ\scripts\snap_screen.py"
```

# take_photo.py

```
import cv2
import os
import sys
import time
from datetime import datetime


def snap_photo():
    # Folder to save the images
    path = r"C:\Users\princ\Pictures\Camera_Snaps"

    try:
        if not os.path.exists(path):
            os.makedirs(path)

        filename = f"photo_{datetime.now().strftime('%H%M%S')}.jpg"
        fullpath = os.path.join(path, filename)

        # 0 is usually the default built-in webcam.
        # CAP_DSHOW is required for Windows background scripts to load instantly.
        cap = cv2.VideoCapture(0, cv2.CAP_DSHOW)

        if not cap.isOpened():
            print("Error: Could not access the webcam. Is it in use by another app?")
            return

        # Warm-up time for sensor to adjust light/focus
        time.sleep(1.5)

        # Read the frame
        ret, frame = cap.read()

        if ret:
            cv2.imwrite(fullpath, frame)
            print(f"Photo snapped and saved: {fullpath}")
        else:
            print("Error: Camera opened but failed to capture the image.")

    except Exception as e:
        print(f"Camera Script Error: {e}")
    finally:
        # ALWAYS release the camera, otherwise the LED stays on and it's locked out
        if 'cap' in locals() and cap.isOpened():
            cap.release()


if __name__ == "__main__":
    snap_photo()
    sys.stdout.flush()  # Push the log to OpenClaw
    sys.exit(0)         # Kill the background thread instantly
```

```
python "C:\Users\princ\scripts\take_photo.py"
```



🎉 YESSS! That's the "Aha Moment"! 🔥

I just cracked the code: Local AI + WhatsApp + YouTube automation on a 4GB RTX 2050 — no cloud, no subscriptions, no compromises. That's exactly the frugal, powerful setup I was always aiming for. 🙌



```
"Everyone talks about running LLMs locally. Almost no one shows you how to make them *useful* on 4GB VRAM." - Lovnish Verma

*Last updated: May 2026*
```
