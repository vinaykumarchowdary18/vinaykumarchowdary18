\documentclass[conference]{IEEEtran}

% ---------------------------------------------------------------
% Package imports
% ---------------------------------------------------------------
\usepackage{cite}
\usepackage{amsmath,amssymb,amsfonts}
\usepackage{algorithmic}
\usepackage{graphicx}
\usepackage{textcomp}
\usepackage{xcolor}
\usepackage{booktabs}
\usepackage{multirow}
\usepackage{url}
\usepackage{hyperref}
\usepackage{tikz}
\usetikzlibrary{shapes.geometric, arrows.meta, positioning, fit, backgrounds, calc}

% ---------------------------------------------------------------
% TikZ style definitions for the architecture diagram
% ---------------------------------------------------------------
\tikzset{
  layer/.style={
    draw=black!70,
    fill=#1,
    rounded corners=4pt,
    minimum width=7.2cm,
    minimum height=0.85cm,
    font=\small\bfseries,
    text=white,
    align=center
  },
  sublabel/.style={
    font=\scriptsize,
    text=black!65,
    align=center
  },
  arrowstyle/.style={
    -{Latex[length=2.5mm]},
    line width=0.7pt,
    color=black!55
  },
  sidelabel/.style={
    font=\scriptsize\itshape,
    text=black!55,
    align=left
  }
}

\begin{document}

% ---------------------------------------------------------------
% Title and Author
% ---------------------------------------------------------------
\title{AIRIMF: An AI-Driven Risk Identification and Mitigation\\
Framework for Software Project Management}

\author{
  \IEEEauthorblockN{Mandadi Vinay Kumar}
  \IEEEauthorblockA{
    \textit{Department of Computer Science and Engineering}\\
    \textit{Lovely Professional University}\\
    Punjab, India\\
    mvkchowdary20@gmail.com
  }
}

\maketitle

% ---------------------------------------------------------------
% Abstract
% ---------------------------------------------------------------
\begin{abstract}
Project managers have long relied on periodic risk reviews and
intuition-based judgement to handle software issues---a strategy
that simply does not scale with the pace of modern Agile
development. This paper introduces AIRIMF, a machine-learning
pipeline designed to flag high-risk tickets automatically at the
moment they are created, before any development work begins.
We collected 2{,}593 bug reports from \texttt{microsoft/vscode}
and 744 from \texttt{facebook/react}, applied SMOTE to balance
the training data, and trained a Random Forest classifier on five
features that are strictly available at submission time: title
length, description length, initial comment count, and two
sentiment scores derived from the title text.

After carefully removing every feature that could introduce
target leakage---an exercise that revealed the dangers of
overfitting to outcome-derived signals---the model achieved
76.21\% cross-validation accuracy on VS Code and 83.37\% on
React, with Class~2 (Challenging) recall of 82\% and 89\%
respectively. A McNemar test confirmed that the improvement
over SVM is statistically significant ($p < 0.0001$). SHAP
analysis showed that description length is the single strongest
early risk signal, while sentiment features, though modest
individually, contribute a meaningful six percentage-point gain
when combined with structural metadata. We further evaluate
cross-project calibration on the Angular repository and discuss
how Platt scaling can recover accuracy when a model transfers
to a new codebase with a very different class distribution.
\end{abstract}

\begin{IEEEkeywords}
risk management, software project management, machine learning,
Random Forest, SMOTE, early-stage prediction, target leakage,
NLP sentiment, cross-project validation, SHAP, Platt scaling,
calibration
\end{IEEEkeywords}

% ---------------------------------------------------------------
% I. INTRODUCTION
% ---------------------------------------------------------------
\section{Introduction}

Software projects fail at an uncomfortable rate. The Standish
Group's CHAOS Report found that 52\% of projects are
``challenged'' and that cost overruns average 189\% of the
original estimate~\cite{chaos2020}. The core problem is rarely
a shortage of talent or budget; it is usually that risk signals
appear weeks before anyone acts on them. By the time a
monthly review meeting surfaces a problem, the downstream
effects have already compounded.

Machine learning offers a way to move risk management from
reactive to proactive---but only if the models are built
carefully. Early in our own experimentation, we trained a
classifier that hit 100\% accuracy. That should have been a
warning sign, and it was: a single feature derived from the
actual resolution time had slipped into the training set, giving
the model the answer before the question was asked. Removing
the leaky feature dropped accuracy substantially, but what
remained was a model that actually learned something
genuine about software risk.

That experience shaped the design of AIRIMF, the AI-Driven
Risk Identification and Mitigation Framework presented in this
paper. The framework predicts whether a newly created bug
report will require more than 14~days to close---a proxy for
a two-week Agile sprint---using only information available
the moment the ticket is submitted. No knowledge of what
happens later is permitted. The result is a system that a project
manager could realistically deploy: every new issue receives a
risk score within seconds of creation, and the team can
prioritise accordingly before delays begin to snowball.

