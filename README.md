# OmniAgent AI — Multimodal Agent Using Bodhan AI

**OmniAgent AI** is a Python-based agentic AI application that understands natural-language instructions, plans multi-step workflows using an LLM, and executes multilingual tasks through Bodhan AI services integrated with the Model Context Protocol (MCP).

The project combines **Gemini for intelligent tool planning** with **Bodhan AI for Indian-language processing**, including OCR, translation, speech transcription, and speech generation.

## 1. Key Features

- **LLM-driven agent:** Uses Gemini to interpret user instructions and select tools dynamically.
- **Model Context Protocol:** Integrates Bodhan AI capabilities through a local MCP server.
- **Multimodal processing:** Supports image text extraction, text translation, speech transcription, and text-to-speech generation.
- **Multi-step orchestration:** Uses the result of one tool as input to another.
- **Execution budgeting:** Limits the number of tool calls, including retry attempts.
- **Retry handling:** Handles supported transient LLM and tool execution failures.
- **WAV audio chunking:** Supports chunk-based speech transcription.
- **File access validation:** Restricts tool file access to configured input locations.
- **Execution tracing:** Records planning decisions, tool calls, results, and lifecycle events.
- **Structured reporting:** Produces JSON reports, Markdown summaries, and JSONL traces.
- **Cost visibility:** Tracks tool usage and distinguishes known costs from unavailable pricing information.

## 2. Technology Stack

| Component | Technology |
|---|---|
| Programming language | Python 3.11 |
| Agent planning | Gemini 2.5 Flash |
| LLM integration | OpenAI-compatible API |
| Tool communication | Model Context Protocol (MCP) |
| Multilingual AI services | Bodhan AI |
| Application interface | Python CLI |
| Configuration | Environment variables and `.env` |
| Reporting | JSON, Markdown |
| Execution tracing | JSON Lines (JSONL) |
| Testing | Python offline regression tests |

## 3. System Architecture

```text
                         USER TASK
                             |
                             v
                    OmniAgent AI CLI
                             |
                             v
                      Agent Controller
                             |
              +--------------+--------------+
              |              |              |
              v              v              v
         Gemini LLM      File Manager   Execution Budget
          (Planner)           |              |
              |              |              |
              +--------------+--------------+
                             |
                             v
                     Plan Validation
                             |
                             v
                       MCP Client
                             |
                             v
                    Local MCP Server
                             |
             +---------------+---------------+
             |               |               |
             v               v               v
       Translation         Speech            OCR
             |          Processing           |
             |               |               |
             +---------------+---------------+
                             |
                             v
                       Bodhan AI API
                             |
                             v
                       Tool Results
                             |
                             v
                   Agent Continues or Ends
                             |
              +--------------+--------------+
              |              |              |
              v              v              v
          report.json     result.md      trace.jsonl
```

### How the Agent Works

1. The user submits a natural-language task through the CLI.
2. The agent connects to the local Bodhan MCP server and discovers available tools.
3. Gemini analyzes the task and chooses an appropriate action.
4. The agent validates the proposed tool call and its arguments.
5. The execution budget is checked before invoking the tool.
6. The MCP server communicates with the corresponding Bodhan AI API.
7. The tool result is returned to the agent.
8. Gemini decides whether another tool is required or the task is complete.
9. The agent generates execution reports and traces.

The LLM handles planning and orchestration. Bodhan AI handles the specialized multilingual processing.

## 4. Available MCP Tools

| Tool | Description |
|---|---|
| `translate_text` | Translates text between supported languages |
| `transcribe_audio` | Converts speech audio into text |
| `generate_speech` | Generates speech audio from text |
| `extract_text` | Extracts text from images using OCR |

The MCP server also exposes `server_status` for configuration and service-status inspection.

Actual language, voice, and format support depends on the configured Bodhan AI APIs.

## 5. Project Structure

```text
OmniAgentAI/
|
|-- src/
|   |-- bodhan_agent/
|   |   |-- __init__.py
|   |   |-- __main__.py
|   |   |-- agent.py
|   |   |-- audio.py
|   |   |-- budget.py
|   |   |-- cli.py
|   |   |-- cost.py
|   |   |-- files.py
|   |   |-- planner_retry.py
|   |   |-- reporting.py
|   |   |-- retry.py
|   |   |-- tracing.py
|   |   |-- transcription.py
|   |   |-- transcription_mcp.py
|   |
|   |-- bodhan_mcp/
|       |-- __init__.py
|       |-- __main__.py
|       |-- audio_tools.py
|       |-- ocr_tools.py
|       |-- server.py
|       |-- translation_tools.py
|       |-- tts_tools.py
|
|-- tests/
|-- data/
|-- demo_inputs/
|-- demo_outputs/
|-- .env.example
|-- .gitignore
|-- pyproject.toml
|-- run_tests.py
|-- README.md
```

