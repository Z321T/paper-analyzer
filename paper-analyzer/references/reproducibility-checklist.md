# Reproducibility Assessment Checklist

## Assessment Summary Format

At the end of the reproducibility assessment, provide a summary:

```
### 可复现性评分

| 维度 | 评分 | 说明 |
|:-----|:----:|:-----|
| 代码开源 | [✅/⚠️/❌] | [Details] |
| 数据可用 | [✅/⚠️/❌] | [Details] |
| 训练细节 | [✅/⚠️/❌] | [Details] |
| 消融实验 | [✅/⚠️/❌] | [Details] |
| 评估指标 | [✅/⚠️/❌] | [Details] |

**总体可复现性**: [高/中/低/不可复现]
**关键障碍**: [What would block someone from reproducing this work]
```

Legend:
- ✅ = Satisfactory
- ⚠️ = Partially addressed, some gaps
- ❌ = Missing or seriously insufficient

---

## Dimension 1: Code Availability

### Check Items

- [ ] Is there a GitHub repository link?
- [ ] Is the repo public (not 404)?
- [ ] Does it contain the core training code (not just evaluation)?
- [ ] Are dependencies listed (requirements.txt / environment.yml)?
- [ ] Are there instructions for running the code?
- [ ] Is the license permissive?
- [ ] Is there a checkpoint/model weights download link?
- [ ] Are the configs/hyperparameters for reproducing key results provided?

### Rating

- **✅**: Code + weights + instructions all available
- **⚠️**: Partial release (e.g., inference only, or missing key components)
- **❌**: No code or placeholder only

---

## Dimension 2: Data Availability

### Check Items

- [ ] Is the dataset publicly available?
- [ ] If synthetic: is the generation pipeline/code provided?
- [ ] If proprietary: is there a clear description of why it can't be released?
- [ ] Are data statistics clearly reported?
- [ ] Are train/val/test splits clearly defined?
- [ ] Are preprocessing steps documented?
- [ ] Are examples provided to understand data format?

### Rating

- **✅**: Full dataset or generation code available
- **⚠️**: Partial (e.g., only test set, or samples only)
- **❌**: No data and no clear path to obtain/recreate

---

## Dimension 3: Training Detail Completeness

### Check Items

- [ ] Model architecture fully specified (backbone, layers, dimensions)?
- [ ] Training objective(s) clearly stated with formulas?
- [ ] Optimizer, learning rate, scheduler specified?
- [ ] Batch size, number of epochs/steps specified?
- [ ] Hardware used (GPU type, count, memory)?
- [ ] Training time / wall clock time reported?
- [ ] Random seeds specified?
- [ ] Number of runs for error bars?
- [ ] Prompt templates (if applicable) provided?
- [ ] Data augmentation details (if applicable)?

### Common Gaps

- "Trained with Adam" but no learning rate
- "Standard augmentation" without specifics
- Hyperparameters mentioned in text but values differ from appendix
- Learning rate schedule mentioned but warmup steps missing

### Rating

- **✅**: All details present; another researcher could replicate with the paper alone
- **⚠️**: Most details present but 1-2 non-critical gaps
- **❌**: Major details missing; replication would require guessing

---

## Dimension 4: Ablation Study Completeness

### Check Items

- [ ] Is each proposed module/component ablated?
- [ ] Are loss terms individually validated?
- [ ] Are hyperparameter sensitivity studies included?
- [ ] Is the contribution of each component quantified?
- [ ] Are "negative results" or failed attempts discussed?
- [ ] Is there an analysis of when/why the method fails?
- [ ] Are alternative design choices compared (not just "with vs without")?

### Red Flags

- Only one big ablation: "Ours vs Ours w/o everything" — doesn't tell which component matters
- Ablations only on one dataset or one setting
- No hyperparameter sensitivity analysis
- Each component adds <1% but presented as important

### Rating

- **✅**: Exhaustive ablations covering all components across settings
- **⚠️**: Key components ablated but some gaps
- **❌**: No ablation or trivial single ablation

---

## Dimension 5: Evaluation Metric Appropriateness

### Check Items

- [ ] Are metrics standard for this task/domain?
- [ ] If a new metric is proposed: is it justified? Validated?
- [ ] Are statistical significance tests reported?
- [ ] Are confidence intervals or error bars shown?
- [ ] Are results reported on multiple random seeds?
- [ ] If using LLM-as-a-Judge: is the judge model specified? Prompt provided? Correlation with human judgment reported?
- [ ] Are metric calculations clearly defined (macro vs micro, aggregation method)?

### Common Issues

- Accuracy on imbalanced dataset without F1/AUROC
- "Human evaluation" without details on raters, scale, instructions, agreement
- LLM-as-a-Judge without calibration or human correlation
- Point estimates without any measure of variance

### Rating

- **✅**: Standard metrics + statistical tests + error bars + multiple seeds
- **⚠️**: Reasonable metrics but missing statistical rigor
- **❌**: Inappropriate metrics or missing critical information

---

## Overall Reproducibility Assessment

Combine the five dimensions into an overall assessment:

| 评级 | 含义 |
|:-----|:-----|
| **高** | Expert in the field could reproduce key results within reasonable time |
| **中** | Possible but would require significant effort or filling in gaps |
| **低** | Major barriers; unlikely to be reproduced without author assistance |
| **不可复现** | Critical information missing; claims cannot be independently verified |

---

## Compute Requirements Assessment (bonus)

If training from scratch is required:
- Estimated GPU hours
- Estimated cost (at cloud pricing)
- Is it practical for most academic labs?

Example: "Training requires 8×A100 for 3 days ≈ 576 GPU-hours ≈ $1,152 at $2/GPU-hr. This is reasonable for most well-funded labs."