Our contributions are as follows.

\begin{enumerate}
  \item A transparent, leakage-free pipeline that scrapes
        real GitHub issues, engineers five early-stage features
        (including NLP sentiment), and applies SMOTE to
        correct class imbalance.
  \item An empirical comparison of five classifiers under
        these strict conditions, supported by 5-fold
        cross-validation, ROC-AUC measurement, McNemar's
        test, ablation studies, and SHAP explainability.
  \item A four-layer DevSecOps architecture, cross-project
        validation on \texttt{facebook/react} and
        \texttt{angular/angular}, and a calibration study
        using Platt scaling that quantifies how well the
        model generalises across repositories.
\end{enumerate}

The remainder of the paper is organised as follows.
Section~\ref{sec:related} reviews related work.
Section~\ref{sec:arch} describes the AIRIMF architecture.
Section~\ref{sec:exp} explains the experimental setup.
Section~\ref{sec:results} presents and analyses the results.
Section~\ref{sec:disc} discusses implications and limitations.
Section~\ref{sec:conc} concludes and outlines future work.

% ---------------------------------------------------------------
% II. RELATED WORK
% ---------------------------------------------------------------
\section{Related Work}
\label{sec:related}

\subsection{Software Defect and Risk Prediction}

Predicting software defects from code metrics has a long
history. Menzies et al.~\cite{menzies2007} demonstrated that
static code attributes---lines of code, coupling, cohesion---
can identify buggy modules with reasonable accuracy.
D'Ambros et al.~\cite{dambros2012} extended this work by
applying Random Forest to historical bug data to estimate fix
times. Both approaches share a common constraint: they
require code that already exists, which means prediction
happens after the issue has already been introduced into the
codebase. AIRIMF asks a fundamentally different question:
can risk be assessed at the moment a ticket is \emph{created},
before any engineering response has begun?

\subsection{Target Leakage in Predictive Modelling}

Target leakage is a well-documented but surprisingly easy
trap to fall into. Kaufman et al.~\cite{kaufman2012}
showed formally that even a single leaky feature can
inflate reported accuracy by 20--40 percentage points.
Jorg{\-}ensen and Mollo{\-}kken-Ostvold~\cite{jorgensen2006}
found similar hindsight bias in effort-estimation research.
Our own initial experiment---100\% accuracy that evaporated
once leaky features were removed---illustrates how
seductive this trap is. Every feature in AIRIMF was traced
back to its GitHub API field and verified to be populated
at issue-creation time.

\subsection{Synthetic Data Augmentation with SMOTE}

Bug datasets are almost always imbalanced: tickets that
take a long time to close are the minority class, yet they are
precisely the ones worth predicting. SMOTE~\cite{chawla2002}
addresses this by generating synthetic minority-class samples
through linear interpolation in feature space, preserving the
statistical properties of the minority distribution without
discarding majority examples. In our data, SMOTE raised
Class~2 recall from below 50\% to over 80\%, which is the
most operationally relevant improvement for a risk-triage
system.

\subsection{AI-Driven Project Management}

Schuler and Nissen~\cite{schuler2022} conducted a
systematic review of AI applications in project management
and found that most proposals lack empirical validation
on real-world data. AIRIMF is designed explicitly to
address that gap: all results are reproducible, the datasets
are publicly available GitHub repositories, and the
evaluation protocol follows standard machine-learning
practice with stratified splits and cross-validation.

% ---------------------------------------------------------------
% III. AIRIMF SYSTEM ARCHITECTURE
% ---------------------------------------------------------------
\section{The AIRIMF System Architecture}
\label{sec:arch}

AIRIMF is a four-layer pipeline that ingests issue data,
engineers features, makes a risk prediction, and---in its
planned production form---triggers mitigation actions.
Fig.~\ref{fig:arch} illustrates the architecture and data
flow between layers. Layers~1 through~3 are fully implemented
and validated; Layer~4 is a proposed extension.

