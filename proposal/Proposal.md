**Tejal Jadhav**  
**CSCI 6970 – MS Course Project**  
**Term: Fall 2026**

# Evaluating Evidential Support of Citations in AI-Generated Content

## 1. Introduction

Large Language Models (LLMs) are increasingly used to generate information-rich content such as answers, summaries, reports, and research-oriented text. Many LLM-based systems can also provide citations to external sources, allowing users to trace generated claims back to supporting evidence. Although the presence of citations can make AI-generated content appear more trustworthy, a citation does not necessarily mean that the cited source actually supports the claim being made.

For example, an AI-generated statement may report that a study found a 30% improvement, while the cited source reports only a 10% improvement. In other cases, generated content may strengthen the meaning of the original evidence, such as describing an observed association as a causal relationship. A citation may also be relevant to the general topic of a claim while failing to provide sufficient evidence for the specific statement. These cases create an important distinction between simply providing a citation and providing evidence that adequately supports a generated claim.

Automatically assessing this relationship is an important Natural Language Processing (NLP) problem because it requires understanding both the generated claim and the cited evidence and determining the degree to which their meanings are consistent. Reliable citation-support evaluation could help identify cases in which AI-generated content overstates, misrepresents, or is insufficiently supported by its cited evidence.

This project will investigate the problem of evaluating evidential support for citations in AI-generated content. The project will examine existing approaches to citation and attribution evaluation, develop a clear operational definition of evidential support, and experimentally evaluate NLP-based methods for determining how well cited evidence supports AI-generated claims. The study will particularly examine challenging cases where evidence provides only partial or incomplete support, with the goal of understanding both the capabilities and limitations of current NLP approaches.


## 2. Literature Review

The increasing use of Large Language Models (LLMs) for information-seeking tasks has created growing interest in whether generated information can be reliably traced to supporting evidence. Early work by Rashkin et al. (2023) formalized this problem through the Attributable to Identified Sources (AIS) framework. AIS evaluates whether generated information can reasonably be attributed to a provided source using an "according to" test. Although the framework established an important foundation for evaluating source attribution across tasks such as question answering, summarization, and table-to-text generation, its primary formulation treats attribution as a binary decision: generated information is either fully attributable or not fully attributable. Rashkin et al. acknowledged that more fine-grained measures of attribution could be useful.

Subsequent research extended attribution evaluation specifically to LLM-generated content containing citations. Gao et al. (2023) introduced ALCE, a benchmark for evaluating LLM-generated answers with citations using measures such as citation recall and citation precision. ALCE uses Natural Language Inference (NLI) to determine whether cited evidence entails a generated statement. While human annotators could distinguish between citations that fully support, partially support, or do not support a statement, the automatic evaluation framework was less effective at recognizing partial support. This highlighted an important limitation of binary entailment-based approaches: a citation may contain relevant evidence without supporting every component of a generated statement.

Other work has attempted to provide more detailed attribution judgments. Yue et al. (2023) introduced AttrScore, which distinguishes among attributable, extrapolatory, and contradictory relationships between generated claims and reference evidence. However, their error analysis showed that attribution models remained sensitive to fine-grained differences involving numerical values, dates, contextual information, and logical relationships. Similarly, AttributionBench (Li et al., 2024) evaluated attribution models across seven datasets and found that fine-grained information insensitivity accounted for a substantial proportion of model errors. These findings suggest that determining broad semantic relevance or entailment is not sufficient when small differences between a claim and its evidence can change whether the claim is actually supported.

Recent research has therefore moved toward more fine-grained citation evaluation. Zhang et al. (2024) explicitly evaluated full, partial, and no support using 12,681 human-annotated statement-citation pairs. Their comparison of similarity-based, NLI-based, and LLM-based evaluation methods showed that existing approaches perform substantially better when distinguishing clearly supported from unsupported statements than when partial support is involved. The authors consequently identify improved fine-grained training resources and more interpretable evaluation methods as important directions for future research.

