#  Project Guardian - Securing an LLM against Prompt Injection

This project was developed for the Gen AI course (ECE 2025). The objective is to create a conversational agent capable of engaging in normal dialogue while protecting a corporate secret (a password) against increasingly complex manipulation attempts.

##  Installation and Configuration

To run the project locally, follow these steps:

### 1. Cloning and Dependencies

```bash
git clone https://github.com/Quentin-Carminot/GEN-AI.git
pip install -r requirements.txt
```

### 2. Gemini API Key Configuration

The project uses Gemini 1.5 Flash as an external security auditor.

1. Create a `.env` file in the root directory of the project.
2. Add your API key inside:
```text
GEMINI_API_KEY=...
```

The `.env` file is ignored by Git for security reasons

### 3. Launching

```bash
python -m streamlit run app.py
```

##  Security Architecture (Defense in Depth)

We chose not to rely on a single method but rather to stack multiple layers of protection. If one layer is bypassed, the next one takes over.

### Layer 1: Input Guardrail

This is our first line of defense. Before the Qwen model even sees the query, a Python script scans the text for suspicious keywords related to "Jailbreaking" (e.g., ignore instructions, override, bypass, divulge).

* Purpose: Immediately blocks simple and direct attacks, saving computational resources.

### Layer 2: System Prompt (Conditioning)

We configured a robust "System Prompt" that defines the Guardian's identity. It includes 7 strict rules, such as the prohibition of translating the secret or responding to simulated administrative orders.

* Purpose: Guides the LLM's behavior so it prioritizes security over user compliance.

### Layer 3: Context Window Management

The code implements a sliding window that only keeps the system prompt and the last 6 messages in memory.

* Purpose: Prevents "Context Stuffing" attacks where an attacker tries to drown out the initial security instructions under thousands of words to make the model forget them.

### Layer 4: Local Output Filter (Regex & Skeleton)

If the model attempts to reveal the secret, this layer intercepts it. We use "Leetspeak" normalization and a "Skeleton Check" that removes all special characters and spaces.

* Example: If the model generates `E-C-E.p@ss`, the script converts it to `ecepass`, detects the leak, and blocks the message.
* Purpose: Blocks obfuscation attempts (strange spelling, symbols) that raw text filters would miss.

### Layer 5: Final AI Audit (Gemini API)

This is the ultimate layer. The generated response is sent to the Gemini 1.5 Flash API, which acts as an independent auditor. Gemini analyzes the semantic meaning of the sentence to see if it contains a subtle leak or clue.

* Purpose: Detects "intelligent" leaks that a classic Regex script cannot understand (e.g., a riddle or a rhyme revealing the secret).

---

##  Tech Stack

* Local LLM: Qwen 2.5 (0.5B) – lightweight enough to run on local CPU/GPU.
* Cloud LLM: Google Gemini 2.5 Flash (via API).
* Interface: Streamlit.
* Security: Python (re, dotenv), Transformers (Attention masks & Temperature control).
