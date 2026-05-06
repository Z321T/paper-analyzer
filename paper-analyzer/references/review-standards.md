# Review Standards for Paper Analysis

## Review Format

Use the professional peer-review format: **Summary / Strengths / Weaknesses / Questions**.

---

## Summary Writing Guide

The summary should:
1. Restate the problem and the paper's claimed contribution (1-2 sentences)
2. Summarize the technical approach (1-2 sentences)
3. Give an overall assessment (1 sentence)

Keep it under 150 words. Do not repeat the abstract — summarize from a reviewer's perspective.

---

## Strengths Assessment

When identifying strengths, evaluate across these dimensions:

| 维度 | 评估标准 |
|:-----|:---------|
| **新颖性** (Novelty) | 问题定义、方法设计、或发现是否真正新颖？ |
| **重要性** (Significance) | 解决了领域中的重要问题吗？谁会关心这个结果？ |
| **技术质量** (Technical Quality) | 方法设计是否合理？推导是否严谨？实现是否可靠？ |
| **实验充分性** (Experimental Adequacy) | 实验是否覆盖了所有声明？对比是否公平？ |
| **呈现质量** (Presentation) | 写作是否清晰？图表是否有帮助？ |
| **实用性** (Practical Value) | 方法是否可部署？是否需要过多资源？ |

Each strength should cite specific evidence (section number, figure, table).

---

## Weaknesses Assessment

### Classification

**Major Issues** (影响论文的接受决定):
- 方法有根本性缺陷
- 核心声明缺乏实验支撑
- 与现有工作的对比不公平或不充分
- 实验结果不支持结论
- 遗漏关键相关工作
- 可复现性严重问题

**Minor Issues** (可通过修改解决):
- 写作或呈现问题
- 缺少消融实验但非致命
- 数据集或评估的次要局限
- 超参数选择缺乏论证
- 图表可读性

### Common Flaws to Check

1. **过声明 (Overclaiming)**: 标题/摘要声称的内容远超实验实际支撑的范围
2. **不公平对比**: 基线的实现或调参不充分，对比条件不对等
3. **信息泄露**: 训练集和测试集之间存在不应有的重叠
4. **确认偏误**: 只展示支持结论的实验，忽略不利证据
5. **Ablation不够**: 消融实验不完整，无法确定每个模块的真实贡献
6. **随机种子不足**: 没有多种子实验或误差线
7. **过时基线**: 对比的基线不是当前 SOTA
8. **指标选择**: 使用了对任务不敏感或不恰当的评估指标
9. **规模局限**: 实验只在小型数据集或单一领域进行
10. **"Secrecy by obscurity"**: 关键实现细节缺失，故意模糊

---

## Claim-Evidence Mapping

This is the core of critical reading. For each major claim in the paper:

### Procedure

1. **Extract all major claims** from: Abstract + Introduction (end) + Conclusion
2. **For each claim, find**:
   - Which table/figure/section supports it?
   - Is the evidence quantitative or qualitative?
   - What's the sample size / statistical power?
   - Are there confounds or alternative explanations?
3. **Rate the evidence**: Strong / Adequate / Weak / None
4. **Flag gaps**: When a claim is broad but evidence is narrow

### Example from PersonaVLM

| 声明 | 支撑 | 证据力度 |
|:-----|:-----|:---------|
| "长期个性化" | 最长实验 = 128k 上下文（约数百轮对话） | Adequate — 覆盖数月交互是否算"长期"有待商榷 |
| "超越 GPT-4o" | 多基准 + 自动化评估 + Gemini评判 | Strong — 多维度验证 |
| "自包含，保护隐私" | 使用 7B 开源模型本地运行 | Strong — 设计级别支撑 |

### Red Flags

- Claim: "Our method achieves state-of-the-art performance" → Check: which datasets? which baselines? which metrics? only one cherry-picked setting?
- Claim: "This demonstrates the importance of X" → Check: is X the only difference? is the improvement significant?
- Claim: "Our approach is general" → Check: tested on how many domains/tasks/models?
- Claim: "First to do X" → Check: literature review thorough? ArXiv search?

---

## Questions for Authors

Questions should be:
1. **Actionable**: the authors could reasonably answer
2. **Specific**: cite exact sections, tables, figures
3. **Diagnostic**: answers would change the review assessment
4. **Non-obvious**: not answered already in the paper text

Bad: "Why did you choose this method?" (too vague)
Good: "In Section 3.2, you use cosine decay for λ. How sensitive is performance to the decay rate? Table 3 shows results for λ₀=0.3, but what happens at λ₀=0.1 or λ₀=0.5?"

---

## Rating Guidelines

### Overall Rating

| Rating | Criteria |
|:-------|:---------|
| **Strong Accept** | Novel, significant, technically flawless, exhaustive experiments, well-written |
| **Accept** | Novel or significant, technically sound, solid experiments |
| **Weak Accept** | Incremental but solid, or novel but experiments incomplete |
| **Borderline** | Interesting idea but major concerns about correctness or completeness |
| **Reject** | Fatal flaw, insufficient novelty, or claims unsupported by evidence |

### Confidence

| Level | Criteria |
|:------|:---------|
| **High** | Expert in this exact sub-area, fully understand method and experiments |
| **Medium** | Familiar with the area, understand the main contributions |
| **Low** | Adjacent area, may miss nuances |

If confidence is Low, note it explicitly and qualify the review accordingly.