The practical importance of this problem has also been demonstrated in deployed and domain-specific settings. Liu et al. (2023) manually evaluated citations produced by several generative search engines and found that only 51.5% of generated sentences were fully supported by citations, despite the systems producing fluent and apparently useful responses. Their annotation framework distinguished full, partial, and no support and also identified sentence-level evaluation as a limitation because a single sentence may contain multiple independently verifiable claims. More recently, Wu et al. (2025) introduced SourceCheckup for evaluating citations in medical LLM responses. Their system decomposes responses into independently verifiable statements and automatically determines whether cited medical sources support those statements. Although the automated evaluator achieved 88.7% agreement with the consensus of medical experts, the final verification decision remained binary, and the authors observed that unsupported statements frequently deviated only partially from their cited sources.

Researchers have also begun questioning whether evidential support alone adequately represents citation quality. CiteEval (Xu et al., 2025) argues that citation evaluation should consider factors beyond entailment, including missing evidence, misleading or redundant citations, alternative available sources, and source quality. CiteEval consequently introduces a more comprehensive evaluation framework and a 1–5 citation-quality rating scheme. At the same time, work such as ALiiCE (Xu et al., 2025) addresses the granularity problem by decomposing sentences into atomic claims and evaluating citations at the claim level rather than treating an entire sentence as a single unit. ALiiCE demonstrates that atomic-level analysis can correct errors produced by sentence-level citation evaluation, while also observing that citation correctness and citation usefulness are not necessarily equivalent.

Taken together, the literature shows a clear progression from binary source attribution, to citation-level entailment evaluation, to fine-grained support and claim-level analysis. However, existing research also consistently demonstrates that automatic evaluators have difficulty with subtle evidential mismatches, particularly cases involving partial support, numerical or contextual discrepancies, missing evidence, and multi-component claims. At the same time, recent frameworks have substantially expanded what citation quality can mean, making it insufficient for new work simply to classify citations as supported or unsupported. These findings motivate further investigation into how evidential support can be measured at a sufficiently fine-grained and interpretable level to identify not only whether a citation fails to support a generated claim, but what specific information within the claim is and is not supported by the cited evidence.


## 3. Research Gap

Despite recent advances in citation and attribution evaluation, automatic methods continue to struggle with fine-grained evidential mismatches. These include differences involving numerical values, dates, entities, qualifiers, contextual information, logical relationships, and additional information introduced in a generated claim but not established by the cited evidence. Research on fine-grained citation evaluation reports substantially weaker performance when partial support is involved, while AttributionBench identifies sensitivity to fine-grained information as a major source of attribution errors. Furthermore, sentence-level evaluation can obscure these problems when a sentence contains multiple independently verifiable claims.

Although existing approaches can provide an overall support or citation-quality judgment, there remains room to investigate methods that provide a more directly interpretable explanation of which specific information within an AI-generated claim is supported, unsupported, or contradicted by the cited evidence. An overall label such as partially supported, for example, indicates that a problem exists but does not necessarily communicate which part of the claim caused that judgment.

Therefore, this project will investigate fine-grained and interpretable evidential support evaluation for AI-generated claims. Using Natural Language Processing (NLP) methods, the project will explore whether generated claims can be decomposed and aligned with cited evidence to produce both an overall support assessment and a localized characterization of the specific claim components that are supported, unsupported, or contradicted. The objective is to determine whether this component-level representation provides a more informative and measurable assessment of citation support than conventional categorical support classification.

This framing is deliberately narrower than claiming that fine-grained citation evaluation itself is new: prior work already studies partial support, atomic claims, and richer citation-quality criteria. The proposed contribution is the localized, interpretable characterization of the evidential mismatch within the claim, evaluated against conventional overall support classification.


## 4. Research Questions

Based on the limitations identified in prior work, this project investigates whether citation-support evaluation can move beyond overall categorical judgments toward a more fine-grained and interpretable assessment of the relationship between AI-generated claims and their cited evidence. Specifically, the project will address the following research questions:

**RQ1.** How accurately can Natural Language Processing (NLP) methods determine the overall level of evidential support between an AI-generated claim and its cited evidence?

**RQ2.** Can fine-grained claim decomposition and evidence alignment identify which specific components of an AI-generated claim are supported, unsupported, or contradicted by the cited evidence?

