# GitHub 上传完整清单 — CC Explore Skill

> 按顺序操作，每步打勾 ✅

---

## 📋 上传前检查

- [ ] 确认仓库名（推荐 `cc-thread`，备选 `mindbridge` / `threadkeep`）
- [ ] 全局搜索替换 README + 安装说明 中的 `cc-explore` → 你的仓库名
- [ ] 全局搜索替换 `yyyxxx-art` → 你的 GitHub 用户名（已替换）
- [ ] 确认 LICENSE 文件存在且作者名正确
- [ ] 确认 README 中已标注 **"Claude Code Skill"** badge

---

## 🏗️ GitHub.com 操作

- [ ] 登录 github.com → 右上角 + → New repository
- [ ] Repository name: `cc-thread`（或你选的名称）
- [ ] Description 栏粘贴下面这段话：

```
🧠 A Claude Code Skill for cross-session knowledge management. Two slash-commands (/cc-save + /cc-load) let AI remember every conversation's turning points and auto-restore context on next launch. Built-in index validation, context condensation, ghost-context skip — 10 long-term risk mitigations baked in. Not file management. Attention flow management.
```

- [ ] 勾选 Public（公开）
- [ ] **不勾选** Add a README file
- [ ] **不勾选** Add .gitignore
- [ ] **不勾选** Choose a license（我们已经有了）
- [ ] 点击 Create repository

---

## 💻 本地终端操作

```bash
# 1. 进入项目目录
cd ~/Desktop/CC的探讨/CC的探讨-skill-v2.0-20260622

# 2. 关联远程仓库
git remote add origin https://github.com/yyyxxx-art/cc-thread.git

# 3. 推送
git push -u origin main

# 4. 验证 — 浏览器打开你的仓库地址，确认文件都在
```

---

## 🏷️ GitHub 仓库设置（推送后操作）

- [ ] **About 区域**（仓库右侧齿轮图标）
  - Description: 上面那段英文描述
  - Website: 留空（或填你的博客）
  - Topics 标签添加以下 6 个：
    ```
    claude-code    claude-skill    knowledge-management
    context-continuity    developer-tools    productivity
    ```

- [ ] **Settings → General → Features**
  - ✅ Issues（保持开启）
  - ✅ Discussions（建议开启，方便用户交流）

---

## 📢 发布后推广（可选）

- [ ] 分享到 X / Twitter：`Built a Claude Code Skill that...`
- [ ] 分享到 Reddit：`r/ClaudeAI` / `r/programming`
- [ ] 分享到掘金 / V2EX / 知乎（中文社区）
- [ ] 在 Claude Code 官方 Discord 展示

---

## 🔄 后续维护

- [ ] 当有人提 Issue 时及时回复
- [ ] 当有人提 PR 时认真 Review
- [ ] 当 Claude Code 更新 Skill 机制时同步更新本 Skill
- [ ] 定期检查 `.index.json` schema 是否需要升级

---

## 📝 如何让别人一眼认出这是个 Skill？

已做的标识：

| 标识 | 位置 | 效果 |
|------|------|------|
| 🏷️ `Claude Code Skill` badge | README 顶部 | 一眼识别 |
| 📁 `skills/` 目录 | 项目根目录 | 符合 Claude Code 约定 |
| 💡 "What is a Skill?" 说明 | README 安装部分 | 新人也能理解 |
| 📋 `~/.claude/skills/` 路径 | 安装指令 | 标明安装目标 |
| 🏷️ Topics 标签含 `claude-skill` | GitHub About | 搜索可见 |
