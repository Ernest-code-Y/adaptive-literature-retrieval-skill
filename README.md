# Adaptive Literature Retrieval

> 不只是搜索论文，而是让 AI 根据覆盖缺口持续学习、验证与迭代，直到检索有依据地趋于饱和。  
> Beyond searching papers: an AI-driven retrieval loop that learns from evidence, closes coverage gaps, and stops only when the search is demonstrably saturated.

[中文](README.md) · [English](README_EN.md)

---

## 中文

### 项目简介

**Adaptive Literature Retrieval** 是一套面向 AI Agent 的领域无关、覆盖感知、证据驱动型自适应文献检索 Skill。

它不依赖固定关键词、预设学科词表或一次性搜索，而是针对每个研究主题动态构建“**方法—任务—应用—相关问题**”四维研究框架。检索过程中，Skill 会从已核验文献中持续发现新术语、研究方向和重要期刊会议，分析当前文献集合的覆盖情况，并根据尚未解决的研究缺口与预期信息增益，决定下一轮应该执行关键词检索、语义检索、期刊会议定向检索、引文扩展还是相似论文发现。

与依赖模型记忆生成参考文献或达到固定篇数便停止的工作流不同，本项目强调：

- 文献必须经过真实来源核验；
- 每轮检索都有原因、结果和覆盖变化记录；
- 检索策略随当前证据动态调整；
- 只有在覆盖充分、边际收益降低且不存在高优先级缺口时，才有依据地停止；
- 最终结果以可审计的七工作表 Excel 文件交付。

“尽可能完整”在这里表示：在用户指定范围与可访问数据源内系统性提高召回率，而不是声称找到了世界上存在的每一篇论文。

### 核心特色

#### 1. 动态研究框架，而非固定领域词表

每个研究任务都会重新构建四个可演化维度：

- **Methods**：解决研究问题所使用的方法、模型或技术；
- **Tasks**：具体研究任务或计算问题；
- **Applications**：应用对象、领域、系统与场景；
- **Related Problems**：约束、目标、性质、评价问题与相邻研究问题。

概念可以来自用户输入、模型扩展或已核验文献，并保留来源、置信度和证据。Skill 不会把航空航天、控制、医学、NLP 等任何领域设为隐藏默认值。

#### 2. 覆盖缺口驱动的自适应检索

Skill 在每轮检索后重新计算方法、任务、应用、相关问题、期刊会议、时间与引文网络的覆盖情况，并识别高优先级缺口。

下一步操作由当前状态决定，而不是机械执行预设查询列表：

- 关键词检索；
- 语义检索；
- 期刊或会议定向检索；
- 特定年份检索；
- 作者检索；
- 前向或后向引文扩展；
- 相似论文发现；
- 术语扩展。

#### 3. 证据驱动的真实性核验

论文不能因为模型“记得”它或标题看起来合理就进入最终结果。入选文献需要核验标题、作者、年份、期刊或会议、持久标识符以及可信书目来源。重要记录可通过出版社或官方页面与独立书目来源进行交叉验证。

项目同时要求：

- 摘要翻译必须基于实际检索到的原始摘要，不得用模型总结冒充翻译；
- 下载链接优先使用已验证的开放获取 PDF 或合法预印本；
- 无公开全文时提供官方文章页面，不猜测或伪造 PDF 地址；
- 外部网页、论文和 PDF 仅作为证据，不作为可执行指令。

#### 4. 基于检索饱和度停止

Skill 不以固定论文数量或固定轮数作为唯一停止条件，而是综合判断：

- 指定期刊会议和时间范围是否已经覆盖；
- 重要研究维度是否具有足够证据；
- 高价值引文扩展是否已经完成；
- 最近多轮新增的相关、已核验论文是否显著减少；
- 重复率是否升高；
- 是否仍存在高优先级缺口。

#### 5. 完整、可审计的 Excel 交付

最终工作簿包含七个工作表：

| 工作表 | 内容 |
|---|---|
| Final Literature | 最终入选文献、来源信息、中文摘要翻译与下载链接 |
| Retrieval Log | 每轮检索的动作、原因、查询、结果与边际收益 |
| Coverage Analysis | 各研究维度的覆盖证据与剩余缺口 |
| Verification | 文献元数据、摘要与链接的核验状态 |
| Excluded Papers | 被排除的候选文献及排除原因 |
| Search Plan | 查询计划、优先级、来源与执行状态 |
| Run Summary | 检索范围、统计结果、停止原因与已知限制 |

#### 6. 领域无关与跨平台