**RQ3.** Which types of fine-grained evidential mismatches—such as numerical discrepancies, entity or temporal differences, qualifiers, scope changes, unsupported additions, and contradictions—are most difficult for automatic citation-support methods to identify?

Together, these questions examine citation support at two levels: overall support classification and fine-grained localization of evidential mismatches. The third research question provides an error-analysis component to determine where existing and proposed methods succeed or fail.


## 5. Dataset and Data Plan

This project will primarily use the publicly available dataset released by Liu, Zhang, and Liang (2023) in *Evaluating Verifiability in Generative Search Engines*. The dataset contains AI-generated responses and their associated citations, together with human annotations assessing the evidential relationship between generated statements and cited sources. Individual citations are annotated as providing full support, partial support, or no support for their associated statements. For citations judged to provide full or partial support, the dataset also contains human-identified evidence from the cited source that supports the annotation.

The existing annotations will provide the primary data for evaluating overall citation-support classification. However, the proposed research investigates a finer-grained question that is not directly represented by the existing labels: which specific information within a generated claim is supported, unsupported, or contradicted by the cited evidence. Following initial dataset inspection, a stratified subset of examples will therefore be selected for manual claim-component annotation. A pilot annotation will first be conducted to refine the annotation guidelines and estimate the required annotation effort before determining the final subset size. The annotations will identify specific types of evidential mismatch, including numerical discrepancies, temporal or entity differences, unsupported qualifiers, changes in scope, unsupported additions, and contradictions.

The project will therefore use the existing human-annotated data for overall support evaluation while creating a smaller fine-grained annotation subset for component-level localization and error analysis. If needed, an additional publicly available attribution dataset may be used as a secondary evaluation set to examine whether the resulting methods generalize beyond the primary dataset.


## 6. Proposed Methodology

The proposed approach will evaluate the evidential relationship between an AI-generated claim and its cited evidence at both an overall and a fine-grained level. The input to the system will consist of a generated claim and the relevant text from its cited source, and the output will include an overall support judgment together with an identification of the specific claim components that are supported, unsupported, or contradicted by the evidence.

For this project, a claim component will be considered **supported** when the cited evidence provides sufficient information to establish that component, **unsupported** when the required information cannot be established from the cited evidence, and **contradicted** when the cited evidence directly conflicts with the component. At the overall claim level, a claim will be considered **fully supported** when all of its independently verifiable components are supported and **partially supported** when only a subset of those components is supported. Claims whose relevant components cannot be established from the evidence or directly conflict with it will be categorized accordingly as unsupported or contradicted. These operational definitions will also guide the manual annotation of the fine-grained evaluation subset.

As a baseline, the project will first evaluate a Natural Language Inference (NLI) approach, where the cited evidence is treated as the premise and the generated claim as the hypothesis. This will provide a conventional entailment-based measure of citation support similar to approaches used in previous attribution research. A Large Language Model (LLM)-based baseline will also be evaluated by prompting an existing model to assess the support relationship between the claim and evidence.

The proposed fine-grained approach will decompose generated claims into smaller, independently verifiable components or atomic claims. Each component will then be aligned with the relevant cited evidence and evaluated separately for evidential support. These component-level judgments will be combined to produce an overall support assessment while preserving information about the specific components responsible for that judgment. This will allow the system to distinguish, for example, between a claim that is completely unsupported and one in which the general statement is supported but a numerical value, entity, qualifier, or other detail is not.

The methods will be compared against human-annotated support labels using standard classification measures such as precision, recall, F1-score, and accuracy. For the manually annotated fine-grained subset, component-level predictions will additionally be evaluated for their ability to correctly identify the unsupported or contradicted portions of a claim. An error analysis will examine performance across different mismatch categories, including numerical, temporal, entity, qualifier, scope, unsupported-addition, and contradiction errors.

The initial experiments will focus on prompting and existing pretrained models rather than requiring model training from scratch. Depending on baseline performance and the amount of suitable labeled data available, parameter-efficient fine-tuning may subsequently be investigated as an extension for improving fine-grained support detection.


## 7. Evaluation and Experimental Plan

The evaluation will examine both **overall citation-support classification** and **fine-grained localization of evidential mismatches**. The dataset will be divided into appropriate development and test sets to ensure that the methods are evaluated on examples not used during development.

