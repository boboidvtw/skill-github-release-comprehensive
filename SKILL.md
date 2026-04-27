# GitHub Release 綜合文檔 Skill

自動化生成專業的GitHub Release Notes，適用於任何開源或商業項目。

## 用途

生成結構完整、內容豐富的Release Notes，包含：
- 版本概述與完成度評估
- 功能矩陣與版本對比
- 詳細的代碼統計
- 新功能說明與實現細節
- 已知限制與版本路線圖
- 多階段Phase規劃（Phase 3, 4, v1.0等）
- 測試驗證清單
- 貢獻指南與聯絡方式

## 快速開始

### 安裝方式

**方式1：複製到你的Skills目錄**
```bash
cp -r ./skill-github-release-comprehensive ~/.claude/skills-lib/
```

**方式2：Clone到本地**
```bash
git clone https://github.com/<your-username>/skill-github-release-comprehensive.git
cp -r skill-github-release-comprehensive ~/.claude/skills-lib/
```

### 基本使用

```bash
# 在Claude Code中調用
/github-release-comprehensive <project-name> <version> <completion%> <phase>

# 範例
/github-release-comprehensive my-awesome-app v1.0.0 92 "Phase 2 Complete"
```

### 自動觸發

當Claude Code檢測到Project完成重大phase時，會自動建議使用此Skill。

## 觸發條件

```
手動：使用上述命令或輸入：
"推送GitHub Release，使用綜合文檔格式"

自動：當完成重大Project phase時系統會建議
```

## 環境前置檢查（必須先執行）

**在任何步驟前，先執行以下檢查，根據結果決定執行模式：**

### 檢查腳本
```bash
echo "=== GitHub Release Skill 環境檢查 ==="

# 1. 確認在git repo中
if git rev-parse --is-inside-work-tree > /dev/null 2>&1; then
  echo "✅ git repo: OK"
else
  echo "❌ 不在git repo中 → 請先執行 git init 或切換到正確目錄"
  exit 1
fi

# 2. 確認gh CLI已安裝
if command -v gh > /dev/null 2>&1; then
  echo "✅ gh CLI: $(gh --version | head -1)"
else
  echo "⚠️  gh CLI未安裝 → 將只生成本地release-notes.md"
  GH_AVAILABLE=false
fi

# 3. 確認gh已認證（不彈出登入）
if [ "${GH_AVAILABLE}" != "false" ]; then
  if gh auth status > /dev/null 2>&1; then
    echo "✅ GitHub帳號: $(gh api user --jq .login 2>/dev/null || echo '已認證')"
  else
    echo "⚠️  GitHub未登入 → 將只生成本地release-notes.md"
    GH_AVAILABLE=false
  fi
fi

# 4. 確認網絡可達GitHub
if [ "${GH_AVAILABLE}" != "false" ]; then
  if gh api rate_limit > /dev/null 2>&1; then
    echo "✅ GitHub連線: OK"
  else
    echo "⚠️  無法連線GitHub → 將只生成本地release-notes.md"
    GH_AVAILABLE=false
  fi
fi

echo ""
if [ "${GH_AVAILABLE}" = "false" ]; then
  echo "📋 執行模式：本地模式（生成release-notes.md，不推送）"
else
  echo "🚀 執行模式：完整模式（生成 + 推送GitHub）"
fi
```

### 檢查結果與執行模式

| 狀態 | gh安裝 | gh認證 | 網絡 | 執行模式 |
|---|---|---|---|---|
| 完整模式 | ✅ | ✅ | ✅ | 生成 + 自動推送GitHub |
| 本地模式 | ❌ | - | - | 只生成release-notes.md |
| 本地模式 | ✅ | ❌ | - | 只生成release-notes.md |
| 本地模式 | ✅ | ✅ | ❌ | 只生成release-notes.md |

### 優雅降級處理

**完整模式**：生成 release-notes.md → 自動 `gh release create` → 顯示URL

**本地模式**（任一檢查失敗）：
```
✅ Release Notes已生成 → release-notes.md

若需推送到GitHub，請：

方式1: gh CLI（推薦）
  gh auth login
  gh release create <tag> --notes-file release-notes.md

方式2: GitHub網頁
  1. 前往 github.com/<user>/<repo>/releases/new
  2. 填入Tag版本號
  3. 複製 release-notes.md 內容貼入說明欄位
  4. 點擊 Publish release

方式3: 儲存備用
  release-notes.md 已保存於當前目錄，可稍後再推送
```

