---
name: jx-gzjj
description: |
  AI 工作交接 - 生成交接文档。
  当用户完成模块开发，需要将工作交接给其他 AI 窗口时使用。
  触发词："/jx-gzjj"、"工作交接"、"生成交接文档"、"交接工作"
trigegrs:
  - jx-gzjj
  - 工作交接
  - 生成交接文档
  - 交接工作
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - AskUserQuestion
---

# /jx-gzjj - 生成交接文档

保存当前工作上下文，生成标准化交接文档。

## 工作流程

### Step 1: 初始化目录

```bash
PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
HANDOVER_DIR="$PROJECT_ROOT/.jx_skill/handover"
mkdir -p "$HANDOVER_DIR"

# 确保 .gitignore
if [ -f "$PROJECT_ROOT/.gitignore" ]; then
  grep -q "^.jx_skill/" "$PROJECT_ROOT/.gitignore" || echo ".jx_skill/" >> "$PROJECT_ROOT/.gitignore"
else
  echo ".jx_skill/" > "$PROJECT_ROOT/.gitignore"
fi

echo "HANDOVER_DIR=$HANDOVER_DIR"
```

### Step 2: 计算编号

```bash
# 找出最大编号
MAX_X=$(ls "$HANDOVER_DIR"/time_*.md 2>/dev/null | grep -o 'time_[0-9]*' | sed 's/time_//' | sort -n | tail -1)
X=$((MAX_X + 1))
[ -z "$MAX_X" ] && X=1
echo "X=$X"
```

### Step 3: 提取工作信息

从当前对话提取：
- **已完成文件**：代码文件路径、功能说明
- **关键决策**：做了什么选择、为什么
- **踩坑记录**：遇到的问题、原因、方案
- **下一步**：待办清单、当前卡点
- **用户画像**：沟通风格、技术背景

### Step 4: 生成交接文档

**命名**：`time_{X}_基于{entry}的{feature}交接文档-{时间戳}.md`

读取 `references/handover-template.md` 填充内容。

### Step 5: 确认输出

```
✅ 交接文档已生成
📁 {HANDOVER_DIR}/time_{X}_...
💡 可用 /jx-xrtk 读取此文档继续工作
```

---

## 输出示例

```markdown
# 工作交接文档

**交接编号**：time_3
**项目**：claude-jdcloud_kimi
**时间**：2026-05-02 15:30

---

## 已完成工作

| 文件 | 功能 | 状态 |
|-----|------|------|
| src/auth/login.ts | 登录逻辑 | ✅ |
| src/auth/jwt.ts | JWT 签发验证 | ✅ |

## 关键决策

- **选择JWT而非Session**：支持分布式部署

## 踩坑记录

| 坑 | 原因 | 解决 |
|---|------|------|
| token过期不提示 | 未拦截401 | 加拦截器 |

## 下一步

- [ ] 集成到主路由
- [ ] 写登录页面
```

---

## 规则

- 使用相对路径（repo-relative）
- 状态标记：✅ 完成 / 🔄 进行中 / ⏳ 待开始
- 支持 `[[文档名]]` Obsidian 双链
