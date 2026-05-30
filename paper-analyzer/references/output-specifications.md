# Output Specifications for Paper Analyzer

## General Rules

- Output language: match the user's query language by default. If the paper is in English but user asks in Chinese, output Chinese. The user can explicitly request a specific output language.
- Every mode output must start with the paper title, authors, affiliations, and a one-line TL;DR.
- Use Mermaid code blocks for diagrams in Skim and Deep modes only. Place diagrams immediately after the relevant text section. Do NOT generate Mermaid diagrams in Comprehensive or Review modes.

---

## Mode 1: Skim (速读)

**Trigger**: "速读" / "快速看一下" / "skim" / "大概" / "brief" / "overview"

**Output structure** (target: under 800 words):

```
## [Paper Title]

**作者**: [Authors], [Affiliations]
**发表**: [Venue, Year] | **代码**: [URL or "未开源"]

### 一句话总结
[One sentence: what problem, what method, what result]

### 核心问题
[2-3 sentences on the problem being solved]

### 方法概要
[2-3 sentences on the approach. Include one Mermaid flowchart if method has >2 stages]

### 关键结果
- [Bullet 1: main quantitative result]
- [Bullet 2: comparison to baseline/SOTA]
- [Bullet 3: any surprising finding]

### 适合谁读
[1 sentence: who would benefit from reading this paper in detail]
```

---

## Mode 2: Comprehensive (详解)

**Trigger**: "详解" / "comprehensive" / "逐段" / "全文讲解" / "按章节" / "通读"

**Purpose**: A faithful, section-by-section walkthrough of the paper following the original structure. The reader should come away understanding *what the paper says* in each section, without analytical overlays.

**Core principle**: Explain the paper as written — do not critique, evaluate, or reorganize. This is an expository document, not an analytical one.

**Output structure**:

```
## [Paper Title]

**作者**: [Authors], **单位**: [Affiliations]
**发表**: [Venue, Year] | **代码**: [URL or "未开源"] | **项目页**: [URL]

**一句话 TL;DR**: [One sentence summarizing the paper]

---

### 目录
[Auto-generated table of contents matching the paper's actual section structure]

---

### 1. [Section Name — matching the paper's section title exactly]

[For each section and subsection, provide:]

#### 1.1 [Subsection Name]

**原文要点**:
- Key arguments, claims, and findings as stated by the authors
- Important formulas preserved verbatim with brief explanations of each variable
- Descriptions of key figures and tables: what they show, what patterns matter

**归纳** (brief, at end of major sections only):
- How this section connects to the paper's overall argument
- Forward references to later sections if the paper makes them

[Repeat this pattern for every section of the paper, following the paper's own numbering.]

---

### N. 附录要点 (Appendix Highlights)

[If the paper has an appendix, cover:]
- Hyperparameter values not stated in the main text
- Additional ablation studies or experiments
- Prompt templates or data processing details
- Any other supplementary information critical for understanding or reproduction

---

### 组会汇报建议 (Presentation Tips, optional)

[If the user is preparing a presentation:]
- **推荐叙事线索**: Suggested narrative arc
- **时间分配建议**: Time allocation for each section
- **重点突出的"卖点"**: Key selling points to emphasize
- **可能被问到的问题**: Anticipated Q&A with suggested responses
```

**Key rules for Comprehensive mode**:

| Rule | Rationale |
|:-----|:----------|
| Follow the paper's own section order and numbering | The reader uses this as a reading companion alongside the original paper |
| Explain faithfully — no critique, no evaluation | Analysis belongs to Deep mode; this mode serves a different reader need |
| Do NOT generate new Mermaid diagrams | The paper has its own figures; describe them, don't replace them |
| Do NOT include reproducibility assessment | This is an expository document, not an evaluation |
| Do NOT include innovation contribution rating tables | Analytical content belongs to Deep mode |
| Do NOT include claim-evidence mapping | Critical content belongs to Review mode |
| Cover appendix content explicitly | Many readers skip the appendix; this mode brings key details forward |
| Use "原文要点" labels for faithful explanation, "归纳" for brief synthesis | Clear labeling prevents the reader from confusing author claims with your synthesis |

---

## Mode 3: Deep (精读)

**Trigger**: "精读" / "deep" / "深入" / "组会" / "presentation" / default (no mode specified)

Note: "详细" alone is ambiguous — default to Deep and mention Comprehensive mode is also available.

**Output structure** (full analysis):