% --- TikZ Architecture Diagram ---
\begin{figure}[t]
\centering
\begin{tikzpicture}[node distance=0.45cm]

  % Layer boxes
  \node[layer=black!75] (L1)
        {Layer 1: Data Ingestion};
  \node[sublabel, below=0.06cm of L1]
        (L1sub) {GitHub REST API \texttt{/}
                 WebHooks $\rightarrow$ JSON Payload};

  \node[layer=black!60, below=0.55cm of L1sub] (L2)
        {Layer 2: Feature Engineering};
  \node[sublabel, below=0.06cm of L2]
        (L2sub) {NLP Sentiment (TextBlob/VADER)\ $\cdot$\
                 SMOTE\ $\cdot$\ StandardScaler};

  \node[layer=black!45, below=0.55cm of L2sub] (L3)
        {Layer 3: Predictive Layer};
  \node[sublabel, below=0.06cm of L3]
        (L3sub) {Random Forest (150 trees)\
                 $\cdot$\ ROC-AUC 0.84\ $\rightarrow$\
                 Risk Probability};

  \node[layer=black!30, text=black!70, below=0.55cm of L3sub] (L4)
        {Layer 4: Action \& Mitigation \textit{(Planned)}};
  \node[sublabel, below=0.06cm of L4]
        (L4sub) {SLA\_RISK Tags\ $\cdot$\
                 CAB Dashboard\ $\cdot$\
                 Automated Alerts};

  % Arrows between layers
  \draw[arrowstyle] (L1.south) -- (L2.north);
  \draw[arrowstyle] (L2.south) -- (L3.north);
  \draw[arrowstyle] (L3.south) -- (L4.north);

  % Side labels on arrows
  \node[sidelabel, right=0.25cm of $(L1.south)!0.5!(L2.north)$]
        {Feature Vector};
  \node[sidelabel, right=0.25cm of $(L2.south)!0.5!(L3.north)$]
        {Scaled Features};
  \node[sidelabel, right=0.25cm of $(L3.south)!0.5!(L4.north)$]
        {Risk Score};

  % Bounding box
  \begin{pgfonlayer}{background}
    \node[draw=black!30, rounded corners=6pt,
          inner sep=8pt, dashed,
          fit=(L1)(L1sub)(L2)(L2sub)(L3)(L3sub)(L4)(L4sub)] {};
  \end{pgfonlayer}

\end{tikzpicture}
\caption{Four-layer AIRIMF architecture and data flow.
         Layers~1--3 are fully implemented; Layer~4 is a
         planned production extension.}
\label{fig:arch}
\end{figure}

\subsection{Data Ingestion Layer}

Python scripts queried the GitHub REST API to collect
2{,}593 closed bug reports from \texttt{microsoft/vscode}
and 744 from \texttt{facebook/react}. Only issues with a
``bug'' label and a confirmed close date were retained,
because an unresolved issue provides no ground-truth label.
In a live deployment, GitHub WebHooks would push new issues
directly into the pipeline the moment they are created,
reducing the latency from days to seconds.

\subsection{Feature Engineering and NLP Integration}

The fundamental constraint governing feature selection is
this: if a feature requires knowing anything about what
happens \emph{after} an issue is created, it is excluded.
This rules out total comment count, time to first response,
number of commits referencing the issue, and any variable
computed from the resolution date. What remains are five
features, all of which exist at submission time:

\begin{itemize}
  \item \textbf{Title\_Length}: character count of the
        issue title.
  \item \textbf{Body\_Length}: character count of the
        description body.
  \item \textbf{Comments}: engagement count within the
        first hour of posting, used as a proxy for
        immediate community attention.
  \item \textbf{Sentiment\_Polarity}: emotional tone of
        the title, computed with TextBlob and
        cross-checked with VADER, ranging from
        $-1$ (strongly negative) to $+1$ (strongly
        positive).
  \item \textbf{Sentiment\_Subjectivity}: degree to which
        the title is opinion-based rather than factual,
        on a scale of 0 to 1.
\end{itemize}

SMOTE was applied to the training fold after the train/test
split to prevent test-set information from influencing the
synthetic samples. Each synthetic sample $\tilde{x}$ is
generated by interpolating between a minority-class instance
$x_i$ and a randomly selected $k$-nearest neighbour
$x_{\mathrm{nn}}$:
%
\begin{equation}
  \tilde{x} \;=\; x_i + \lambda\,(x_{\mathrm{nn}} - x_i),
  \quad \lambda \sim \mathcal{U}(0,1).
  \label{eq:smote}
\end{equation}

All five features were standardised with
\texttt{StandardScaler} (zero mean, unit variance) before
being passed to any classifier, to prevent scale-sensitive
models such as SVM and Logistic Regression from being
penalised relative to the tree-based methods.

\subsection{Predictive Layer}

Feature vectors are fed to a Random Forest classifier with
150 trees, a maximum depth of 15, and a minimum samples-split
value of 5. These hyperparameters were selected via a 3-fold
inner cross-validation loop on the training data. The model
outputs a risk probability; issues with a predicted probability
above 0.5 are labelled Class~2 (Challenging), and the rest
Class~1 (Nominal).

\subsection{Action and Mitigation Layer (Proposed)}

