# 小说重构工具包 · Novel Rewriting Toolkit

> AI 驱动的小说改写工具包——基于 Claude Code，从素材小说到原创成品的全自动流水线。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[English Docs](README_EN.md) | [详细使用说明](README_CN.md)

## 这是什么

把一本素材小说变成完全原创的改写成品——全新的名字、地图、力量体系、对白——同时保留原作的故事结构和情绪节奏。

一套运行在 **Claude Code** 里的**三阶段自动化流水线**：

```
素材小说 → 流程一·OUTLINE → 流程二·RESTRUCTURE → 流程三·WRITE → 原创成品
```

## 核心特性

- **100% 原创输出**：所有专有名词替换、所有对白重写、所有描写重编
- **支持换皮**：素材小说可改编为任意题材（末世、都市、科幻、玄幻、悬疑等）
- **质量护栏**：内置反 AI 风检测、设定一致性检查、版权安全验证
- **断点续跑**：每一步都落盘保存，随时中断、随时接续，不丢进度
- **一句启动**：每阶段只需一句指令，Claude Code 全程自动驾驶

## 工作流程

### 流程一 · OUTLINE（原作蒸馏）
把素材小说蒸馏成结构化档案，保留原作名词：
- 自动拆分章节
- 提取世界观、势力、力量体系、人物档案
- 生成逐章大纲（场景/事件/冲突/打斗/情绪/互动/转折/目标字数 8 维度）
- 输出原作专有名词全量映射

### 流程二 · RESTRUCTURE（大纲重构）
把大纲转化为原创作品框架：
- **步骤负一**：四档蒸馏——故事弧识别、节奏模式、主题校准基线、避雷词库
- **步骤零**：主题转换——生成新世界观、力量体系、角色身份，命名约束在 3500 常用字内
- **步骤一**：按节奏改写每章大纲，借鉴度分级控制

### 流程三 · WRITE（正文创作）
逐章写出最终小说：
- 推荐 Opus 级别模型保证文风质量
- 每章强制执行反 AI 风规则
- 每 10 章滚动质检
- 自动生成章节摘要、追踪角色状态
- 最终输出：每章 2000-2300 字，可直接上传番茄/起点等平台

## 仓库结构

```
├── README.md                      # 本文件（中文主页）
├── README_EN.md                   # 英文文档
├── README_CN.md                   # 详细中文使用说明
├── RULES.md                       # 写作规则 & 禁忌清单（AI 强制执行）
├── WRITING_PARADIGM.md            # 写作范式 & 风格指南
├── WRITE_SKILL.md                 # 章节写作技能规范
├── SESSION_CONTROL.md             # 会话生命周期 & 续写逻辑
├── theme.md                       # 主题系统设计
├── MANUAL_OUTLINE.md              # 流程一手册（单线模式）
├── MANUAL_OUTLINE_PARALLEL.md     # 流程一手册（并行模式）
├── MANUAL_RESTRUCTURE.md          # 流程二手册
├── MANUAL_WRITE.md                # 流程三手册
├── polish_skill/                  # 润色子系统
│   ├── anti_ai_style_rules.md     # 反 AI 风检测规则
│   └── system_prompt.md           # 润色 Agent 系统提示词
├── external_style_samples/        # 投放外部风格样本
└── themes/                        # 主题定义文件
```

## 环境要求

- **Claude Code**（需 Claude Opus 或 Sonnet API 访问权限）
- **Python 3.8+**（仅辅助合并脚本需要）
- 一本素材小说 `.txt` 文件（UTF-8 或 GBK 编码均可）

## 快速开始

1. **克隆仓库**，用 Claude Code 打开：
   ```bash
   git clone https://github.com/sssst1118/novel-rewriting-toolkit.git
   cd novel-rewriting-toolkit
   code .
   ```

2. **把素材小说丢进项目根目录**（任意文件名，UTF-8/GBK/GB18030 都能识别）。

3. **（可选）** 往 `external_style_samples/` 里扔几篇你喜欢的小说风格样本。

4. **启动流程一**，对 Claude Code 说：
   ```
   按 MANUAL_OUTLINE.md 执行
   ```
   素材超过 500 章建议用并行模式加速：
   ```
   按 MANUAL_OUTLINE_PARALLEL.md 执行，段 ID = A，负责章号范围 0001-0500
   ```

5. 流水线自动推进。跑到流程二·步骤负一·C 时会自动停下，Claude Code 会问你：**"跑什么主题？"**——告诉它你想要的题材（比如"末世""都市异能""玄幻修仙"等）。

## 时间估算（以 1500 章素材为例）

| 阶段 | 会话数 | 用户体感时长 |
|------|--------|------------|
| 流程一（单线） | 10-15 | ~1-2 周 |
| 流程一（2 路并行） | 同上但分窗口 | ~3-5 天 |
| 流程二 | 37-51 | ~1 周 |
| 流程三 | 100-150 | 1-2 个月 |
| **合计** | **~150-200** | **1.5-2.5 个月** |

## 重要说明

- **版权安全**：本工具包的设计目标是产出独立原创作品。全部专有名词替换、全部对白重写、全部描写转化。最终成品应为独立原创，无版权争议。
- **不是一键生成**：这是一套给作者用的强力工具，帮你系统性地转化叙事框架并保持品控。你每天只需花 10-20 分钟启动续写会话。
- **模型推荐**：流程三强烈推荐 Opus 4.7+（风格控制能力是关键）。流程一和二可以用 Sonnet 降低成本。
- **同一工程不要中途切主题**：主题特异产物会覆盖，切主题等于推倒重来。要做不同题材，整份模板复制走当新工程跑。

## 协议

MIT License — 详见 [LICENSE](LICENSE)

## 致谢

基于 [Claude Code](https://claude.ai/code) 和 [Anthropic API](https://anthropic.com/api) 构建。