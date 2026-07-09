# 原创小说 · 逐章创作执行手册（主题中立）

## 本文档说明
这是 Claude Code 的唯一执行依据。
本项目是一部原创长篇小说的创作工程，**主题由 `theme.md` 的 `active_theme:` 字段决定**（启动 WRITE 前必须已选定，由 RESTRUCTURE 步骤负一·C 后停下让用户指定，详见 [MANUAL_RESTRUCTURE.md](MANUAL_RESTRUCTURE.md) 5.5 主题选定门）。本工程是**单主题模式**：每本素材小说只产出一种皮，不开多窗口并行不同题材。
世界观设定已完成（见 settings_final/，由 MANUAL_RESTRUCTURE.md 步骤零产出，是 WRITE 的硬前置）。
故事大纲来源于 outline_final/（由 MANUAL_RESTRUCTURE.md 步骤一增量产出）。**WRITE 支持两种模式**：
- **全量模式**：outline_final/ 已完整（`ALL_DONE.txt` 存在）→ 一直写到末章
- **追赶模式**：outline_final/ 仍在被 RESTRUCTURE 步骤一增量写入 → WRITE 在另一个窗口并行启动，留 5 章安全缓冲跟在 RESTRUCTURE 已 done 批后面（详见"自动判断当前进度"）

Claude Code 的任务是按大纲逐章展开创作。
读取本文档后从当前进度开始，按章逐步执行。
执行前先读取 `RULES.md`、`theme.md`（套用文风规范、命名风格、典型爽点）和 **`SESSION_CONTROL.md`**（续写控制规则，决定本会话停下时机）。

**停下时机全部按 [SESSION_CONTROL.md](SESSION_CONTROL.md) 规则**：
- 预算硬线 800K tokens；`remaining_capacity < 1.5 章` 时完成当前章后停
- 质量信号（重复率 / 字段完整度 / 字数越出 2000-2300 区间）任一触发即降级到完成当前章后停
- 自我评估每 10 章一次（与下方"每 10 章风格自查"合并），落盘到 `quality_log.md`
- 用户插话 / 工具失败 / `rewrite_inconsistencies.md` 出现 → 立刻或完成当前章后停
- 停下时强制写 `NEED_NEW_SESSION.txt`（模板见 SESSION_CONTROL.md §6），不再用本手册下方"上下文不够时"那段（已被 SESSION_CONTROL.md 覆盖）

**本手册主体是主题中立的**。所有主题特异内容从 settings_final/（已套 theme.md 的产物）和 theme.md 指向的主题文件的"文风规范"章节读取。

---

## 路径约定（单主题模式）

**主题中立档案**（项目根目录，只读不写）：

```
项目根目录/
├── theme.md / themes/             ← 主题选定与配置
├── RULES.md                       ← 写作规则
├── polish_skill/                  ← 阶段四 WRITE_SKILL 配套素材（反 AI 风规则）
├── external_style_samples/        ← 外部网文风格样本
├── chapters/ settings/ name_mappings/ outline/   ← 流程一原始产物
├── outline_arcs.md outline_arc_patterns.md       ← 步骤负一·A
├── outline_rhythm/                ← 步骤负一·B
├── theme_baseline.md              ← 步骤负一·C
├── forbidden_terms.md forbidden_chars.txt        ← 步骤负一·D
└── check_naming.py check_originality.py          ← 校验脚本
```

**主题特异档案**（也直接落工程根目录，本工程一份）：

```
项目根目录/
├── settings_final/             ← RESTRUCTURE 步骤零产出（WRITE 只读）
├── name_mappings_final/        ← RESTRUCTURE 步骤零产出（WRITE 不读）
├── outline_final/              ← RESTRUCTURE 步骤一产出（WRITE 必读，每章一份）
├── restructure_progress.md     ← 追赶模式下 WRITE 必读
├── rewrite/                    ← WRITE 创作正文最终版（每章一份，已 WRITE_SKILL 修订）
│   └── ALL_DONE.txt            ← 整本书完成标志（仅全量模式可创建）
├── summaries/                  ← 每章摘要（WRITE 产出）
├── characters_state/           ← 角色动态状态（WRITE 产出）
│   ├── index.md
│   └── ...
├── rewrite_index.txt           ← 创作进度索引
├── check_report.md             ← 自查报告
└── rewrite_inconsistencies.md  ← outline_final 与 settings_final 不一致记录
```

