# 使用说明（先读这一份）

本工程把一本素材小说改写成原创换皮长篇，**单主题模式**：每本素材只产出一种皮，三阶段全自动接力。

```
素材小说.txt → 流程一·OUTLINE → 流程二·RESTRUCTURE → 流程三·WRITE → rewrite/ 上传商业平台
```

```powershell
$env:HTTPS_PROXY="http://127.0.0.1:7897"
$env:HTTP_PROXY="http://127.0.0.1:7897"
code
```

---

## 第一步·准备工作（5 分钟，只做一次）

### 1. 把素材丢根目录

把原作 txt 文件丢进项目根目录，任意名字（文件名不重要），编码 UTF-8 / GBK / GB18030 都能识别。例：

```
项目根目录/
├── 莽荒纪.txt              ← 你的素材
├── 使用说明.md
├── MANUAL_OUTLINE.md
├── ...
```

### 2. 主题暂不决定（设计的）

本工程**主题不预先决定**。`themes/` 目录目前为空，[theme.md](theme.md) 的 `active_theme:` 字段也是空的。

- **流程一 OUTLINE** 和 **流程二·步骤负一·A/B/C/D** 全部主题中立，可以直接跑
- **步骤负一·C** 跑完会产出 `theme_baseline.md`（你这本素材的量化基线）
- **此时会停下**，由你告诉 AI"跑 X 主题"（例如"跑末世""跑都市异能"）
- AI 据 `theme_baseline.md` 设计一份 `themes/theme_X.md`，回填 `active_theme:`，再继续步骤零

**同一工程不要中途切主题**——主题特异档案产物落到根目录直接覆盖，切了等于推倒重来。要做不同题材，把模板目录整份复制走当新工程跑。

详见 [theme.md](theme.md)"主题决定时点"和"主题设计规约"小节。

### 3.（可选）投喂网文风格样本

往 `external_style_samples/` 目录扔几篇你喜欢的网文风格样本，流程三创作时会作为 few-shot 参考让最终成品文风对齐。不放也能跑，质量略低。

### 4. 检查清单

启动前最后核一次：

- [ ] 素材 txt 在根目录
- [ ] `theme.md` 在（`active_theme:` 字段可空，跑到步骤负一·C 后再回填）
- [ ] `RULES.md` 在
- [ ] `polish_skill/anti_ai_style_rules.md` 在（流程三需要）

---

## 启动指令一览

**任何阶段都是同一个套路**：第一次启动用"执行"，会话上下文不够停下后开新会话用"继续"或"接续"，会自动从断点续作。

| 阶段 | 第一次启动指令 | 接力指令（重启会话用） |
|---|---|---|
| 流程一·单线 | `按 MANUAL_OUTLINE.md 执行` | `按 MANUAL_OUTLINE.md 继续` |
| 流程一·并行（A 段） | `按 MANUAL_OUTLINE_PARALLEL.md 执行，段 ID = A，负责章号范围 0001-0500` | `按 MANUAL_OUTLINE_PARALLEL.md 接续，段 ID = A` |
| 流程一·并行（B 段） | `按 MANUAL_OUTLINE_PARALLEL.md 执行，段 ID = B，负责章号范围 0501-1000` | `按 MANUAL_OUTLINE_PARALLEL.md 接续，段 ID = B` |
| 流程二 | `按 MANUAL_RESTRUCTURE.md 执行` | `按 MANUAL_RESTRUCTURE.md 继续` |
| 流程三 | `按 MANUAL_WRITE.md 执行` | `按 MANUAL_WRITE.md 继续` |

> 流程一·并行是把素材小说**按章号范围分段并行**蒸馏（加速流程一），不是多主题并行。本工程仍然是单主题。

### 通用说明

- 这些指令都会触发"自动判断进度"，不会重复已完成的工作，不会问你二次确认
- 多个会话之间靠落盘的进度档（`progress.md` / `restructure_progress.md` / `rewrite_index.txt` / `outline_rhythm/index.md` 等）传递状态
- 如果你想知道当前进度，可以问"现在跑到哪了"，会读对应阶段的进度档报告

---

## 流程一·OUTLINE（原作蒸馏，主题中立）

**目标**：把素材小说蒸馏成结构化档案，全程**保留原作名词不换皮**（换皮交给流程二）。

### 单线模式（最简单，慢）

启动指令：`按 MANUAL_OUTLINE.md 执行`

会自动按顺序做：