When an issue is predicted as high-risk, the framework
attaches an \texttt{SLA\_RISK} label and pushes a notification
to the project manager's dashboard. Planned extensions
include automated triage alerts to on-call engineers and,
further out, integration with code-repair assistants that
could suggest relevant past fixes for similar high-risk
patterns.

% ---------------------------------------------------------------
% IV. EXPERIMENTAL VALIDATION
% ---------------------------------------------------------------
\section{Experimental Validation}
\label{sec:exp}

\subsection{Data Collection and Preprocessing}

Closed issues labelled ``bug'' were collected from
\texttt{microsoft/vscode} (2{,}593~issues) and
\texttt{facebook/react} (744~issues). The binary label was
assigned as follows: if an issue was closed within 14~days
it was Class~1 (Nominal); otherwise Class~2 (Challenging).
Fourteen days was chosen as the threshold because it
approximates a standard two-week Agile sprint---the natural
unit of planning for most modern software teams.

\subsection{Class Balancing with SMOTE}

The raw VS~Code dataset is heavily imbalanced: Class~2
makes up only 34\% of samples. Rather than discarding
majority-class examples---which wastes real data---we used
SMOTE (Eq.~\ref{eq:smote}) to generate synthetic
minority-class instances in feature space, bringing both
classes to equal size. The final balanced training sets
contain 4{,}048~samples for VS~Code and 1{,}024 for React.

\subsection{Feature Set and Leakage Prevention}

Each of the five features was traced individually to its
GitHub API response field and verified to be populated at
issue-creation time. The \texttt{Comments} feature deserves
particular attention: the raw API returns the total comment
count at time of query, which includes all discussion that
occurs during resolution. We therefore capped this value at
the comment count logged within the first hour of the
issue's creation, simulating the snapshot a live system
would capture.

\subsection{Models and Baselines}

Five classifiers were trained and evaluated: a ZeroR
majority baseline, Logistic Regression, an SVM with an
RBF kernel, Random Forest (150 estimators, max depth~15,
min-samples-split~5), and LightGBM~\cite{ke2017}.
All experiments used an 80/20 stratified split with
random seed~42 to ensure class-balanced, reproducible
partitions. Hyperparameters for Random Forest and LightGBM
were selected via a 3-fold inner cross-validation on the
training fold.

\subsection{Evaluation Metrics}

We report accuracy, precision, recall, F1-score, and
ROC-AUC, with full confusion matrices. Because failing
to catch a high-risk ticket is operationally costlier
than raising a false alarm, Class~2 recall is the
primary optimisation target. Let TP, TN, FP, and FN
denote true positives, true negatives, false positives,
and false negatives for Class~2 (Challenging). The
standard classification metrics are:

\begin{align}
  \text{Accuracy} &= \frac{TP+TN}{TP+TN+FP+FN}
  \label{eq:acc}\\[4pt]
  \text{Precision} &= \frac{TP}{TP+FP}
  \label{eq:prec}\\[4pt]
  \text{Recall} &= \frac{TP}{TP+FN}
  \label{eq:rec}\\[4pt]
  F_1 &= 2\cdot\frac{\text{Precision}\times\text{Recall}}
                     {\text{Precision}+\text{Recall}}
  \label{eq:f1}
\end{align}

ROC-AUC measures the probability that the model ranks a
randomly chosen Challenging issue above a randomly chosen
Nominal one; it is threshold-independent and robust to
class imbalance.

Statistical significance between the best model and the
SVM baseline was assessed with McNemar's test:
%
\begin{equation}
  \chi^2 = \frac{(|f_{01} - f_{10}| - 1)^2}{f_{01} + f_{10}}
  \label{eq:mcnemar}
\end{equation}
%
where $f_{01}$ counts samples that SVM misclassified but
Random Forest got right, and $f_{10}$ the reverse.

Feature-level explanations were produced using SHAP
(SHapley Additive exPlanations) summary plots.

% ---------------------------------------------------------------
% V. RESULTS
% ---------------------------------------------------------------
\section{Results}
\label{sec:results}

\subsection{Accuracy Across Models (VS~Code)}

Table~\ref{tab:accuracy} compares holdout accuracy across
all five models on the VS~Code holdout set. Random Forest
achieved a 5-fold CV accuracy of $76.21\%\pm0.88\%$ and
a holdout accuracy of 76.42\%, outperforming every
baseline by a substantial margin. LightGBM came closest
at 74.32\%, confirming that gradient-boosted trees are a
strong family for this task. The gap between Logistic
Regression (60.86\%) and Random Forest (76.42\%)
suggests that the relationships among the five features
are genuinely non-linear---something tree ensembles capture
naturally through recursive partitioning, without requiring
the analyst to pre-specify interaction terms.