**主题选择**：WRITE 启动前主题必须已选定（`active_theme:` 已指向有效 `themes/theme_xxx.md`）；未选定 → 停下报告（理论上不该发生，因 RESTRUCTURE 步骤零的前置就要求主题已选定）。**同一工程不要中途切主题**——产物已落根目录直接覆盖，切了等于推倒重来。要做不同题材，把模板目录整份复制走当新工程跑。

## 流程位置

```
阶段一 OUTLINE → 阶段二 RESTRUCTURE → 【阶段三·本手册】 → 上传番茄
```

**本手册的单章流程内嵌 WRITE_SKILL 修订**：每章 Claude Code 生成初稿后，**Claude Code 自己读 WRITE_SKILL 文档（`polish_skill/anti_ai_style_rules.md` + `external_style_samples/`）按规则修订一遍**，再保存到 rewrite/。

整个流程在 Claude Code 内部完成，**不需要切换到网页端**。

`rewrite/` 目录的每一章都是**已修订版**，可以直接上传商业平台。

**本手册完成 = `rewrite/ALL_DONE.txt` 出现**（每章都经过 WRITE_SKILL 修订）。

---

## 模型推荐（重要：影响成品质量与 AI 检测通过率）

| 任务 | 推荐模型 | 理由 |
|---|---|---|
| **阶段三正文创作（本手册）** | **Opus 4.7** | 风格控制能力强，能更好遵循 few-shot 示例和反 AI 风规则；写战斗 / 心理 / 对白比 Sonnet 自然得多 |
| 备选 | Sonnet 4.6 | 成本低 5 倍，但 AI 风识别率比 Opus 高 20%+ |

**重要**：本手册的"反 AI 风规范"和"自查 7 项"在 Opus 上效果显著，在 Sonnet 上效果有限。如果你打算上传商业平台（番茄/起点），强烈建议用 Opus。

---

## 创作红线（全程强制）

1. 只使用 settings_final/ 中的名词体系。
2. 100% 原创表达：对白、描写、叙事全部原创。
3. 纯正文输出：只输出正文，不加解释、注释、提问。
4. 文风统一：全书保持一致的叙事视角、语言风格和节奏。
5. 本项目只读取 settings_final/、outline_final/、summaries/、characters_state/、rewrite/（仅自查阶段需要回读）、polish_skill/、external_style_samples/；**严禁读取 setting.md / name_mapping.md / settings/ / name_mappings/ / outline/ 等原作衍生档案，以及 name_mappings_final/（rewrite 阶段不需要）**。

---

## 自动判断当前进度

读取本文档后，按以下顺序检查：

0. **先确认主题**：读 `theme.md` 取 `active_theme:` 字段，打开该文件确认存在且字段齐全。`theme.md` 缺失或 `active_theme:` 为空 / 注释 / 缺失 → 停下报告"主题未选定，请先在 RESTRUCTURE 流程跑到步骤负一·C 后由用户指定主题再回头跑 WRITE"。启动后第一行输出当前主题文件名作为确认。
1. `settings_final/` 目录是否存在且包含 `index.md`、`system_design.md`、`world_overview.md` 至少这三个文件？
   - 不满足 → 停下来告诉用户："缺少 settings_final/，请提供（由 MANUAL_RESTRUCTURE.md 流程产出）。"
2. `outline_final/` 目录是否存在且**至少有一个章节文件**？
   - 不存在 / 空 → 停下来告诉用户："outline_final/ 尚未启动，请先完成 MANUAL_RESTRUCTURE.md 步骤负一/零，并至少跑出一批步骤一章节。"
