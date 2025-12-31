# 🛡️ Secure LLM Project - *Guardian of the Secret*

This project showcases the implementation of security guardrails designed to protect a Large Language Model against **Prompt Injection** attacks.

## 🚀 Features
- **Input Filtering**: User prompts are analyzed before being forwarded to the model.
- **Output Sanitization**: Model responses are scanned via Regex and Leetspeak normalization to prevent sensitive data leakage.
- **Context Window Management**: Memory limitations are enforced to mitigate “context stuffing” attacks.
- **Streamlit Interface**: Interactive chat interface for secure model interaction.

## 🧠 Reasoning Strategy
This project implements a **Multi-Layer Defense Architecture**:
1. **System Prompt Constraints**: Imperative instructions define and restrict the LLM’s behavior at the foundational level.
2. **Deterministic Post-Processing**: Since LLMs are probabilistic by nature, an additional Python/Regex layer validates each response as a final arbiter to ensure no password-like patterns are ever disclosed.
3. **Entropy Reduction**: The model’s `temperature` is set to 0.4 to stabilize output and reduce improvisation risks.

## 🛠 Installation
1. `pip install -r requirements.txt`
2. `streamlit run app.py`
