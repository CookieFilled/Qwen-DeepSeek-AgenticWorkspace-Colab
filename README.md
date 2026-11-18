# Autonomous Agentic Workspace (`DeepSeek-V4-Pro · Qwen3.5-9B`)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/CookieFilled/Qwen-DeepSeek-AgenticWorkspace-Colab/blob/main/DeepSeek_V4_Pro_Qwen35_Workspace_Colab.ipynb)
[![GPU](https://img.shields.io/badge/Google%20Colab-T4%20%7C%20L4%20%7C%20A100-F9AB00?logo=googlecolab&logoColor=white)](https://colab.research.google.com/github/CookieFilled/Qwen-DeepSeek-AgenticWorkspace-Colab/blob/main/DeepSeek_V4_Pro_Qwen35_Workspace_Colab.ipynb)
[![Inference](https://img.shields.io/badge/Engine-CUDA%20llama--server-76B900?logo=nvidia&logoColor=white)](https://github.com/ggml-org/llama.cpp)
[![Skills](https://img.shields.io/badge/Workflows-Portable%20SKILL.md-22D3EE)](#included-starter-skills)

A self-contained autonomous AI engineering, research, and desktop automation studio built for **Google Colab GPUs (T4 / L4 / A100)**. Powered by a native CUDA `llama-server` backend running **DeepSeek-V4-Pro · Qwen3.5-9B** with paired multimodal vision (`mmproj`), uncensored web and image search, headless Playwright Chromium, a live `1920x1080` X11/noVNC virtual desktop, and a FastAPI + SSE web studio exposed over Cloudflare Tunnel.

<p align="center">
  <img src="assets/01-studio-dashboard.jpg" alt="Minimalist Agentic Studio Dashboard" width="100%" />
</p>

---

## Key Features

### Inference & Hybrid AI Routing
- **Native CUDA `llama-server` + Auto-Healing Watchdog:** Automatically fetches prebuilt `sm_75`+ CUDA `llama-server` binaries (with prebuilt `llama-cpp-python` CUDA wheel fallback), enables `--flash-attn on` and `q8_0` K/V cache quantization, supports up to `49,152`-token context with rolling turn compaction, and restarts automatically if the backend crashes.
- **Instant Local / Gemini API Switcher:** Toggle between your local Colab GPU model and the **Gemini API** (`gemini-2.5-flash`, `gemini-2.5-pro`, `gemini-2.0-flash`) directly from the top bar without losing chat or workspace state.
- **Configurable Reasoning & Step Budgets:** Switch reasoning depth (`OFF` / `BRIEF` / `DEEP`), adjust autonomous execution steps (`4`–`120` or `ENDLESS`), and toggle `AUTO-VISION` per turn.

### Vision, Search & Browser Automation
- **Multimodal Vision with Coordinate Grid Grounding:** Pairs a GGUF `mmproj` projector with automatic cyan coordinate overlays (`x200..x1800`, `y200..y1000`) on desktop screenshots so the agent can visually verify images and click exact pixel coordinates.
- **Uncensored Web, Image & AI Art Engine:** Multi-engine web search (`safesearch=off`, `region=wt-wt`) with DuckDuckGo HTML and Brave fallbacks, clean HTTP-to-Markdown page extraction, anti-hotlink image downloading, and verbatim AI image generation.
- **Headless Chromium + Live `1920x1080` X11 Virtual Desktop:** Automates indexed DOM elements (`[0]`, `[1]`, ...) via Playwright Chromium alongside an interactive `Xvfb :99` + `Fluxbox` + `x11vnc` + `noVNC` desktop stream right inside the browser.

### Artifacts & Extensible Skills
- **Deterministic Motion Graphics & Office Suite:** Renders frame-accurate `60fps` HTML5 Canvas/CSS animations (`window.seekToFrame`) into H.264 `.mp4` videos with FFmpeg, and builds styled `.pdf` reports (WeasyPrint), runnable `.ipynb` notebooks, `.xlsx` workbooks with formulas, `.pptx` decks, and `.docx` files.
- **Portable `SKILL.md` Workflow System:** Progressive-disclosure skill loader supporting one-click GitHub repository/subpath cloning and `.md` / `.zip` bundle uploads.

---

## Full-Width Workspace Walkthrough

### 1. Live `1920x1080` X11 & Chromium Viewport (`VIEWPORT` Tab)
Control a full Ubuntu virtual display (`DISPLAY=:99`) directly from the inspector panel. Capture instant X11 snapshots or click **EXPAND & USE** to open an interactive `noVNC` session in-browser.

<p align="center">
  <img src="assets/04-x11-viewport.jpg" alt="X11 Chromium Live Viewport" width="100%" />
</p>

---

### 2. Multi-Step Autonomous Tool Execution & Fallbacks
Every tool call (`browser_action`, `image_search`, `run_bash`, `file_ops`, etc.) streams live `EXEC` and `DONE` traces with automatic error recovery when pages time out or hit CAPTCHAs.

<p align="center">
  <img src="assets/05-browser-automation.jpeg" alt="Autonomous Browser Execution Trace" width="100%" />
</p>

---

### 3. Inline Verified Media & Downloaded Asset Cache (`MEDIA` Tab)
Downloaded search results and generated images render inline in the conversation with resolution metadata and source links, while the **MEDIA** inspector caches every file for one-click `PATH` copying.

<p align="center">
  <img src="assets/02-media-cache.jpeg" alt="Media Cache and Inline Search Results" width="100%" />
</p>

---

### 4. Portable `SKILL.md` Manager (`SKILLS` Tab)
Install new agent capabilities on the fly by pasting any GitHub URL (including folder subpaths) or uploading a `SKILL.md` / `.zip` package.

<p align="center">
  <img src="assets/03-skills-manager.jpeg" alt="Portable SKILL.md Manager" width="100%" />
</p>

---

### 5. Real-Time GPU Telemetry, API Switcher & Server Logs (`SYSTEM` Tab)
Monitor Tesla T4 VRAM allocation, `llama-server` health and recovery count, live `llama.cpp` logs, public Cloudflare Tunnel URLs, and optional Gemini API credentials in one place.

<p align="center">
  <img src="assets/06-system-telemetry.jpeg" alt="Runtime GPU Telemetry and System Tab" width="100%" />
</p>

---

## Architecture & Built-in Toolset

### System Stack

| Layer | Implementation |
| :--- | :--- |
| **Default Model** | `mradermacher/DeepSeek-V4-Pro-Qwen3.5-9B-UNCENSORED-heretic-GGUF` (`Q4_K_M` default; supports `Q5_K_M`, `Q6_K`, `Q8_0`) |
| **Inference Engine** | Native CUDA `llama-server` (`127.0.0.1:8000/v1`, OpenAI-compatible) with `llama-cpp-python[server]` fallback |
| **Web Studio & Proxy** | FastAPI + Uvicorn (`127.0.0.1:7860`), Server-Sent Events (SSE) streaming, WebSocket-to-TCP `websockify` VNC bridge |
| **Public Access** | Automatic `cloudflared` quick tunnel (`https://*.trycloudflare.com`) + embedded Google Colab iframe |
| **Virtual Display** | `Xvfb :99` (`1920x1080x24`), `fluxbox`, `xdotool`, `scrot`, `x11vnc` (`5900`), and `/usr/share/novnc` |

### Built-in Agent Tools (`11` Native Tools)

- **`web_search`** — Uncensored multi-backend web search (`safesearch=off`) across DDGS, DuckDuckGo HTML, and Brave.
- **`fetch_url`** — Clean HTTP-to-Markdown article and documentation reader (`BeautifulSoup` + `markdownify`).
- **`image_search`** — Uncensored multi-engine image search with local download and verification into `/content/workspace/artifacts/images/`.
- **`generate_image`** — Uncensored 18+ AI image generation via Perchance (with Pollinations Flux fallback).
- **`view_image`** — Multimodal vision inspection with automatic pixel coordinate grid overlay on desktop captures.
- **`browser_action`** — Playwright Chromium automation (`goto`, `get_markdown`, `click`, `fill`, `scroll`, `eval_js`, `screenshot`, `render_video`).
- **`desktop_action`** — Direct `1920x1080` X11 GUI mouse and keyboard control (`screenshot`, `click`, `double_click`, `right_click`, `scroll`, `type`, `key`).
- **`run_bash`** — Non-interactive root Ubuntu shell execution inside `/content/workspace` (`DISPLAY=:99`).
- **`file_ops`** — Read, write, append, surgical edit, and recursive directory listing.
- **`activate_skill` / `install_skill`** — Load or clone portable `SKILL.md` workflows on demand.

### Included Starter Skills

| Skill | Purpose |
| :--- | :--- |
| **`document-studio`** | Builds styled PDFs (WeasyPrint), runnable `.ipynb` notebooks, `.xlsx` workbooks with formulas, `.pptx` decks, and `.docx` reports with embedded images. |
| **`motion-video`** | Synthesizes deterministic frame-by-frame HTML5 Canvas/CSS animations and encodes `1080p` H.264 `.mp4` videos with FFmpeg. |
| **`web-researcher`** | Coordinates multi-source web research, clean markdown extraction, and verified visual asset collection. |
| **`desktop-operator`** | Launches headed Chromium and operates GUI applications across the `1920x1080` X11 desktop. |
| **`image-studio`** | Generates verbatim high-detail AI imagery directly into the workspace media gallery. |

---

## Quick Start

1. Click the **Open in Colab** badge at the top of this README.
2. In Google Colab, select **Runtime → Change runtime type → T4 GPU** (or L4 / A100).
3. Run **Cells 1 → 5** sequentially:
   - **Cell 1 (`Environment Setup & Inference Backend`):** Verifies NVIDIA GPU, optionally mounts Google Drive (`MOUNT_GOOGLE_DRIVE`), installs CUDA `llama-server`, Playwright Chromium, FFmpeg, WeasyPrint, `cloudflared`, and starts the `1920x1080` X11 + VNC stack.
   - **Cell 2 (`Model Download & LLM Server`):** Downloads the GGUF weights and `mmproj` vision projector from Hugging Face and boots the local server with a background auto-recovery watchdog.
   - **Cell 3 (`Portable SKILL.md Manager`):** Initializes `/content/workspace/skills` and registers the 5 built-in workflows.
   - **Cell 4 (`Agent Tools & Execution Loop`):** Loads the 11 agent tools, XML tool-call parser, context compactor, and streaming execution loop.
   - **Cell 5 (`Studio Web UI & Tunnel Host`):** Starts the FastAPI studio on port `7860`, prints your public `*.trycloudflare.com` URL, embeds the UI in Colab, and runs the live health monitor.

---

## Author

Built by **[CookieFilled](https://github.com/CookieFilled)**.