3. **判定运行模式**（决定 WRITE 上界 `WRITE_LIMIT`）：
   - **全量模式**：`outline_final/ALL_DONE.txt` 存在 → RESTRUCTURE 已完成，`WRITE_LIMIT = ∞`（写到 outline_final/ 末章）
   - **追赶模式**：`ALL_DONE.txt` 不存在但 outline_final/ 有文件 → RESTRUCTURE 仍在跑步骤一，与 WRITE 并行：
     - 读 `restructure_progress.md` 末尾，找**最后一行 status=done 的批**，取该批章号区间的末章号 `R`
     - `WRITE_LIMIT = R - 5`（留 5 章安全缓冲，覆盖一整批 + 自查重写窗口；批是 5 章一批，缓冲一整批可避免读到 Round 2 修订中的章节）
     - 若 `R - 5 < 1`（RESTRUCTURE 第一批还没 done）→ 停下："RESTRUCTURE 步骤一首批未完成，等首批 done 后再启动 WRITE。"
     - 若 `restructure_progress.md` 不存在 → 停下："restructure_progress.md 缺失，无法判定 RESTRUCTURE 进度，先确认步骤一已正常启动。"
4. `rewrite/` 目录下有没有文件？
   - 没有 → 创建 rewrite/ 和 summaries/ 目录，从第 1 章开始创作
   - 有 → 找到最后一个编号 `W`，从 `W + 1` 继续创作
5. **追赶边界检查**（每章开始前）：
   - 若**下一章章号 `> WRITE_LIMIT`** → 停下报告："✅ 已追上 RESTRUCTURE 安全边界（已写到 ch{W}，RESTRUCTURE 最末 done 批=ch{R}，缓冲=5）。等 RESTRUCTURE 再推进若干批后重启 WRITE 续作。"
   - 若**下一章 `outline_final/XXXX.txt` 不存在**（理论上不应发生，因为 WRITE_LIMIT ≤ R）→ 停下报告并写 `rewrite_inconsistencies.md` 记录。

前置条件全部满足后直接开始，不提问，不等确认。

**追赶模式注意事项**：
- 不要试图自己跑 RESTRUCTURE 的步骤一去推进 outline_final/ ， 那是另一个会话/窗口的职责。WRITE 只消费、不生产 outline_final/。
- 每次会话启动都重读 `restructure_progress.md` 重新算 `WRITE_LIMIT`；不要缓存上次会话的值。

---

## 逐章创作流程

### 单章流程

对每一章，按以下顺序执行：

1. **读取**（按章号映射所属卷，只 load 必要的子集）：

   **创作核心档案**：
   - `settings_final/index.md`（章节段映射 + 主线人物索引）
   - `settings_final/system_design.md`（力量阶位 / 装备 / 资源体系）
   - `settings_final/world_overview.md`（世界观 + 势力政治张力 + 独特符号资产）
   - `settings_final/master_arc.md`（全书主线骨架，含本章主角力量阶段与心理状态）
   - `settings_final/foreshadowing_library.md`（跨章伏笔规划库，**必读**：判断本章是否要埋/引爆伏笔）
   - `settings_final/character_arcs.md`（主线配角弧线档案，**必读**：判断本章配角是否处在弧线关键节点）
   - `settings_final/chXXXX_YYYY.md`：本章所属章节段（按章号 / 100 整除算所属段，例如 ch0537 → ch0501_0600.md）
   - `characters_state/index.md`（全体角色简表）
   - `characters_state/<本章登场角色>.md`：仅 load 本章 outline 中点名出现的角色（按 outline_final/XXXX 中【人物互动】节确定）
   - 上一章摘要 `summaries/XXXX.txt`（第一章跳过）
   - `outline_final/XXXX.txt`（对应章节的大纲，注意其中的"读者情绪目标"和"爽点类型"）

   **WRITE_SKILL 修订核心档案**（每章修订必读）：
   - `polish_skill/anti_ai_style_rules.md`（反 AI 风规则总集）
   - `external_style_samples/` 全部样本（few-shot 风格参考）

   **不读** `name_mappings_final/`（术语已固化在 settings_final/ 与 outline_final/ 中，无需再查映射）。
   **不读** 其他章节段的 `chXXXX_YYYY.md`（除非 outline 明确引用了跨段角色/势力，且该角色不在 `index.md` 主线人物索引中）。

