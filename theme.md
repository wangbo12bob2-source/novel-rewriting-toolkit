# 主题配置

本工程采用**单主题模式**：每本素材小说只产出一种皮，不开多窗口并行不同题材。

## 当前状态：未选定主题

```
# active_theme: themes/theme_xxx.md   ← 待用户决定主题后填入
```

`themes/` 目录目前为空。**主题不预先决定**——先跑流程一 OUTLINE + 流程二·步骤负一·A/B/C/D（这些都是主题中立的，从原作蒸馏），等步骤负一·C 产出 `theme_baseline.md` 后，由用户告诉 AI"跑 X 主题"，AI 据 baseline 设计一份 `themes/theme_X.md`，回填 `active_theme:` 字段，再继续流程二·步骤零。

## 主题决定时点

```
流程一·OUTLINE                 → outline/ 全章蒸馏（主题中立）
流程二·步骤负一·A/B            → outline_arcs.md, outline_rhythm/（主题中立）
流程二·步骤负一·C              → theme_baseline.md（你这本源素材的量化基线，主题中立）
                                    ↓
                          *这里是用户决定主题的时点*
                          用户说"跑 X 主题" → AI 据 baseline 设计 themes/theme_X.md
                                    ↓
流程二·步骤负一·D              → forbidden_terms.md（主题中立）
流程二·步骤零                  → 用 themes/theme_X.md 套出 settings_final/
流程二·步骤一                  → outline_final/
流程三·WRITE                   → rewrite/
```

## 切换主题（开始一本新书时）

如果要为另一本素材开新工程，把模板目录整份复制走当新工程跑。**同一工程不要中途切主题**——产物已落到 `settings_final/` `outline_final/` `rewrite/` 等根目录档，切了等于推倒重来。

## 主题中立 vs 主题特异

| 类型 | 文件 | 是否依赖主题 |
|---|---|---|
| **主题中立** | `chapters/`、`settings/`、`name_mappings/`、`outline/`、`outline_arcs.md`、`outline_arc_patterns.md`、`outline_rhythm/`、`theme_baseline.md`、`forbidden_terms.md`、`forbidden_chars.txt` | 从素材小说蒸馏，与主题无关 |
| **主题特异** | `settings_final/`、`name_mappings_final/`、`outline_final/`、`rewrite/`、`summaries/`、`characters_state/`、`book_meta.md`、各类进度档 | 套主题产出，落工程根目录 |

## 主题设计规约（用户决定主题后由 AI 现写 themes/theme_X.md）

### 设计前置：必须对照 theme_baseline.md

`theme_baseline.md`（由 [MANUAL_RESTRUCTURE.md](MANUAL_RESTRUCTURE.md) 步骤负一·C 产出）是**主题设计的客观基线**，从原作蒸馏：
- 力量阶位数与章号分布
- 世界扩展节点
- 爽点频次分布
- 配角群像配比
- 文风量化特点

**任何 `themes/theme_xxx.md` 的设计字段必须对照 theme_baseline.md**，每项偏离需有显式理由（写在文件末尾"校准对照"小节）。

### 文件必须包含的 12 章节

1. 主题定位（一句话）
2. **世界扩展路径（关键，决定主题能否承载长篇）**
   - 主题不能限定在单一尺度。必须有完整的多阶段地图扩展路线。
   - 每个阶段对应：章号区间 + 地图层级 + 对应原作层级 + 主角主要舞台
   - **章号区间数值取自 `theme_baseline.md`**，不得凭空指定
   - 终点必须能匹配原作"创世级"尺度（不能止步于"全球巅峰"或"行星级"）
   - **核心原则**：主题不是"小盒子"，是"多层洋葱"。每一层都要完整设计。
3. **力量阶位序列**（system_design.md 套这个）
   - 与世界扩展路径**逐阶段对应**，每个地图层级有相应阶位
   - **阶位数 + 章号区间取自 `theme_baseline.md`**
   - 终点阶位必须达到"创世级"
4. **主题顶点说明**：本主题"力量天花板"是什么（必须是创世级，不是地球级）
5. 装备 / 资源体系（**按地图层级分阶段**，每阶段装备品质有跃升）
6. 社会结构（world_overview.md 套这个，**按地图层级分阶段**：城市层→国家层→全球层→多服层→宇宙层→创造层）
7. 现实联动机制（早期阶段适用，后期超凡阶段可弱化或转化为"高维联动"）
8. 典型爽点（至少 8 种，**爽点种类数对照 baseline 爽点频次表**，**覆盖各阶段**：早期日常爽 + 中期破圈爽 + 后期史诗爽）
9. 力量质变事件模板（master_arc.md 套这个，**对应 baseline 的 10 个力量突破时机 + 地图层级跃升时机**）
10. 文风规范（**按阶段调整**：早期日常感、中期升级感、后期史诗感、终末创世感；**心理密度对照 baseline 文风字段**）
11. 命名风格（按阶段适配：早期接地气、后期带宇宙感/法则感）
12. **校准对照表**（强制）：列出本 theme 的关键字段（阶位数 / 章号区间 / 跃升点 / 爽点 / 配角配比 / 文风）与 `theme_baseline.md` 的对照。每项标注"匹配"或"偏离 + 理由"。

### 校准对照表模板

```markdown
## 校准对照（与 theme_baseline.md）

| 字段 | 原作基线 | 本 theme | 偏差 | 理由（如偏离） |
|---|---|---|---|---|
| 阶位数 | （读 baseline）| | 匹配 / 偏离 | |
| 阶位章号区间 | 见 baseline 表 | 见本文力量阶位表 | 匹配 / 偏离 | |
| 世界扩展节点 | （读 baseline）| | 匹配 / 偏离 | |
| 跃升章号 | （读 baseline）| | 匹配 / 偏离 | |
| 爽点种类 | （读 baseline）| | 匹配 / 偏离 | |
| 主线配角配比 | 师 X / 红 X / 敌 X / 同 X / 中 X | 同 | 匹配 / 偏离 | |
| 文风心理密度 | 每章 X 段 | 同 | 匹配 / 偏离 | |
| 终点尺度 | 创世级 | 创世级 | 匹配 | |
```