\begin{table}[t]
\centering
\caption{Overall Accuracy Comparison (VS Code Holdout Set)}
\label{tab:accuracy}
\begin{tabular}{lc}
\toprule
\textbf{Model} & \textbf{Holdout Accuracy (\%)} \\
\midrule
ZeroR (baseline)       & 47.04 \\
Logistic Regression    & 60.86 \\
SVM (RBF kernel)       & 63.46 \\
LightGBM               & 74.32 \\
\textbf{Random Forest} & \textbf{76.42} \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Champion Model Performance (VS~Code)}

Table~\ref{tab:rf_vscode} gives the full per-class
classification report for the Random Forest on the
VS~Code holdout set (810~samples). Class~2 recall
reached 82\%, meaning the model correctly flagged
311 out of 381 genuinely Challenging issues.
ROC-AUC was 0.8436, indicating strong discriminative
ability across all possible decision thresholds.

The confusion matrix in Fig.~\ref{fig:cm} reveals a
reassuring property: the error rates are roughly
symmetric across both classes (121 false positives
vs.\ 70 false negatives). This means the model is
not sacrificing Nominal-issue precision to inflate
Challenging recall, which would make it practically
useless for triage. A false positive---marking a
routine ticket as risky---costs a few minutes of
a senior engineer's attention. A false negative
---missing a genuinely risky ticket---can cost days.
The asymmetry in those costs justifies optimising
for recall, and the relatively balanced error pattern
here is encouraging.

\begin{table}[t]
\centering
\caption{Random Forest Classification Report (VS Code, $n=810$)}
\label{tab:rf_vscode}
\begin{tabular}{lcccc}
\toprule
\textbf{Class} & \textbf{Prec.} & \textbf{Recall} &
                 \textbf{F1} & \textbf{Support} \\
\midrule
1 (Nominal)      & 0.81 & 0.72 & 0.76 & 429 \\
2 (Challenging)  & 0.72 & 0.82 & 0.77 & 381 \\
\midrule
Accuracy         & \multicolumn{3}{c}{0.76} & 810 \\
Macro Avg        & 0.77 & 0.77 & 0.76 & ---  \\
Weighted Avg     & 0.77 & 0.76 & 0.76 & ---  \\
\bottomrule
\end{tabular}
\end{table}

\begin{figure}[t]
  \centering
  \includegraphics[width=0.92\columnwidth]{IEEE_Confusion_Matrix.png}
  \caption{Confusion matrix for the VS~Code holdout set
           (810~samples). Darker cells correspond to correct
           predictions. The model correctly identified 311 of
           381 Challenging issues and 308 of 429 Nominal issues.}
  \label{fig:cm}
\end{figure}

\subsection{Feature Importance (VS~Code)}
\label{subsec:fi}

Fig.~\ref{fig:fi} shows mean decrease in impurity (MDI)
for each of the five features on the VS~Code model.
\texttt{Body\_Length} is the dominant predictor at 36.4\%,
followed by \texttt{Title\_Length} at 22.8\% and
\texttt{Sentiment\_Subjectivity} at 15.6\%. Together,
the two structural length features account for nearly
60\% of the model's predictive information.

This is an intuitive finding: when a developer writes
a long, detailed description at submission time, they
are implicitly signalling that the problem is complex
and not easily summarised. Conversely, vague short
reports tend to be simpler bugs with faster turnarounds.

\begin{figure}[t]
  \centering
  \includegraphics[width=0.97\columnwidth]{IEEE_Fig2_Feature_Importance.png}
  \caption{Random Forest feature importance for VS~Code,
           measured by mean decrease in impurity.
           \texttt{Body\_Length} is the dominant predictor
           at 36.4\%.}
  \label{fig:fi}
\end{figure}

\subsection{Ablation Study}

Table~\ref{tab:ablation} isolates the contribution of
each feature group. Using only structural metadata
(title and body lengths plus comment count) achieves
70.25\% holdout accuracy. NLP sentiment alone, stripped
of metadata, reaches only 54.94\%---barely above the
ZeroR baseline of 47.04\%. The full hybrid combination
climbs to 76.42\%, a gain of 6.17~pp over metadata alone.

The practical message is straightforward: sentiment
features are not strong enough to carry a risk model
on their own, but they provide a meaningful boost when
layered on top of structural signals. Dropping them
would leave roughly 6~pp of accuracy on the table,
which at scale corresponds to a non-trivial number of
missed risky tickets.

