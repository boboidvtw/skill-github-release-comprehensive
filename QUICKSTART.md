# 快速開始 (5 分鐘)

## 安裝 (1 分鐘)

```bash
# 1. Clone到Skill目錄
git clone https://github.com/<your-username>/skill-github-release-comprehensive.git
cp -r skill-github-release-comprehensive ~/.claude/skills-lib/

# 2. 驗證安裝
ls ~/.claude/skills-lib/github-release-comprehensive/SKILL.md
# ✅ 看到檔案 = 成功
```

## 基本使用 (2 分鐘)

### Step 1: 進入你的項目目錄
```bash
cd ~/my-awesome-project
```

確保項目有：
- ✅ `ROADMAP.md` 或 `README.md`
- ✅ `tests/` 目錄
- ✅ `git log` 可用

### Step 2: 在Claude Code中調用

```bash
/github-release-comprehensive my-awesome-project v2.1.0 89 "Phase 2 Complete"
```

### Step 3: 填寫信息（互動式）

Claude Code會引導你填寫：
```
1. 前一版本（v2.0.0）
2. 新增功能（6+項）
3. 代碼統計（自動計算）
4. 已知限制（待Phase 3修復）
5. Roadmap規劃（Phase 3, 4, v3.0）
```

### Step 4: 自動生成 & 推送

```
✅ Release Notes生成完成 (900+ 字)
✅ 推送到GitHub
✅ Verify: https://github.com/<user>/<repo>/releases/tag/v2.1.0
```

## 完整範例（3 分鐘）

假設你在一個React項目上完成了v2.0.0：

### 準備
```bash
cd ~/my-react-app

# 確認項目狀態
git status          # ✅ 所有changes已commit
git log --oneline   # ✅ 看到commit歷史
ls ROADMAP.md       # ✅ 存在
ls tests/           # ✅ 存在且有測試檔案
```

### 執行Skill
```bash
# 在Claude Code中
/github-release-comprehensive my-react-app v2.0.0 92 "Stable Release"
```

### 互動內容示例

**Claude**: 我需要以下信息來生成Release Notes...

**你**: 提供這些信息
```yaml
前一版本: v1.5.0
完成度: 92%

新功能:
  1. 暗黑模式支援
  2. 實時協作編輯
  3. 性能優化 (3x faster)

代碼統計:
  新增: 2345行
  測試覆蓋: 85%
  breaking changes: 1 (v1 API deprecated)

已知限制:
  - 移動端UI (v2.1 修復)
  - Safari兼容性 (v2.1 修復)

Roadmap:
  Phase 3 (2-3周): 移動端完善
  Phase 4 (4-6周): 國際化
  v3.0 (2-3月): AI助手集成
```

### 自動輸出
```markdown
# v2.0.0 Released 🚀

**完成度**: 92/100
**Phase**: Stable Release
**日期**: 2026-04-27

## 功能矩陣

| 功能 | v1.5.0 | v2.0.0 |
|---|---|---|
| 暗黑模式 | ❌ | ✅ |
| 協作編輯 | ❌ | ✅ |
| 性能 | baseline | 3x faster |

## 代碼統計

+2345 行新增
- src/ui: +892行
- src/collab: +624行
- tests/: +489行
- docs: +340行

## 新增功能

### 1. 暗黑模式
完整CSS變數主題系統，自動根據系統偏好切換...

### 2. 實時協作
WebSocket + OT算法，支援多用戶同時編輯...

### 3. 性能優化
Code splitting + Tree shaking，首頁加載從2.8s→1.4s...

## 已知限制

- 移動端UI (計劃 v2.1)
- Safari兼容性 (計劃 v2.1)

## 路線圖

### Phase 3 (2-3周)
- 移動端UI完善
- Safari修復
- Accessibility改進

### Phase 4 (4-6周)
- 國際化支援
- 簡體中文翻譯
- 自訂主題編輯器

### v3.0 (2-3月)
- AI助手集成
- 企業SSO支援
- 雲端備份

...
```

### 推送到GitHub
```bash
gh release create v2.0.0 --notes-file release-notes.md

# ✅ Release created successfully
# 🔗 https://github.com/<user>/<repo>/releases/tag/v2.0.0
```

## 常見問題

**Q: 需要手動填寫所有信息嗎？**  
A: 代碼統計會自動計算。需要手動提供的只有新功能和Roadmap規劃。

**Q: 可以編輯生成的Release Notes嗎？**  
A: 可以。編輯 `release-notes.md` 檔案後再推送。

**Q: 已經推送的Release可以編輯嗎？**  
A: 可以。使用 `gh release edit v2.0.0 --notes-file release-notes.md`

**Q: 不確定寫什麼？**  
A: 查看 `examples/` 目錄中的範例，複製並修改。

## 下一步

1. 查看 `examples/webapp-example.yaml` 理解完整結構
2. 閱讀 `SKILL.md` 了解高級功能
3. 查看 `INSTALL.md` 故障排查

## 獲得幫助

- 📖 閱讀 [README.md](./README.md)
- 🔧 查看 [INSTALL.md](./INSTALL.md) 故障排查
- 📝 查看 [examples/](./examples/) 範例
- 🐛 提交 [Issue](https://github.com/<your-username>/skill-github-release-comprehensive/issues)

---

就這麼簡單！🎉

5分鐘內你就能生成專業的GitHub Release Notes。
