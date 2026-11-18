# ✦ OmniAgent AI
### Multimodal & Multilingual Agentic AI Powered by Gemini, Bodhan AI and MCP

**An intelligent AI agent that understands natural-language instructions, plans multi-step workflows, and performs Indian-language processing using specialized AI tools.**

[![Python](https://img.shields.io/badge/Python-3.11-blue)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-Web_App-FF4B4B)](https://streamlit.io/)
[![Gemini](https://img.shields.io/badge/LLM-Gemini_2.5_Flash-4285F4)](https://ai.google.dev/)
[![MCP](https://img.shields.io/badge/Protocol-MCP-purple)](https://modelcontextprotocol.io/)
![Status](https://img.shields.io/badge/Status-Deployment_Testing-orange)

### 🌐 Live Application

**[Launch OmniAgent AI](YOUR_STREAMLIT_LIVE_URL)**

**[GitHub Repository](https://github.com/samirmore7/OmniAgentAI)**

> The Streamlit deployment URL must be added after the application is successfully published.

---

## 📌 Overview

OmniAgent AI is a Python-based multimodal AI application that combines the reasoning capabilities of **Gemini 2.5 Flash** with specialized multilingual services provided by **Bodhan AI**.

Instead of manually selecting individual AI tools, users can describe their task in natural language. OmniAgent determines the required operations, executes them through the **Model Context Protocol (MCP)**, and returns structured results.

The project provides both a command-line interface and an interactive Streamlit web application.

## ✨ Key Features

- **Intelligent AI planning:** Gemini interprets user instructions and selects appropriate tools.
- **Multilingual OCR:** Extract text from supported images using Bodhan AI.
- **Language translation:** Translate text between supported languages.
- **Speech transcription:** Convert WAV speech recordings into text.
- **Text-to-speech:** Generate speech using supported Bodhan AI voices.
- **Multi-step workflows:** Automatically chain operations such as OCR followed by translation.
- **MCP integration:** Communicate with specialized AI tools through a local MCP server.
- **Interactive Streamlit UI:** Upload files, enter instructions, execute workflows, and view results.
- **Four interface themes:** Midnight, Aurora, Royal, and Light.
- **Seven interface languages:** English, Hindi, Marathi, Tamil, Telugu, Bengali, and Kannada, with partial translation fallback.
- **Execution controls:** Configure planning steps and maximum tool calls.
- **Retry handling:** Retry supported transient planning and tool failures.
- **Structured reporting:** Generate JSON reports, Markdown summaries, and JSONL execution traces.
- **Cost visibility:** Track tool usage while distinguishing known and unknown costs.

## 🛠️ Technology Stack

| Component | Technology |
|---|---|
| Programming Language | Python 3.11 |
| LLM / Agent Planner | Gemini 2.5 Flash |
| LLM Integration | OpenAI-compatible API |
| Multilingual AI Services | Bodhan AI |
| Tool Communication | Model Context Protocol (MCP) |
| Web Interface | Streamlit |
| HTTP Integration | HTTPX |
| Data Validation | Pydantic |
| Configuration | Environment variables, python-dotenv |
| Testing | Python regression tests |
| Reports | JSON, Markdown, JSONL |
| Deployment Target | Streamlit Community Cloud |

## 🏗️ System Architecture

```text
                  USER
                    |
          +---------+---------+
          |                   |
          v                   v
    Streamlit UI          Python CLI
          |                   |
          +---------+---------+
                    |
                    v
             OmniAgent Core
                    |
          +---------+---------+
          |                   |
          v                   v
     Gemini Planner      Budget Manager
          |                   |
          +---------+---------+
                    |
                    v
             MCP Client
                    |
                    v
             MCP Server
                    |
       +------------+------------+
       |            |            |
       v            v            v
      OCR       Translation    Speech
       |            |            |
       +------------+------------+
                    |
                    v
               Bodhan AI
                    |
                    v
              Tool Results
                    |
                    v
            Agent Orchestration
                    |
                    v
          Structured Output
                    |
       +------------+------------+
       |            |            |
       v            v            v
   Streamlit      JSON / MD     JSONL
    Results        Reports      Traces
```

### How It Works

1. The user submits a natural-language task and uploads supported files.
2. OmniAgent connects to the Bodhan MCP server.
3. Gemini analyzes the task and selects an appropriate tool.
4. The agent validates tool arguments and checks the execution budget.
5. The MCP server invokes the corresponding Bodhan AI service.
6. The tool result is returned to OmniAgent.
7. The agent continues with another tool if required.
8. Final results and execution reports are generated.

**Gemini handles planning and orchestration, while Bodhan AI performs specialized multilingual processing.**

## 🔧 Available MCP Tools

| Tool | Function |
|---|---|
| `extract_text` | Extracts text from images using OCR |
| `translate_text` | Translates text between supported languages |
| `transcribe_audio` | Converts speech audio into text |
| `generate_speech` | Generates speech from text |
| `server_status` | Provides server and configuration status information |

Actual language, voice, and file-format support depends on the configured Bodhan AI services.

## 🖥️ Streamlit Web Application

The Streamlit interface provides an interactive workspace for using OmniAgent without manually entering CLI commands.

### Interface Capabilities

- Natural-language task input
- PNG, JPG, JPEG, and WAV file uploads
- Quick-start examples for OCR, translation, and transcription
- Customizable planning and tool-call limits
- Four visual themes
- Multilingual interface options
- Execution status and metrics
- Step-by-step tool results
- Extracted and translated text display
- Tool activity inspection
- Downloadable execution reports

### Run the Web Application Locally

```bash
python -m streamlit run streamlit_app.py
```

The application will open in your browser, typically at:

```text
http://localhost:8501
```

## 📁 Project Structure

```text
OmniAgentAI/
│
├── src/
│   ├── bodhan_agent/
│   │   ├── agent.py
│   │   ├── cli.py
│   │   ├── audio.py
│   │   ├── budget.py
│   │   ├── cost.py
│   │   ├── files.py
│   │   ├── planner_retry.py
│   │   ├── retry.py
│   │   ├── reporting.py
│   │   ├── tracing.py
│   │   ├── transcription.py
│   │   └── transcription_mcp.py
│   │
│   └── bodhan_mcp/
│       ├── server.py
│       ├── audio_tools.py
│       ├── ocr_tools.py
│       ├── translation_tools.py
│       └── tts_tools.py
│
├── tests/
├── data/
├── demo_inputs/
├── demo_outputs/
│
├── streamlit_app.py
├── requirements.txt
├── pyproject.toml
├── run_tests.py
├── .env.example
├── .gitignore
└── README.md
```

## ⚙️ Installation and Setup

### Prerequisites

- Python 3.11
- Git
- Gemini API credentials
- Bodhan AI API credentials
- Internet connection

### 1. Clone the Repository

```bash
git clone https://github.com/samirmore7/OmniAgentAI.git
cd OmniAgentAI
```

### 2. Create a Virtual Environment

```bash
python -m venv .venv
```

On Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

On Linux/macOS:

```bash
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
python -m pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the project root using `.env.example` as a reference.

```dotenv
# Gemini LLM
LLM_API_KEY=your_gemini_api_key
LLM_BASE_URL=your_existing_llm_base_url
LLM_MODEL=gemini-2.5-flash

# Bodhan AI
BODHAN_BASE_URL=your_bodhan_base_url
BODHAN_OCR_KEY=your_ocr_api_key
BODHAN_TRANSLATE_KEY=your_translation_api_key
BODHAN_TRANSCRIBE_KEY=your_transcription_api_key
BODHAN_SPEAK_KEY=your_speech_api_key
```

Use the exact endpoint values required by your API providers.

**Never commit `.env` or real API credentials to GitHub.**

## 🚀 Deployment — Streamlit Community Cloud

The application is configured for deployment from GitHub.

### Deployment Steps

1. Visit [Streamlit Community Cloud](https://share.streamlit.io/).
2. Sign in with GitHub.
3. Select the `samirmore7/OmniAgentAI` repository.
4. Choose the `main` branch.
5. Set the main file path to `streamlit_app.py`.
6. Configure API credentials using Streamlit Secrets.
7. Deploy the application.
8. Verify OCR, translation, and speech workflows in the hosted environment.

### Streamlit Secrets

Add the following entries in your app's Secrets settings:

```toml
LLM_API_KEY = "YOUR_GEMINI_API_KEY"
LLM_BASE_URL = "YOUR_LLM_BASE_URL"
LLM_MODEL = "gemini-2.5-flash"

BODHAN_BASE_URL = "YOUR_BODHAN_BASE_URL"
BODHAN_OCR_KEY = "YOUR_OCR_KEY"
BODHAN_TRANSLATE_KEY = "YOUR_TRANSLATE_KEY"
BODHAN_TRANSCRIBE_KEY = "YOUR_TRANSCRIBE_KEY"
BODHAN_SPEAK_KEY = "YOUR_SPEAK_KEY"
```

**Deployment note:** The backend uses a local MCP subprocess, so successful cloud execution also depends on Linux compatibility, subprocess startup, and the availability of required packages and credentials.

## 🧪 Verified Demonstration

### Task

Extract Hindi text from an image and translate it into English.

**Input file:** `demo_inputs/sample_ocr.png`

### Step 1 — OCR

Tool: `extract_text`

**Extracted text:**

```text
नमस्ते भारत
```

### Step 2 — Translation

Tool: `translate_text`

**Translated text:**

```text
hello india
```

### Execution Summary

| Metric | Result |
|---|---|
| Execution Status | Success |
| Tools Executed | `extract_text`, `translate_text` |
| Completed Steps | 2 |
| Maximum Tool Calls | 2 |
| Tool Calls Used | 2 |
| Remaining Calls | 0 |
| Final Translation | `hello india` |

The live backend workflow completed successfully within its configured tool-call budget.

### Demonstration Artifacts

- `demo_inputs/sample_ocr.png` — OCR sample input
- `demo_outputs/report.json` — Structured execution report
- `demo_outputs/result.md` — Human-readable summary
- `demo_outputs/trace.jsonl` — Execution trace

## 📊 Execution Budget and Cost Tracking

OmniAgent supports a configurable maximum number of MCP tool-call attempts.

For example:

```text
--max-tool-calls 2
```

The budget includes retry attempts.

The application also records:

- Total tool calls
- Calls grouped by tool
- Known costs when available
- Calls with unknown pricing
- Whether cost reporting is complete

**A successful workflow can exhaust its tool-call budget without being considered a failure.**

Actual INR spending is not guaranteed when provider pricing information is unavailable. The project does not enforce a monetary spending ceiling.

## 📋 Reporting and Tracing

OmniAgent generates three primary artifacts.

### `report.json`

Contains task information, execution status, completed steps, tool responses, budget usage, and cost metadata.

### `result.md`

Provides a readable summary of the execution and results.

### `trace.jsonl`

Records execution events including planning, tool calls, tool results, and completion.

Execution traces may include user-provided content. Review them before sharing publicly.

## ✅ Testing and Validation

Run the offline regression suite:

```bash
python run_tests.py
```

### Latest Verified Offline Test Results

| Test Suite | Passed Checks |
|---|---:|
| `test_audio.py` | 5 |
| `test_chunked_transcription.py` | 4 |
| `test_transcription_mcp.py` | 9 |
| `test_agent_chunk_routing.py` | 5 |
| `test_planner_retry.py` | 5 |
| **Total** | **28** |

All 28 offline checks passed in the recorded test run.

Additional live verification has demonstrated successful OCR-to-translation and Hindi WAV transcription workflows.

Hosted Streamlit execution requires separate validation after deployment.

## 🔒 Security Considerations

- Store API credentials in environment variables or Streamlit Secrets.
- Never commit `.env` files or real credentials.
- Restrict file access to configured input directories.
- Avoid uploading sensitive documents or recordings to public demonstrations.
- Review execution traces and reports before publishing them.
- Monitor provider quotas and usage.
- Use appropriate authentication and abuse protection before offering unrestricted public access.

## ⚠️ Known Limitations

1. Live AI workflows require valid Gemini and Bodhan AI credentials.
2. Provider quotas and network failures may interrupt execution.
3. LLM tool selection may not always produce the intended workflow.
4. WAV is the supported audio upload format in the Streamlit interface.
5. Execution budgets limit tool calls, not monetary spending.
6. Some interface translations fall back to English.
7. Public deployment requires verification of MCP subprocess behavior on Linux.
8. Generated reports and traces may contain user-submitted information.

## 📈 Project Status

**Implemented and locally demonstrated:**

- Gemini-powered agent planning
- Bodhan MCP tool integration
- Multilingual OCR and translation
- Speech transcription
- Speech generation tool integration
- Multi-step workflow orchestration
- Tool-call budgeting
- Retry handling
- WAV audio chunking
- Structured reports and traces
- Cost visibility
- Streamlit user interface
- Offline regression testing

**Remaining verification:** Successful end-to-end execution on Streamlit Community Cloud.

## 👨‍💻 Developer

**Samir More**

B.Tech — Computer Science & Engineering (Artificial Intelligence & Analytics)

- [GitHub](https://github.com/samirmore7)
- [Portfolio](https://samirmoreportfolio.netlify.app/)

---

**OmniAgent AI — Bridging intelligent agent orchestration and multilingual AI through Gemini, Bodhan AI, and MCP.**