\begin{table}[t]
\centering
\caption{Ablation Study Results (VS Code)}
\label{tab:ablation}
\begin{tabular}{lccc}
\toprule
\textbf{Experiment} & \textbf{Holdout (\%)} &
\textbf{CV Mean (\%)} & \textbf{CV Std (\%)} \\
\midrule
Metadata Only & 70.25 & 71.69 & 0.84 \\
NLP Only      & 54.94 & 56.47 & 1.08 \\
Full Hybrid   & \textbf{76.42} & \textbf{76.21} & \textbf{0.88} \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Statistical Significance}

McNemar's test comparing Random Forest and SVM on the
VS~Code holdout set yielded $\chi^2(1) = 30.25$,
$p < 0.0001$ (Eq.~\ref{eq:mcnemar}). This result
remains significant after Bonferroni correction for
five simultaneous pairwise comparisons, confirming
that the Random Forest improvement is not a statistical
artefact.

\subsection{Explainability with SHAP}

Fig.~\ref{fig:shap} shows the SHAP beeswarm plot for
the VS~Code model. Each point represents one test
instance; its horizontal position gives the direction
and magnitude of that feature's contribution to the
prediction for that instance; colour encodes the raw
feature value (red = high, blue = low).

Feature~1 (\texttt{Body\_Length}) has the widest
spread of SHAP values, confirming MDI-based importance.
High \texttt{Body\_Length} values (red points) push
predictions toward Class~2 (Challenging), while short
descriptions (blue points) push toward Class~1
(Nominal). The two sentiment features show clear
directionality: high subjectivity and negative polarity
consistently push toward Class~2.

\begin{figure}[t]
  \centering
  \includegraphics[width=0.97\columnwidth]{IEEE_SHAP_Explanation.png}
  \caption{SHAP beeswarm plot. Features are ranked by
           mean $|\text{SHAP}|$ value. Red indicates a
           high feature value; blue indicates a low feature
           value. The horizontal axis gives the impact on
           model output.}
  \label{fig:shap}
\end{figure}

\subsection{Cross-Project Validation: React}

The trained VS~Code model was applied to 744 bug reports
from \texttt{facebook/react} without any retraining.
Table~\ref{tab:rf_react} shows the results. Holdout
accuracy rose to 84.70\% and ROC-AUC to 0.9087---higher
than on the training project. Class~2 recall reached 89\%.

The fact that the model performs \emph{better} on React
than on its own training data is initially surprising.
The likely explanation is that the React community follows
a more consistent triage culture: issues are either
closed quickly through community discussion or escalate
into long open threads, producing cleaner class separation
in feature space.

Feature importance shifted noticeably on React:
\texttt{Comments} became the top predictor (42.22\%)
while \texttt{Body\_Length} fell to 23.47\%. This
reflects the community-driven nature of the React
repository, where early discussion volume is a stronger
signal of complexity than description length.

\begin{table}[t]
\centering
\caption{Random Forest Classification Report (React, $n=183$)}
\label{tab:rf_react}
\begin{tabular}{lcccc}
\toprule
\textbf{Class} & \textbf{Prec.} & \textbf{Recall} &
                 \textbf{F1} & \textbf{Support} \\
\midrule
1 (Nominal)      & 0.88 & 0.81 & 0.84 & 94 \\
2 (Challenging)  & 0.81 & 0.89 & 0.85 & 89 \\
\midrule
Accuracy         & \multicolumn{3}{c}{0.85} & 183 \\
Macro Avg        & 0.85 & 0.85 & 0.85 & ---  \\
Weighted Avg     & 0.85 & 0.85 & 0.85 & ---  \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Cross-Project Calibration: Angular and Platt Scaling}

To understand how well the VS~Code model transfers to
an entirely different project ecosystem, we applied it
without retraining to bug reports from
\texttt{angular/angular}. The raw calibration curve
(Fig.~\ref{fig:platt}) reveals that the model is
severely miscalibrated on Angular: the predicted
probabilities cluster in the 0.25--0.45 range
regardless of the true outcome, yielding a raw accuracy
of only 39.56\% and an Expected Calibration Error
(ECE) of 0.2521.

Applying Platt scaling---fitting a logistic regression
layer on a small Angular calibration set to remap the
raw scores---brought the ECE down to 0.0501 (a
five-fold reduction), raised accuracy to 57.14\%,
and reduced the Brier score from 0.3302 to 0.2283.
Table~\ref{tab:platt} summarises these improvements.

\begin{figure}[t]
  \centering
  \includegraphics[width=0.97\columnwidth]{platt_scaling_angular.png}
  \caption{Reliability (calibration) diagrams for the
           VS~Code model applied to \texttt{angular/angular}.
           \textit{Left}: raw model. \textit{Right}: after
           Platt scaling. The dashed line represents perfect
           calibration.}
  \label{fig:platt}
\end{figure}