For overall citation-support evaluation, the **Natural Language Inference (NLI)** baseline, **Large Language Model (LLM)** baseline, and proposed fine-grained approach will be compared using the human-annotated support labels. Performance will be measured using **accuracy, precision, recall, and F1-score**, with particular attention to partial-support cases, since previous research has shown that these cases are more difficult for automatic evaluators.

For fine-grained evaluation, the manually annotated subset will be used to determine whether the proposed approach correctly identifies the specific components of a claim that are supported, unsupported, or contradicted by the cited evidence. Performance will be evaluated both at the component level and in terms of whether fine-grained analysis improves the resulting overall support judgment compared with approaches that evaluate the complete claim directly.

Finally, an error analysis will examine performance across different types of evidential mismatch, including numerical discrepancies, temporal differences, entity mismatches, unsupported qualifiers, scope changes, unsupported additions, and contradictions. This analysis will help identify which types of citation-support errors are handled effectively and which remain challenging for current **Natural Language Processing (NLP)** methods.

Together, these experiments will address the three research questions by evaluating **overall support detection, fine-grained mismatch localization, and performance across different types of evidential errors**.


## 8. Expected Contributions

This project is expected to contribute an empirical investigation of fine-grained and interpretable citation-support evaluation for AI-generated content. Rather than treating citation verification solely as an overall classification problem, the study will examine whether the specific information responsible for a support judgment can be identified and localized within a generated claim.

The expected contributions are threefold. First, the project will provide a systematic comparison of conventional Natural Language Inference (NLI) and Large Language Model (LLM)-based citation-support methods with a fine-grained claim-decomposition approach. Second, it will investigate whether component-level evidence alignment can provide interpretable support assessments by identifying which portions of a claim are supported, unsupported, or contradicted by the cited evidence. Third, the project will provide an error analysis of different evidential mismatch types, including numerical, temporal, entity, qualifier, scope, unsupported-addition, and contradiction errors, to better understand where current citation-evaluation methods succeed and fail.

The resulting analysis is intended to provide insight into whether fine-grained evidence alignment offers meaningful advantages over conventional overall support classification and to identify remaining challenges in automatically evaluating the evidential support of citations in AI-generated content.


## 9. Breadth and Mastery

This project integrates several Natural Language Processing (NLP) concepts covered in the curriculum. **Dependency parsing and linguistic structure** are relevant to decomposing complex generated statements into smaller, independently verifiable claim components. **Self-attention and Transformer architectures** provide the foundation for the pretrained models used for Natural Language Inference (NLI) and Large Language Model (LLM)-based citation evaluation. **Pretraining and transfer learning** are central to adapting existing language models to citation-support evaluation, with parameter-efficient fine-tuning considered as an extension if the available labeled data supports it. **Prompting** will be used to establish an LLM-based citation-evaluation baseline, while concepts from **Natural Language Generation (NLG)** motivate the broader problem of evaluating whether generated language remains faithful to external evidence.

Beyond applying individual NLP techniques, the project requires designing and implementing a complete experimental pipeline for claim decomposition, evidence alignment, citation-support classification, and fine-grained error analysis. Multiple approaches will be evaluated under consistent experimental conditions using human-annotated data and held-out evaluation examples. This combination of NLP model implementation, controlled experimentation, quantitative evaluation, and analysis of model failure modes is intended to demonstrate both breadth across the NLP curriculum and deeper mastery of the specific problem of evidential support evaluation.


## 10. Project Schedule

The project will begin on **August 31, 2026** and proceed according to the following weekly milestones.

