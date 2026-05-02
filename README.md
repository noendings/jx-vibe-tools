# JX Vibe Tools

**螺旋上升式 AI 协同开发工作流** — 支持多 AI 窗口间的无缝任务交接。

---

## 安装

### 安装单个技能

```bash
# 工作交接
npx skills add noendings/jx-vibe-tools@jx-gzjj

# 接手工作
npx skills add noendings/jx-vibe-tools@jx-xrtk

# 分支研究
npx skills add noendings/jx-vibe-tools@jx-branch

# 合并成果
npx skills add noendings/jx-vibe-tools@jx-merge

# 口语结构化
npx skills add noendings/jx-vibe-tools@jx-tone
```

### 一键安装全部（推荐）

```bash
# 安装元安装器
npx skills add noendings/jx-vibe-tools@jx-meta

# 运行一键安装
/jx-install-all
```

或使用元安装器自动安装全部：
```bash
npx skills add noendings/jx-vibe-tools@jx-meta -g && jx-install-all
```

---

## 技能清单

| 技能 | 指令 | 功能 |
|------|------|------|
| **jx-meta** | `/jx-install-all` | 一键安装全部 JX Vibe Tools |
| **jx-gzjj** | `/jx-gzjj` | 生成交接文档 |
| **jx-xrtk** | `/jx-xrtk` | 读取交接文档，继续工作 |
| **jx-branch** | `/jx-branch-open`<br>`/jx-branch-done` | 开启/完成分支研究 |
| **jx-merge** | `/jx-merge` | 合并分支研究成果 |
| **jx-tone** | `/jx-tone`<br>`/jx-tone-pua` | 口语转结构化 |

---

## 典型使用流程

### 场景：播放器项目中音频解码需要深入研究

```
# 主窗口（播放器整体架构）
→ 发现音频解码需要深入研究
→ /jx-branch-open

# 新窗口（音频专项研究）
→ 研究 FFmpeg vs Web Audio API
→ 对比测试，踩坑记录
→ /jx-branch-done

# 主窗口（继续播放器开发）
→ /jx-merge
→ 选择音频研究 → 合并
→ 继续开发
```

---

## 目录结构

首次使用任意指令时，自动创建：

```
你的项目/
├── .jx_skill/              # Git 忽略，不上库
│   ├── handover/           # 工作交接文档
│   │   └── time_1_xxx.md
│   ├── branch_research/    # 分支研究归档
│   │   ├── research_xxx/   # 临时目录
│   │   └── 关于xxx的研究/   # 归档目录
│   └── jx-vibe-tool-log.md # 变更流水
└── CLAUDE.md               # 研究引用索引
```

---

## 技术特点

- **文件即接口**：研究以 Markdown 文档形式存在
- **本地隔离**：`.jx_skill/` Git 忽略，研究成果仅本地
- **双链关联**：支持 Obsidian `[[文档名]]` 双链语法
- **螺旋上升**：主分支定型 → 拆分研究 → 合并升级

---

## License

MIT