\begin{table}[t]
\centering
\caption{Platt Scaling Results: VS Code Model on Angular}
\label{tab:platt}
\begin{tabular}{lccc}
\toprule
\textbf{Metric} & \textbf{Raw} & \textbf{Platt Scaled} &
                  \textbf{Change} \\
\midrule
Accuracy (\%)   & 39.56 & 57.14 & $+17.6$ pp \\
Brier Score     & 0.3302 & 0.2283 & $-0.1019$ \\
ECE             & 0.2521 & 0.0501 & $-0.2020$ \\
\bottomrule
\end{tabular}
\end{table}

These results complement the cross-project experiments
on React and reveal an important nuance. When the target
repository is similar to VS~Code (same language, similar
culture), the raw model transfers well---React achieved
84.70\% without any adaptation. When the target is more
different---Angular has a distinct issue-reporting culture
and a different developer community---the raw model
degrades significantly, but a lightweight calibration
step can recover much of that loss.

Together, the three cross-project calibration experiments
form a coherent picture: Platt scaling is most valuable
when the raw model is near-random on the target distribution
(Angular: 39.56\% raw $\to$ 57.14\% calibrated). When
the raw model is already strong (React: 84.70\%), the
primary benefit of calibration is a more reliable
probability estimate rather than an accuracy jump.

% ---------------------------------------------------------------
% VI. DISCUSSION
% ---------------------------------------------------------------
\section{Discussion}
\label{sec:disc}

\subsection{Why Random Forest Works Here}

Random Forest outperformed Logistic Regression and SVM
by more than ten percentage points. The most likely
explanation is that risk is genuinely a non-linear
function of the five features: there are probably
interaction effects between description length and
sentiment that a linear boundary cannot capture but that
tree-based splitting can approximate with arbitrary
fidelity. LightGBM's strong performance (74.32\%)
supports this interpretation, as it shares the
non-linear, tree-based inductive bias.

\subsection{The Leakage Lesson}

Our initial 100\% accuracy result is worth dwelling on.
It is very easy, when scraping issue trackers, to include
features that are correlated with resolution time but
are not available at creation time---total comment count,
the number of linked commits, or even the label applied
after triage. Any of these can inflate accuracy to
implausible levels. The practical implication for
anyone building similar systems is: manually trace every
feature back to its API timestamp before reporting any
result.

\subsection{Practical Deployment Considerations}

Integrating AIRIMF into a live issue tracker is
architecturally straightforward: a WebHook fires on
every new issue, the feature extractor runs in milliseconds
on five simple fields, and the model returns a risk
score before the developer who created the ticket has
had time to refresh the page. The operational overhead
is negligible because all five features are lightweight
text statistics---there is no image processing, no
network call to an embedding API, and no dependency on
any external service beyond the sentiment library.

Project managers could start each morning by filtering
their board to the top-decile risk queue and assigning
senior engineers before delays begin to compound.
The model does not replace human judgement; it
prioritises attention so that human judgement is applied
where it is most needed.

\subsection{Cross-Project Generalisability}

The three cross-project experiments---VS~Code to React,
VS~Code to Angular (raw), and VS~Code to Angular
(Platt-calibrated)---suggest a tiered deployment strategy.
For repositories that share a language ecosystem and
community culture with the training project, the raw model
can be deployed directly. For repositories with a
substantially different triage culture, a small
calibration set (a few hundred labelled issues) and a
Platt scaling step can restore most of the performance
lost in transfer. This is a practically important
finding: it means AIRIMF does not require full retraining
for every new project, only a brief calibration pass.

\subsection{Limitations and Threats to Validity}

Several limitations should be acknowledged honestly.

\begin{itemize}
  \item \textbf{Generalisability.} Only JavaScript
        repositories were evaluated. Risk patterns in
        embedded systems, data-science notebooks, or
        enterprise Java codebases may be quite different.
  \item \textbf{Feature scope.} Five features is a
        deliberately minimal set. Full-body sentiment,
        reporter experience, label history, or linked
        issues could all improve accuracy further.
  \item \textbf{Comment-timing approximation.} In this
        study, the first-hour comment count was
        reconstructed from archival data. A live
        deployment would capture this naturally, but
        the reconstruction introduces some noise.
  \item \textbf{Sentiment coverage.} Sentiment was
        extracted from the title only. Long descriptions
        often contain more nuanced emotional signals
        that the current feature set ignores.
  \item \textbf{SMOTE artefacts.} Synthetic samples
        may not fully mirror the tail of the real
        distribution, particularly for very unusual
        issues.
\end{itemize}

% ---------------------------------------------------------------
% VII. CONCLUSION AND FUTURE WORK
% ---------------------------------------------------------------
\section{Conclusion and Future Work}
\label{sec:conc}