| Week | Planned Milestone |
|---|---|
| **Week 1** | Finalize project scope, research questions, literature review, dataset selection, and experimental design. Begin acquiring and inspecting the primary dataset. |
| **Week 2** | Prepare and preprocess the dataset. Examine the distribution of full-support, partial-support, and no-support examples and establish development and test splits. |
| **Week 3** | Implement the Natural Language Inference (NLI) baseline for overall citation-support classification. |
| **Week 4** | Implement the Large Language Model (LLM)-based baseline and compare initial performance with the NLI baseline. |
| **Week 5** | Develop guidelines for fine-grained claim-component annotation and evidential mismatch categories. Conduct a pilot annotation and refine the annotation scheme. |
| **Week 6** | Complete the fine-grained evaluation subset and implement decomposition of generated claims into independently verifiable components. |
| **Week 7** | Develop the component-level evidence-alignment and support-assessment approach. |
| **Week 8** | Integrate component-level judgments into an overall support assessment and conduct the first complete evaluation of the proposed approach. |
| **Week 9** | Compare the fine-grained approach with the Natural Language Inference (NLI) and Large Language Model (LLM) baselines and refine the approach based on development-set results. |
| **Week 10** | Run final experiments on held-out test data and calculate overall and component-level evaluation metrics. |
| **Week 11** | Conduct error analysis across numerical, temporal, entity, qualifier, scope, unsupported-addition, and contradiction cases. Analyze results for the three research questions. |
| **Week 12** | Consolidate experimental results, prepare tables and figures, and draft the final report. |
| **Week 13** | Complete the discussion, limitations, conclusions, proofreading, and final revisions. |

**Final submission date:** November 30, 2026, by 11:59 PM MT.


## 11. References

Gao, T., Yen, H., Yu, J., & Chen, D. (2023). Enabling large language models to generate text with citations. In *Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing* (pp. 6465–6488). Association for Computational Linguistics. https://doi.org/10.18653/v1/2023.emnlp-main.398

Li, Y., Yue, X., Liao, Z., & Sun, H. (2024). AttributionBench: How hard is automatic attribution evaluation? In *Findings of the Association for Computational Linguistics: ACL 2024* (pp. 14919–14935). Association for Computational Linguistics. https://doi.org/10.18653/v1/2024.findings-acl.886

Liu, N. F., Zhang, T., & Liang, P. (2023). Evaluating verifiability in generative search engines. In *Findings of the Association for Computational Linguistics: EMNLP 2023* (pp. 7001–7025). Association for Computational Linguistics. https://doi.org/10.18653/v1/2023.findings-emnlp.467

Rashkin, H., Nikolaev, V., Lamm, M., Aroyo, L., Collins, M., Das, D., Petrov, S., Tomar, G. S., Turc, I., & Reitter, D. (2023). Measuring attribution in natural language generation models. *Computational Linguistics, 49*(4), 777–840. https://doi.org/10.1162/coli_a_00486

Wu, K., Wu, E., Wei, K., Zhang, A., Casasola, A., Nguyen, T., Riantawan, S., Shi, P., Ho, D., & Zou, J. (2025). An automated framework for assessing how well LLMs cite relevant medical references. *Nature Communications, 16*, 3615. https://doi.org/10.1038/s41467-025-58551-6

Xu, Y., Gao, J., Yu, X., Bi, B., Shen, H., & Cheng, X. (2025). ALiiCE: Evaluating positional fine-grained citation generation. In *Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 1: Long Papers)* (pp. 545–561). Association for Computational Linguistics. https://doi.org/10.18653/v1/2025.naacl-long.23

Xu, Y., Qi, P., Chen, J., Liu, K., Han, R., Liu, L., Min, B., Castelli, V., Gupta, A., & Wang, Z. (2025). CiteEval: Principle-driven citation evaluation for source attribution. In *Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)* (pp. 32759–32778). Association for Computational Linguistics. https://doi.org/10.18653/v1/2025.acl-long.1574

Yue, X., Wang, B., Chen, Z., Zhang, K., Su, Y., & Sun, H. (2023). Automatic evaluation of attribution by large language models. In *Findings of the Association for Computational Linguistics: EMNLP 2023* (pp. 4615–4635). Association for Computational Linguistics. https://doi.org/10.18653/v1/2023.findings-emnlp.307

Zhang, W., Aliannejadi, M., Yuan, Y., Pei, J., Huang, J.-H., & Kanoulas, E. (2024). Towards fine-grained citation evaluation in generated text: A comparative analysis of faithfulness metrics. In *Proceedings of the 17th International Natural Language Generation Conference* (pp. 427–439). Association for Computational Linguistics. https://doi.org/10.18653/v1/2024.inlg-main.35