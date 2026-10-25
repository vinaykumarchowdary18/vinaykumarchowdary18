# Vinay Kumar Mandadi

**Applied AI · Cloud Systems · Technology Management**

B.Tech Computer Science and Engineering (Cloud Computing), Lovely Professional University, completed July 2026<br/>
Looking to study applied AI and technology management at Master's level, 2027 intake

[mvkchowdary20@gmail.com](mailto:mvkchowdary20@gmail.com) · [LinkedIn](https://linkedin.com/in/vinay-kumar-chowdary) · [Zenodo preprints](https://zenodo.org/search?q=mandadi+vinay) · [DevSignal HQ](https://github.com/marketplace/devsignal-hq)

---

## About

I build systems that work in production, not just on paper. My focus is applied AI and the cloud infrastructure it runs on: DevOps pipelines, containerised multi-agent systems, and models whose confidence can be trusted when the data changes.

A recurring finding in my own work is that within-project performance is a poor guide to cross-project performance, and that most reported accuracy figures hide this. In CI failure prediction, AUC-ROC falls from 0.845 within-repository to 0.687 across repositories. In intrusion detection, accuracy falls from 99.02% to 31.9% when the dataset changes. Both gaps are in my write-ups because the gap is the result worth reporting, and in both cases post-hoc calibration recovers a usable part of it.

One of my studies became a GitHub App, DevSignal HQ, which I built and deployed end to end.

---

## Experience

**Contract AI Researcher, Project Seal India (Handshake AI)**, July 2026 to August 2026<br/>
<sub>Wrote multi-hop evaluation tasks with verified source trajectories.</sub>

---

## Publications

**Vinay Kumar Mandadi.** Explainable and Trustworthy Network Intrusion Detection: From Multi-Class SHAP Analysis to Confidence-Aware Deployment. *13th International Conference on Intelligent Systems (IEEE IS'26)*, Varna, Bulgaria, September 2026. **Accepted.** Not published in the proceedings.<br/>
<sub>Multi-class SHAP attribution over CIC-IDS2017 (99.02% accuracy, macro F1 0.906). Cross-dataset evaluation on UNSW-NB15 shows accuracy collapsing to 31.9%; Platt scaling recovers 68.0%. FastAPI inference service, Streamlit dashboard.</sub><br/>
[Preprint (v2) · DOI 10.5281/zenodo.21260892](https://doi.org/10.5281/zenodo.21260892)

**Vinay Kumar Mandadi.** AIRIMF: An AI-Driven Risk Identification and Mitigation Framework for Software Project Management. *Preprint.*<br/>
<sub>Leakage-free Random Forest with SMOTE over GitHub issue metadata. 76% cross-validated accuracy on VS Code, 83% on React. Platt scaling reduces expected calibration error fivefold across projects.</sub><br/>
[Preprint · DOI 10.5281/zenodo.20691021](https://doi.org/10.5281/zenodo.20691021)

Combating Synthetic Media Manipulation Using Deep Learning-Based Deepfake Detection Systems. *8BIC 2026.* **Accepted.** Not published. Co-author.<br/>
<sub>Adaptive Forensic Deepfake Detection Algorithm: spatial CNN features combined with frame-to-frame temporal differences into a weighted Deepfake Artifact Score. Evaluated on a balanced dataset of 10,000 samples.</sub>

---

## Software

**OpenGrab Code**: An offline VS Code AI extension that runs a quantized Qwen2.5-Coder model locally.

**[DevSignal HQ](https://github.com/marketplace/devsignal-hq)**: A GitHub App that scores the failure risk of a CI run before the workflow finishes.<br/>
<sub>Built from my CI failure study (28,836 GitHub Actions runs across 456 repositories; AUC-ROC 0.845 within-repository, 0.687 across repositories). Webhook-triggered, JWT-authenticated, scores returned within seconds of a push. Flask, deployed on Railway. Listed on the GitHub Marketplace in Beta; now a completed project, no longer commercially active.</sub>

**PaperLens**: A service that checks a manuscript for AI-generated text and for overlap with the published literature.<br/>
<sub>Plagiarism side queries the open scholarly APIs (Semantic Scholar, OpenAlex, CrossRef and CORE) for semantic overlap rather than string matching. AI-detection side runs an ensemble and reports per-agent disagreement instead of a single verdict, which is the same calibration argument as my papers: an unqualified score on a borderline document is worse than an honest spread. FastAPI, React, OAuth, async task queue.</sub>

**Veridian**: A multi-model agent that cross-examines its own answer before returning it.<br/>
<sub>Analyst drafts, two independent Auditors critique in parallel, an Arbiter scores consensus. Retrieval-grounded. MCP server, layered input validation, full audit trail. Kaggle AI Agents Intensive Capstone 2026.</sub>

**AMAV**: A Proposer, Critic and Arbiter pipeline for validating machine-generated research summaries.<br/>
<sub>Four LLMs across three providers; emits confidence scores, reasoning traces and dissent logs rather than a single unattributed answer.</sub>

**AssetFlow**: Enterprise asset and resource management backend. *Odoo Hackathon 2026, sole backend developer.*<br/>
<sub>Django 6.0. Role-based access control enforced server-side, double-allocation and time-slot overlap prevented at the model layer, maintenance state machine, 12 unit and integration tests over the business rules.</sub>

A game-playing agent for the Kaggle PTCG AI Battle competition.<br/>
<sub>Strategy derived from replay analysis of losses: diagnosing failure modes and iterating the policy against them.</sub>

---

## Currently

Studying Japanese, working toward JLPT N2. Open to conversations about applied AI, cloud systems and technology management; email is the best way to reach me.

---

## Background

**Methods**: Repository mining, empirical study design, leakage-free evaluation, calibration under domain shift (Platt scaling, ECE), SHAP attribution, class imbalance handling, cross-project validation

**Languages & tools**: Python, scikit-learn, SHAP, pandas, NumPy, PyTorch, TypeScript, SQL

**Infrastructure**: Oracle Cloud Infrastructure (Professional: Multicloud Architect, DevOps; Associate: AI Foundations, Data Platform Foundations), Google Cloud, AWS, Terraform, Docker, Kubernetes, GitHub Actions, Jenkins

**Services**: FastAPI, Flask, Django, Node.js, React, PostgreSQL, MongoDB

**Also**: GDG Solution Challenge 2025 (#105 of 3,700+ teams). GRE 324 (Q168/170). Second-generation operator of a family stone-crushing plant: equipment uptime, scheduling, compliance.

<sub>English: full professional (MOI certified) · Telugu: native · Hindi: fluent · Japanese: coursework to N4 level, preparing for JLPT N2</sub>
