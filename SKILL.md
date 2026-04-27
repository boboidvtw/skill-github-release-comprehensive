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

### 3. GitHub推送 (5 min)
```bash
gh release create <tag> \
  --title "Release <version>" \
  --notes-file release-notes.md
```

### 4. 驗證 (5 min)
```bash
# 檢視GitHub頁面
# 確認Notes完整且格式正確
# 驗證資源鏈接
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

在推送前驗證：
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
