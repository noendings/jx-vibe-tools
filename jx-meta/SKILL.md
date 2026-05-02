---
name: jx-meta
description: |
  JX Vibe Tools 元安装器 - 一键安装全部 AI 协同开发技能。
  包含：jx-gzjj(工作交接)、jx-xrtk(接手工作)、jx-branch(分支研究)、
  jx-merge(合并成果)、jx-tone(口语结构化)。
  触发词："/jx-install-all"、"jx-meta"、"安装全部技能"
triggers:
  - jx-install-all
  - jx-meta
  - 安装全部技能
  - 一键安装
allowed-tools:
  - Bash
  - AskUserQuestion
---

# /jx-install-all - 一键安装全部 JX Vibe Tools

## 使用场景

- 新用户首次使用，想一次性安装所有技能
- 检查哪些 JX 技能缺失并补全

## 工作流程

### Step 1: 检查已安装技能

```bash
npx skills list 2>/dev/null | grep -E "^jx-" | awk '{print $1}'
```

### Step 2: 确定待安装列表

JX Vibe Tools 完整列表：
- `jx-gzjj` - 生成交接文档
- `jx-xrtk` - 读取交接，继续工作
- `jx-branch` - 开启/完成分支研究
- `jx-merge` - 合并分支研究成果
- `jx-tone` - 口语转结构化

### Step 3: 展示状态并询问

```
JX Vibe Tools 安装状态：

✅ 已安装：
   - jx-gzjj
   - jx-tone

⏳ 未安装：
   - jx-xrtk
   - jx-branch
   - jx-merge

是否安装缺失的 3 个技能？
```

### Step 4: 批量安装

```bash
# 循环安装未安装的技能
for skill in jx-xrtk jx-branch jx-merge; do
  echo "Installing $skill..."
  npx skills add noendings/jx-vibe-tools@$skill -g -y
done
```

### Step 5: 输出结果

```
✅ 安装完成

已安装技能：
- jx-gzjj    → /jx-gzjj
- jx-xrtk    → /jx-xrtk
- jx-branch  → /jx-branch-open, /jx-branch-done
- jx-merge   → /jx-merge
- jx-tone    → /jx-tone, /jx-tone-pua

⚠️ 重要：请重启 Claude Code 使技能生效
   按 Ctrl+C 退出，然后重新运行 claude

使用指南：
- 工作交接：/jx-gzjj
- 新人填坑：/jx-xrtk
- 开始研究：/jx-branch-open

详细说明：npx skills info jx-gzjj
```

## 更新全部技能

```bash
npx skills update jx-gzjj jx-xrtk jx-branch jx-merge jx-tone
```

## 卸载

```bash
npx skills remove jx-gzjj jx-xrtk jx-branch jx-merge jx-tone jx-meta -g
```
