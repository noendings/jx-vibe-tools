---
name: jx-merge
description: |
  合并分支研究 - 将分支研究成果吸收到主项目。
  当用户完成分支研究，需要将成果合并回主项目时使用。
  触发词："/jx-merge"、"合并研究"、"吸收成果"、"merge研究"
triggers:
  - jx-merge
  - 合并研究
  - 吸收成果
  - merge研究
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - AskUserQuestion
---

# /jx-merge - 合并分支研究

将分支研究的成果合并到主项目。

## 工作流程

### Step 1: 扫描可用研究

```bash
BRANCH_DIR="$(pwd)/.jx_skill/branch_research"

if [ -d "$BRANCH_DIR" ]; then
  # 列出非 research_ 开头的已归档目录
  find "$BRANCH_DIR" -maxdepth 1 -type d ! -name "research_*" ! -name "branch_research" | sort -r | head -10
else
  echo "NO_RESEARCH"
fi
```

### Step 2: 列出研究让用户选择

```
可用分支研究（按时间倒序）：

1. 关于音频解码方案的研究探索及后续行动报告
   创建：2026-05-02 | 状态：已完成，待合并

2. 关于性能优化的研究探索及后续行动报告
   创建：2026-05-01 | 状态：已合并

请选择：
```

### Step 3: 读取研究内容

读取选中目录的 `readme.md`，提取：
- 核心结论
- 推荐方案
- 可复用代码
- 踩坑记录
- 对主项目的建议

### Step 4: 选择合并策略

```
🔀 选择合并策略：

A. 作为衍生扩展并入（推荐）
   → 保留原模块，新增扩展能力
   → 适合：原有功能保留，增加新选项

B. 直接替换原模块
   → {检测到可替换模块：xxx}
   → 适合：原方案完全废弃

C. 吸收经验优化现有模块
   → 基于研究结论改进，不新增文件
   → 适合：原方案可行，需要调优
```

### Step 5: 执行合并

**策略A**：
- 创建新文件（如 decoder-ffmpeg.ts）
- 更新文档说明扩展

**策略B**：
- 备份原模块（如 decoder.ts.bak）
- 替换为新实现
- 更新引用

**策略C**：
- 基于研究优化现有代码
- 添加注释说明

### Step 6: 更新变更日志

追加到 `.jx_skill/jx-vibe-tool-log.md`：

```markdown
## 2026-05-02 15:00 - 合并分支研究

**来源**：关于音频解码方案的研究
**策略**：A（衍生扩展）
**变更**：
- 原状态：仅支持 Web Audio API
- 新状态：新增 FFmpeg 方案，支持2种解码器
**影响**：src/audio/decoder-ffmpeg.ts（新增）
```

### Step 7: 更新 CLAUDE.md（可选）

添加研究引用到 CLAUDE.md 的研究记录章节。

### Step 8: 输出确认

```
✅ 研究成果已合并

📁 来源：关于音频解码方案的研究探索及后续行动报告
🔀 策略：A - 衍生扩展
📝 变更日志：.jx_skill/jx-vibe-tool-log.md

新增文件：
- src/audio/decoder-ffmpeg.ts

变更摘要：
原方案：仅 Web Audio API
新方案：Web Audio + FFmpeg 双支持
```

---

## 规则

- 备份重要文件再替换
- 更新日志记录原→新状态
- 不修改 .jx_skill/branch_research/ 中的研究文档
- 主项目文件 Git tracked，研究文档 Git ignored
