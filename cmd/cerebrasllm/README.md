# Cerebras CLI Utility (cerebrasllm)

[Read this in Russian | Читать на русском](README_RU.md)

An ultra-fast command-line interface for interacting with the **Cerebras Cloud AI API** (LPU Inference Engine) directly via the Windows command line. This utility features extreme generation speeds using Cerebras custom hardware and supports both text and vision modalities (using `gemma-4-31b`).

## ✨ Features

*   **Sub-second Latency**: Built on top of Cerebras LPUs (Language Processing Units) running inference at thousands of tokens per second.
*   **Multimodality**: Support for text and images/vision tasks using Google's `gemma-4-31b`.
*   **Unified Ecosystem**: Fully integrated with the ClipGen-m ecosystem. Share the same config directory structure and unified session persistence (`mistral_chats`) as the rest of the CLI tools.
*   **API Key Rotation**: Automatically shuffles the key pool to bypass rate limits on Cerebras.
*   **Native Windows Encoding**: Seamlessly translates console encodings (CP866/1251 to UTF-8) for perfect Cyrillic output.

## 🛠 Setup and Installation

Requires [Go 1.25+](https://go.dev/dl/).

```bash
# Navigate to directory
cd cmd/cerebrasllm

# Build executable
go build -o cerebrasllm.exe
```

## ⚙️ Configuration

Settings are stored in `%AppData%\clipgen-m\cerebras.conf`.

**Add your API Key:**
Get your key at [Cerebras Cloud Console](https://cloud.cerebras.ai/).
```powershell
cerebrasllm.exe --save-key ccl-your_api_key_here
```

You can add multiple keys. The utility will rotate through them automatically on requests.

## 🚀 Usage Examples

### 1. Simple text generation (defaults to gemma-4-31b)
```powershell
echo "Explain quantum physics in simple terms" | cerebrasllm.exe
```

### 2. Deep reasoning/Coding tasks (using gpt-oss-120b)
```powershell
echo "Write a concurrent worker pool in Go" | cerebrasllm.exe -m code
```

### 3. Image Analysis / Vision (using gemma-4-31b)
```powershell
echo "Describe the elements in this chart" | cerebrasllm.exe -f "C:\Data\chart.png"
```

### 4. Stateful Conversation (Chat mode)
```cmd
echo "Hi, my name is Alex" | cerebrasllm.exe -chat session_01
echo "What's my name?" | cerebrasllm.exe -chat session_01
```

## 📚 Command-Line Reference

| Flag | Description | Example |
| :--- | :--- | :--- |
| `-f` | Path to file (Image or Text). Can be repeated. | `-f "pic.png"` |
| `-s` | System Prompt (Role/Instruction). | `-s "Act as translator"` |
| `-j` | JSON Mode. Forces the response to be in JSON. | `-j` |
| `-m` | Mode: `auto`, `general`, `code`, `vision`. | `-m vision` |
| `-t` | Temperature (0.0 - 2.0). | `-t 0.7` |
| `-chat`| Unique Chat ID for history persistence. | `-chat session_1` |
| `-v` | Verbose. Logs details to stderr and file. | `-v` |
| `--save-key`| Saves API key to config file and exits. | `--save-key ccl-...` |

## 🧠 Under the Hood

### Model Hierarchy
- **General**: `gemma-4-31b` -> `gpt-oss-120b` -> `zai-glm-4.7`
- **Code**: `gemma-4-31b` -> `gpt-oss-120b` -> `zai-glm-4.7`
- **Vision**: `gemma-4-31b` (only Gemma 4 is multimodal currently)

## 📁 Logs and Storage
*   **Config**: `%AppData%\clipgen-m\cerebras.conf`
*   **Error Logs**: `%AppData%\clipgen-m\cerebras_err.log`
*   **Chat History**: `%AppData%\clipgen-m\mistral_chats\` (Shared with other modules).
