---
name: jx-branch
description: |
  分支研究管理 - 完整的研究工作流（开题→研究→完成→合并）。
  包含三个子命令：/jx-branch-open 开启研究，/jx-branch-done 完成归档，/jx-merge 合并成果。
  当需要做技术调研、方案对比时，开分支深入研究，完成后合并到主项目。
  触发词："/jx-branch-open"、"/jx-branch-done"、"/jx-merge"、"开分支"、"完成研究"、"合并研究"
triggers:
  - jx-branch-open
  - jx-branch-done
  - jx-merge
  - 开分支
  - 完成研究
  - 合并研究
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - AskUserQuestion
---

# /jx-branch - 分支研究管理（完整工作流）

## 子命令

| 命令 | 功能 | 阶段 |
|------|------|------|
| `/jx-branch-open` | 开启新的分支研究会话 | ① 开题 |
| `/jx-branch-done` | 完成并归档分支研究 | ② 归档 |
| `/jx-merge` | 将归档的研究成果合并到主项目 | ③ 合并 |

**使用流程**：
```
主项目发现问题 → /jx-branch-open 开题
                    ↓
              新窗口独立研究
                    ↓
              /jx-branch-done 归档
                    ↓
              回主窗口 /jx-merge 合并
```

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

分支研究完成，需要归档成果。

**支持两种模式**：
1. **标准模式**：从临时目录 `research_*` 归档（默认）
2. **导入模式**：将任意位置的已有研究成果导入归档

### 模式选择

**使用 AskUserQuestion 询问**：

```
请选择归档方式：

A. 标准模式（推荐）
   → 从最近的分支研究临时目录归档
   → 适合：用 /jx-branch-open 开启的研究

B. 导入已有研究
   → 将其他位置的已有研究成果导入
   → 适合：已有文档/代码需要纳入管理
```

### 模式 A：标准归档

#### Step 1: 查找临时目录

```bash
BRANCH_DIR="$(pwd)/.jx_skill/branch_research"

TEMP_DIR=$(find "$BRANCH_DIR" -maxdepth 1 -type d -name "research_*" -mmin -30 | head -1)
[ -z "$TEMP_DIR" ] && find "$BRANCH_DIR" -maxdepth 1 -type d -name "research_*" | head -5

echo "TEMP_DIR=$TEMP_DIR"
```

#### Step 2: 分析研究内容

提取：研究主题、尝试方案、核心结论、可复用代码、踩坑记录、遗留问题。

### 模式 B：导入已有研究

#### Step 1: 指定研究成果位置

**使用 AskUserQuestion 询问**：

```
请输入已有研究成果的路径：

[输入框] 路径：

提示：
- 可以是相对路径（如：docs/性能优化研究/）
- 可以是绝对路径（如：D:/projects/研究/音频方案/）
- 该目录应包含研究文档、代码、笔记等
```

#### Step 2: 读取并分析内容

```bash
IMPORT_DIR="{用户输入的路径}"

# 列出目录内容
ls -la "$IMPORT_DIR"

# 查找关键文件
find "$IMPORT_DIR" -type f \( -name "*.md" -o -name "*.txt" -o -name "*.py" -o -name "*.js" \) | head -10
```

#### Step 3: 提取研究主题

根据目录内容和文件标题，提取：
- 研究主题（建议命名）
- 已有文档清单
- 核心结论（从 README/文档中读取）

### 通用步骤：生成标准归档

#### 生成标准命名

建议：`关于{主题}的研究探索及后续行动报告`

确认重命名。

#### 生成/完善 readme.md

- **模式 A**：从临时目录创建新的 readme.md
- **模式 B**：
  - 如果已有 README.md，读取并转换格式
  - 如果没有，基于文件内容生成 readme.md

目录结构：
```
.jx_skill/branch_research/关于{主题}的研究探索及后续行动报告/
├── readme.md              # 研究成果说明书
├── {原始文件保留}/        # 导入模式的原始文件（可选）
└── ...
```

### 输出确认

```
✅ 分支研究已归档

📁 .jx_skill/branch_research/关于{主题}的研究探索及后续行动报告/
📄 readme.md

在主项目运行 /jx-merge 吸收研究成果。
```

---

## /jx-merge

### 使用场景

将归档的分支研究成果合并到主项目。

### Step 1: 扫描可用研究

```bash
BRANCH_DIR="$(pwd)/.jx_skill/branch_research"

if [ -d "$BRANCH_DIR" ]; then
  find "$BRANCH_DIR" -maxdepth 1 -type d ! -name "research_*" ! -name "branch_research" | sort -r | head -10
else
  echo "NO_RESEARCH"
fi
```

### Step 2: 列出研究让用户选择

```
可用分支研究：

1. 关于音频解码方案的研究探索及后续行动报告
   创建：2026-05-02 | 状态：已完成，待合并

2. 关于性能优化的研究探索及后续行动报告
   创建：2026-05-01 | 状态：已合并

请选择：
```

### Step 3: 读取研究内容

读取选中目录的 `readme.md`，提取核心结论、推荐方案、可复用代码、踩坑记录。

### Step 4: 选择合并策略

```
🔀 选择合并策略：

A. 作为衍生扩展并入（推荐）
   → 保留原模块，新增扩展能力

B. 直接替换原模块
   → 原方案完全废弃

C. 吸收经验优化现有模块
   → 不新增文件，仅优化
```

### Step 5: 执行合并

根据策略执行对应操作。

### Step 6: 更新变更日志

追加到 `.jx_skill/jx-vibe-tool-log.md`。

### Step 7: 输出确认

```
✅ 研究成果已合并

📁 来源：关于{主题}的研究
🔀 策略：{策略}
📝 变更日志：.jx_skill/jx-vibe-tool-log.md
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

---

## 工作流示例

### 场景一：从零开始研究（标准流程）

```
# 主窗口（发现需要深入研究音频解码）
→ /jx-branch-open
→ 输入主题：音频解码方案对比

# 新窗口（专项研究）
→ 对比 FFmpeg vs Web Audio API
→ 做测试，踩坑记录
→ /jx-branch-done（选择 A. 标准模式）

# 主窗口（继续播放器开发）
→ /jx-merge
→ 选择音频解码研究
→ 策略A：新增 decoder-ffmpeg.ts
→ 继续开发
```

### 场景二：已有研究成果需要导入

```
# 其他窗口已经研究过，有文档和代码
# 研究内容在：D:/projects/性能优化研究/

→ /jx-branch-done（选择 B. 导入已有研究）
→ 输入路径：D:/projects/性能优化研究/
→ AI 分析目录内容，提取主题和结论
→ 生成标准格式的 readme.md
→ 归档到：关于性能优化的研究探索及后续行动报告/

# 主窗口
→ /jx-merge
→ 选择性能优化研究
→ 合并成果
```

### 场景三：多阶段研究逐步归档

```
# 第一次研究
→ /jx-branch-open（主题：方案A调研）
→ 研究...
→ /jx-branch-done（归档）

# 第二次补充研究（已有部分成果）
→ /jx-branch-done（选择 B. 导入已有研究）
→ 导入新发现的内容
→ AI 合并更新到已有归档

# 最后合并
→ /jx-merge（合并完整研究成果）
```
