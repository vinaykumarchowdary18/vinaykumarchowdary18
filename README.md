### Hi, I'm Vinay 👋

**Model Calibration · Out-of-Distribution Generalization · Failure Prediction in Operational Systems**

<!-- BEFORE PUBLISHING: create a Google Scholar profile and an ORCID iD (~10 min each), then replace the two placeholder URLs below and delete this comment. -->
[![Email](https://img.shields.io/badge/Email-1f1f1f?style=flat-square&logo=gmail&logoColor=white)](mailto:mvkchowdary20@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-1f1f1f?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/vinay-kumar-chowdary)
[![Google Scholar](https://img.shields.io/badge/Google%20Scholar-1f1f1f?style=flat-square&logo=googlescholar&logoColor=white)](https://scholar.google.com/)
[![ORCID](https://img.shields.io/badge/ORCID-1f1f1f?style=flat-square&logo=orcid&logoColor=white)](https://orcid.org/)
[![Zenodo](https://img.shields.io/badge/Zenodo-1f1f1f?style=flat-square&logo=zenodo&logoColor=white)](https://zenodo.org/me/uploads)

I'm a Computer Science graduate from Lovely Professional University (2026). My research asks one question across several domains: **when a model meets data it wasn't trained on, does its confidence still mean anything — and can we predict the failure before it happens?**

The application areas differ — CI/CD pipelines, network intrusion detection, software risk, synthetic media — but the methodology is consistent: build the model, measure honestly what happens when the distribution shifts, explain the decision with post-hoc attribution, then recover trustworthy confidence through calibration. Three of my four papers apply Platt scaling to that problem; three of four report the cross-domain number that goes *down*. A model reporting 99% accuracy that fails silently on new data is more dangerous than one reporting 68% and knowing it.

I've authored **three independent studies** in this area alongside **collaborative research** with co-authors, and I'm equally at home shipping the result — one paper became a product now live on the GitHub Marketplace. I hold **OCI Professional certifications in DevOps and Multicloud Architecture**, so I operate the CI/CD infrastructure my research studies rather than only modelling it.

**I'm looking for MS or PhD positions starting in 2027** — in empirical software engineering, mining software repositories, trustworthy ML, or AIOps. I'd welcome the chance to do this work inside a group rather than alone. Enquiries welcome: [mvkchowdary20@gmail.com](mailto:mvkchowdary20@gmail.com)

---

## Research

**Explainable and Trustworthy Network Intrusion Detection: From Multi-Class SHAP Analysis to Confidence-Aware Deployment**<br/>
Sole author · **Accepted, IEEE IS'26** — 13th International Conference on Intelligent Systems, Varna, Bulgaria, Sept 2026<br/>
99.02% in-domain accuracy on CIC-IDS2017 (macro F1 0.906), collapsing to 31.9% under domain shift to UNSW-NB15. Platt scaling recovers 68.0% out-of-distribution accuracy. Served through a FastAPI endpoint with a Streamlit dashboard.<br/>
[Preprint (DOI)](https://doi.org/10.5281/zenodo.20698544) · `SHAP` `scikit-learn` `FastAPI` `CIC-IDS2017` `UNSW-NB15`

<br/>

**Wide vs. Deep: Predicting CI Failures from Repository Signals**<br/>
Sole author · *under review*<br/>
Empirical study over 28,836 CI runs across 456 repositories. Introduces the `prev_failed` signal and decomposes true-positive against false-negative behaviour using SHAP. AUC-ROC falls from 0.845 within-repository to 0.687 cross-repository — the generalisation gap is the contribution, not a caveat.<br/>
Productised as [DevSignal HQ](https://github.com/marketplace/devsignal-hq) · `SHAP` `GitHub Actions` `scikit-learn`

<br/>

**AIRIMF: AI-Driven Risk Identification and Mitigation Framework**<br/>
Sole author · *submitted, IEEE ICMACC 2026*<br/>
Leakage-free Random Forest pipeline with SMOTE for software risk prediction from GitHub ticket metadata. 76% cross-validated accuracy on VS Code, 83% on React. Platt scaling cuts expected calibration error fivefold on cross-project transfer.<br/>
[Preprint (DOI)](https://doi.org/10.5281/zenodo.20691021) · `scikit-learn` `SHAP` `SMOTE`

<br/>

**Dual-Branch Deep Learning Framework for Deepfake Detection**<br/>
Co-author · **Accepted, 8BIC 2026**<br/>
EfficientNetV2-M over RGB combined with a CNN over the DCT frequency domain for synthetic media authentication. 94.4% accuracy on FaceForensics++, with GradCAM attribution for interpretability.<br/>
`PyTorch` `EfficientNetV2-M` `GradCAM` `FaceForensics++`

---

## Software

**[DevSignal HQ](https://github.com/marketplace/devsignal-hq)** — CI failure prediction, live on the GitHub Marketplace.<br/>
The paper above, shipped. Pre-run scoring fires within seconds of a push, before GitHub finishes executing the workflow. One-click GitHub App install with automatic webhook setup, JWT authentication, and a live HIGH / MEDIUM / HEALTHY dashboard.

<br/>

**Veridian** — Adversarial business intelligence agent.<br/>
Built for the Kaggle AI Agents Intensive Capstone 2026. An Analyst drafts, two Auditors critique in parallel, and an Arbiter scores consensus — all grounded in live retrieval. MCP server, five-layer security pipeline, full audit trail.

<br/>

**AMAV** — Adversarial Multi-Agent Validation.<br/>
A Proposer–Critic–Arbiter loop across four LLMs producing confidence-scored documents with reasoning traces and dissent logging. Built so my own literature work would be verifiable rather than merely plausible.

---

## Experience

- **Founder & sole developer**, DevSignal HQ — research to product, [GitHub Marketplace](https://github.com/marketplace/devsignal-hq) · 2026
- **Research contributor**, Handshake AI (Project Seal India) — adversarial evaluation of frontier models; multi-hop question design with verified source trajectories · 2026
- **Backend developer (sole)**, AssetFlow — Odoo Hackathon 2026; Django 6.0, RBAC, state machines, 12 integration tests
- **Second-generation operator**, family stone-crushing plant — equipment uptime, crew scheduling, logistics, compliance · ongoing

**National Finalist**, GDG Solution Challenge 2025 — #105 of 3,700+ teams · **Diamond League**, [Google Cloud Skills Boost](https://www.skills.google/public_profiles/be7d9bdd-b470-4a25-97f8-e747214f53a1) (20,802 pts, 80 labs)

---

## Skills

- **Languages** — Python (advanced), TypeScript, SQL, Bash, Java
- **ML & research tooling** — scikit-learn, SHAP, pandas, NumPy, PyTorch, SMOTE, Platt scaling
- **Methods** — calibration under domain shift (ECE, reliability curves), post-hoc attribution, leakage-free experimental design, cross-repository and cross-project generalisation, empirical study design over large repository corpora
- **Cloud & infrastructure** — Oracle Cloud Infrastructure (Professional: Multicloud Architect, DevOps), Google Cloud, AWS, Terraform, Docker, Kubernetes
- **CI/CD & operations** — GitHub Actions, Jenkins, GitLab CI, webhook automation, pipeline instrumentation
- **Services & data** — FastAPI, Flask, Django, Node.js, React; PostgreSQL, MongoDB, Firestore

---

## Education

**B.Tech, Computer Science and Engineering** — Lovely Professional University, 2026<br/>
**GRE** — 324 (Quantitative 168/170, Verbal 156/170)<br/>
**Languages** — Telugu (native) · English (full professional, MOI certified) · Hindi (fluent) · Japanese (university coursework to N4 level; preparing for JLPT N2)

<details>
<summary><b>Certifications</b></summary>

<br/>

**OCI Multicloud Architect Professional** (2025) · **OCI DevOps Professional** (2025) · OCI Generative AI (2025) · OCI AI Foundations Associate (2025) · OCI Data Platform Foundations (2025) · OCI Networking (2025) · Google Cloud Skills Boost Diamond League (2026) · Google Cloud ACE training (2025) · Microsoft AI Skills Fest (2026) · AWS Cloud Practitioner Essentials · Docker, Kubernetes & OpenShift (IBM) · Linux Commands & Shell Scripting (IBM)

[Oracle certification repository](https://github.com/vinaykumarchowdary18/Certifications/tree/main/Oracle) · [Google Skills profile](https://www.skills.google/public_profiles/be7d9bdd-b470-4a25-97f8-e747214f53a1)

</details>
