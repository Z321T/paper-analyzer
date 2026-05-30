---
name: paper-analyzer
description: Comprehensive academic paper analysis for AI reading, reviewing, and presentation preparation. Use when the user mentions analyzing a paper, reading a paper, understanding a paper, preparing a presentation about a paper, reviewing a paper, or comparing papers. Triggers: "分析论文", "读论文", "paper analysis", "论文", "paper review", "审稿", "组会", "presentation", "paper comparison", "论文对比", "速读", "精读", "详解", "逐段", "全面分析", ".pdf" with research context. Supports any language paper (English, Chinese, bilingual) and outputs in the user's query language.
---

# Paper Analyzer

General-purpose academic paper analysis skill. Reads PDF and/or markdown papers and produces structured analysis across five reading modes (plus paper comparison) with diagram generation where appropriate.

## Mode Selection

At the start of every session, detect the user's intent from their query. The skill supports five modes plus paper comparison:

| User says | Mode | Description |
|:----------|:-----|:------------|
| "速读" / "skim" / "快速" / "大概" / "overview" / "brief" | **Skim** | Quick scan, ~800 words, core message only |
| "详解" / "comprehensive" / "逐段" / "全文讲解" / "按章节" / "通读" | **Comprehensive** | Faithful section-by-section walkthrough following the paper's own structure; no new diagrams or critical evaluation |
| "精读" / "deep" / "深入" / "组会" / "presentation" / no explicit mode | **Deep** | Analytical reading with mind maps, flowcharts, comparison tables, innovation ratings, and reproducibility assessment |
| "审稿" / "review" / "批判" / "找问题" / "peer review" | **Review** | Critical review with Summary/Strengths/Weaknesses/Questions and claim-evidence mapping |
| "全面" / "full" / "全面分析" / "full analysis" | **Full** | Combination: first Comprehensive, then Deep — two independent outputs in sequence |
| 2-3 papers mentioned together with "对比" / "compare" / "区别" | **Comparison** | Side-by-side comparison across dimensions |

If mode is ambiguous (e.g., "详细" alone), default to **Deep** and mention that Comprehensive mode is also available if the user wants a section-by-section walkthrough.

## Workflow

### Step 1: Ingest Paper(s)

- If the user provides a PDF path, read it with the Read tool.
- If a markdown translation exists alongside the PDF, read both. Note discrepancies if any.
- If the paper is in the current directory, auto-discover: look for `.pdf` and `.md` files.
- If no paper is found, ask the user to provide the file path.

### Step 2: Detect Mode and Language

- Detect the reading mode from the user's query (see table above).
- Detect the output language: match the user's query language. Chinese query → Chinese output. English query → English output. The user can override: "用中文输出" / "output in English".
- If the paper itself contains bilingual versions, use the translation to improve understanding but cite the original.
- **Full mode**: When "全面" / "full" / "全面分析" / "full analysis" is detected, produce two independent outputs in sequence — first **Comprehensive** (详解), then **Deep** (精读). Each follows its own complete template.

### Step 3: Generate Analysis

Follow the output specification for the selected mode. All full specifications are in reference files — read them when needed:

- **[references/output-specifications.md](references/output-specifications.md)** — Complete output templates for Skim, Comprehensive, Deep, Review, Full, and Comparison modes. **Read this for every analysis** to follow the exact section structure.
- **[references/review-standards.md](references/review-standards.md)** — Professional peer review criteria, claim-evidence mapping methodology, common flaws checklist, and question-writing guidelines. **Read this for Review mode or when the user asks critical questions.**
- **[references/reproducibility-checklist.md](references/reproducibility-checklist.md)** — Five-dimension reproducibility assessment (code, data, training details, ablation, metrics). **Read this for Deep and Review modes.**
- **[references/mermaid-templates.md](references/mermaid-templates.md)** — Mermaid diagram templates: mindmap, method flowchart, training pipeline, memory architecture, experiment pipeline, comparison matrix, decision flow. **Read this when generating diagrams for Deep mode.**