2. **创作初稿**：

   **基础要求**：
   - 按大纲的情节走向展开完整章节
   - **字数硬区间 2000-2300 字**（番茄发表标准，区间硬卡：<2000 或 >2300 即异常需返工）。⚠️ outline_final/ 标的【目标字数】~3500 是大纲信息容量参考，**不作 WRITE 字数依据**——WRITE 一律按 2000-2300 落地。outline_final/ 已按 **2:1 合并对应** outline/（新章 N ←→ 原作 2N-1, 2N，详见 [MANUAL_RESTRUCTURE.md 顶部"重要更新·约束 A"](MANUAL_RESTRUCTURE.md)），信息密度因合并翻倍——正文按番茄风格把这翻倍的密度压进 2000-2300 字（见 [WRITING_PARADIGM.md](WRITING_PARADIGM.md)）。该区间已是番茄发表最优长度，无需再拆短。
   - **必须命中大纲中的"读者情绪目标"**：如果目标是"燃"，全章节奏要快、冲突要激烈；如果目标是"心疼"，要铺垫情感细节
   - **如果有爽点类型，必须写出爽感**：打脸要写旁观者的震惊、升级要写系统提示刷屏、装备要写品质光芒
   - 使用 settings_final/ 中的名词体系
   - 参考 characters_state/<角色>.md 中角色的当前情绪和状态来写，不能和前文矛盾
   - 100% 原创对白，对话要体现角色个性
   - 章节标题使用大纲中拟定的标题
   - 如果字数不够，增加环境描写、心理刻画、对话互动来充实，不要注水凑字

   **【反 AI 风规范】（强制，决定能否过 AI 检测）**：

   AI 默认会写出"AI 风"特征文字，被网文平台 AI 检测识别。本规范强制规避以下问题：

   **句式层面**：
   - ❌ 禁用强逻辑连接词：「于是」「因此」「所以」「然后」「首先」「其次」「最后」（除非对白中口语化使用）
   - ❌ 禁用三段并列结构：「他既……又……还……」「她不仅……而且……更是……」
   - ❌ **禁用破折号 ——**：所有场景一律禁用，是 AI 写作的高识别特征。需要停顿/转折/补充用「，」「。」「：」「（）」替代
   - ❌ **禁用比喻句(全类)**:正文不准写比喻句,**明喻/暗喻/借喻全禁**。明喻("像 X 一样""如同 X""仿佛 X""宛如 X""犹如 X""恰似 X""好像 X""X 似的""像 X")、暗喻("是 X""成了 X""化作 X""变成 X"中把人/事/物当成另一物的)、借喻(直接用喻体替本体)统统改写。连带禁字"像/如/如同/仿佛/宛如/犹如/恰似/好像/似的/一样"在正文中不出现(表"相同"的"一样"用"相同/同款/同等"替代)。改用:① 直接动词具象动作 ② 直接写本体物理细节 ③ 数据/具体参照物 ④ 五感直写(色温/分贝/气味/温度/质感)。判定:写完扫一遍,任何把 A 跟 B 拉到一起说的句子都是比喻,改掉。
   - ❌ **禁用程度副词**:正文禁「很 / 非常 / 超级 / 十分 / 极其 / 极为 / 极度 / 特别 / 相当 / 异常 / 格外 / 分外 / 万分 / 无比 / 极 / 忒 / 贼 / 巨」。对白每角色每章 ≤ 2 次。改用具象动作 / 数据 / 五感 / 后果 / 直接删。「他很愤怒」→「指节捏白,牙关咯咯」。
   - ❌ 禁止超长句:单句不超过 35 字,超过必拆短(旧 25 字限制已由 [WRITING_PARADIGM.md](WRITING_PARADIGM.md) 放宽)
   - ✅ **句长平均 18-30 字,长短交替,不刻意凑短**(旧"70% ≤ 15 字"作废,详见 WRITING_PARADIGM §一·原则 3 与 §七变更声明)
   - ✅ **段落默认 3-6 句**(旧"每段 ≤ 3 句"作废),"1 段 1 句"只用于爆点段,全章 ≤ 3 处
   - ✅ **信息密度优先**:每段 ≥ 2 个信息点,纯氛围/纯铺垫/纯心理段不允许独立成段(详见 WRITING_PARADIGM §一·原则 1 + §二技法)
   - ✅ **爽点前置**:章节爽点出现在前 1/3,场景一开场先甩结果,段首 1-2 句直接给冲击(详见 WRITING_PARADIGM §一·原则 2 + §三尺度)

   **心理描写层面**：
   - ❌ 禁用套路心理表达：「他感到愤怒」「心如刀绞」「激动得说不出话」「五味杂陈」「心里五味陈杂」「不可置信」
   - ❌ 禁用「他/她想道：……」式直接心理叙述（大段使用）
   - ❌ **禁程度副词修饰情绪**:「非常愤怒 / 很失落 / 超级紧张」改用动作 / 生理反应 / 环境异变
   - ✅ **心理描写用动作 / 表情 / 环境 / 短促独白替代**
     - 错误：「陆昭感到非常愤怒」
     - 正确：「陆昭的指节嘎吱作响。窗外那盏路灯，无端炸了。」

   **叙事层面**：
   - ❌ 禁用客观全知视角解释（"由于…的原因，主角……"）
   - ❌ 禁用过度精确的描述（"他用力 7.5 分的劲道挥出一拳"）
   - ✅ 主角视角 + 内心独白 + 即时感官（看到、听到、闻到）
   - ✅ 信息密度有起伏：高潮段密集、缓冲段稀疏

   **人类化痕迹**：
   - ✅ 适量加入口语化表达：「呵」「啧」「卧槽」「我去」（角色对白和心理）
   - ✅ 适量加入短句感叹：「快。再快。还要更快。」
   - ✅ 关键反应用残句：「不对……这不对！」（不强求完整句）

   **打斗场面专项**：
   - ❌ 禁用招式精确描述（"他用 0.3 秒挥出剑光"）
   - ❌ 禁用平铺直叙的招式罗列（"他先用 A，然后用 B，最后用 C"）
   - ✅ 用感官冲击 + 短句节奏 + 局部特写
     - 错误：「他挥剑刺向对方，剑光闪烁，速度极快」
     - 正确：「剑光只是一线。  
       一线白。  
       对方的喉结，已经多了一个洞。」

   **【风格样本参考】**：
   读取 `settings_final/style_sample.md`（来自原作 outline 的文风样本）+ `external_style_samples/` 目录（如果存在，外部网文风格样本）。**模仿这些样本的句长、节奏、心理密度、对白方式**。

