# 🛡️ AI-Powered Malicious URL Detection System

An Institutional Development Project (IDP) leveraging Large Language Models (LLMs) to perform zero-shot semantic threat analysis on web URLs in real time.

---

## 📌 Project Overview

Traditional malicious URL detection relies heavily on static blacklists (e.g., Google Safe Browsing) or classical machine learning models (XGBoost, Random Forest) trained on handcrafted lexical features. While efficient, these systems struggle against short-lived zero-day phishing domains, typosquatting attacks, and obfuscated URLs.

This project addresses those limitations by utilizing the **Google GenAI SDK (`gemini-3.5-flash-lite`)** to analyze domain intent, lexical structure, and potential social engineering indicators without relying on rigid offline datasets or manual feature extraction.

---

## ✨ Key Features

* **Zero-Shot Semantic Inspection:** Analyzes URL string patterns, brand impersonation attempts, and directory structures directly through LLM reasoning.
* **Deterministic Structured Output:** Enforces strict JSON schema validation returning `is_malicious`, `threat_type`, `confidence_score`, and plain-text reasoning.
* **Real-Time Interactive Dashboard:** Built with Streamlit, providing real-time threat alert banners, dynamic metric cards, and a raw JSON inspector.
* **Resilience to Lexical Obfuscation:** Detects subtle typosquatting tricks (e.g., Cyrillic homoglyphs, visual spoofing like `paypaI.com`), alternative IP encodings (DWORD, Hex), and suspicious URL parameters.

---

## 🛠️ Tech Stack

* **Language:** Python 3.10+
* **Frontend UI:** [Streamlit](https://streamlit.io/)
* **AI Engine:** Google GenAI SDK (`google-genai`)
* **Primary Model:** `gemini-3.5-flash-lite`

---

## 🏗️ System Architecture

```text
                       ┌─────────────────────────┐
                       │     User Enters URL     │
                       └────────────┬────────────┘
                                    │
                                    ▼
                       ┌─────────────────────────┐
                       │   Streamlit Web UI      │
                       └────────────┬────────────┘
                                    │
                                    ▼
                       ┌─────────────────────────┐
                       │  Google GenAI SDK Client│
                       │ (gemini-3.5-flash-lite) │
                       └────────────┬────────────┘
                                    │ (JSON Enforcement)
                                    ▼
                       ┌─────────────────────────┐
                       │  Structured Output      │
                       │  - is_malicious (bool)  │
                       │  - threat_type (str)    │
                       │  - confidence (float)   │
                       │  - reason (str)         │
                       └────────────┬────────────┘
                                    │
                                    ▼
                       ┌─────────────────────────┐
                       │ Streamlit Alert Banners │
                       │  & Metric Visualizations│
                       └─────────────────────────┘

🚀 Quick Start Guide
1. Prerequisites
Ensure Python 3.10 or higher is installed:

Bash
python --version
2. Installation
Clone the repository and install dependencies:

Bash
# Clone repository
git clone [https://github.com/your-username/malicious-url-detector.git](https://github.com/your-username/malicious-url-detector.git)
cd malicious-url-detector

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install streamlit google-genai
3. Configuration
Open app.py and replace YOUR_GEMINI_API_KEY_HERE with your actual Gemini API key:

Python
API_KEY = "AIzaSy..."
(Alternatively, set the environment variable export GEMINI_API_KEY="AIzaSy...")

4. Run the Application
Launch the dashboard:

Bash
streamlit run app.py
The application will open automatically at http://localhost:8501.

📂 Project Structure
Plaintext
├── app.py              # Main application containing Streamlit UI and GenAI logic
├── requirements.txt    # Python dependencies
└── README.md           # Project documentation
📊 Sample Output Schema
JSON
{
  "url": "[http://paypaI.com-login-security.info/verify](http://paypaI.com-login-security.info/verify)",
  "is_malicious": true,
  "threat_type": "phishing",
  "confidence_score": 0.98,
  "reason": "The domain utilizes typosquatting (capital 'I' replacing 'l' in 'paypaI') combined with defensive keyword stacking ('login-security') on a suspicious top-level domain (.info) to impersonate PayPal."
}
🗺️ Future Roadmap
Phase 1: Pre-LLM Telemetry Engine: requests redirect tracing (unshortening bit.ly) and WHOIS domain age extraction.

Phase 2: Multimodal Visual Verification: Playwright headless DOM screenshots passed to Gemini Vision to detect fake visual UIs.

Phase 3: Browser Extension: Decoupling backend logic into a FastAPI service with a Manifest V3 Chrome Extension.

Phase 4: Offline SLM Deployment: Fine-tuning a quantized 3B model (e.g., Llama-3.2-3B / Phi-3.5) for local zero-cloud scanning via Ollama.

