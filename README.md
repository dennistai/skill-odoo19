# Odoo 19 Skill for Claude Code

Claude Code skill，提供 Odoo 19 ERP 官方文件與相容性參考資料。

## 用途

此 skill 適用於以下情境：

- 開發 Odoo 19 模組
- 自訂商業邏輯、ORM 模式
- 建立 views / controllers / QWeb templates
- 排查 Odoo 19 應用問題
- 從 Odoo 18 升級至 Odoo 19 的相容性修正

## 目錄結構

```
odoo19/
├── SKILL.md                          # Skill 定義與快速參考
└── references/
    ├── odoo19-compatibility.md       # Odoo 19 相容性問題與注意事項
    ├── controllers.md                # Controllers 文件
    ├── orm.md                        # ORM 文件
    ├── views.md                      # Views 文件
    ├── tutorials.md                  # Tutorials 文件
    └── index.md                      # 索引
```

## Odoo 19 相容性重點摘要

| 項目 | 變更說明 |
|------|----------|
| `attrs` 屬性 | 已棄用，改用獨立的 `readonly`/`invisible`/`required` 動態表達式 |
| `<tree>` 標籤 | 改為 `<list>` |
| `ir.cron` | 移除 `numbercall`、`doall`、`active` 欄位 |
| Search View `<group>` | 不支援 `expand` 和 `string` 屬性 |
| `_sql_constraints` | 已棄用，改用 `models.Constraint` |
| `group_operator` | 改為 `aggregator` |
| Settings View | 使用新的 `<app>`、`<block>`、`<setting>` 結構 |
| Chatter | 支援簡化的 `<chatter/>` 語法 |

完整說明請參閱 `odoo19/references/odoo19-compatibility.md`。

## 安裝

將此 repository clone 到 Claude Code skill 目錄即可使用：

```bash
git clone <repo-url> ~/.claude/skills/skill-odoo19
```

## 授權

本 skill 基於 Odoo 官方文件整理而成。