3. **自我修改**（必做，不可跳过）：
   回读初稿，按以下 **7 项**逐条检查并修改：

   - **① 情绪目标命中**：读完这章，读者是否会产生大纲要求的情绪？如果没有，找到薄弱点加强
   - **② 重复表达**：有没有重复的形容词、比喻、句式？找到就换掉，同一个词在一章内不超过 2 次
   - **③ 对话个性**：每个角色说话方式是否有区别？按 settings_final/volXX.md 和 characters_state/<角色>.md 中的角色性格调整语气、用词、句长
   - **④ 情绪表达**：是"告诉读者情绪"还是"让读者感受到情绪"？把"他感到愤怒"改成用动作、表情、环境来体现愤怒
   - **⑤ 开头变化**：本章开头方式是否和前一章不同？如果相同，换一种方式（环境切入/对话开场/动作开场/内心独白/倒叙悬念/配角视角，轮换使用）
   - **⑥ 意外细节**：有没有至少一个让读者意想不到的小细节或小反转？没有就加一个
   - **⑦【新增·反 AI 风自检】**：通读全章，自问以下 7 个问题，任一个"是"则必须改：
     - 是否出现了「于是 / 因此 / 所以 / 然后 / 首先 / 其次 / 最后」等强逻辑词？（对白除外）
     - 是否出现了「他感到 / 心如刀绞 / 激动得说不出话 / 五味杂陈 / 不可置信」等套路心理？
     - 是否出现了三段并列结构（"既……又……还……"）？
     - **是否出现了破折号 `——`？**（任何场景都禁，发现即改）
     - **是否出现了任何比喻句?**(明喻/暗喻/借喻全禁。扫禁字"像/如/如同/仿佛/宛如/犹如/恰似/好像/似的/一样",出现即改;再扫"是 X""成了 X""化作 X""变成 X"句式,把人/事/物当成另一物来说的,改成具象动作或五感直写)
     - **是否出现「很 / 非常 / 超级 / 十分 / 极其」等程度副词?**(对白除外。出现即改或删)
     - 是否出现超过 35 字的超长句?(旧 25 字限制已由 WRITING_PARADIGM.md 放宽到 35)
     - **是否每段至少 2 个信息点?有没有纯氛围/纯铺垫的空段?**(WRITING_PARADIGM §一·原则 1)
     - **全章爽点是否在前 1/3 出现?段首是否直接给冲击?**(WRITING_PARADIGM §一·原则 2)
     - **是否出现"1 段 1 句"超过 3 处?**(只允许爆点段使用,全章 ≤ 3 处。WRITING_PARADIGM §一·原则 3)
     - **整体读起来像不像 AI 写的?**(信任直觉。如果读起来"太规整""太逻辑""太精确",就是 AI 风)
     - **整体读起来累不累?**(网文读者要的是吞噬感,不是诗歌朗诵。一行一句、大量空白、读两章就想划走 = 不合格)

   修改完成后，进入步骤 4 的 WRITE_SKILL 修订。

