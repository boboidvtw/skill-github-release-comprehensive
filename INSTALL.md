# 安裝與故障排查

## 系統要求

- **Claude Code** (最新版本)
- **GitHub CLI** (`gh` 命令)
  ```bash
  # 驗證安裝
  gh --version
  ```
- **Git** (2.x+)
- **Python** 3.8+ (如使用本地scripts)

## 安裝步驟

### 1. 克隆或下載

**方式A: 使用git clone**
```bash
git clone https://github.com/<your-username>/skill-github-release-comprehensive.git
```

**方式B: 直接下載**
```bash
# 下載ZIP並解壓到 ~/.claude/skills-lib/
unzip skill-github-release-comprehensive-main.zip
mv skill-github-release-comprehensive ~/.claude/skills-lib/
```

### 2. 驗證安裝

```bash
# 檢查目錄結構
ls -la ~/.claude/skills-lib/github-release-comprehensive/

# 應該看到：
# SKILL.md
# README.md
# LICENSE
# INSTALL.md
# examples/
```

### 3. 初始化（可選）

如果想在當前Claude Code session中立即使用：

```bash
# 在Claude Code中執行
cd ~/.claude/skills-lib/github-release-comprehensive
cat SKILL.md  # 驗證文件讀取
```

## GitHub CLI 配置

### 認證

```bash
# 登入GitHub
gh auth login

# 選擇:
# ? What is your preferred protocol for Git operations? HTTPS
# ? Authenticate to GitHub? Yes
# ? How would you like to authenticate GitHub CLI? Login with a web browser
```

### 驗證認證

```bash
gh auth status
# 應該看到: Logged in to github.com as <your-username>
```

## 使用驗證

### 快速測試

在Claude Code中執行：

```
/github-release-comprehensive test-project v1.0.0 100 "Release"
```

應該會看到Skill被識別並開始生成Release Notes。

### 檢查可用性

在Claude Code中：
```
有哪些skills可用？
```

應該在列表中看到 `github-release-comprehensive`

## 故障排查

### 問題1: Skill未被識別

**症狀**: `/github-release-comprehensive` 命令無法執行

**解決方案**:
```bash
# 驗證文件位置
ls ~/.claude/skills-lib/github-release-comprehensive/SKILL.md

# 確保權限正確
chmod 644 ~/.claude/skills-lib/github-release-comprehensive/SKILL.md

# 重啟Claude Code
# 或執行: /reload-skills (如果available)
```

### 問題2: GitHub CLI 不工作

**症狀**: `gh: command not found`

**解決方案**:
```bash
# 安裝GitHub CLI
# macOS
brew install gh

# Linux (Debian/Ubuntu)
sudo apt install gh

# Windows (使用Chocolatey)
choco install gh

# 或從 https://github.com/cli/cli 下載
```

驗證安裝：
```bash
gh --version
# 應該輸出: gh version X.X.X
```

### 問題3: GitHub 認證失敗

**症狀**: `gh: error: SomeError: authentication failed`

**解決方案**:
```bash
# 重新認證
gh auth logout
gh auth login

# 或使用token認證
gh auth login --with-token < token.txt
```

### 問題4: Release Notes 生成失敗

**症狀**: 執行Skill後沒有生成Release Notes

**檢查清單**:
- [ ] 項目中存在 `ROADMAP.md` 或 `README.md`
- [ ] 項目是git repository (`git status` 有效)
- [ ] 有足夠的commit歷史 (`git log --oneline`)
- [ ] `tests/` 目錄存在（用於計算覆蓋率）
- [ ] 當前目錄是項目根目錄

**調試步驟**:
```bash
# 檢查git history
git log --oneline -20

# 檢查文件結構
ls -la
# 應該看到: ROADMAP.md, README.md, tests/, src/ 等

# 檢查git統計
git log --stat | head -50
```

### 問題5: 無法推送到GitHub

**症狀**: `gh release create` 命令失敗

**可能原因**:
```bash
# 1. 檢查遠程repo
git remote -v
# 應該看到 origin 指向正確的repo

# 2. 檢查tag是否已存在
git tag -l | grep <version>

# 3. 檢查權限
gh repo view --json nameWithOwner
```

**解決方案**:
```bash
# 確認repo信息
gh repo view

# 如果有conflict，先備份notes
cp release-notes.md release-notes.backup.md

# 重新認證
gh auth logout
gh auth login
```

## 進階配置

### 自訂Skill行為

編輯 `SKILL.md` 中的「生成Release Notes結構」段落來自訂輸出格式。

### 集成其他工具

可以與以下工具集成：
- **Conventional Commits**: 自動解析commit類型
- **Semantic Versioning**: 驗證version格式
- **GitHub Actions**: 自動化Release流程

### 環境變數（可選）

```bash
# 設定default GitHub用戶
export GH_USER=<your-username>

# 設定default editor
export EDITOR=nano
```

## 更新Skill

### 檢查更新

```bash
cd ~/.claude/skills-lib/github-release-comprehensive
git remote -v
git fetch origin
```

### 應用更新

```bash
git pull origin main

# 驗證更新
cat SKILL.md | head -20
```

## 卸載

```bash
# 移除Skill
rm -rf ~/.claude/skills-lib/github-release-comprehensive

# 驗證卸載
ls ~/.claude/skills-lib/ | grep github-release
# 應該沒有結果
```

## 獲得幫助

如遇問題：

1. **檢查日誌** — Claude Code通常會在console輸出錯誤信息
2. **查看範例** — `examples/` 目錄有完整範例
3. **閱讀文檔** — README.md 和 SKILL.md 有詳細說明
4. **提交Issue** — https://github.com/<your-username>/skill-github-release-comprehensive/issues

提交Issue時，包括：
```
- 系統信息: `uname -a` 或 Windows版本
- Claude Code版本: `/version` 或設定中查看
- gh版本: `gh --version`
- 錯誤信息完整副本
- 重現步驟
```

## 進階使用

### 批量生成Release Notes

```bash
# 如果想為多個項目生成Release Notes：

for project in project1 project2 project3; do
  cd ~/$project
  echo "Generating release for $project..."
  /github-release-comprehensive $project <version> <completion> <phase>
done
```

### 集成到CI/CD

在GitHub Actions中使用：

```yaml
name: Generate Release Notes
on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Generate Release Notes
        run: |
          gh release create ${{ github.ref_name }} \
            --generate-notes \
            --draft
```

---

**最後更新**: 2026-04-27  
**版本**: 1.0
