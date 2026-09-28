# 🩺 Healthcare Intelligence System

> From symptoms to specialists — an AI-powered assistant for symptom analysis, health guidance and doctor discovery.

![AI](https://img.shields.io/badge/AI-NLP-blue)
![ML](https://img.shields.io/badge/ML-RF%20%7C%20SVM%20%7C%20XGBoost-green)
![XAI](https://img.shields.io/badge/Explainable%20AI-SHAP-orange)
![Platform](https://img.shields.io/badge/Platform-Web%20%2B%20Mobile-lightgrey)
![Status](https://img.shields.io/badge/Status-Proposed%20Design-yellow)

---

## 📖 Overview

Healthcare Intelligence Platform is an AI-driven web and mobile application that helps users understand their symptoms, get reliable health information, and find the right doctor. Users describe symptoms by **text or voice**, and the platform predicts the most likely conditions, explains *why*, recommends the appropriate specialist, and locates doctors in their city.

## ❗ Problem & Vision

| The Gap | Our Answer |
|---|---|
| **Unreliable self-diagnosis** — searching symptoms online returns scattered, conflicting and often alarming results. | **Analyze** — ML models predict the top-3 likely conditions from text or voice symptoms. |
| **Which specialist to see?** — people can't tell which type of doctor matches their symptoms. | **Act** — maps the disease to a specialist and finds doctors by city. |
| **Information without reasons** — predictions with no explanation are hard to trust and act on. | **Explain** — SHAP shows which symptoms drove each prediction. |
| | **Ask** — a RAG-powered chatbot answers health questions in plain language. |

## ✨ Features

- 🗣️ **Text & voice symptom input** — describe symptoms in everyday language; speech is transcribed via Speech-to-Text.
- 🧠 **NLP symptom extraction** — tokenisation, lemmatisation, and synonym normalisation (e.g. "feeling hot" → `fever`).
- 📊 **ML prediction engine** — Random Forest, SVM and XGBoost compared and combined in an ensemble.
- 🔍 **Explainable AI** — SHAP feature importance shows which symptoms pushed the model toward a result.
- 🏆 **Top-3 predictions with confidence** — users see alternatives, not a single verdict.
- 💬 **AI Health Chatbot (RAG)** — answers grounded in a curated medical knowledge base.
- 🩻 **Health info** — description, precautions and warning signs for each predicted condition.
- 👨‍⚕️ **Specialist mapping** — disease → recommended specialist (e.g. Migraine → Neurologist).
- 📍 **Doctor discovery** — search by specialist + city; results show doctor, hospital and contact/website.
- 📄 **AI health report** — compiled after every analysis and saved to user history.
- 🔐 **Authentication & dashboard** — register, login, profile, and history.

## 🏗️ System Architecture

```
┌──────────────────────────────────────────────┐
│  Web App (Streamlit / React)  ·  Mobile App  │
│              (React Native)                  │
└──────────────────────┬───────────────────────┘
                       │
        ┌──────────────┴───────────────┐
        │  Authentication  ·  Dashboard │
        └──────────────┬───────────────┘
                       │
   ┌───────────────────┼────────────────────┐
   ▼                   ▼                    ▼
Symptom Analysis   AI Health Chatbot      History
(NLP → ML → XAI)      (RAG engine)     (saved reports)

After a prediction:
Health Info → Specialist Mapping → Doctor Search → AI Health Report → Save to History
```

### Module 1 — Symptom Analysis

```
Text / Voice ──► Speech-to-Text ──► NLP Processing ──► Symptom Vector ──► ML Models ──► Top-3 + SHAP
```

1. **Input** — typed text, or voice transcribed to text. Both paths converge into one clean text string.
2. **NLP** — *Symptom Extraction* (spaCy / NLTK) → *Symptom Normalization* (synonym dictionary) → *Feature Engineering* (fixed-length binary vector).
3. **Prediction** — Random Forest, SVM and XGBoost are compared on Accuracy, Precision, Recall and F1; the best model or an ensemble is selected.
4. **Explainability** — SHAP highlights the symptoms that most influenced the prediction.

**Example**

```
"splitting headache and feeling hot"  →  [headache, fever]  →  [1, 1, 0, 0, 0, …]
```

**Sample output** *(illustrative)*

| Condition | Confidence |
|---|---|
| Common Cold | 58% |
| Influenza | 27% |
| Allergic Rhinitis | 11% |

### Module 2 — AI Health Chatbot (RAG)

```
Text/Voice → Question Processing → Intent Detection → Knowledge Retrieval → RAG / AI Engine → Health Answer
```

**Why RAG?**
- Answers grounded in a medical knowledge base
- Fewer made-up facts than a stand-alone LLM
- Update knowledge without retraining models

### Module 3 — Report & History

Every analysis generates an **AI Health Report** containing:

1. Symptoms reported by the user
2. Top-3 predicted conditions with confidence
3. Important symptoms from the SHAP explanation
4. Description, precautions and warning signs
5. Recommended specialist type
6. Doctors, hospitals and contact details

Reports are saved per user (date, symptoms, predictions, report) so past analyses can be revisited at any time.

### Specialist Mapping (samples)

| Disease | Specialist |
|---|---|
| Hypertension | Cardiologist |
| Migraine | Neurologist |
| Diabetes | Endocrinologist |
| Fungal infection | Dermatologist |
| Gastroenteritis | Gastroenterologist |

The full table covers every disease the model predicts.

## 🧰 Tech Stack (Proposed)

| Layer | Technologies |
|---|---|
| **Frontend** | Streamlit / React (web), React Native (mobile), browser/device mic for voice |
| **Backend & Auth** | Python, FastAPI / Flask, JWT-based login and sessions, SQL / Firebase for user history |
| **NLP & Speech** | spaCy / NLTK, Whisper / SpeechRecognition, synonym-based symptom mapping |
| **ML & Explainability** | scikit-learn (Random Forest, SVM), XGBoost + ensemble voting, SHAP |
| **Chatbot (RAG)** | Curated medical knowledge base, embeddings + vector store (FAISS), LLM for grounded answers |
| **Location & Reports** | Places / Maps API for doctors, city-based specialist search, PDF report generation |

## 🔒 Security & Privacy

- Passwords are hashed
- Token-based (JWT) sessions
- Health data is stored per user and kept private

## 🚀 Getting Started

> ⚠️ Add your real setup steps here once the implementation is in place. A typical layout:

```bash
# Clone the repository
git clone https://github.com/<your-username>/healthcare-intelligence-platform.git
cd healthcare-intelligence-platform

# Backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Run the API
uvicorn app.main:app --reload   # adjust to your entry point
```

## 🗺️ Roadmap

- [ ] Multilingual voice & chat (regional languages)
- [ ] Wearable & vitals integration (heart rate, SpO₂, sleep)
- [ ] Appointment booking / teleconsultation
- [ ] Image & lab-report analysis (scans, skin photos, test reports)

## ⚕️ Medical Disclaimer

This platform provides **information only** and is **not a substitute for professional medical advice, diagnosis or treatment**. Always consult a qualified healthcare provider. In an emergency, contact your local emergency services immediately.

## 🤝 Contributing

Contributions are welcome! Please open an issue to discuss what you'd like to change, then submit a pull request.

## 📄 License

Distributed under the MIT License. See `LICENSE` for details. *(Change if you use a different license.)*