### 修復引導

若需切換至完整模式，Skill會提供對應修復步驟：

**未安裝gh CLI**：
```bash
# macOS
brew install gh

# Windows (Chocolatey)
choco install gh

# Linux
sudo apt install gh

# 或從官網下載: https://cli.github.com
```

**gh未認證**：
```bash
gh auth login
# 選擇 GitHub.com → HTTPS → Web browser
# 在瀏覽器中完成授權即可
```

**重新執行環境檢查**：
```bash
# 修復後再次確認
gh auth status
gh api user --jq .login
```

---

## 執行流程

### 第1步：蒐集項目信息
```
輸入：
  - 項目名稱
  - 版本號 (e.g., v0.2.0)
  - 當前完成度 (e.g., 87/100)
  - 當前Phase (e.g., Phase 2)

讀取檔案：
  - ROADMAP.md (取得Phase規劃)
  - README.md (取得功能列表)
  - tests/ (計算測試覆蓋)
  - git log (統計代碼變更)
```

### 第2步：生成Release Notes結構
```
1. 版本概述
   - 發佈日期
   - 版本標籤與完成度
   - 簡要亮點

2. 核心成果
   - Phase實施清單（✅/🟡/🔴）
   - 代碼統計（新增行數、檔案數）
   - 功能對比表（vs前一版本）

3. 新增功能詳解
   - 6+ 個主要功能
   - 含代碼片段與使用範例
   - 技術架構改進說明

4. 已知限制
   - 列出當前限制
   - 標註預計完成版本
   - 解釋為何延後

5. 多階段Roadmap
   - Phase 3 (高優先級，1-2周)
   - Phase 4 (中優先級，2-4周)
   - v1.0 (長期規劃，1-2月)
   - 各Phase包含具體任務

6. 測試驗證清單
   - 已完成測試項目
   - 待完成測試項目
   - 覆蓋率目標vs實現

7. 貢獻指南
   - Fork → Branch → Implement → Test → PR
   - 測試覆蓋要求
   - Commit格式規範
   - PR描述模板

8. 聯絡與授權
   - GitHub Issues鏈接
   - Email/聯絡方式
   - 授權信息
```

### 第3步：推送到GitHub
```bash
# 使用gh CLI推送Release
gh release create <tag> --notes-file release-notes.md

# 確保包含：
  - Release標題
  - 詳細Notes（900+ 字）
  - 相關資源鏈接
```

## 輸入/輸出

### 輸入參數
```yaml
project_name: "Nomad-Unified-Agent"
version: "v0.2.0"
completion_percentage: 87
current_phase: "Phase 2"
previous_version: "v0.1.0"

# 可選參數
code_stats:
  added_lines: 1162
  test_coverage: 45%
  
features:
  - name: "LLM自動偵測"
    description: "支援8+平台"
    code_example: "..."
    
known_limitations:
  - name: "前端UI"
    description: "基礎實現，缺少對話歷史"
    planned_version: "v0.3"
    
roadmap_phases:
  Phase 3:
    timeline: "1-2周"
    items:
      - "運行測試驗證"
      - "前端React完善"
  Phase 4:
    timeline: "2-4周"
    items:
      - "Docker容器化"
      - "性能優化"
```

### 輸出
```
GitHub Release Notes (900-1200 字)
├─ 版本概述
├─ 功能矩陣對比表
├─ 代碼統計與檔案清單
├─ 新功能詳解（6+項）
├─ 技術亮點
├─ 已知限制與版本規劃
├─ 多階段Roadmap (Phase 3-4-v1.0)
├─ 測試驗證清單
├─ 貢獻指南
└─ 聯絡資訊
```

## 實施步驟

### 1. 蒐集信息 (15 min)
```bash
# 讀取ROADMAP.md獲取Phase規劃
# 讀取git log計算統計
# 檢視tests/覆蓋率
# 確認新增功能列表
```

### 2. 生成Release Notes (30 min)
```bash
# 撰寫詳細的Release Notes文檔
# 包含所有必要段落與表格
# 驗證鏈接與引用正確性
```

