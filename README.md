# Vinay Kumar Mandadi

**Empirical Software Engineering · Mining Software Repositories · Calibration under Domain Shift**

B.Tech Computer Science and Engineering, Lovely Professional University (2026)<br/>
Applying for MS / PhD positions, 2027 intake

[mvkchowdary20@gmail.com](mailto:mvkchowdary20@gmail.com) · [LinkedIn](https://linkedin.com/in/vinay-kumar-chowdary) · [Zenodo preprints](https://zenodo.org/me/uploads) · [DevSignal HQ](https://github.com/marketplace/devsignal-hq)

---

## Research

I mine large repository corpora to predict failures in automated systems, and study what happens to those predictions when the data distribution moves.

The recurring finding across my work is that within-project performance is a poor guide to cross-project performance, and that most reported accuracy figures hide this. In CI failure prediction, AUC-ROC falls from 0.845 within-repository to 0.687 across repositories. In intrusion detection, accuracy falls from 99.02% to 31.9% when the dataset changes. Both numbers are in my papers because the gap is the result worth reporting — and in both cases post-hoc calibration recovers a usable part of it. I want to continue on cross-project generalisation, calibrated confidence, and explanation methods that survive distribution shift.

I also build the tools. One of my studies is now a GitHub App that developers install, which I think is the right end state for software engineering research.

### Publications

**Vinay Kumar Mandadi.** Explainable and Trustworthy Network Intrusion Detection: From Multi-Class SHAP Analysis to Confidence-Aware Deployment. *13th International Conference on Intelligent Systems (IEEE IS'26)*, Varna, Bulgaria, September 2026. **Accepted.**<br/>
<sub>Multi-class SHAP attribution over CIC-IDS2017 (99.02% accuracy, macro F1 0.906). Cross-dataset evaluation on UNSW-NB15 shows accuracy collapsing to 31.9%; Platt scaling recovers 68.0%. FastAPI inference service, Streamlit dashboard.</sub><br/>
[Preprint · DOI 10.5281/zenodo.20698544](https://doi.org/10.5281/zenodo.20698544)

**Vinay Kumar Mandadi.** Wide vs. Deep: Predicting CI Failures from Repository Signals. *Under review.*<br/>
<sub>28,836 GitHub Actions runs mined across 456 repositories. Introduces the `prev_failed` signal; SHAP decomposition of true-positive against false-negative predictions. Within-repository AUC-ROC 0.845, cross-repository 0.687. Implemented as DevSignal HQ.</sub>

**Vinay Kumar Mandadi.** AIRIMF: AI-Driven Risk Identification and Mitigation Framework. *Submitted to IEEE ICMACC 2026.*<br/>
<sub>Leakage-free Random Forest with SMOTE over GitHub issue metadata. 76% cross-validated accuracy on VS Code, 83% on React. Platt scaling reduces expected calibration error fivefold across projects.</sub><br/>
[Preprint · DOI 10.5281/zenodo.20691021](https://doi.org/10.5281/zenodo.20691021)

Dual-Branch Deep Learning Framework for Deepfake Detection. *8BIC 2026.* **Accepted.** Co-author.<br/>
<sub>EfficientNetV2-M over RGB with a CNN over DCT coefficients. 94.4% on FaceForensics++, GradCAM attribution.</sub>

---

## Software

**[DevSignal HQ](https://github.com/marketplace/devsignal-hq)** — A GitHub App that scores the failure risk of a CI run before the workflow finishes.<br/>
<sub>Implements the model from *Wide vs. Deep*. Webhook-triggered, JWT-authenticated, scores returned within seconds of a push. Flask, deployed on Railway. Live on the GitHub Marketplace.</sub>

**PaperLens** — A service that checks a manuscript for AI-generated text and for overlap with the published literature.<br/>
<sub>Plagiarism side queries the open scholarly APIs — Semantic Scholar, OpenAlex, CrossRef, CORE — for semantic overlap rather than string matching. AI-detection side runs an ensemble and reports per-agent disagreement instead of a single verdict, which is the same calibration argument as my papers: an unqualified score on a borderline document is worse than an honest spread. FastAPI, React, OAuth, async task queue.</sub>

**Veridian** — A multi-model agent that cross-examines its own answer before returning it.<br/>
<sub>Analyst drafts, two independent Auditors critique in parallel, an Arbiter scores consensus. Retrieval-grounded. MCP server, layered input validation, full audit trail. Kaggle AI Agents Intensive Capstone 2026.</sub>

**AMAV** — A Proposer–Critic–Arbiter pipeline for validating machine-generated research summaries.<br/>
<sub>Four LLMs across three providers; emits confidence scores, reasoning traces and dissent logs rather than a single unattributed answer.</sub>

 A game-playing agent for the Kaggle PTCG AI Battle competition.<br/>
<sub>Strategy derived from replay analysis of losses — diagnosing failure modes and iterating the policy against them.</sub>

**AssetFlow** — Enterprise asset and resource management backend. *Odoo Hackathon 2026, sole backend developer.*<br/>
<sub>Django 6.0. Role-based access control enforced server-side, double-allocation and time-slot overlap prevented at the model layer, maintenance state machine, 12 unit and integration tests over the business rules.</sub>

---

## Currently

- Preparing the camera-ready version of the IS'26 paper (August 2026)
- Extending the CI failure work toward calibrated cross-repository prediction
- Writing multi-hop evaluation tasks with verified source trajectories for Handshake AI (Project Seal India)
- Studying Japanese, working toward JLPT N2

I am looking for a research group in empirical software engineering, mining software repositories, or trustworthy machine learning. I have done this work alone so far and would like to do it with people. Enquiries from supervisors are welcome.

---

## Background

**Methods** — Repository mining, empirical study design, leakage-free evaluation, calibration under domain shift (Platt scaling, ECE), SHAP attribution, class imbalance handling, cross-project validation

**Languages & tools** — Python, scikit-learn, SHAP, pandas, NumPy, PyTorch, TypeScript, SQL

**Infrastructure** — Oracle Cloud Infrastructure (Professional: Multicloud Architect, DevOps), Google Cloud, AWS, Terraform, Docker, Kubernetes, GitHub Actions, Jenkins

**Services** — FastAPI, Flask, Django, Node.js, React, PostgreSQL, MongoDB

**Also** — National Finalist, GDG Solution Challenge 2025 (#105 of 3,700+ teams). GRE 324 (Q168/170). Second-generation operator of a family stone-crushing plant — equipment uptime, scheduling, compliance.

<sub>English: full professional (MOI certified) · Telugu: native · Hindi: fluent · Japanese: coursework to N4 level, preparing for JLPT N2</sub>
