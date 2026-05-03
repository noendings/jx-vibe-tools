# JX Vibe Tools

**AI 协同开发工作流** — 让多个 AI 窗口像团队一样协作。

```bash
npx skills add noendings/jx-vibe-tools
```

然后用 `/jx` 开始工作。

---

## 一句话说明

**主 AI 把控全局 + 辅助 AI 局部分支研究 + 文件接口无缝交接**

---

## 快速开始

```bash
# 1. 安装
npx skills add noendings/jx-vibe-tools -g

# 2. 重启 Claude Code
Ctrl+C 退出，然后重新运行 claude

# 3. 开始使用
/jx
```

---

## 功能列表

运行 `/jx` 选择功能：

| # | 功能 | 说明 |
|---|------|------|
| 1 | **工作交接** | 保存进度，生成交接文档 |
| 2 | **接手工作** | 读取交接，继续开发 |
| 3 | **分支研究** | 开启独立研究会话 |
| 4 | **完成研究** | 归档研究成果 |
| 5 | **合并成果** | 将研究合并到主项目 |
| 6 | **口语转结构化** | 整理口语化需求 |

---

## 典型工作流

```
# 主窗口（播放器架构）
→ /jx → 3. 分支研究
→ 发现音频解码需要深入研究

# 新窗口（专项研究）
→ 研究 FFmpeg vs Web Audio API
→ 测试、踩坑记录
→ /jx → 4. 完成研究

# 主窗口（继续开发）
→ /jx → 5. 合并成果
→ 选择音频研究 → 合并
→ 继续开发
```

---

## 目录结构

自动创建：

```
你的项目/
├── .jx_skill/              # Git 忽略
│   ├── handover/           # 交接文档
│   ├── branch_research/    # 研究归档
│   └── jx-vibe-tool-log.md
└── jx-project-overview-roadmap.md
```

---

## 直接调用子命令

也可跳过菜单直接调用：

| 指令 | 功能 |
|------|------|
| `/jx-gzjj` | 生成交接文档 |
| `/jx-xrtk` | 读取交接文档 |
| `/jx-branch-open` | 开启分支研究 |
| `/jx-branch-done` | 完成研究归档 |
| `/jx-merge` | 合并研究成果 |
| `/jx-tone` | 口语转结构化 |

---

## 安装子命令（如需要）

如果 `/jx-gzjj` 等提示 Unknown command，单独安装：

```bash
npx skills add noendings/jx-vibe-tools@jx-gzjj -g
npx skills add noendings/jx-vibe-tools@jx-branch -g
npx skills add noendings/jx-vibe-tools@jx-tone -g
```

然后重启 Claude Code。

---

## 为什么需要这个？

- **百万上下文 AI**：把控整体架构
- **200K 上下文 AI**：深入局部研究
- **问题**：研究成果如何传递？
- **解决**：文件接口 + 标准化交接

---

## License

MIT