### 3. 推送 (5 min)

**完整模式（GitHub可用）**：
```bash
gh release create <tag> \
  --title "Release <version>" \
  --notes-file release-notes.md

# 輸出：https://github.com/<user>/<repo>/releases/tag/<version>
```

**本地模式（GitHub不可用）**：
```
✅ release-notes.md 已儲存至當前目錄
📋 請稍後使用以下任一方式推送：
   - gh release create <tag> --notes-file release-notes.md
   - 前往 GitHub 網頁手動貼上內容
```

### 4. 驗證 (5 min)
```bash
# 完整模式：直接檢視返回的GitHub URL
# 本地模式：確認 release-notes.md 內容完整
cat release-notes.md | wc -l  # 應 > 50 行
```

## 使用範例

### 範例1：Web應用（MyAwesomeApp v1.0.0）

**輸入**：
```yaml
project: "MyAwesomeApp"
version: "v1.0.0"
completion: 92
phase: "Phase 2 Complete"
previous_version: "v0.9.0"

code_stats:
  added_lines: 2845
  test_coverage: 78%

features:
  - name: "用戶認證系統"
    description: "支援OAuth2 + JWT"
    code_example: "auth module in src/auth/"
  
  - name: "實時通知"
    description: "WebSocket推送"
    code_example: "socket.io integration"

known_limitations:
  - name: "移動端UI"
    description: "基礎響應式，待完善"
    planned_version: "v1.1"
```

**輸出**：900+ 字Release Notes，含功能矩陣、代碼統計、Phase規劃

### 範例2：Python庫（DataProcessor v2.0.0）

**輸入**：
```yaml
project: "DataProcessor"
version: "v2.0.0"
completion: 85
phase: "Phase 2 Stable"
previous_version: "v1.5.0"

code_stats:
  added_lines: 1250
  test_coverage: 82%
  breaking_changes: 3

features:
  - name: "並行處理"
  - name: "新的API v2"
  - name: "性能優化（3x faster）"

roadmap_phases:
  Phase 3:
    timeline: "2-4周"
    items:
      - "集成Dask分佈式"
      - "GPU加速支援"
```

**輸出**：完整Release Notes + 向後兼容指南

## 快速檢查清單

### 環境檢查（Skill自動執行）
- [ ] 在git repo中（`git status` 有效）
- [ ] gh CLI已安裝（`gh --version`）
- [ ] gh已認證（`gh auth status`）
- [ ] GitHub網絡可達（`gh api rate_limit`）

若上述任一失敗 → 自動切換本地模式，仍可生成release-notes.md

### 內容檢查（在生成前確認）
- [ ] ROADMAP.md 已更新為最新進度
- [ ] README.md 包含快速開始步驟
- [ ] 代碼統計準確（git log驗證）
- [ ] 測試覆蓋率已計算
- [ ] 已知限制清單完整
- [ ] Phase規劃具體且可達成
- [ ] 貢獻指南清晰
- [ ] GitHub鏈接正確
- [ ] Release標籤合法（e.g., v0.2.0）

## 相關Skill與工具

- `git-workflow` — Commit規範與PR流程
- `project-documentation` — 項目文檔標準化
- `testing-summary` — 測試驗證報告
- GitHub CLI (`gh release create`) — 自動推送Release

## 已知限制與後續改進

### 當前限制
- 需手動提供功能清單（未來可自動解析git log）
- 不支援自動生成性能基準對比
- 多語言支援需手動翻譯

### 已解決（v1.1）
- ✅ ~~GitHub不可用時卡住~~ → 加入前置環境檢查 + 優雅降級至本地模式

### 規劃中的改進
- [ ] 自動生成功能矩陣表格（比較git log）
- [ ] 自動計算代碼統計（git diff解析）
- [ ] 生成性能基準對比（如applicable）
- [ ] 集成Changelog自動生成
- [ ] 多語言Release Notes支援（簡體中文、英文等）
- [ ] 自動生成Contributors清單

## 貢獻

歡迎改進此Skill！請提出Issue或PR。

---

**版本**: 1.0  
**授權**: MIT  
**倉庫**: https://github.com/<your-username>/skill-github-release-comprehensive  
**維護者**: Claude Code Community
