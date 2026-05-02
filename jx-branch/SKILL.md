---
name: jx-branch
description: |
  分支研究管理 - 开启/完成分支研究。
  包含两个子命令：/jx-branch-open 开启研究，/jx-branch-done 完成归档。
  当需要做技术调研、方案对比、独立研究时使用。
  触发词："/jx-branch-open"、"/jx-branch-done"、"开分支"、"完成研究"
triggers:
  - jx-branch-open
  - jx-branch-done
  - 开分支
  - 完成研究
allowed-tools:
  - Bash
  - Read
  - Write
  - Glob
  - AskUserQuestion
---

# /jx-branch - 分支研究管理

## 子命令

| 命令 | 功能 |
|------|------|
| `/jx-branch-open` | 开启新的分支研究会话 |
| `/jx-branch-done` | 完成并归档分支研究 |

---

## /jx-branch-open

### 使用场景

主项目发现某个模块需要深入研究，新开独立会话做专项探索。

### Step 1: 确认主题

复述用户意图，确认研究主题。

### Step 2: 创建临时目录

```bash
PROJECT_ROOT=$(pwd)
BRANCH_DIR="$PROJECT_ROOT/.jx_skill/branch_research"
mkdir -p "$BRANCH_DIR"

# 创建带时间戳的临时目录
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
TEMP_DIR="$BRANCH_DIR/research_${TIMESTAMP}"
mkdir -p "$TEMP_DIR"

# 确保 .gitignore
[ -f "$PROJECT_ROOT/.gitignore" ] && grep -q "^.jx_skill/" "$PROJECT_ROOT/.gitignore" || echo ".jx_skill/" >> "$PROJECT_ROOT/.gitignore"

echo "TEMP_DIR=$TEMP_DIR"
```

### Step 3: 输出确认

```
✅ 分支研究会话已开启

📁 临时目录：.jx_skill/branch_research/research_20260502_153000/

可在此会话中完成研究。
完成后运行 /jx-branch-done 归档。
```

---

## /jx-branch-done

### 使用场景

分支研究完成，需要归档成果供主项目合并。

### Step 1: 查找临时目录

```bash
BRANCH_DIR="$(pwd)/.jx_skill/branch_research"

# 找最新的临时目录（30分钟内）
TEMP_DIR=$(find "$BRANCH_DIR" -maxdepth 1 -type d -name "research_*" -mmin -30 | head -1)

# 如果没找到，列出所有让用户选
[ -z "$TEMP_DIR" ] && find "$BRANCH_DIR" -maxdepth 1 -type d -name "research_*" | head -5

echo "TEMP_DIR=$TEMP_DIR"
```

### Step 2: 分析研究内容

扫描当前会话，提取：
- 研究主题
- 尝试的方案
- 核心结论
- 可复用代码
- 踩坑记录
- 遗留问题

### Step 3: 生成标准命名

建议：`关于{主题}的研究探索及后续行动报告`

确认重命名。

### Step 4: 生成 readme.md

```
{FINAL_DIR}/
└── readme.md
```

### Step 5: 输出确认

```
✅ 分支研究已归档

📁 .jx_skill/branch_research/关于{主题}的研究探索及后续行动报告/
📄 readme.md

在主项目运行 /jx-merge 吸收研究成果。
```

---

## readme.md 模板

```markdown
---
topic: {主题}
parent: {关联模块}
created: {timestamp}
status: completed
---

# 关于{主题}的研究探索及后续行动报告

## 研究背景
{为什么研究这个}

## 方案探索

| 方案 | 描述 | 结果 | 原因 |
|-----|------|------|------|
| A | xxx | ❌ 放弃 | {原因} |
| B | xxx | ✅ 采用 | {原因} |

## 核心结论
{一句话总结}

## 可复用代码
```{lang}
{code}
```

## 踩坑记录
- **{坑}**：{原因} → {方案}

## 待解决问题
- [ ] {遗留}

## 对主项目的建议
{如何应用}

## 使用此研究
在主项目运行 `/jx-merge`，选择本研究。
```