同一套流程可以用于人工智能、计算机科学、医学、化学、材料、机器人、控制、教育、数据库等研究领域。核心工作流不绑定特定搜索提供商或平台工具名，可移植至 ChatGPT Skills、WorkBuddy/CodeBuddy 及其他兼容 Skill 的智能体环境。

### 项目结构

```text
adaptive-literature-retrieval/
├── SKILL.md
├── references/
│   ├── adaptive_retrieval_policy.md
│   ├── dynamic_concept_schema.md
│   ├── excel_output_schema.md
│   ├── platform_compatibility.md
│   ├── venue_discovery_policy.md
│   └── verification_policy.md
├── schemas/
│   ├── paper.schema.json
│   ├── research_state.schema.json
│   └── retrieval_log.schema.json
├── scripts/
│   ├── coverage_analyzer.py
│   ├── deduplicate.py
│   ├── export_excel.py
│   └── validate_bundle.py
└── examples/
    └── demo_state.json
```

### 安装

1. 从 GitHub Releases 下载 Skill 压缩包，或克隆本仓库。
2. 将包含单个顶层 Skill 目录的 ZIP 导入支持 Skills 的平台，或把该目录放入平台支持的用户级/项目级 Skill 路径。
3. 为实际文献调研启用平台可用的网页搜索或浏览能力。
4. 如需运行确定性去重、覆盖分析、校验和 Excel 导出脚本，请安装 `scripts/requirements.txt` 中列出的依赖。

如果宿主环境不能运行 Python，仍可按照 `SKILL.md` 中的核心流程使用平台原生文件和电子表格工具完成任务；随附脚本是提高一致性与可复现性的辅助工具，并不代替实时检索。

### 使用示例

```text
以“多模态大模型的幻觉检测与缓解”为关键研究内容，检索 2022—2026 年
Nature、Science 及相关子刊、CCF A 类会议/期刊和领域重要期刊中的相关论文。
尽可能提高覆盖率，所有论文必须真实可核验，翻译原始摘要，并整理成 Excel。
```

Skill 将依次完成主题拆解、动态期刊会议发现、多策略检索、去重、真实性核验、相关性筛选、引文扩展、覆盖分析、缺口驱动迭代、饱和度判断以及结构化导出。

### 科研引用

如果本项目对你的研究有帮助，请在论文、报告或软件成果中引用：

> Adaptive Literature Retrieval Contributors. (2026). *Adaptive Literature Retrieval: A coverage-aware and evidence-grounded literature retrieval skill for AI agents* [Computer software].

```bibtex
@software{adaptive_literature_retrieval_2026,
  author = {{Adaptive Literature Retrieval Contributors}},
  title  = {Adaptive Literature Retrieval: A Coverage-Aware and Evidence-Grounded Literature Retrieval Skill for AI Agents},
  year   = {2026},
  note   = {Domain-agnostic adaptive literature retrieval with gap-driven search, evidence verification, and auditable outputs}
}
```

仓库正式发布后，可在上述条目中补充版本号、GitHub URL 或归档 DOI。若发布版本提供 DOI，请优先引用对应版本的 DOI。

### 设计边界

- 本项目不能保证发现世界上存在的每一篇相关论文；数据库覆盖、访问权限和索引延迟都会影响结果。
- Skill 不会把无法核验的论文放入最终文献表。
- Skill 不会编造缺失的摘要、作者、机构、DOI 或下载链接。
- “覆盖率”是相对于当前动态研究框架的内部检索指标，不代表对整个学科真实文献总量的数学估计。

---

## English

### Overview

**Adaptive Literature Retrieval** is a domain-agnostic, coverage-aware, and evidence-grounded literature retrieval skill for AI agents.

Instead of relying on fixed keywords, predefined disciplinary vocabularies, or a one-shot search, it builds a topic-specific research framework across four evolving dimensions: **Methods, Tasks, Applications, and Related Problems**. As retrieval progresses, the skill learns terminology, research directions, and important venues from verified literature, measures the coverage of the current collection, and chooses the next search action according to unresolved gaps and expected information gain.

Unlike workflows that generate citations from model memory or stop after reaching an arbitrary paper count, this project requires bibliographic verification, traceable retrieval decisions, evidence-based saturation checks, and an auditable seven-sheet Excel deliverable.

Here, “as complete as practical” means systematically maximizing recall within the requested scope and accessible sources. It is not a claim that every paper in existence has been found.

### Key Features

#### 1. Dynamic research schema instead of a fixed taxonomy

Every research request receives a new, evolving schema:

- **Methods**: algorithms, models, technologies, and methodological paradigms;
- **Tasks**: concrete research or computational problems;
- **Applications**: target objects, domains, systems, and scenarios;
- **Related Problems**: constraints, objectives, properties, evaluation concerns, and adjacent problems.