4. **WRITE_SKILL 修订（必要步骤，Claude Code 自己执行）**：

   读取 `polish_skill/anti_ai_style_rules.md` 和 `external_style_samples/` 全部样本，对刚才自我修改后的章节做**第二轮修订**，专门处理 AI 风。

   **修订原则**：
   - 严格遵守 `anti_ai_style_rules.md` 中的所有规则（短句、不用强逻辑词、不用套路心理表达等）
   - 模仿 `external_style_samples/` 中样本的句长、节奏、心理密度、对白方式
   - **保留原章的所有专有名词、情节、人物关系、对白核心意思不变**
   - **只改文风，不改内容**

   **修订流程**：
   1. 通读自我修改后的章节，识别 AI 风密集区域
   2. 优先改：心理描写套路 + 长句 + 强逻辑词 + 招式罗列
   3. 按 `anti_ai_style_rules.md` 的"自检清单"逐项核
   4. 不通过的继续改，直到清单全过

   **修订完成自检**(必做,与 [WRITING_PARADIGM.md §六](WRITING_PARADIGM.md) 同步):

   **网文向范式**:
   - [ ] **每段 ≥ 2 个信息点?**没有纯氛围/纯铺垫/纯心理的独立空段?
   - [ ] **段落默认 3-6 句**?没有连续大段都是"1 句 1 段"的碎片化?
   - [ ] **"1 段 1 句"全章 ≤ 3 处**?(只允许爆点段使用)
   - [ ] **句长平均 18-30 字**?没有大量 ≤ 10 字的强行短句?
   - [ ] **全章爽点在前 1/3 出现**?章节开头有钩子?段首 1-2 句给冲击?

   **反 AI 风硬禁(不可放宽)**:
   - [ ] 没有「于是 / 因此 / 所以」等强逻辑词?
   - [ ] 没有套路心理表达(心如刀绞 / 五味杂陈等)?
   - [ ] **没有破折号 `——`?**
   - [ ] **没有任何比喻句?**(明喻/暗喻/借喻全禁。扫禁字"像/如/如同/仿佛/宛如/犹如/恰似/好像/似的/一样"应零出现;暗喻句式"是 X / 成了 X / 化作 X"也不能把人/事/物当成另一物)
   - [ ] **没有程度副词「很 / 非常 / 超级 / 十分 / 极其」等?**(正文零出现;对白每角色每章 ≤ 2 次)
   - [ ] 没有超长句(> 35 字)?
   - [ ] 打斗场面具象动作 + 五感直写 + 数据参照物?
   - [ ] 对白口语化?

   **整体直觉**:
   - [ ] 自己读一遍,是"小说"而不是"诗歌"?
   - [ ] 一口气读完不累?中间没有想划走的位置?
   - [ ] 番茄/起点读者扫一眼会停下来读?

   全部通过 → 进入步骤 5。任一不通过 → 重改。

