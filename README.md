# paper-analyzer 论文分析技能

## 文件结构

```
.claude/skills/paper-analyzer/
├── SKILL.md                                  (105行)
└── references/
    ├── output-specifications.md               (255行)
    ├── review-standards.md                    (136行)
    ├── reproducibility-checklist.md           (174行)
    └── mermaid-templates.md                   (262行)
```

## 各文件职责

| 文件                         | 职责                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| SKILL.md                     | 总控：模式识别、工作流定义、功能索引、多语言处理             |
| output-specifications.md     | 四种模式（速读/精读/审稿/对比）的完整输出模板，精确到每个节  |
| review-standards.md          | 专业审稿标准：Summary/Strengths/Weaknesses/Questions格式 + 声明-证据映射方法论 + 10种常见缺陷检查清单 |
| reproducibility-checklist.md | 五维度可复现性评估（代码/数据/训练细节/消融/指标），每个维度有具体检查项和评分标准 |
| mermaid-templates.md         | 7套Mermaid模板：mindmap、方法流程图、训练流水线、记忆/数据流架构、实验流水线、对比矩阵、决策流 |

## 15项功能覆盖

| F#   | 功能          | 覆盖位置                                    |
| ---- | ------------- | ------------------------------------------- |
| F1   | 主要工作解读  | Deep mode §1                                |
| F2   | 方法分析      | Deep mode §2                                |
| F3   | 实验分析      | Deep mode §3                                |
| F4   | 创新点提炼    | Deep mode §4                                |
| F5   | 思维导图      | mermaid-templates.md Template 1             |
| F6   | 方法流程图    | mermaid-templates.md Template 2             |
| F7   | 审稿意见      | Review mode + review-standards.md           |
| F8   | 三层阅读模式  | SKILL.md mode selection table               |
| F9   | 论文对比模式  | Comparison mode in output-specifications.md |
| F10  | 可复现性评估  | reproducibility-checklist.md                |
| F11  | 汇报材料生成  | Deep mode + §9 presentation suggestions     |
| F12  | 技术细节追问  | SKILL.md interactive follow-up              |
| F13  | 多语言支持    | SKILL.md multi-language section             |
| F14  | 声明-证据映射 | review-standards.md claim-evidence section  |
| F15  | 补充材料处理  | SKILL.md supplementary material section     |

## 使用方式

以后拿到任何新论文，直接说：

- **"速读这篇论文"** → 800字快速扫描
- **"帮我精读/分析这篇论文"** → 完整7段分析 + 思维导图 + 流程图
- **"以审稿人视角审这篇论文"** → Summary/Strengths/Weaknesses/Questions + 声明-证据映射
- **"对比这3篇论文"** → 多维度对比矩阵 + 选型建议

系统会自动识别模式、识别论文语言、加载对应参考文件来生成输出。

---
