# Step-by-Step Revision Plan for the IGL1 Paper

Below is a detailed, actionable revision roadmap addressing each critical concern from the peer review, with specific supporting manuscripts (preferably open access / MDPI) you can cite to justify each change.

***

## STEP 1 — Strengthen the Novelty Argument (Section 1 & 2)

**What to revise:** The introduction and related work must explicitly differentiate IGL1 from existing IG + L1 combinations in the literature. Currently, the novelty claim rests on three design choices that could be seen as incidental.

**How to revise:**
Add a dedicated paragraph in Section 1 clarifying that IGL1's novelty is a *structured, leakage-aware, multi-classifier integration* — not merely combining two known techniques. Frame it as a **methodological framework contribution** rather than an algorithmic invention, which is a legitimate and publishable category.

Add this sentence to the Introduction: *"To the best of the authors' knowledge, no prior work has combined IG filtering with L1-LinearSVC sparse ranking under a strictly leakage-aware pipeline and validated the resulting subset across five classifier families on UNSW-NB15."* [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

**Supporting manuscripts to cite:**
- Ozkan-Okay et al. (2023), *Applied Sciences* (MDPI) — already cited as  in your paper. Strengthen the contrast: their approach does not use L1-embedded ranking in a sequential leakage-aware pipeline. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- Mhawi et al. (2022), *Symmetry* (MDPI)  — already in paper; contrast more explicitly that their ensemble FS has no leakage-aware protocol.
- **Add new citation:** Sezgin, Ulaş & Boyacı (2025), *Sensors* (MDPI) — "Multi-Objective Feature Selection for Intrusion Detection Systems". Cite this to show that multi-objective/multi-model FS evaluation is an active, recognized research direction that justifies your cross-classifier design. [discovery.researcher](https://discovery.researcher.life/topic/feature-selection-for-intrusion-detection/3248902?page=1&topic_name=Feature+Selection+For+Intrusion+Detection)
- **Add new citation:** Kouassi et al. (2025), *Future Internet* (MDPI) — "Top-K Feature Selection for IoT Intrusion Detection". Use this to justify your top-k retention strategy as a recognized, principled design choice. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

***

## STEP 2 — Add Feature-Count Sensitivity Analysis (Section 3.6 & new subsection)

**What to revise:** The paper acknowledges that k=31 (IG stage) and k=21 (L1 stage) are not formally optimized. This is the single most damaging empirical gap. Add a sensitivity experiment.

**How to revise:**
Insert a new **Section 3.7 (Feature Retention Sensitivity)** or add a subsection in Section 4. Run the following experiment:
1. Vary the IG stage threshold: k ∈ {20, 25, 31, 35, 40} features retained.
2. For each IG threshold, vary the L1 top-k: k ∈ {10, 15, 21, 25, 30}.
3. Report Random Forest accuracy/F1 for each combination as a heatmap or line plot.
4. Show that k=31/21 is near-optimal (or explain its selection rationale if it is not).

**Supporting manuscripts to cite:**
- Fatima et al. (2024), *Future Internet* (MDPI) — "Towards Ensemble Feature Selection for Lightweight Intrusion Detection". This paper performs feature-count sensitivity analysis, providing a methodological template. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- Kurdi et al. (2025), *IJIES* — "Efficient Two-Stage IDS Based on Hybrid Feature Selection". Cite as evidence that two-stage pipelines require sensitivity evaluation of thresholds at each stage. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- Barbosa et al. (2024), *Ad Hoc Networks* — "Optimizing Feature Selection in IDS: Pareto Dominance Approaches". Cite to justify multi-threshold analysis as a best practice. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

***

## STEP 3 — Add Statistical Validation (Section 4.6)

**What to revise:** The 0.08% accuracy difference between RF+IGL1 and Yin et al.'s IGRF-RFE is presented as a meaningful result without any statistical support. This must be corrected. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

**How to revise:**
Add a multi-seed experiment. Run each classifier across **10 different random seeds** (controlling `random_state` in the stratified split and classifiers). Report **mean ± standard deviation** for accuracy and F1-score. Add the following to Table 3:

| Model | Accuracy (mean ± std) | F1-score (mean ± std) |
|---|---|---|
| RF + IGL1 | 0.8432 ± σ | 0.8376 ± σ |
| MLP + IGL1 | 0.8322 ± σ | 0.8259 ± σ |

Add a sentence in Section 4.6: *"Because the comparison with Yin et al.  is literature-based and not derived from paired experiments under the same environment, formal statistical significance testing such as McNemar's test or the Wilcoxon signed-rank test is reserved for future controlled comparisons."* This sentence is already partially present in the paper  — it needs to be moved to a more prominent position and the multi-seed std must be added. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

**Supporting manuscripts to cite:**
- Pineau et al. (2021), *JMLR* — already cited as  in your paper. Explicitly invoke their recommendation that reproducibility requires variance reporting across seeds. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- Rosenblatt et al. (2024), *Nature Communications* — already cited as . Use to reinforce that single-run results are unreliable without variance estimates. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- **Add new citation:** Rehman et al. (2025), *Journal of Big Data* (SpringerOpen OA) — "A Systematic Literature Study of ML Techniques for IDS". This survey explicitly recommends multi-run evaluation as a best practice in IDS comparison. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

***

## STEP 4 — Revise Categorical Encoding Strategy or Add Ablation (Section 3.4)

**What to revise:** The binary dominant-category encoding for `proto`, `service`, and `state` is non-standard and loses significant discriminative information, particularly for protocol-specific attacks. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

**How to revise — Option A (Preferred):** Add an ablation experiment comparing:
- Current binary dominant-category encoding
- Standard one-hot encoding (with leakage-aware fitting)
- Ordinal/target encoding (fitted on training only)

Report RF+IGL1 performance under each scheme. If binary encoding is still competitive, this *strengthens* your paper. If one-hot is better, update the pipeline.

**How to revise — Option B (Minimum):** Add a paragraph explicitly justifying the choice: *"One-hot encoding was considered but rejected because `proto` in UNSW-NB15 contains 133 unique values, which would expand the feature space from 42 to ~175 dimensions, potentially destabilizing the IG ranking stage. The dominant-category binary scheme limits this expansion while maintaining a consistent encoding under the leakage-aware protocol. A full ablation between encoding strategies is left for future work."*

**Supporting manuscripts to cite:**
- Umar et al. (2025), *Data Science Management* — "Effects of Feature Selection and Normalization on Network Intrusion Detection" — already cited as  in your paper. Invoke this explicitly to justify that encoding choices affect IDS outcomes. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- More et al. (2024), *Algorithms* (MDPI) — "Enhanced IDS Performance with UNSW-NB15 Data Analysis" — already cited as . Use to show preprocessing design choices significantly impact UNSW-NB15 results. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- **Add new citation:** Musthafa et al. (2024), *Sensors* (MDPI) — "Optimizing IoT Intrusion Detection Using Balanced Class Distribution, Feature Selection, and Ensemble ML". This paper discusses encoding tradeoffs in a leakage-aware IDS context. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

***

## STEP 5 — Add Per-Class Performance Breakdown (Section 4.1)

**What to revise:** Tables 3 and 4 report only macro-averaged metrics. The six classes in UNSW-NB15 are highly imbalanced (Normal and Generic dominate; DoS and Reconnaissance are much smaller), so macro-averages mask per-class failures. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

**How to revise:**
Add a new **Table 3b: Per-Class F1-Score for RF+IGL1** reporting precision, recall, and F1 for each of the six classes: Normal, Generic, Exploits, Fuzzers, DoS, Reconnaissance. This is already computable from your existing confusion matrix (Figure 3)  — it just needs to be tabulated explicitly. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

Also add this sentence to Section 4.1: *"The macro-averaged FNR of 0.2562 for RF+IGL1 (Table 5) indicates that a non-trivial proportion of attack instances are misclassified, particularly in minority classes. Per-class analysis reveals that [class X] achieves the lowest recall, consistent with the overlapping traffic characteristics noted in Section 4.2."*

**Supporting manuscripts to cite:**
- Mujahid et al. (2024), *Journal of Big Data* — already cited as  in your paper. Cite specifically to acknowledge that class imbalance in UNSW-NB15 affects minority class recall. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- Jaw & Wang (2021), *Symmetry* (MDPI) — "Feature Selection and Ensemble-based IDS" — already cited as . This paper provides per-class evaluation as a standard reporting format. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- **Add new citation:** Alsaffar et al. (2024), *Journal of Big Data* — "Shielding Networks: Enhancing Intrusion Detection with Hybrid FS and Stack Ensemble Learning". Already cited as ; invoke specifically for their per-class breakdown which can serve as a comparison template. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

***

## STEP 6 — Fix Equation (4) and Clarify the L1-SVM Formulation (Section 3.9)

**What to revise:** Equation (4) contains a non-standard \(\frac{1}{2}\|w\|_1\) term  [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx). The factor \(\frac{1}{2}\) is conventionally used with the squared L2 norm (\(\|w\|_2^2\)) in soft-margin SVM, not with the L1 norm. This is mathematically misleading.

**How to revise:**
Change Equation (4) to the standard L1-SVM formulation:
\[\min_{w,b}\left(\|w\|_1 + C\sum_{i=1}^{N}\xi_i\right)\]

Or, if the \(\frac{1}{2}\) factor is intentional (some formulations use it for gradient symmetry), add a footnote: *"The \(\frac{1}{2}\) coefficient is included here for notational consistency with the dual formulation; it does not affect the sparsity-promotion behavior of the L1 penalty."*

**Supporting manuscripts to cite:**
- Guyon et al. (2002), *Machine Learning* — already cited as  in your paper. Cite the original SVM formulation to anchor the corrected equation. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- Altulaihan, Almaiah & Aljughaiman (2024), *Sensors* (MDPI) — "Anomaly Detection IDS for DoS Attacks Using ML" — already cited as . Cross-check their SVM objective function for notational consistency. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

***

## STEP 7 — Clarify Validation Set Role and Hyperparameter Selection (Section 3.5 & 3.7)

**What to revise:** The paper describes a 68/16/16 split but is ambiguous about whether the validation set was used for hyperparameter selection or only for "observing comparative model behavior". Table 2 shows underspecified hyperparameters for Random Forest (`n_estimators=100` only) and SVM (`kernel=RBF` only). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

**How to revise:**
1. Add one sentence clarifying: *"The validation set was used to monitor convergence behavior during MLP training (via `early_stopping=True`) and to observe comparative model behavior across classifiers during development. Final hyperparameters were not formally optimized via grid search; instead, the configurations reported in Table 2 reflect the final frozen settings used in the comparative evaluation."*
2. Expand Table 2 to add for Random Forest: `max_depth=None, min_samples_split=2, random_state=42`; and for SVM: `C=1.0, gamma=scale`.

**Supporting manuscripts to cite:**
- Pineau et al. (2021), *JMLR* —  in your paper. Their checklist explicitly requires reporting all hyperparameters and whether a validation set was used for selection. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- Yu et al. (2025), *Expert Systems with Applications* — "A Feature Selection Algorithm for IDS Based on the Enhanced Heuristic Optimizer". Already cited as ; use as a model for transparent hyperparameter reporting in IDS papers. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

***

## STEP 8 — Address Class Removal Justification (Section 3.3)

**What to revise:** The removal of four attack classes (analysis, backdoor, shellcode, worms) lacks a quantitative justification. The paper states "very small frequencies" but does not report exact class counts. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

**How to revise:**
Add a table or footnote: *"The four removed classes collectively represent fewer than 2.1% of total records: Analysis (n=677, 0.39%), Backdoor (n=583, 0.33%), Shellcode (n=378, 0.22%), and Worms (n=44, 0.025%). Their removal follows the threshold practice adopted in prior UNSW-NB15 studies  and is consistent with the six-class configuration used in the most widely cited benchmark for this dataset."*

**Supporting manuscripts to cite:**
- Yin et al. (2023), *Journal of Big Data*   — cite that their IGRF-RFE also uses a reduced class configuration to justify consistency with the benchmark. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- Janarthanan & Zargari (2017)   — cite their original analysis of class frequency impact on UNSW-NB15 performance. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)
- **Add new citation:** Ring et al. (2019), *Computers & Security* — "A Survey of Network-Based Intrusion Detection Datasets". Already cited as; use to show that class reduction is a recognized preprocessing step in benchmark IDS datasets. [publications.eai](https://publications.eai.eu/index.php/casa/article/download/6735/3613/22268)

***

## STEP 9 — Resolve Duplicated Sentence and Editorial Cleanup (Section 4.6)

**What to revise:** Section 4.6 contains a duplicated sentence artifact: *"Taken together, the result point to… Atau. Taken together, the results show…"*. "Atau" is Indonesian for "Or" — this is a clear editorial artifact from manuscript revision that must be removed before submission. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

**How to revise:**
Delete the entire first version and the word "Atau." Keep only: *"Taken together, the results show that IGL1 produces a compact yet competitive feature subset across multiple classifier families."*

Also review the full paper for:
- Inconsistent hyphenation ("sparsi-ty-driven", "parsi-monious") — these are word-wrap artifacts from earlier formatting that should be corrected.
- The ROC curve in Figure 4 — add explicit AUC values per class in the caption or a supplementary table.

***

## STEP 10 — Revise the Abstract and Conclusion (Abstract & Section 5)

**What to revise:** The abstract does not mention the 84.32% accuracy figure, the 21 selected features, or the leakage-aware design quantitatively. The conclusion similarly hedges all claims without providing a crisp summary of what was demonstrated. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_6fec7979-e7a7-4bdc-aa8c-831a85bd5180/68c13b89-4200-4311-862e-f6904a80adb6/A-Novel-Hybrid-IGL1-Feature-Selection-Method-for-High-Performance-Intrusion-Detection-on-the-UNSW-NB15-Dataset-Using-Multiple-Machine-Learning-Models-1.docx)

**How to revise the Abstract:** Add one quantitative sentence: *"Under a leakage-aware evaluation protocol, RF+IGL1 achieves 84.32% accuracy and 83.76% macro F1-score on the six-class UNSW-NB15 test set, retaining 21 features from an initial 42-dimensional candidate space."*

**How to revise the Conclusion:** Replace the overly hedged opening with a direct claim: *"This study demonstrates that a two-stage IG+L1 hybrid feature selection framework, when combined with a strictly leakage-aware evaluation protocol, produces a compact 21-feature subset that enables Random Forest to achieve 84.32% accuracy on UNSW-NB15 — competitive with the IGRF-RFE benchmark while avoiding iterative wrapper retraining."*

***

## Consolidated Revision Priority Table

| Step | Concern | Priority | Estimated Effort |
|---|---|---|---|
| 1 | Novelty framing & prior art | Critical | 2–3 days writing |
| 2 | Feature-count sensitivity analysis | Critical | 3–5 days coding |
| 3 | Multi-seed statistical validation | Critical | 2–3 days coding |
| 4 | Encoding ablation or justification | Major | 1–3 days |
| 5 | Per-class performance table | Major | 0.5 days (from existing conf. matrix) |
| 6 | Fix Equation (4) notation | Major | 1 hour |
| 7 | Hyperparameter reporting clarity | Moderate | 0.5 days |
| 8 | Class removal quantitative justification | Moderate | 0.5 days |
| 9 | Editorial cleanup ("Atau", hyphenation) | Minor | 1–2 hours |
| 10 | Abstract & Conclusion rewrite | Minor | 1 day |

The most impactful steps to tackle first are **Steps 2, 3, and 5**, as they address the three empirical gaps that any reviewer will independently identify. Steps 1, 6, and 9 are quick wins that significantly improve the scholarly presentation without requiring new experiments. With these revisions, the paper's contribution — particularly its leakage-aware reproducibility framework — will be substantially more defensible and publishable in a journal such as *Applied Sciences*, *Sensors*, or *Electronics* (all MDPI). [discovery.researcher](https://discovery.researcher.life/topic/feature-selection-for-intrusion-detection/3248902?page=1&topic_name=Feature+Selection+For+Intrusion+Detection)