| 步骤 | 产物 | 备注 |
|---|---|---|
| 拆章 | `chapters/0001_第X章_*.txt`、`chapters_index.txt` | 一次性 |
| 建设定骨架 | `settings/index.md` + `settings/volXX.md` | 主线人物/势力/地点（10-20 个封顶） |
| 提原作专有名词 | `name_mappings/{characters,locations,factions,equipment,skills,misc}.md` | 按类别分文件 |
| 逐章总结大纲 | `outline/0001_第X章_*.txt` 每章一份 | 8 维度模板：场景/事件/冲突/打斗/情绪/互动/转折/目标字数 |
| 完成标记 | `outline/ALL_DONE.txt` + `MIGRATION_DONE.txt` | 触发流程二启动 |

**接力**：会话上下文不够时主动停下并写 `progress.md`，你重启会话说 `按 MANUAL_OUTLINE.md 继续` 即可。

**产能**：~100-150 章/天/会话。**1500 章规模 ~10-15 天**。

### 并行模式（加速，需要你管 2-3 个窗口）

适合素材规模大（>500 章）且想省墙钟时间。同时开 2-3 个会话窗口，每个跑一段：

```
窗口 1：按 MANUAL_OUTLINE_PARALLEL.md 执行，段 ID = A，负责章号范围 0001-0500
窗口 2：按 MANUAL_OUTLINE_PARALLEL.md 执行，段 ID = B，负责章号范围 0501-1000
窗口 3：按 MANUAL_OUTLINE_PARALLEL.md 执行，段 ID = C，负责章号范围 1001-1500
```

每窗口接力时只说段 ID：`按 MANUAL_OUTLINE_PARALLEL.md 接续，段 ID = A`

各段全部完成后，跑合并脚本（项目根目录）：

```powershell
python merge_parallel_outputs.py
```

详细规则见 [MANUAL_OUTLINE_PARALLEL.md](MANUAL_OUTLINE_PARALLEL.md)。**2 路并行 ~省一半墙钟**。

### 流程一完成标志

`outline/ALL_DONE.txt` 出现，意味着可以启动流程二。

---

## 流程二·RESTRUCTURE（重构大纲，分两段）

启动指令：`按 MANUAL_RESTRUCTURE.md 执行`（一句话从头到尾，自动判断子阶段）

接力指令：`按 MANUAL_RESTRUCTURE.md 继续`

### 步骤负一：四档蒸馏（**主题中立，从原作蒸馏，一次性**）

| 子步骤 | 产物 | 用途 |
|---|---|---|
| **A 故事弧** | `outline_arcs.md` + `outline_arc_patterns.md` | 识别全书 ~25 弧 + 抽象模式骨架（不可识别 IP） |
| **B 节奏蒸馏** | `outline_rhythm/chXXXX_YYYY.md`（按 100 章一段） | 每章一行 9 字段（弧/节奏/爽点/强度/钩子/借鉴度档位） |
| **C 主题校准基线** | `theme_baseline.md` | 蒸馏阶位数/章号分布/爽点频次/配角配比/文风量化值，**主题设计依据** |
| **D 避雷词库** | `forbidden_terms.md` + `forbidden_chars.txt` | 原作专有名词禁用表+拆字避雷字集（步骤一硬扫每章） |

**步骤负一·C 跑完后是你决定主题的关键节点**：

`theme_baseline.md` 出现后流程会停下，写 `NEED_THEME.txt` 报告等你指定主题。你打开 `theme_baseline.md` 看原作量化指标，告诉 AI 跑什么主题（例如"跑末世""跑都市异能"），AI 会据 baseline 设计一份 `themes/theme_X.md`（必备字段见 [theme.md](theme.md) "主题设计规约"小节，强制带"校准对照表"），回填 `theme.md` 的 `active_theme:`，再说 `按 MANUAL_RESTRUCTURE.md 继续` 接着跑步骤负一·D 和步骤零。

### 步骤零：主题转换（**主题特异，本工程一份**）

落到工程根目录，分 A/B/C 三个子步骤：

- **A/B 子步骤**·产 `settings_final/` + `name_mappings_final/`：
  - `settings_final/`：套主题的世界观/力量体系/势力/血肉档案（dialogue_voicing、combat_rhythm、emotion_curve、hook_patterns、character_arcs、foreshadowing_library、style_sample、inspiration_map）
  - `name_mappings_final/`：原作元素→新主题元素映射，受**单字零重叠 + 必须 3500 常用字 + 禁生僻字**约束，命名校验脚本强卡