### Important Modules

- `agent.py` — Agent orchestration, planning, tool execution, and workflow control.
- `cli.py` — Command-line interface and execution options.
- `server.py` — MCP server exposing Bodhan AI capabilities.
- `budget.py` — Tool-call budget management.
- `cost.py` — API usage and cost tracking.
- `planner_retry.py` — Retry handling for LLM planning.
- `retry.py` — Retry logic for tool execution.
- `audio.py` — Audio file processing.
- `transcription.py` — Audio transcription and chunking utilities.
- `transcription_mcp.py` — MCP-based chunked transcription.
- `files.py` — Input file management and access validation.
- `reporting.py` — Execution report generation.
- `tracing.py` — Execution event logging.

## 6. Installation

### Prerequisites

- Python 3.11
- Git
- Internet connection
- Bodhan AI API credentials
- Gemini API key

### Step 1 — Clone the Repository

```bash
git clone YOUR_GITHUB_REPOSITORY_URL
cd OmniAgentAI
```

Replace `YOUR_GITHUB_REPOSITORY_URL` with the actual repository URL after publication.

### Step 2 — Create a Virtual Environment

On Windows PowerShell:

```powershell
python -m venv .venv
```

Activate the environment:

```powershell
.\.venv\Scripts\Activate.ps1
```

### Step 3 — Install Dependencies

```powershell
python -m pip install -e .
```

### Step 4 — Configure Environment Variables

Copy the example configuration:

```powershell
Copy-Item .env.example .env
```

Open `.env` and configure your credentials:

```dotenv
# Bodhan AI API
BODHAN_BASE_URL=https://api.bodhan.ai/v1

BODHAN_TRANSCRIBE_KEY=your_transcription_key
BODHAN_TRANSLATE_KEY=your_translation_key
BODHAN_SPEAK_KEY=your_speech_key
BODHAN_OCR_KEY=your_ocr_key

# Gemini LLM
LLM_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai/
LLM_MODEL=gemini-2.5-flash
LLM_API_KEY=your_gemini_api_key
```

**Security:** Never commit `.env` or real API keys. The repository's `.gitignore` excludes `.env`.

## 7. Running OmniAgent AI

Open a PowerShell terminal in the project root.

Set the Python source path:

```powershell
$env:PYTHONPATH = (Resolve-Path .\src).Path
```

### Example 1 — Hindi OCR and English Translation

```powershell
python -m bodhan_agent.cli `
  --task "Extract Hindi text from data/sample_ocr.png and translate it into English." `
  --files data `
  --out out `
  --max-steps 4 `
  --max-tool-calls 2
```

Expected workflow:

```text
User Task
   |
   v
extract_text
   |
   v
Hindi Text
   |
   v
translate_text
   |
   v
English Translation
   |
   v
Task Completed
```

### Example 2 — Hindi Speech Transcription

```powershell
python -m bodhan_agent.cli `
  --task "Transcribe the Hindi speech in data/sample_hindi.wav." `
  --files data `
  --out out `
  --max-steps 4 `
  --max-tool-calls 2
```

The agent can process WAV audio through the transcription tool. Longer WAV files can be split into chunks, with each MCP call counting toward the execution budget.

### CLI Parameters

| Parameter | Purpose |
|---|---|
| `--task` | Natural-language instruction |
| `--files` | Allowed input directory |
| `--out` | Output directory |
| `--max-steps` | Maximum agent planning steps |
| `--max-tool-calls` | Maximum MCP tool-call attempts |

An INR spending-limit option is not provided because complete API pricing information is unavailable.

## 8. Verified Live Demonstration

### Task

Extract Hindi text from an image and translate it into English.

### Input

`demo_inputs/sample_ocr.png`

### Execution

**Step 1: OCR**

Tool: `extract_text`

Extracted text:

```text
नमस्ते भारत
```

**Step 2: Translation**

Tool: `translate_text`

Translated text:

```text
hello india
```

### Execution Summary

