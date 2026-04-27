# skill-github-release-comprehensive

自動化生成專業的GitHub Release Notes — 適用於任何項目。

## 功能

✨ **一鍵生成Release Notes**
- 版本概述與完成度評估
- 功能矩陣與版本對比表
- 詳細代碼統計與檔案清單
- 新功能說明（含代碼片段）
- 已知限制與版本規劃
- 多階段Roadmap（Phase規劃）
- 測試驗證清單
- 貢獻指南

📊 **生成結構**
```
Release Notes (900-1200 字)
├─ 版本概述 & 完成度
├─ 功能矩陣對比 (vs前一版本)
├─ 代碼統計 (+新增行數, 檔案數)
├─ 6+ 新功能詳解 (含範例)
├─ 技術亮點與架構改進
├─ 已知限制 & 版本規劃
├─ Phase 3/4/v1.0 完整路線圖
├─ 測試驗證清單
├─ 貢獻指南
└─ 聯絡與授權信息
```

## 安裝

### 前置要求
- Claude Code (with extended context)
- GitHub CLI (`gh` command)
- Git repository with ROADMAP.md and tests/

### 步驟

**1. 複製到Skill目錄**
```bash
git clone https://github.com/<your-username>/skill-github-release-comprehensive.git
cp -r skill-github-release-comprehensive ~/.claude/skills-lib/
```

**2. 驗證安裝**
```bash
# 在Claude Code中檢查
ls ~/.claude/skills-lib/github-release-comprehensive/
# 應看到: SKILL.md, README.md, LICENSE, examples/
```

## 使用

### 基本用法

在Claude Code中：
```bash
/github-release-comprehensive <project-name> <version> <completion%> <phase>
```

### 範例

```bash
# 發佈Web應用
/github-release-comprehensive my-webapp v2.1.0 89 "Phase 2 Complete"

# 發佈Python庫
/github-release-comprehensive data-lib v1.5.0 78 "Stable Release"

# 發佈遊戲引擎
/github-release-comprehensive game-engine v3.0.0 92 "Release Candidate"
```

### 互動模式

```bash
Claude Code會根據以下信息生成Release Notes：

1. 項目名稱與版本
2. 完成度評估 (e.g., 87/100)
3. 當前Phase (e.g., Phase 2)
4. 前一版本對比
5. 新增功能清單
6. 已知限制
7. Roadmap規劃
8. 貢獻指南

輸入這些信息後，Skill自動生成900+ 字的Release Notes
並推送到GitHub。
```

## 快速開始範例

假設你完成了v2.0.0版本，完成度92%：

### Step 1: 準備項目文件

```
my-project/
├── ROADMAP.md          # 含Phase規劃
├── README.md           # 快速開始指南
├── tests/              # 測試檔案
├── git history         # 含commit信息
└── src/                # 代碼
```

### Step 2: 在Claude Code中調用

```
/github-release-comprehensive my-project v2.0.0 92 "Phase 2 Complete"
```

### Step 3: 提供信息

```yaml
前一版本: v1.5.0

新功能:
  - 實時同步 (WebSocket整合)
  - 暗黑模式 (CSS變數系統)
  - 性能優化 (3x faster)

代碼統計:
  新增: 2345 行
  測試覆蓋: 85%

已知限制:
  - 移動端UI (待v2.1改善)
  - 國際化支援 (計劃Phase 3)

Roadmap:
  Phase 3: 浏覽器擴展支援 (1-2周)
  Phase 4: 雲端同步 (2-4周)
  v3.0: AI推薦引擎 (2-3月)
```

### Step 4: 自動生成

Skill會生成：

```markdown
# v2.0.0 Released 🚀

**完成度**: 92/100 (+5 vs v1.5.0)
**Phase**: Phase 2 Complete
**日期**: 2026-04-27

## 新功能矩陣

| 功能 | v1.5.0 | v2.0.0 | 說明 |
|---|---|---|---|
| 實時同步 | ❌ | ✅ | WebSocket + Redis |
| 暗黑模式 | ❌ | ✅ | CSS變數系統 |
| 性能優化 | - | ✅ | 3x faster rendering |

## 代碼統計

```
+2345 行新增代碼
  • src/realtime: +892 行
  • src/ui: +624 行
  • tests/: +489 行
  • docs: +340 行
```

[更多詳情...] 
```

### Step 5: 推送到GitHub

```bash
gh release create v2.0.0 --notes-file release-notes.md
```

## 配置選項

在調用時可傳入YAML配置：

```yaml
project_name: "my-app"
version: "v2.0.0"
completion_percentage: 92
current_phase: "Phase 2 Complete"
previous_version: "v1.5.0"

code_stats:
  added_lines: 2345
  deleted_lines: 156
  test_coverage: 85%
  breaking_changes: 2

features:
  - name: "實時同步"
    description: "支援多設備同步"
    code_example: "src/realtime/sync.ts:1-50"
  
  - name: "暗黑模式"
    description: "完整的CSS變數主題系統"

known_limitations:
  - name: "移動端UI"
    description: "基礎響應式，待完善"
    planned_version: "v2.1"
    timeline: "1-2周"

roadmap_phases:
  Phase 3:
    timeline: "1-2周"
    items:
      - "移動端完善"
      - "離線模式"
      - "數據導出"
  
  Phase 4:
    timeline: "2-4周"
    items:
      - "AI推薦"
      - "團隊協作"

contact_info:
  github_issues: "https://github.com/<user>/<repo>/issues"
  email: "contact@example.com"
  maintainers: ["Alice", "Bob"]
```

## 範例項目

查看 `examples/` 目錄獲得完整範例：

- `examples/webapp-example.yaml` — Web應用
- `examples/library-example.yaml` — Python/JavaScript庫
- `examples/gameengine-example.yaml` — 遊戲引擎
- `examples/cli-tool-example.yaml` — CLI工具

## 常見問題

**Q: 如何自動計算代碼統計？**

A: Skill會自動讀取git log和tests/目錄。你只需確保：
- `git log` 可用（已提交的commits）
- `tests/` 目錄存在
- ROADMAP.md 包含Phase規劃

**Q: 可以自訂Release Notes格式嗎？**

A: 可以。編輯 `SKILL.md` 的「生成Release Notes結構」段落，Skill會根據修改調整輸出。

**Q: 支援哪些語言？**

A: 當前支援繁體中文、簡體中文、英文。其他語言可通過提交PR添加。

**Q: 如何處理breaking changes？**

A: 在 `code_stats` 中設定 `breaking_changes: <數字>`，Skill會在Release Notes中特別強調。

## 貢獻

歡迎提交Issue和PR！

1. Fork此repo
2. 創建feature branch (`git checkout -b feature/your-feature`)
3. 提交changes (`git commit -m 'Add feature'`)
4. Push到branch (`git push origin feature/your-feature`)
5. 開PR

## 授權

MIT License — 詳見 [LICENSE](./LICENSE)

## 相關資源

- [GitHub CLI 文檔](https://cli.github.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Semantic Versioning](https://semver.org/)

## 支持

遇到問題？

1. 檢查 [examples/](./examples/) 目錄
2. 查看 [INSTALL.md](./INSTALL.md) 故障排查
3. 提交 [GitHub Issue](https://github.com/<your-username>/skill-github-release-comprehensive/issues)

---

**版本**: 1.0  
**授權**: MIT  
**更新**: 2026-04-27