- **C 子步骤**·产 `book_meta.md`（番茄风格元信息 + 封面 AI 提示词）：
  - **3 个候选书名**（6-12 字，按番茄读者偏好的句式）
  - **简介**（180-300 字，3 段结构，无破折号无强逻辑词，必带数字+旁观反应+钩子）
  - **卷名表**（每卷 4-8 字，与 outline_final 章节段对齐）
  - **番茄分类标签**（频道/大类/小类 + 5-8 个关键词标签）
  - **主角设定**（姓名/身份/金手指/目标，番茄作品页直接复制）
  - **封面 AI 提示词**（按主题自动选视觉风格，三套：即梦中文式 / Midjourney 英文 tag / Stable Diffusion 正反向提示词，含推荐参数和负面词）

**约 2-3 个会话**。

### 步骤一：按节奏改写（**主题特异，5 章一批增量**）

每批读：outline/ 本批±3 章 + `outline_rhythm/` 当前段 + settings_final/ + 弧档案 → 产出 `outline_final/XXXX.txt`，每章 ~3500 字（信息容量参考；WRITE 阶段会按番茄发表标准实际写 2000-2300 字）。

**借鉴度分级**（来自 outline_rhythm 第 9 字段）：
- 独特意象章 (~1%)：50% 借鉴，独特符号必须全新
- 高潮章 (~5%)：75% 借鉴节奏+爽感+冲突结构
- 中等章 (~70%)：85% 借鉴
- 日常章 (~25%)：95% 借鉴

每批改完硬扫 `forbidden_terms.md`，命中红线词报错重写。

**1500 章 ~30-40 个会话**。

### 流程二完成标志

`outline_final/ALL_DONE.txt` 出现，意味着可以启动流程三全量模式。

也可以**追赶模式并行**：流程二还在跑步骤一时，另开一个窗口启动流程三，会自动留 5 章安全缓冲跟在已 done 批后面。

---

## 流程三·WRITE（正文创作）

启动指令：`按 MANUAL_WRITE.md 执行`

接力指令：`按 MANUAL_WRITE.md 继续`

### 前置

- `settings_final/` 至少包含 `index.md` / `system_design.md` / `world_overview.md`
- `outline_final/` 至少有一批章节文件
- 全量模式额外需要：`outline_final/ALL_DONE.txt` 存在

### 单章流程（自动循环）

每章会做：

1. **读必要档案**（cache 友好）：settings_final/ 各档 + outline_final/ 本章 + characters_state/ 出场角色 + 上一章 summaries/
2. **创作初稿** **2000-2300 字**（番茄发表标准），遵守反 AI 风规范：
   - 70%+ 句子≤15 字
   - 禁强逻辑词（于是/因此/所以/然后/首先/其次/最后）
   - 禁套路心理（心如刀绞/五味杂陈/激动得说不出话）
   - 心理用动作/环境/短促独白替代
   - 打斗用感官冲击+短句节奏+局部特写
3. **自我修改 7 项**（情绪命中/重复表达/对话个性/情绪表达/开头变化/意外细节/反 AI 风自检）
4. **WRITE_SKILL 二轮修订**（用 `polish_skill/anti_ai_style_rules.md` + `external_style_samples/` 再扫一遍）
5. **保存**到 `rewrite/XXXX.txt`
6. **生成摘要** → `summaries/XXXX.txt`
7. **更新角色状态** → `characters_state/<角色>.md`
8. **追加进度** → `rewrite_index.txt`
9. 自动写下一章

### 自动质检节点

- **每 10 章**：高频词清理 / 对话区分度 / 开头模式 / 节奏 / **AI 风密度自查**（>40% 强制返工）
- **每 50 章**：设定一致性自查 → `check_report.md`，发现问题自动修复对应章

### 模型推荐

- **强推 Opus 4.7**：风格控制强，AI 风识别率显著低于 Sonnet
- 备选 Sonnet 4.6：成本低 5 倍，但 AI 风识别率高 20%+

### 产能

- ~10-15 章/会话（受上下文限制）
- 1500 章 ~100-150 个会话
- 用户体感 1-2 个月（每天若干次启动接力）

### 流程三完成标志

`rewrite/ALL_DONE.txt` 出现，整本完成。`rewrite/` 下每章 **2000-2300 字**，命中番茄发表区间，可直接上传番茄/起点不用拆短。

---

## 关键决策点（你需要拍板的几个时刻）

| 时点 | 决策 | 输入 |
|---|---|---|
| 流程二·步骤负一·C 后 | 用现成主题还是自造/调整主题字段对齐 baseline | `theme_baseline.md`（原作量化基线） |
| 流程三启动前 | 是否投喂外部文风样本 | 你看过的网文，扔进 `external_style_samples/` |
| 流程三每 50 章 | 是否人工干预质量问题 | `check_report.md` |

