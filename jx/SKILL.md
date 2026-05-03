---
name: jx
description: |
  JX Vibe Tools 统一入口 - AI 协同开发工作流工具箱。
  包含：工作交接、分支研究、成果合并、口语结构化。
  触发词："/jx"、"/jxtools"、"工作流工具"
triggers:
  - jx
  - jxtools
  - 工作流工具
  - 协作工具
allowed-tools:
  - Bash
  - Read
  - Write
  - AskUserQuestion
---

# JX Vibe Tools - AI 协同开发工作流

**一句话说明**：让多个 AI 窗口像团队一样协作，工作无缝交接。

## 快速开始

运行 `/jx` 显示功能菜单，选择需要的工具。

---

## 功能菜单

运行 `/jx` 后选择：

| 选项 | 功能 | 指令 |
|------|------|------|
| **1** | 工作交接 - 保存当前进度，生成交接文档 | `/jx-gzjj` |
| **2** | 接手工作 - 读取交接文档，继续开发 | `/jx-xrtk` |
| **3** | 分支研究 - 开启独立研究会话 | `/jx-branch-open` |
| **4** | 完成研究 - 归档研究成果 | `/jx-branch-done` |
| **5** | 合并成果 - 将研究合并到主项目 | `/jx-merge` |
| **6** | 口语转结构化 - 整理口语化需求 | `/jx-tone` |

---

## 典型工作流

### 场景：播放器项目中音频解码需要深入研究

```
# 主窗口（播放器整体架构）
→ /jx → 选择 3. 分支研究
→ /jx-branch-open

# 新窗口（音频专项研究）
→ 研究 FFmpeg vs Web Audio API
→ 做测试，踩坑记录
→ /jx-branch-done

# 主窗口（继续播放器开发）
→ 回主窗口
→ /jx → 选择 5. 合并成果
→ /jx-merge
→ 选择音频研究 → 合并
→ 继续开发
```

---

## 子命令直接调用

也可以直接调用，跳过菜单：

| 指令 | 功能 |
|------|------|
| `/jx-gzjj` | 生成交接文档 |
| `/jx-xrtk` | 读取交接文档 |
| `/jx-branch-open` | 开启分支研究 |
| `/jx-branch-done` | 完成研究归档 |
| `/jx-merge` | 合并研究成果 |
| `/jx-tone` | 口语转结构化 |

---

## 目录结构

首次使用任意功能时，自动创建：

```
你的项目/
├── .jx_skill/
│   ├── handover/           # 工作交接文档
│   │   └── time_1_xxx.md
│   ├── branch_research/    # 分支研究归档
│   │   └── 关于xxx的研究/
│   └── jx-vibe-tool-log.md
└── jx-project-overview-roadmap.md  # 项目总览
```

---

## 安装说明

如果子命令不可用，需单独安装：

```bash
npx skills add noendings/jx-vibe-tools@jx-gzjj -g
npx skills add noendings/jx-vibe-tools@jx-branch -g
npx skills add noendings/jx-vibe-tools@jx-tone -g
```

安装后重启 Claude Code：
- 按 `Ctrl+C` 退出
- 重新运行 `claude`

---

## 工作流程说明

**为什么要多个 AI 窗口协作？**
- 主 AI（大上下文）：把控整体架构
- 辅助 AI（小上下文）：深入局部研究

**JX Vibe Tools 解决什么问题？**
- 研究完成后，成果如何传递给主 AI？
- 新 AI 如何快速了解历史工作？
- 通过**文件即接口**，实现无缝交接。