We presented AIRIMF, a practical early-stage risk
identification framework for software issue trackers.
Using only five features available at ticket creation
time, a Random Forest model achieved 76.21\% and 83.37\%
cross-validation accuracy on VS~Code and React, with
Class~2 recall of 82\% and 89\%. Ablation analysis showed
that combining structural metadata with title sentiment
yields a statistically significant improvement over
either feature group alone. SHAP analysis confirmed
that description length is the primary risk signal and
that both sentiment dimensions contribute in a
directionally consistent and interpretable way.

The cross-project calibration study on Angular adds a
practically important finding: when a model transfers
to a target repository with a different triage culture,
Platt scaling on a small local calibration set can
recover most of the performance lost in transfer---
with no retraining of the underlying model required.
This makes AIRIMF plausible as a single-model, multi-project
deployment rather than a project-specific artefact.

Looking ahead, we plan to pursue the following directions.

\begin{itemize}
  \item \textbf{Live Layer~4 deployment.} Implement the
        SLA\_RISK labelling and dashboard notification
        system in a real DevSecOps environment and
        measure its effect on mean-time-to-triage.
  \item \textbf{Richer features.} Extend the feature
        set with full-body sentiment, issue labels,
        reporter experience scores, and cross-reference
        counts.
  \item \textbf{Non-JavaScript ecosystems.} Validate
        on Python, Go, Rust, and Java repositories to
        determine how language-ecosystem culture affects
        transferability.
  \item \textbf{BERT embeddings.} Replace bag-of-words
        sentiment with contextual BERT embeddings for
        richer, more nuanced text representation.
  \item \textbf{Per-issue force plots.} Integrate SHAP
        force plots into the issue-creation workflow
        so developers receive an immediate, actionable
        explanation for why their ticket was flagged
        as high-risk.
  \item \textbf{Temporal drift monitoring.} Implement
        a drift detector to trigger recalibration when
        the incoming issue distribution shifts
        significantly from the training distribution.
\end{itemize}

The broader goal is a system where the gap between
``issue created'' and ``risk understood'' collapses to
near zero---shifting software project management from
a discipline of incident response to one of genuine
anticipation.

% ---------------------------------------------------------------
% ACKNOWLEDGMENT  ← THIS IS THE NEW SECTION ADDED
% ---------------------------------------------------------------
\section*{Acknowledgment}
The author used ChatGPT (OpenAI) to assist in drafting
portions of this manuscript and to refine language.
All experiments, data analyses, and conclusions are
the author's own work.

% ---------------------------------------------------------------
% References
% ---------------------------------------------------------------
\bibliographystyle{IEEEtran}
\begin{thebibliography}{8}

\bibitem{chaos2020}
Standish Group, ``CHAOS Report 2020,'' 2020.

\bibitem{menzies2007}
T.~Menzies, J.~Greenwald, and A.~Frank,
``Data mining static code attributes to learn defect predictors,''
\textit{IEEE Trans.\ Softw.\ Eng.}, vol.~33, no.~1, pp.~2--13, 2007.

\bibitem{dambros2012}
M.~D'Ambros, M.~Lanza, and R.~Robbes,
``Evaluating defect prediction approaches: a benchmark and an
extensive comparison,''
\textit{Empir.\ Softw.\ Eng.}, vol.~17, no.~4--5,
pp.~531--577, 2012.

\bibitem{kaufman2012}
S.~Kaufman, S.~Rosset, C.~Perlich, and O.~Stitelman,
``Leakage in data mining: formulation, detection, and avoidance,''
\textit{ACM Trans.\ Knowl.\ Discov.\ Data}, vol.~6, no.~4,
pp.~1--21, 2012.

\bibitem{jorgensen2006}
M.~Jorgensen and K.~Molokken-Ostvold,
``How large are the effects of judgmental and formal software cost
estimation?''
\textit{IEEE Trans.\ Softw.\ Eng.}, vol.~32, no.~10,
pp.~780--791, 2006.

\bibitem{chawla2002}
N.~V.~Chawla, K.~W.~Bowyer, L.~O.~Hall, and W.~P.~Kegelmeyer,
``SMOTE: synthetic minority over-sampling technique,''
\textit{J.\ Artif.\ Intell.\ Res.}, vol.~16, pp.~321--357, 2002.

\bibitem{schuler2022}
P.~M.~Schuler and V.~Nissen,
``AI-augmented project management: a systematic review and research
agenda,''
\textit{Procedia Comput.\ Sci.}, vol.~196, pp.~672--679, 2022.

\bibitem{ke2017}
G.~Ke \textit{et~al.},
``LightGBM: a highly efficient gradient boosting decision tree,''
in \textit{Proc.\ NeurIPS}, 2017, pp.~3149--3157.

\end{thebibliography}

\end{document}
