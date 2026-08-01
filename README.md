<div align="center">

# Vinay Kumar Mandadi

**Software Engineering · AIOps · Explainable AI**

Reliability and calibrated trust in automated operational systems — CI/CD pipelines, network defence, and the machine learning that runs both.

[![Email](https://img.shields.io/badge/Email-mvkchowdary20@gmail.com-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:mvkchowdary20@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-vinay--kumar--chowdary-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/vinay-kumar-chowdary)
[![Zenodo](https://img.shields.io/badge/Zenodo-Preprints-024BBB?style=flat-square&logo=zenodo&logoColor=white)](https://zenodo.org/me/uploads)
[![DevSignal HQ](https://img.shields.io/badge/DevSignal_HQ-GitHub_Marketplace-2088FF?style=flat-square&logo=github&logoColor=white)](https://github.com/marketplace/devsignal-hq)

</div>

---

## About

B.Tech in Computer Science and Engineering (Lovely Professional University, 2026). I work on **empirical software engineering and AIOps** — specifically, predicting failures in automated operational systems and deciding when a model's output is trustworthy enough to act on.

That question runs through everything below. In CI/CD, it asks whether a developer should trust a pre-run build risk score. In network defence, it asks whether an automated system should act on a classifier's verdict under domain shift. Both are the same problem, and calibration is where I keep ending up.

Four papers, three as sole author. One shipped product built directly from my own research. Two Oracle Cloud Professional certifications in the infrastructure my research studies.

---

## News

- **Jul 2026** — *Explainable and Trustworthy Network Intrusion Detection* **accepted** at IEEE IS'26 (Varna, Bulgaria).
- **2026** — Deepfake detection paper accepted at 8BIC 2026.
- **2026** — DevSignal HQ submitted to GitHub Marketplace.
- **2026** — GDG Solution Challenge National Finalist (#105 of 3,700+ teams).

---

## Publications

**[1] Explainable and Trustworthy Network Intrusion Detection: From Multi-Class SHAP Analysis to Confidence-Aware Deployment**
*Sole author* · **Accepted — IEEE IS'26**, 13th International Conference on Intelligent Systems, Varna, Bulgaria, 3–5 September 2026 (IEEE Conference #71416)

SHAP-based explainable multi-class intrusion detection on CIC-IDS2017 — 99.02% in-domain accuracy, macro F1 0.906. Quantifies catastrophic degradation under domain shift to UNSW-NB15 (31.9%), then recovers 68.0% out-of-distribution accuracy through Platt scaling. Deployed as a FastAPI endpoint with a Streamlit dashboard.

> Reviewer scores — Scope 5/5 · Organization 5/5 · Significance 4/5 · Novelty 4/5 · Technical quality 4/5

[![DOI](https://img.shields.io/badge/Preprint-10.5281%2Fzenodo.20698544-024BBB?style=flat-square&logo=zenodo&logoColor=white)](https://doi.org/10.5281/zenodo.20698544)
`Python` `SHAP` `scikit-learn` `FastAPI` `Streamlit` `CIC-IDS2017` `UNSW-NB15`

<br/>

**[2] Wide vs. Deep: Predicting CI Failures from Repository Signals**
*Sole author* · Under review

Empirical study across **28,836 CI runs and 456 repositories**. Introduces the `prev_failed` signal, examines calibration under domain shift, and decomposes true-positive versus false-negative behaviour with SHAP. AUC-ROC 0.845 within-repository, degrading to 0.687 cross-repository — the generalisation gap is the finding, not a footnote. Productised as DevSignal HQ.

`Python` `SHAP` `GitHub Actions` `scikit-learn`

<br/>

**[3] AIRIMF: AI-Driven Risk Identification and Mitigation Framework**
*Sole author* · Submitted to IEEE ICMACC 2026 · Preprint available

Leakage-free Random Forest pipeline with SMOTE for software risk prediction from GitHub ticket metadata. 76% cross-validated accuracy on VS Code, 83% on React. Platt scaling reduces expected calibration error five-fold on cross-project transfer.

[![DOI](https://img.shields.io/badge/Preprint-10.5281%2Fzenodo.20691021-024BBB?style=flat-square&logo=zenodo&logoColor=white)](https://doi.org/10.5281/zenodo.20691021)
`Python` `scikit-learn` `SHAP` `NLP`

<br/>

**[4] Dual-Branch Deep Learning Framework for Deepfake Detection**
*Co-author* · Accepted — 8BIC 2026

EfficientNetV2-M over RGB combined with a CNN over the DCT frequency domain for synthetic media authentication. 94.4% accuracy on FaceForensics++, with GradCAM heatmaps for interpretability.

`Python` `PyTorch` `EfficientNetV2-M` `GradCAM`

---

## Research Software

### DevSignal HQ — CI Failure Prediction
[![Marketplace](https://img.shields.io/badge/GitHub_Marketplace-DevSignal_HQ-2088FF?style=flat-square&logo=github&logoColor=white)](https://github.com/marketplace/devsignal-hq)

Predicts GitHub Actions CI failures before a run completes. Pre-run scoring fires within seconds of a push; the alert reaches you before GitHub finishes executing. Built directly from publication [2] — the research findings are the model.

- Pre-run ML scoring using the `prev_failed` signal and SHAP-derived features
- One-click GitHub App install with automatic webhook configuration
- Live prediction dashboard (HIGH / MEDIUM / HEALTHY)
- Flask · GitHub App API · JWT authentication · Railway deployment

**Why it matters:** empirical software engineering has a standing complaint that research tooling never reaches practitioners. This one did.

### Veridian — Adversarial Business Intelligence Agent
*Kaggle AI Agents Intensive Capstone 2026 — Agents for Business track*

Four models argue before an answer is returned. An Analyst (Gemini) drafts, two Auditors critique in parallel, and an Arbiter (GPT-4o-mini) scores consensus. Grounded in live web retrieval via Tavily.

- MCP server implementation · five-layer security pipeline · full audit trail
- `Python` `FastAPI` `Gemini` `Tavily` `MCP`

### AMAV — Adversarial Multi-Agent Validation
*Research validation framework*

A Proposer–Critic–Arbiter loop across four LLMs producing confidence-scored structured documents with reasoning traces and dissent logging. Built to make my own literature work verifiable rather than plausible.

- `Python` `Gemini` `Groq` `OpenRouter` `Tavily`

---

## Selected Projects

**AssetFlow** — Enterprise asset and resource management · *Odoo Hackathon 2026, sole backend developer*
Django 6.0 · RBAC with server-side enforcement · double-allocation and time-slot overlap protection at the model layer · maintenance state machine · 12 unit and integration tests covering all business rules.

**Secure Aadhaar Voting Platform** — Cloud-native GCP application
App Engine autoscaling · Cloud SQL · IAM least-privilege · face verification via Cloud Vision API · secrets in environment configuration.

**StockAI** — Stock market visualisation · [live](https://stockai338211.web.app)
TypeScript strict mode from day one · React Context with error boundaries · automated CI to Firebase Hosting.

**University Deadline Tracker** — Scheduled GitHub Actions pipeline tracking application deadlines with a progress dashboard.

---

## Technical Background

**Primary** — Python, software engineering research tooling, scikit-learn, SHAP, pandas, NumPy
**Cloud & Infrastructure** — Oracle Cloud Infrastructure (Professional level), Google Cloud, AWS; Terraform, Docker, Kubernetes
**CI/CD & Operations** — GitHub Actions, Jenkins, GitLab CI; pipeline instrumentation and failure analysis
**Web & Services** — FastAPI, Flask, Django, Node.js, React, TypeScript
**Data** — PostgreSQL, MySQL, MongoDB, Firestore

**Methods** — Calibration under domain shift (Platt scaling, ECE) · SHAP-based explanation · class imbalance handling (SMOTE) · leakage-free experimental design · cross-repository generalisation · multi-agent LLM orchestration

---

## Certifications

| Certification | Provider | Year |
|---|---|---|
| **OCI Multicloud Architect Professional** | Oracle | 2025 |
| **OCI DevOps Professional** | Oracle | 2025 |
| OCI Generative AI | Oracle | 2025 |
| OCI AI Foundations Associate | Oracle | 2025 |
| OCI Data Platform Foundations | Oracle | 2025 |
| OCI Networking | Oracle | 2025 |
| Google Cloud Skills Boost — Diamond League (20,802 pts · 80 labs) | Google | 2026 |
| Google Cloud Associate Cloud Engineer training | Google | 2025 |

[Full certification repository →](https://github.com/vinaykumarchowdary18/Certifications/tree/main/Oracle) · [Google Skills profile →](https://www.skills.google/public_profiles/be7d9bdd-b470-4a25-97f8-e747214f53a1)

---

## Education & Background

**B.Tech, Computer Science and Engineering** — Lovely Professional University, 2026
**GRE** — 324 (Quantitative 168/170, Verbal 156/170)
**Languages** — Telugu (native) · English (full professional, MOI certified) · Hindi (fluent) · Japanese (elementary, JLPT N4 preparation)

Alongside my studies I help run my family's industrial stone-crushing operation — equipment uptime, crew scheduling, logistics and compliance. It is where I learned that predicting a failure is only useful if someone trusts the prediction enough to act on it, which is more or less what I ended up researching.

---

<div align="center">

**Open to research collaboration in empirical software engineering, AIOps, and trustworthy machine learning.**

[mvkchowdary20@gmail.com](mailto:mvkchowdary20@gmail.com)

</div>