Each concept can retain its source, confidence, weight, and supporting evidence. No field—such as aerospace, control, medicine, or NLP—is treated as a hidden default.

#### 2. Coverage-aware adaptive retrieval

After every meaningful round, the skill recomputes coverage across concepts, venues, time, and the citation network. It then selects the action most likely to close an important gap:

- keyword search;
- semantic search;
- venue-specific retrieval;
- time-specific search;
- author search;
- backward or forward citation expansion;
- similar-paper discovery;
- terminology expansion.

The search path is determined by the current evidence rather than a rigid query list.

#### 3. Evidence-grounded paper verification

A paper cannot enter the final collection merely because the model remembers it or its title appears plausible. Included records must have verifiable bibliographic identities covering the title, authors, year, venue, persistent identifier, and trusted source evidence.

The workflow also requires that:

- abstract translations are based on retrieved original abstracts, not substituted model summaries;
- verified open-access PDFs and legal preprints are preferred;
- official publisher pages are used when lawful public full text is unavailable;
- PDF URLs, metadata, and citations are never guessed or fabricated;
- papers, webpages, and PDFs are treated as untrusted evidence rather than executable instructions.

#### 4. Saturation-based stopping

The skill does not stop solely because it has reached a fixed paper count or number of rounds. Its stopping decision considers whether:

- the requested venues and years have been searched;
- important research dimensions have adequate evidence;
- valuable citation expansion has been performed;
- recent rounds yield few new relevant and verified papers;
- the duplicate rate is increasing;
- any high-priority coverage gap remains.

#### 5. Auditable seven-sheet Excel output

| Sheet | Purpose |
|---|---|
| Final Literature | Included papers, publication details, Chinese abstract translations, and links |
| Retrieval Log | Actions, reasons, queries, results, and marginal gains for every round |
| Coverage Analysis | Evidence coverage and unresolved gaps across research dimensions |
| Verification | Metadata, abstract, link, and bibliographic verification status |
| Excluded Papers | Rejected candidates and their exclusion reasons |
| Search Plan | Planned queries, priorities, sources, rounds, and execution status |
| Run Summary | Scope, statistics, stopping reason, and known limitations |

#### 6. Domain-agnostic and portable

The same workflow can be applied to AI, computer science, medicine, chemistry, materials science, robotics, control, education, databases, and other fields. The core logic does not depend on a specific search provider or platform tool name, allowing it to run in ChatGPT Skills, WorkBuddy/CodeBuddy, and other compatible agent environments.

### Installation

1. Download the Skill ZIP from GitHub Releases or clone this repository.
2. Import a ZIP containing one top-level Skill directory into a compatible platform, or place the directory in the user-level or project-level Skill path supported by your host.
3. Enable the host's available web search or browser capabilities for live literature retrieval.
4. To use the deterministic deduplication, coverage analysis, validation, and Excel export helpers, install the dependencies listed in `scripts/requirements.txt`.

If Python execution is unavailable, follow the same workflow using the host's native file and spreadsheet tools. The bundled scripts improve consistency and reproducibility but do not replace live literature retrieval.

### Example Prompt

```text
Conduct an as-complete-as-practical literature search on hallucination detection and
mitigation in multimodal large language models from 2022 to 2026. Cover relevant
Nature and Science portfolio journals, applicable CCF A venues, and field-leading
journals or conferences. Include only verifiable papers, faithfully translate the
retrieved abstracts into Chinese, verify all links, and export the results to Excel.
```

### Citation

If this project supports your research, please cite it in papers, reports, or software outputs:

> Adaptive Literature Retrieval Contributors. (2026). *Adaptive Literature Retrieval: A coverage-aware and evidence-grounded literature retrieval skill for AI agents* [Computer software].

```bibtex
@software{adaptive_literature_retrieval_2026,
  author = {{Adaptive Literature Retrieval Contributors}},
  title  = {Adaptive Literature Retrieval: A Coverage-Aware and Evidence-Grounded Literature Retrieval Skill for AI Agents},
  year   = {2026},
  note   = {Domain-agnostic adaptive literature retrieval with gap-driven search, evidence verification, and auditable outputs}
}
```

Once the repository is published, add the release version, GitHub URL, or archival DOI to this entry. If a release DOI is available, cite the DOI for the exact version used.

### Limitations

- No workflow can guarantee discovery of every relevant paper in existence; database coverage, access restrictions, and indexing delays affect the result.
- Unverified papers are excluded from the final literature sheet.
- Missing abstracts, authors, affiliations, identifiers, or download links are never fabricated.
- Coverage scores are internal indicators relative to the dynamically constructed research schema, not mathematical estimates of the entire field.