| Metric | Result |
|---|---|
| Status | Success |
| Tools executed | `extract_text`, `translate_text` |
| Completed tool steps | 2 |
| Maximum tool calls | 2 |
| Tool calls used | 2 |
| Remaining calls | 0 |
| Translation | `hello india` |

The agent successfully completed the task within its configured tool-call budget.

### Demo Artifacts

- `demo_inputs/sample_ocr.png` — sample OCR input
- `demo_outputs/report.json` — structured execution report
- `demo_outputs/result.md` — readable execution summary
- `demo_outputs/trace.jsonl` — execution event trace

## 9. Execution Budget

OmniAgent AI enforces a maximum number of MCP tool calls.

For example:

```powershell
--max-tool-calls 2
```

The execution budget counts tool-call attempts, including retries.

The agent checks the remaining budget before making another tool call.

**Important:** A report containing `budget_exhausted: true` does not automatically mean the task failed. A task may complete successfully after using exactly the allowed number of calls.

## 10. Cost Tracking

The application tracks:

- Total tool calls
- Calls by tool
- Known costs when pricing information is available
- Calls with unknown costs
- Whether total cost reporting is complete

When complete pricing information is unavailable, the agent reports the actual INR cost as unknown.

This prevents unsupported cost estimates from being presented as verified spending.

**Limitation:** The project does not enforce an INR spending ceiling.

## 11. Retry and Error Handling

The project includes retry mechanisms for supported transient failures.

These mechanisms cover LLM planning and MCP tool execution.

Retries are subject to the execution budget, so repeated tool calls cannot bypass the configured limit.

The agent also validates planned actions and file access before executing supported tool operations.

## 12. Execution Reports and Tracing

Each agent execution generates output files in the selected output directory.

### report.json

Contains structured execution information, including:

- Task
- Execution status
- Executed tools
- Completed steps
- Budget usage
- Cost information
- Tool results

### result.md

Contains a human-readable summary of the task, workflow, results, budget, and cost information.

### trace.jsonl

Records timestamped execution events, such as:

- Agent start
- MCP connection
- Planning decisions
- Budget reservations
- Tool calls
- Tool results
- Agent completion

Execution traces may contain user inputs, tool arguments, file paths, and API results. Review these files before sharing them publicly.

## 13. Testing

Run the offline regression test runner:

```powershell
python run_tests.py
```

### Latest Verified Regression Results

```text
tests/test_audio.py: 5 checks PASS
tests/test_chunked_transcription.py: 4 checks PASS
tests/test_transcription_mcp.py: 9 checks PASS
tests/test_agent_chunk_routing.py: 5 checks PASS
tests/test_planner_retry.py: 5 checks PASS

PASS: 5/5 suites passed!
All offline regression tests passed!
```

**Total: 28 checks passed across 5 test suites.**

Additional test scripts are included in the `tests/` directory for individual components and live integrations.

Live tests require valid credentials, network connectivity, and sufficient provider quota.

## 14. Known Limitations

1. **API access:** Live execution requires valid Bodhan AI and Gemini credentials.
2. **Rate limits:** Provider quotas may prevent execution or cause temporary failures.
3. **LLM planning:** Tool selection depends on the language model and may not always be correct.
4. **Audio formats:** Chunk-based transcription is implemented for WAV files.
5. **Cost visibility:** Actual INR costs cannot be guaranteed without complete provider pricing.
6. **Execution limits:** Tool-call and planning-step limits may stop longer workflows.
7. **Trace privacy:** Execution logs may contain user-provided information and should be reviewed before publication.

## 15. Security Considerations

- Store API credentials in `.env`.
- Keep `.env` excluded from Git.
- Never include actual API keys in `.env.example`.
- Restrict file access to configured input directories.
- Review generated reports and traces before sharing.
- Avoid committing confidential recordings, documents, or personal data.
- Use sample inputs for public demonstrations.

## 16. Project Status

The following capabilities have been implemented and tested:

- LLM-driven agent planning
- Bodhan MCP tool discovery and execution
- OCR and translation integration
- Speech transcription integration
- Speech generation tool integration
- Multi-step tool orchestration
- Execution budgeting
- Retry handling
- WAV chunked transcription
- Cost visibility
- JSON and Markdown reporting
- JSONL execution tracing
- Offline regression testing

A live OCR-to-translation workflow and live Hindi transcription have been successfully executed.

---

**OmniAgent AI — Multimodal, multilingual agent orchestration powered by Bodhan AI, MCP, and Gemini.**