### Step 4: Generate Diagrams (Deep and Skim modes only)

**Do NOT generate Mermaid diagrams for Comprehensive or Review modes** — those modes are text-only.

For Deep and Skim modes, always include:
- **Mindmap** (Deep mode): Paper structure overview using the mindmap template
- **Method flowchart** (Deep and Skim modes): Technical pipeline using the flowchart template

Use the appropriate template from `mermaid-templates.md`. Adapt node labels to the paper's actual content. Place diagrams immediately after the relevant text section.

### Step 5: Interactive Follow-up

After outputting the analysis, proactively suggest follow-up questions the user might ask:
- For Comprehensive mode: "想进一步了解哪个章节的细节？我可以展开某个公式的推导或某个实验设计的讨论。"
- For Deep mode: "想深入了解哪个模块？我可以展开公式推导、设计动机、或替代方案讨论。"
- For Review mode: "需要对哪个声明做更严格的证据审查？"
- For Comparison mode: "需要我基于你的具体场景给出选型建议吗？"
- For Full mode: "两份文档已输出完毕。需要对其中任何部分做进一步展开吗？"

## Feature Reference

When the user asks for specific capabilities, consult the corresponding reference:

| User asks for | Reference file | Section |
|:--------------|:---------------|:--------|
| Output structure for X mode | output-specifications.md | Mode section |
| Section-by-section walkthrough | output-specifications.md | Comprehensive section |
| Review / critique / find flaws | review-standards.md | Full file |
| Is this reproducible? | reproducibility-checklist.md | Full file |
| How to draw the diagram? | mermaid-templates.md | Matching template |
| Compare 2+ papers | output-specifications.md | Comparison section |
| Prepare presentation slides | output-specifications.md | Deep mode + Presentation section |
| Deep-dive into a module/equation | No reference — answer directly with technical rigor |
| Full analysis (详解+精读) | output-specifications.md | Full mode section |

## Multi-Language Paper Handling

- If paper is in English but user queries in Chinese: output Chinese, translate technical terms but keep original in parentheses on first use, e.g., "个性演化机制 (Personality Evolution Mechanism, PEM)"
- If paper is in Chinese but user queries in English: output English, same translation convention
- If user provides both PDF (English) and MD (Chinese translation): cross-reference both, note any translation issues
- Diagrams: use the output language for node labels

## Supplementary Material Handling

If the paper references an appendix, supplementary material, or external resources:
1. If the user provides the supplementary file, read and integrate it
2. If not provided, flag key details likely in the supplement: "论文提到超参数见附录，如需要详细分析请提供补充材料文件"
3. Common supplement content to check: hyperparameters, prompt templates, additional ablation studies, full dataset statistics, derivation details, qualitative examples

## Guidelines

### Do
- Read the relevant reference file before generating output for a given mode
- Generate Mermaid diagrams for every Deep mode analysis (mindmap + flowchart minimum)
- Cite specific sections, figures, and tables when making claims about the paper
- Provide the reproducibility score table in Deep and Review modes
- Include claim-evidence mapping in Review mode
- Be specific about weaknesses — "lacks ablation" is vague; "Section 4.3 ablates only the full pipeline vs. baseline; individual contribution of each of the three proposed modules is not measured" is useful
- In Comprehensive mode: follow the paper's own section order and numbering; explain faithfully without critique
- In Full mode: produce two clearly separated outputs (Comprehensive first, then Deep), each following its own complete template

### Don't
- Don't generate diagrams without reading mermaid-templates.md first
- Don't generate Mermaid diagrams in Comprehensive or Review modes — those are text-only
- Don't skip the reproducibility assessment in Deep/Review modes
- Don't output generic praise without specific evidence
- Don't confuse mode requirements: Skim should be short, Comprehensive should be faithful and expository, Deep should be analytical, Review should be critical
- Don't include innovation ratings, reproducibility scores, or claim-evidence mapping in Comprehensive mode
- Don't ignore language cues from the user's query