5. **保存**到 `rewrite/XXXX.txt`（UTF-8，已是 WRITE_SKILL 修订后的最终版）

6. **生成摘要**（200 字以内）保存到 `summaries/XXXX.txt`
   内容：本章主要事件、人物行动、剧情推进位置

7. **更新 `characters_state/` 目录**：
   根据本章内容，更新所有出场角色的状态文件。每个角色一个独立文件 `characters_state/<角色名>.md`。字段名按 `theme.md` 的"力量阶位序列"字段命名（LitRPG 用"等级/装备/技能"，宇宙用"文明等级/星舰/规则碎片"等），通用模板：
   ```
   # [角色名]
   当前阶位：[按 theme.md 力量阶位序列]
   核心能力变化：[本章新获得或失去的关键能力/装备/资源]
   情绪状态：[当前心理状态，用具体描述不用抽象词]
   人际关系变化：[与谁的关系发生了什么变化]
   当前目标：[接下来要做什么]
   未解决的心结/伏笔：[还没解决的问题]
   最近3章关键经历：[简述]
   ```
   - 只更新本章有变化的角色，没出场的不动
   - 第一次出现新角色时创建对应文件，并在 `characters_state/index.md` 追加一行简表
   - `characters_state/index.md` 永远是全角色一览（角色名 | 当前阶位 | 身份 | 当前目标 | 文件名）

8. **更新 `rewrite_index.txt`**：
   每完成一章，在 `rewrite_index.txt` 追加一行，格式：
   ```
   编号  |  字数  |  章节标题
   ```
   文件开头保留汇总信息（已完成章数、已完成总字数），每次更新时刷新汇总。

9. **自动继续下一章**，不汇报、不提问、不等待。

### 每 10 章风格自查
每完成 10 章，暂停创作，回读这 10 章执行以下检查并修正：

1. **高频词清理**：统计这 10 章中出现超过 5 次的形容词、比喻、描写句式，全部替换为不同的表达。
2. **对话区分度**：抽取不同角色的对话，确认能仅凭说话方式判断是谁在说。不能区分的，按角色性格改写。
3. **开头模式检查**：确认这 10 章的开头方式至少用了 4 种不同类型，有连续重复的就改掉。
4. **节奏检查**：确认这 10 章中有高潮有缓冲，不是 10 章全紧张或全平淡。
5. **【新增·AI 风密度自查】**：抽查这 10 章中每章随机 3 段（共 30 段），评估"读起来像 AI 写的"段落比例：
   - 比例 < 20% → 通过
   - 比例 20-40% → 标记可疑章节，下次写作主动避免相似句式
   - 比例 > 40% → **强制返回这些章节按反 AI 风规范重写**

修正完成后，输出一行："✅ 已完成第X章到第Y章，累计 XX,XXX 字，风格自查已修正，AI 风密度: X%"，然后继续。

### 上下文不够时（已被 SESSION_CONTROL.md 接管）

完整规则见 [SESSION_CONTROL.md](SESSION_CONTROL.md) §1（预算预测）+ §6（NEED_NEW_SESSION.txt 模板）。本节保留是为了向后兼容，不再独立定义条件，按 SESSION_CONTROL.md 的预算预测在 `remaining_capacity < 1.5 章` 时主动停下，写 `NEED_NEW_SESSION.txt` 后报告用户。

---

## 自查流程

每 50 章自动执行一次：