---

## 续写控制·什么时候自动停下（看 [SESSION_CONTROL.md](SESSION_CONTROL.md)）

每次会话启动后会跑到三层信号任一触发才停。**单会话尽可能跨多个阶段往下推进**，这是"全自动模式"的核心。

### 三层停下信号

| 层 | 信号 | 处置 |
|---|---|---|
| **预算** | 累计 token 用到 800K（1M 上下文的 80%）+ 预测下个单位会爆 | 完成当前单位后停 |
| **质量** | 重复率 ≥3 次 / 字段缺失 / WRITE 字数越出 2000-2300 区间 | 完成当前单位后停 |
| **对话** | 你插话 / 工具失败 ≥2 次 / BLOCKED.txt 出现 / 你说"停" | 立刻或完成当前单位后停 |

每隔一段（流程一每 10 章 / 步骤负一·B 每 100 章 / 步骤一每 5 批 / WRITE 每 10 章）做一次自我评估，结果写到 `quality_log.md`，跨会话累积可回看。

### 跨阶段自动衔接（你不用管）

理想情况下，单会话从流程一可一路自动跑到流程二·步骤一首批，中间不停下报告：

```
流程一 → 流程二·负一·A → B → C → D → 零 → 一·首批 → ...
```

每个阶段产物落盘 = 自动触发下一阶段的前置检测，预算够就继续。

**唯一的例外**：流程二·步骤一 → 流程三 之间不在单会话切。
- 步骤一全部完成（出现 `outline_final/ALL_DONE.txt`）→ 自动切流程三**全量模式**
- 步骤一未完成想跑流程三**追赶模式** → 必须开**新窗口**启动（避免单会话同时背两份 manual 的规则负担拖累质量）

### 中断与恢复

会话停下时会写一份 `NEED_NEW_SESSION.txt`（工程根目录），里面包含：
- 本会话经过的所有阶段
- 最终停在哪一阶段哪一章
- 触发停下的具体信号
- 下次启动应该说什么指令

你看 NEED_NEW_SESSION.txt 末尾那行"下次启动指令"，复制粘贴即可继续。

**主动暂停**：直接关掉会话或对我说"停"，所有产物已落盘，下次重启会话用同样的指令续作。

**回滚某些章**：删掉 `rewrite/<想重写的章号>.txt` 和 `summaries/<同章号>.txt`，重启 WRITE 会话会自动从最小缺失章号续作。

---

## 故障处理（关键文件出现要看）

| 文件出现 | 含义 | 处理 |
|---|---|---|
| `BLOCKED.txt` | 前置条件不满足 | 看文件内的原因说明，补齐前置后重启 |
| `NEED_THEME.txt` | 步骤负一·C 完成，等你决定主题 | 看 `theme_baseline.md` 后告诉 AI"跑 X 主题"，AI 现写 `themes/theme_X.md` 并回填 `theme.md` 的 `active_theme:` |
| `check_report.md` 有问题 | 质量自查发现一致性问题 | 自动修复对应章，无需干预 |
| `rewrite_inconsistencies.md` | outline_final 出现 settings_final 没定义的元素 | 停下报告，由你裁决：补 settings_final 还是改 outline_final |
| `naming_check_report.md` 失败 | 新名命中避雷字集或生僻字 | 步骤零自动重起名重跑校验 |
| `NEED_NEW_SESSION.txt` | 上下文不够主动停下 | 用对应阶段的"继续"指令重启会话 |

---

## 全程时间估算（1500 章规模）

| 阶段 | 单会话产能 | 总会话数 | 用户挂机时长 |
|---|---|---|---|
| 流程一（单线） | 100-150 章/会话 | 10-15 | ~1-2 周 |
| 流程一（并行 2 路） | 同上 | 同上但分 2 窗口 | ~半周-1 周 |
| 流程二·步骤负一 | 全部子步骤 ~5-8 会话 | 5-8 | 1-2 天 |
| 流程二·步骤零 | 一次性 | 2-3 | 1 天 |
| 流程二·步骤一 | 50 章/会话左右 | 30-40 | ~1 周 |
| 流程三 | 10-15 章/会话 | 100-150 | 1-2 个月（每天接力几次） |

总计：**约 1.5-2.5 个月用户体感**（包含每天空闲时启动接力）。

---

## 一句话起手

把素材丢根目录、然后告诉我：

> 按 MANUAL_OUTLINE.md 执行

剩下的我接管，跑到步骤负一·C 后会停下问你跑什么主题。
