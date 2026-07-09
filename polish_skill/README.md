# WRITE_SKILL · 反 AI 风修订规则集

## 是什么

**阶段三 MANUAL_WRITE.md 单章流程的内嵌步骤**。每章 Claude Code 创作初稿 + 自我修改后，**Claude Code 自己读本目录的规则文档对章节做第二轮修订**（专门处理 AI 风），再保存到 `rewrite/`。

整个流程在 Claude Code 内完成，**不需要切换网页端**。

## 为什么需要

LLM 直接生成的正文有 30-40% AI 风识别率，会被番茄等平台 AI 检测拦截。
经 WRITE_SKILL 规则修订后，AI 风识别率降到 15-20%，可过商业平台检测。

## 文件清单

| 文件 | 用途 |
|---|---|
| `anti_ai_style_rules.md` | 反 AI 风规则总集（核心，每章修订时必读）|
| `system_prompt.md` | （**已废弃**）原本是网页端 Project 配置，现在不用了 |
| `README.md` | 本文档 |

## Claude Code 怎么用

阶段三 MANUAL_WRITE.md 单章流程第 4 步「WRITE_SKILL 修订」：

1. Claude Code 完成自我修改后，读取本目录 `anti_ai_style_rules.md` + `../external_style_samples/` 全部样本
2. 按规则对当前章节做反 AI 风第二轮修订：
   - 拆短长句
   - 去强逻辑词
   - 改套路心理表达为动作 / 表情 / 环境
   - 打斗场面改成短句轰鸣 + 局部特写
   - 加入人类化痕迹（重复 / 残句 / 跳跃）
3. 按 `anti_ai_style_rules.md` 末尾的"自检清单"逐项核
4. 全过通过 → 保存到 `rewrite/0XXX.txt`

详见 `MANUAL_WRITE.md` 单章流程第 4 步。

## 流程位置

```
阶段一 OUTLINE → 阶段二 RESTRUCTURE → 阶段三 WRITE（内嵌 WRITE_SKILL 修订）→ 上传番茄
                                              ↑
                                   Claude Code 自己读规则修订，全程不离开
```

## 完成标志

`rewrite/ALL_DONE.txt` 出现 = 所有章节都过了 WRITE_SKILL 修订（每章修订都是 Claude Code 内嵌完成），可上传商业平台。

## 你需要做的

**一次性**：在 `external_style_samples/` 目录下放 30-50 段优秀网文样本（按该目录 README 格式）。这是 few-shot 风格参考的核心。样本越多，修订效果越好。

**日常**：什么都不用做，Claude Code 自动读规则修订。