### 一、设定一致性自查
- 人物名字是否前后一致
- 力量等级是否合理
- 人物性格是否一致
- 剧情逻辑是否自洽
- 地名、组织名是否前后统一

### 二、排版自查
- 章节标题是否清晰
- 段落分段是否舒适

保存到 `check_report.md`。
发现问题自动修正对应章节，覆盖保存。
修正完成后继续创作。

---

## 完成与异常处理

### 全部完成（仅全量模式触发）
当 `outline_final/ALL_DONE.txt` 存在 **且** outline_final/ 中的大纲全部创作完毕：
- 创建文件 `rewrite/ALL_DONE.txt`，内容写"全部创作完毕，共 X 章（每章已经 WRITE_SKILL 修订）。"
- 输出："✅ 全部完成，共 X 章。rewrite/ 已是最终版（每章经过反 AI 风修订），可直接上传商业平台。"
- 停下来告诉用户。

**追赶模式不创建 `rewrite/ALL_DONE.txt`**：追上 `WRITE_LIMIT` 时按"自动判断当前进度"第 5 步停下报告即可，等下一个会话重启时会自动重算 `WRITE_LIMIT` 续作。只有等 RESTRUCTURE 跑出 `outline_final/ALL_DONE.txt` 之后，WRITE 才能写到末章并创建 `rewrite/ALL_DONE.txt`。

### 发现 outline_final 与 settings_final 不一致
如果某章 outline_final 中**出现 settings_final/volXX.md 或 index.md 未定义的角色 / 势力 / 装备 / 系统机制**：
- **不得擅自补 settings_final/**（按章生成时缺少卷边界触发点，自动追加会重演 ch303-727 欠账事故）
- **不得在创作中现编填空**
- **立刻停下**，写入 `rewrite_inconsistencies.md`：`第X章 | 缺失元素：xxx | 在 outline_final 中的引用：xxx`
- 报告用户，由人工裁决：要么回 MANUAL_RESTRUCTURE 流程补对应 settings_final/volXX.md，要么修正 outline_final 当前章

---

## 项目文件结构

```
项目根目录/
├── MANUAL_WRITE.md             ← 本文档（阶段三）
├── RULES.md                    ← 写作规则（最高优先级）
├── theme.md                    ← 主题配置（含文风规范，指向 themes/ 下当前主题）
├── themes/                     ← 主题配置库（默认空，由 RESTRUCTURE 步骤负一·C 后用户指定主题时由 AI 现写填入）
├── external_style_samples/     ← 外部网文风格样本（few-shot 参考，主题中立）
│   ├── README.md
│   └── ...
├── polish_skill/               ← 阶段四 WRITE_SKILL Project 配套素材（主题中立）
│   ├── README.md
│   ├── anti_ai_style_rules.md
│   └── system_prompt.md
├── settings_final/             ← 世界设定（按主题套，RESTRUCTURE 产出，WRITE 只读）
│   ├── index.md
│   ├── system_design.md
│   ├── world_overview.md
│   ├── style_sample.md         ← 内部文风样本（来自原作 outline）
│   └── ...
├── outline_final/              ← 故事大纲（创作的唯一依据，每章一个文件；追赶模式下仍在被 RESTRUCTURE 增量写入）
│   └── ALL_DONE.txt            ← RESTRUCTURE 全部完成才出现
├── restructure_progress.md     ← RESTRUCTURE 步骤一进度（追赶模式必读）
├── rewrite/                    ← 最终版（每章已经过 WRITE_SKILL 修订，可上传商业平台）
│   └── ALL_DONE.txt            ← 整本书完成标志（仅全量模式可创建）
├── rewrite_index.txt           ← 创作进度索引
├── summaries/                  ← 每章摘要
├── characters_state/           ← 角色动态状态
│   ├── index.md
│   └── ...
├── check_report.md             ← 自查报告
└── rewrite_inconsistencies.md  ← outline_final 与 settings_final 不一致记录
```

**单主题模式**：每本素材小说只产出一种皮，主题特异档案直接落工程根目录。要做不同题材，把模板目录整份复制走当新工程跑。
