# JX Vibe Tools

**螺旋上升式 AI 协同开发工作流** — 多 AI 窗口无缝任务交接。

---

## 安装

**逐个安装**（推荐，清晰可控）：

```bash
npx skills add noendings/jx-vibe-tools@jx-gzjj -g
npx skills add noendings/jx-vibe-tools@jx-branch -g
npx skills add noendings/jx-vibe-tools@jx-xrtk -g
npx skills add noendings/jx-vibe-tools@jx-tone -g
```

**⚠️ Windows PowerShell 用户**：分4行逐个执行，不要复制整块。

---

## 安装后重启

```bash
Ctrl+C      # 退出 Claude Code
claude      # 重新启动
```

---

## 功能列表

| 指令 | 功能 |
|------|------|
| `/jx-gzjj` | 生成交接文档 |
| `/jx-xrtk` | 读取交接，继续工作 |
| `/jx-branch-open` | 开启分支研究会话 |
| `/jx-branch-done` | 完成并归档研究（支持导入已有成果） |
| `/jx-merge` | 合并研究成果到主项目 |
| `/jx-tone` | 口语转结构化 |

---

## 典型工作流

```
# 主窗口（发现需要深入研究）
→ /jx-branch-open

# 新窗口（专项研究）
→ 研究、测试、记录
→ /jx-branch-done（可选择导入已有研究）

# 主窗口（合并成果）
→ /jx-merge
→ 继续开发
```

---

## License

MIT