```
## [Paper Title]

**作者**: [Authors] | **单位**: [Affiliations] | **发表**: [Venue, Year]
**代码**: [URL] | **项目页**: [URL] | **引用量**: [if available]

---

### 思维导图

[Full Mermaid mindmap of the paper's structure and contributions]
(See mermaid-templates.md for template)

---

### 1. 问题定位与动机

#### 1.1 核心问题
[What problem does this paper solve? Why is it important?]

#### 1.2 现有方法及不足
[Table comparing prior approaches and their limitations]

#### 1.3 本文定位
[Where does this work fit? Pioneer / Improver / Integrator?]

---

### 2. 方法详解

#### 2.1 整体框架
[Method flowchart via Mermaid — see mermaid-templates.md]

#### 2.2 关键模块逐个分析
For each key module:
- **设计动机**: Why this design?
- **技术细节**: How it works (formulas, algorithms)
- **与前人区别**: What's different from prior work?
- **潜在问题**: Any concerns with this design choice?

#### 2.3 训练/实现细节
- Model backbone
- Training stages / objectives
- Key hyperparameters
- Compute requirements

---

### 3. 实验分析

#### 3.1 实验设计
[What experiments? What datasets? What baselines? What metrics?]
[Experiment pipeline via Mermaid if complex]

#### 3.2 核心结果
[Key results table. Highlight the most important numbers.]

#### 3.3 消融实验
[What ablation studies? What do they prove?]

#### 3.4 结果可信度
[Are the results convincing? Any missing baselines? Any cherry-picking?]

---

### 4. 创新点与贡献

| 序号 | 创新点 | 类型 | 影响力 |
|:-----|:-------|:-----|:-------|
| 1 | [Innovation] | [Conceptual/Methodological/Empirical/Engineering] | [High/Med/Low] |
| 2 | ... | ... | ... |

For each innovation, explain **why it matters** and **what would break without it**.

---

### 5. 相关工作定位

#### 5.1 前置必读工作
[List 3-5 papers you must read to fully understand this one]

#### 5.2 与本工作的本质区别
[Comparison table across key dimensions]

#### 5.3 后续可能方向
[Based on this paper's limitations and contributions, what's next?]

---

### 6. 可复现性评估
(See reproducibility-checklist.md for detailed criteria)
[Summary score and key concerns]

---

### 7. 讨论与思考
- [Discussion point 1]
- [Discussion point 2]
- ...
```

---

## Mode 4: Review (审稿)

**Trigger**: "审稿" / "review" / "批判性" / "找问题" / "peer review"

**Output structure**:

```
## [Paper Title]

### Review Summary

**Overall Rating**: [Strong Accept / Accept / Weak Accept / Borderline / Reject]
**Confidence**: [High / Medium / Low]
**Score**: [X/10 if applicable]

---

### Strengths (优势)

1. [Strength 1 — be specific, cite sections/experiments]
2. [Strength 2]
3. ...

---

### Weaknesses (不足)

#### Major Issues (影响接受决定)

1. **[Issue Title]**
   - **位置**: [Section/Figure/Table]
   - **问题描述**: [What's wrong]
   - **严重性**: [Why this matters]
   - **建议**: [How to fix]

2. ...

#### Minor Issues

1. **[Issue Title]**: [Brief description and fix]

---

### Claim-Evidence Mapping (声明-证据映射)

For each major claim in the paper, check if it's adequately supported:

| # | 论文声明 | 支撑实验 | 证据力度 | 问题 |
|:--|:---------|:---------|:---------|:-----|
| 1 | [Claim] | [Which experiment supports it] | [Strong/Adequate/Weak/None] | [Gap if any] |
| 2 | ... | ... | ... | ... |

**Evidence Strength Legend**:
- **Strong**: Multiple experiments, diverse settings, statistical significance
- **Adequate**: One solid experiment, reasonable settings
- **Weak**: Only qualitative, small scale, cherry-picked
- **None**: Claim has no experimental backing

---

### Questions for Authors (向作者提问)

1. **Q1**: [Question about methodology]
2. **Q2**: [Question about experimental design]
3. **Q3**: [Question about claims or interpretation]
4. **Q4**: [Question about missing details or baselines]

---

### Additional Comments

- [Any other observations for the authors/area chair]
```

---

## Mode 5: Full (全面分析)

**Trigger**: "全面" / "full" / "全面分析" / "full analysis"

**Purpose**: A combination mode that provides both the faithful walkthrough AND the analytical deep-read. This is for readers who want the complete picture — first understand what the paper says, then understand what it means.

**Behavior**: Produce TWO independent outputs in strict sequence:

1. **First output: Comprehensive mode (详解)** — the full section-by-section faithful walkthrough, following the paper's own structure. Complete with appendix coverage and optional presentation tips.

2. **Second output: Deep mode (精读)** — the full analytical reading with mind maps, flowcharts, comparison tables, innovation ratings, and reproducibility assessment.

**Format**:

```
---
## 第一部分：论文详解
---

[Complete Comprehensive mode output as specified in Mode 2]

---
## 第二部分：论文精读分析
---

[Complete Deep mode output as specified in Mode 3]
```

**Key rules for Full mode**:

| Rule | Rationale |
|:-----|:----------|
| Comprehensive always comes first, Deep second | The reader needs to know what the paper says before understanding what it means |
| Both outputs must be complete and self-contained | Each can be read independently; cross-reference only by section name |
| Use a clear visual divider between the two | `---` separators and section headers make the boundary unambiguous |
| Do NOT merge or interleave the two modes | Mixing expository and analytical content defeats the purpose of differentiation |
| Each output follows its own complete template | The Comprehensive section uses the Mode 2 spec; the Deep section uses the Mode 3 spec |
| The follow-up prompt should offer to expand either part | "需要对详解或精读部分的任何内容做进一步展开吗？" |

---

## Paper Comparison Mode

**Trigger**: user provides 2-3 papers and asks for comparison / "对比" / "compare"

**Output structure**:

```
### 对比总览

| 维度 | [Paper A] | [Paper B] | [Paper C] |
|:-----|:----------|:----------|:----------|
| 核心问题 | | | |
| 方法路线 | | | |
| 基座模型 | | | |
| 数据集 | | | |
| 核心指标 | | | |
| 开源 | | | |
| 主要优势 | | | |
| 主要局限 | | | |

### 路线图（Mermaid）
[Show how the three papers relate — independent / sequential / competing]

### 选择建议
- 如果你的场景是 X → 选 A
- 如果你的场景是 Y → 选 B
- 如果你的场景是 Z → 选 C
```
