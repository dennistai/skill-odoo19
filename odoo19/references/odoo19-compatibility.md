# Odoo 19 相容性問題與視圖注意事項

本文件記錄 Odoo 19 的重要變更、不相容語法、以及視圖開發時的注意事項。

## 目錄

1. [XML 資料檔案格式](#xml-資料檔案格式)
2. [attrs 屬性棄用](#attrs-屬性棄用)
3. [ir.cron 排程任務](#ircron-排程任務)
4. [欄位定義 states 參數棄用](#欄位定義-states-參數棄用)
5. [搜尋視圖 (Search View)](#搜尋視圖-search-view)
6. [列表視圖 (List View)](#列表視圖-list-view)
7. [看板視圖無障礙性要求 (Kanban Accessibility)](#看板視圖無障礙性要求-kanban-accessibility)
8. [設定視圖新結構 (Settings View)](#設定視圖新結構-settings-view)
9. [Chatter 簡化語法](#chatter-簡化語法)
10. [郵件範本 (Mail Template)](#郵件範本-mail-template)
11. [權限群組 (Security Groups)](#權限群組-security-groups)
12. [SQL 約束](#sql-約束)
13. [ORM 棄用方法](#orm-棄用方法)
14. [API 裝飾器棄用](#api-裝飾器棄用)
15. [track_visibility 棄用](#track_visibility-棄用)
16. [ir.values 模型移除](#irvalues-模型移除)
17. [res.partner.title 模型移除](#respartnertitle-模型移除)
18. [欄位屬性變更](#欄位屬性變更)
19. [外部 API 端點變更](#外部-api-端點變更)
20. [Controller 類型變更](#controller-類型變更)
21. [庫存模組變更](#庫存模組變更)
22. [環境變數存取棄用](#環境變數存取棄用)
23. [匯入路徑變更](#匯入路徑變更)
24. [Domain API 變更](#domain-api-變更)
25. [新增功能與裝飾器](#新增功能與裝飾器)
26. [其他常見問題](#其他常見問題)

---

## XML 資料檔案格式

### 正確格式

Odoo 19 支援以下兩種 XML 格式：

**格式 1：使用 `<data>` 包裝（推薦用於需要 noupdate 的資料）**
```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo>
    <data noupdate="1">
        <record id="..." model="...">
            ...
        </record>
    </data>
</odoo>
```

**格式 2：直接使用 `<odoo>`（用於不需要 noupdate 的資料）**
```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo>
    <record id="..." model="...">
        ...
    </record>
</odoo>
```

### 重要限制

**`type="html"` 與 CDATA 不能同時使用**

錯誤示範：
```xml
<field name="body_html" type="html">
<![CDATA[
<div>內容</div>
]]>
</field>
```

正確做法：
```xml
<!-- 選項 1：使用 type="html" 但不用 CDATA -->
<field name="body_html" type="html">
<div>內容</div>
</field>

<!-- 選項 2：使用 CDATA 但不用 type="html" -->
<field name="body_html"><![CDATA[<div>內容</div>]]></field>
```

---

## ir.cron 排程任務

### Odoo 19 已移除的欄位

以下欄位在 Odoo 19 的 `ir.cron` 模型中已被移除：

| 欄位 | 說明 |
|------|------|
| `numbercall` | 已移除，不再支援執行次數限制 |
| `doall` | 已移除 |
| `active` | 改用預設值，不需要在 XML 中指定 |

### 正確的 ir.cron 定義

```xml
<record id="ir_cron_my_task" model="ir.cron">
    <field name="name">我的排程任務</field>
    <field name="model_id" ref="model_my_model"/>
    <field name="state">code</field>
    <field name="code">model._cron_my_method()</field>
    <field name="interval_number">1</field>
    <field name="interval_type">hours</field>
</record>
```

### 可用的欄位

- `name` - 任務名稱
- `model_id` - 模型參考
- `state` - 執行類型 (`code`)
- `code` - 執行的程式碼
- `interval_number` - 間隔數值
- `interval_type` - 間隔類型 (`minutes`, `hours`, `days`, `weeks`, `months`)
- `nextcall` - 下次執行時間（可選）
- `user_id` - 執行使用者（可選）

---

## attrs 屬性棄用

### 重要變更

**Odoo 16+ 已不再支援 XML 視圖中的 `attrs` 屬性。** 原本使用 `attrs` 統一設定的 `readonly`、`invisible`、`required` 條件，必須改為獨立的屬性搭配動態表達式。

### 舊的寫法（已棄用）

```xml
<!-- ❌ 已棄用 - attrs 屬性不再支援 -->
<field name="partner_id"
       attrs="{'invisible': [('state', '=', 'done')], 'readonly': [('state', '!=', 'draft')]}"/>

<field name="amount"
       attrs="{'required': [('state', '=', 'confirmed')]}"/>

<div attrs="{'invisible': [('is_manager', '=', False)]}">
    <field name="manager_note"/>
</div>
```

### 新的寫法（Odoo 16+）

```xml
<!-- ✅ 正確 - 使用獨立屬性搭配動態表達式 -->
<field name="partner_id"
       invisible="state == 'done'"
       readonly="state != 'draft'"/>

<field name="amount"
       required="state == 'confirmed'"/>

<div invisible="not is_manager">
    <field name="manager_note"/>
</div>
```

### 語法轉換對照

| 舊語法 (attrs domain) | 新語法 (動態表達式) |
|----------------------|-------------------|
| `[('state', '=', 'done')]` | `state == 'done'` |
| `[('state', '!=', 'draft')]` | `state != 'draft'` |
| `[('state', 'in', ['confirmed', 'done'])]` | `state in ['confirmed', 'done']` |
| `[('is_manager', '=', False)]` | `not is_manager` |
| `[('state', '=', 'done'), ('amount', '>', 0)]` | `state == 'done' and amount > 0` |
| `['|', ('state', '=', 'a'), ('state', '=', 'b')]` | `state == 'a' or state == 'b'` |

### 注意事項

1. **動態表達式使用 Python 語法**：不再使用 domain 格式的 list of tuples，而是使用類 Python 的布林表達式
2. **XML 中的特殊字元**：`<` 需寫成 `&lt;`，`>` 需寫成 `&gt;`
3. **所有 XML 元素均適用**：`<field>`、`<div>`、`<group>`、`<page>` 等元素都使用獨立屬性
4. **繼承視圖修改**：使用 `<attribute name="invisible">...</attribute>` 修改動態屬性

### 常見錯誤訊息

| 錯誤訊息 | 原因 | 解決方案 |
|---------|------|---------|
| `Unknown attribute 'attrs'` | 使用了已棄用的 attrs 屬性 | 將 attrs 拆分為獨立的 `readonly`/`invisible`/`required` 屬性 |

---

## 欄位定義 states 參數棄用

### 重要變更

**Odoo 19 已不再支援欄位定義中的 `states` 參數。** 這是一個重大變更，需要將原本在 Python 模型中定義的狀態相依屬性，改為在視圖 (View) 中使用動態表達式控制。

### 舊的寫法（已棄用）

在 Odoo 18 及更早版本中，可以在欄位定義中使用 `states` 參數：

```python
from odoo import models, fields

class SaleOrder(models.Model):
    _name = 'sale.order'

    state = fields.Selection([
        ('draft', '草稿'),
        ('confirmed', '確認'),
        ('done', '完成'),
    ], default='draft')

    # ❌ 已棄用 - Odoo 19 不再支援
    partner_id = fields.Many2one(
        'res.partner',
        string='客戶',
        states={'confirmed': [('readonly', True)], 'done': [('readonly', True)]}
    )

    # ❌ 已棄用
    order_line = fields.One2many(
        'sale.order.line', 'order_id',
        states={'draft': [('readonly', False)]}
    )

    # ❌ 已棄用
    amount_total = fields.Monetary(
        states={'draft': [('invisible', True)]}
    )
```

### 新的寫法（Odoo 19）

改為在視圖 XML 中使用 `readonly`、`invisible`、`required` 屬性搭配動態表達式：

**Python 模型（只定義欄位，不使用 states）：**

```python
from odoo import models, fields

class SaleOrder(models.Model):
    _name = 'sale.order'

    state = fields.Selection([
        ('draft', '草稿'),
        ('confirmed', '確認'),
        ('done', '完成'),
    ], default='draft')

    # ✅ 正確 - 不使用 states 參數
    partner_id = fields.Many2one('res.partner', string='客戶')
    order_line = fields.One2many('sale.order.line', 'order_id')
    amount_total = fields.Monetary()
```

**視圖 XML（使用動態表達式控制）：**

```xml
<record id="view_sale_order_form" model="ir.ui.view">
    <field name="name">sale.order.form</field>
    <field name="model">sale.order</field>
    <field name="arch" type="xml">
        <form string="銷售訂單">
            <header>
                <field name="state" widget="statusbar"/>
            </header>
            <sheet>
                <!-- ✅ 使用 readonly 動態表達式 -->
                <field name="partner_id" readonly="state != 'draft'"/>

                <!-- ✅ 列表欄位的控制 -->
                <field name="order_line" readonly="state != 'draft'">
                    <list editable="bottom">
                        <field name="product_id"/>
                        <field name="quantity"/>
                    </list>
                </field>

                <!-- ✅ 使用 invisible 動態表達式 -->
                <field name="amount_total" invisible="state == 'draft'"/>
            </sheet>
        </form>
    </field>
</record>
```

### 動態表達式語法

| 表達式 | 說明 |
|--------|------|
| `readonly="state != 'draft'"` | 當狀態不是草稿時，欄位唯讀 |
| `readonly="state in ['confirmed', 'done']"` | 當狀態是確認或完成時，欄位唯讀 |
| `invisible="state == 'draft'"` | 當狀態是草稿時，隱藏欄位 |
| `required="state == 'confirmed'"` | 當狀態是確認時，欄位必填 |
| `readonly="not is_manager"` | 根據布林欄位控制 |
| `readonly="amount_total > 10000"` | 根據數值比較控制 |

### 複合條件

可以使用 `and`、`or`、`not` 組合多個條件：

```xml
<!-- 只有草稿狀態且使用者是管理員時可編輯 -->
<field name="special_field" readonly="state != 'draft' or not is_manager"/>

<!-- 確認狀態且金額超過 10000 時顯示 -->
<field name="approval_note" invisible="state != 'confirmed' or amount_total &lt;= 10000"/>
```

### 注意事項

1. **XML 中的比較運算子**：在 XML 屬性中，`<` 需要寫成 `&lt;`，`>` 需要寫成 `&gt;`
2. **欄位必須在視圖中存在**：動態表達式中引用的欄位必須在同一視圖中定義（可以是 `invisible="1"` 的隱藏欄位）
3. **繼承視圖的處理**：如果繼承視圖需要修改這些屬性，使用 `<attribute name="readonly">...</attribute>`

### 繼承視圖中修改動態屬性

```xml
<record id="view_sale_order_form_inherit" model="ir.ui.view">
    <field name="name">sale.order.form.inherit</field>
    <field name="model">sale.order</field>
    <field name="inherit_id" ref="sale.view_sale_order_form"/>
    <field name="arch" type="xml">
        <!-- 修改現有欄位的 readonly 屬性 -->
        <xpath expr="//field[@name='partner_id']" position="attributes">
            <attribute name="readonly">state not in ['draft', 'sent']</attribute>
        </xpath>
    </field>
</record>
```

### 常見錯誤訊息

| 錯誤訊息 | 原因 | 解決方案 |
|---------|------|---------|
| `Unknown field attribute 'states'` | 使用了已棄用的 states 參數 | 移除 states，改在視圖中使用動態表達式 |
| `Invalid expression in attribute` | 動態表達式語法錯誤 | 檢查欄位名稱和比較運算子 |

---

## 搜尋視圖 (Search View)

### 不支援的屬性

**`<filter>` 元素不支援：**

| 屬性 | 說明 |
|------|------|
| `icon` | 不能在 filter 上使用圖示 |
| `decoration-*` | 不能在 filter 上使用裝飾（如 `decoration-danger`） |

**`<group>` 元素不支援：**

| 屬性 | 說明 |
|------|------|
| `expand` | 已移除，不能使用 `expand="0"` |
| `string` | 分組不需要標題文字 |

### group_by 篩選器必須包含 domain

錯誤示範：
```xml
<group expand="0" string="分組">
    <filter name="group_state" string="狀態" context="{'group_by': 'state'}"/>
</group>
```

正確做法：
```xml
<group>
    <filter name="group_state" string="狀態" domain="[]" context="{'group_by': 'state'}"/>
</group>
```

### 完整的搜尋視圖範例

```xml
<record id="view_my_model_search" model="ir.ui.view">
    <field name="name">my.model.search</field>
    <field name="model">my.model</field>
    <field name="arch" type="xml">
        <search string="搜尋">
            <!-- 搜尋欄位 -->
            <field name="name"/>
            <field name="partner_id"/>

            <!-- 篩選器 -->
            <separator/>
            <filter name="filter_active" string="啟用" domain="[('active', '=', True)]"/>
            <filter name="filter_inactive" string="停用" domain="[('active', '=', False)]"/>

            <!-- 分組（注意：沒有 expand 和 string 屬性） -->
            <group>
                <filter name="group_state" string="狀態" domain="[]" context="{'group_by': 'state'}"/>
                <filter name="group_date" string="日期" domain="[]" context="{'group_by': 'create_date:month'}"/>
            </group>
        </search>
    </field>
</record>
```

---

## 列表視圖 (List View)

### 標籤變更

Odoo 18+ 開始，`<tree>` 標籤已改為 `<list>`：

錯誤示範：
```xml
<tree string="列表">
    <field name="name"/>
</tree>
```

正確做法：
```xml
<list string="列表">
    <field name="name"/>
</list>
```

### column_invisible 屬性

**在 list 視圖中，隱藏整個欄位（整列）必須使用 `column_invisible`，而非 `invisible`。**

`invisible` 在 list 視圖中只會隱藏單一儲存格的內容，而 `column_invisible` 會隱藏整個欄位標題和所有列的該欄位。

錯誤示範：
```xml
<list>
    <!-- ❌ invisible 只會隱藏儲存格內容，欄位標題仍會顯示 -->
    <field name="internal_code" invisible="not is_admin"/>
</list>
```

正確做法：
```xml
<list>
    <!-- ✅ column_invisible 會隱藏整個欄位（含標題） -->
    <field name="internal_code" column_invisible="not is_admin"/>

    <!-- ✅ 也可以用固定值隱藏 -->
    <field name="debug_info" column_invisible="True"/>
</list>
```

### 按鈕的 role 屬性

帶有 `btn` class 的 `<a>` 標籤建議加上 `role="button"`：

```xml
<a class="btn btn-primary" role="button" t-att-href="...">連結</a>
```

---

## 看板視圖無障礙性要求 (Kanban Accessibility)

Odoo 19 強化了無障礙性 (Accessibility) 檢查，在看板視圖 (Kanban View) 中需要特別注意以下要求。

### FontAwesome 圖示必須有 title 屬性

**問題：** `<i>` 標籤帶有 FontAwesome class（如 `fa fa-phone`）必須有 `title` 屬性，用於輔助閱讀器識別。

**警告訊息：**
```
WARNING odoo.addons.base.models.ir_ui_view: A <i> with fa class (fa fa-phone) must have title in its tag, parents, descendants or have text
```

**錯誤示範：**
```xml
<div class="text-muted small mb-2">
    <i class="fa fa-phone"/> <field name="phone"/>
</div>
```

**正確做法：**
```xml
<div class="text-muted small mb-2">
    <i class="fa fa-phone" title="電話"/> <field name="phone"/>
</div>
```

**常見圖示與建議 title：**

| FontAwesome Class | 建議 title |
|-------------------|-----------|
| `fa fa-phone` | 電話 |
| `fa fa-envelope` | 電子郵件 |
| `fa fa-map-marker` | 地址 |
| `fa fa-calendar` | 日期 |
| `fa fa-user` | 使用者 |
| `fa fa-clock-o` | 時間 |
| `fa fa-building` | 公司 |
| `fa fa-tags` | 標籤 |

### 帶有 btn class 的連結必須有 role="button"

**問題：** `<a>` 標籤帶有 `btn` class 時，必須加上 `role="button"` 屬性。

**警告訊息：**
```
WARNING odoo.addons.base.models.ir_ui_view: "<a>" tag with "btn" class must have "button" role
```

**錯誤示範：**
```xml
<a class="dropdown-toggle o-no-caret btn" type="button"
   data-toggle="dropdown" href="#" title="Dropdown menu">
    <span class="fa fa-ellipsis-v"/>
</a>
```

**正確做法：**
```xml
<a class="dropdown-toggle o-no-caret btn" role="button"
   data-toggle="dropdown" href="#" title="Dropdown menu">
    <span class="fa fa-ellipsis-v"/>
</a>
```

### 完整的看板視圖範例

```xml
<record id="view_my_model_kanban" model="ir.ui.view">
    <field name="name">my.model.kanban</field>
    <field name="model">my.model</field>
    <field name="arch" type="xml">
        <kanban default_group_by="state" class="o_kanban_small_column">
            <field name="name"/>
            <field name="phone"/>
            <field name="state"/>
            <templates>
                <t t-name="card">
                    <div class="oe_kanban_global_click">
                        <!-- 下拉選單按鈕 -->
                        <div class="o_dropdown_kanban dropdown">
                            <!-- ✅ 使用 role="button" -->
                            <a class="dropdown-toggle o-no-caret btn" role="button"
                               data-toggle="dropdown" href="#"
                               aria-label="選單" title="選單">
                                <!-- ✅ span 內的圖示不需要 title，因為父元素已有 -->
                                <span class="fa fa-ellipsis-v"/>
                            </a>
                            <div class="dropdown-menu" role="menu">
                                <a type="edit" class="dropdown-item">編輯</a>
                                <a type="delete" class="dropdown-item">刪除</a>
                            </div>
                        </div>

                        <!-- 內容區 -->
                        <div class="o_kanban_record_body">
                            <strong><field name="name"/></strong>
                            <div class="text-muted small">
                                <!-- ✅ 獨立圖示需要 title -->
                                <i class="fa fa-phone" title="電話"/>
                                <field name="phone"/>
                            </div>
                        </div>
                    </div>
                </t>
            </templates>
        </kanban>
    </field>
</record>
```

### 無障礙性檢查清單

開發看板視圖時，請確認：

- [ ] 所有獨立的 `<i class="fa fa-*">` 元素都有 `title` 屬性
- [ ] 所有 `<a class="...btn...">` 元素都有 `role="button"` 屬性
- [ ] 下拉選單按鈕有 `aria-label` 或 `title` 屬性
- [ ] 互動元素有適當的文字描述

---

## 設定視圖新結構 (Settings View)

### 重要變更

**Odoo 19 引入新的設定視圖結構**，使用 `<app>`、`<block>` 和 `<setting>` 元素取代舊的巢狀 `<div>` 結構，使設定頁面更語義化且易維護。

### 舊的寫法（不建議）

```xml
<!-- ❌ 舊的設定視圖結構 -->
<record id="res_config_settings_view_form" model="ir.ui.view">
    <field name="name">res.config.settings.view.form.inherit</field>
    <field name="model">res.config.settings</field>
    <field name="inherit_id" ref="base.res_config_settings_view_form"/>
    <field name="arch" type="xml">
        <xpath expr="//div[hasclass('settings')]" position="inside">
            <div class="app_settings_block" data-string="My Module" data-key="my_module">
                <h2>General Settings</h2>
                <div class="row mt16 o_settings_container">
                    <div class="col-12 col-lg-6 o_setting_box">
                        <div class="o_setting_left_pane">
                            <field name="my_boolean"/>
                        </div>
                        <div class="o_setting_right_pane">
                            <label for="my_boolean"/>
                            <div class="text-muted">Description here</div>
                        </div>
                    </div>
                </div>
            </div>
        </xpath>
    </field>
</record>
```

### 新的寫法（Odoo 19）

```xml
<!-- ✅ 新的設定視圖結構 -->
<record id="res_config_settings_view_form" model="ir.ui.view">
    <field name="name">res.config.settings.view.form.inherit</field>
    <field name="model">res.config.settings</field>
    <field name="inherit_id" ref="base.res_config_settings_view_form"/>
    <field name="arch" type="xml">
        <xpath expr="//form" position="inside">
            <app data-string="My Module" data-key="my_module">
                <block title="General Settings">
                    <setting string="Enable Feature" help="Description here">
                        <field name="my_boolean"/>
                    </setting>
                    <setting string="API Key" help="Enter your API key">
                        <field name="api_key"/>
                    </setting>
                </block>
                <block title="Advanced">
                    <setting string="Debug Mode" help="Enable debug logging">
                        <field name="debug_mode"/>
                    </setting>
                </block>
            </app>
        </xpath>
    </field>
</record>
```

### 元素對照

| 舊結構 | 新結構 | 說明 |
|--------|--------|------|
| `<div class="app_settings_block">` | `<app>` | 應用層級容器 |
| `<h2>` + `<div class="o_settings_container">` | `<block title="...">` | 設定區塊 |
| `<div class="o_setting_box">` + left/right pane | `<setting string="..." help="...">` | 單一設定項目 |

---

## Chatter 簡化語法

### 重要變更

**Odoo 19 支援使用簡化的 `<chatter/>` 標籤**取代舊的多個 `div` 和 widget 組合。

### 舊的寫法（繁瑣）

```xml
<!-- ❌ 舊的 Chatter 寫法 -->
<form>
    <sheet>
        <!-- 表單內容 -->
    </sheet>
    <div class="oe_chatter">
        <field name="message_follower_ids"/>
        <field name="activity_ids"/>
        <field name="message_ids"/>
    </div>
</form>
```

### 新的寫法（Odoo 19）

```xml
<!-- ✅ 新的 Chatter 簡化語法 -->
<form>
    <sheet>
        <!-- 表單內容 -->
    </sheet>
    <chatter/>
</form>
```

### 注意事項

1. `<chatter/>` 標籤必須放在 `<sheet>` 之後
2. 模型必須繼承 `mail.thread`（和可選的 `mail.activity.mixin`）
3. 簡化語法會自動包含追蹤者、活動和訊息記錄

---

## 郵件範本 (Mail Template)

### QWeb 語法變更

**Jinja2 語法與 QWeb 語法：**

在 `type="html"` 的欄位中，建議使用 QWeb 語法：

| Jinja2 (舊) | QWeb (新) |
|-------------|-----------|
| `{{ object.name }}` | `<t t-out="object.name">預設值</t>` |
| `{{ object.field or '預設' }}` | `<t t-out="object.field or '預設'">預設</t>` |

### 完整郵件範本範例

```xml
<record id="email_template_example" model="mail.template">
    <field name="name">範例郵件範本</field>
    <field name="model_id" ref="model_my_model"/>
    <field name="subject">主旨 - {{ object.name }}</field>
    <field name="email_from">{{ (object.company_id.email or user.email_formatted) }}</field>
    <field name="email_to">{{ object.partner_id.email }}</field>
    <field name="body_html" type="html">
<div style="margin: 0px; padding: 0px;">
    <p>親愛的 <t t-out="object.partner_id.name">客戶</t>，</p>
    <p>您的訂單 <t t-out="object.name">SO001</t> 已確認。</p>
    <p>金額：<t t-out="object.amount_total">0.00</t></p>
</div>
    </field>
    <field name="auto_delete">False</field>
</record>
```

---

## 權限群組 (Security Groups)

### Odoo 19 新架構

Odoo 19 使用 `res.groups.privilege` 取代舊的 `category_id`：

**舊的寫法（不建議）：**
```xml
<record id="group_user" model="res.groups">
    <field name="name">使用者</field>
    <field name="category_id" ref="module_category_my_module"/>
</record>
```

**新的寫法（Odoo 19）：**
```xml
<!-- 定義權限層級 -->
<record id="res_groups_privilege_my_module" model="res.groups.privilege">
    <field name="name">My Module</field>
    <field name="sequence">50</field>
    <field name="category_id" ref="module_category_my_module"/>
</record>

<!-- 定義群組 -->
<record id="group_user" model="res.groups">
    <field name="name">使用者</field>
    <field name="sequence">10</field>
    <field name="privilege_id" ref="res_groups_privilege_my_module"/>
    <field name="implied_ids" eval="[(4, ref('base.group_user'))]"/>
</record>
```

---

## SQL 約束

### 重要變更

**Odoo 19 已完全棄用 `_sql_constraints` 屬性**，使用此屬性會產生以下警告訊息：

```
WARNING odoo.registry: Model attribute '_sql_constraints' is no longer supported,
please define model.Constraint on the model.
```

更重要的是，使用 `_sql_constraints` 定義的約束**不會實際生效**，必須改用 `models.Constraint`。

### 語法對照

| 項目 | 舊語法 (`_sql_constraints`) | 新語法 (`models.Constraint`) |
|------|---------------------------|------------------------------|
| 參數順序 | `(名稱, SQL條件, 錯誤訊息)` | `(SQL條件, 名稱, 錯誤訊息)` |
| 屬性名稱 | `_sql_constraints` | `_constraints` |

### 舊的寫法（已棄用，不會生效）

```python
class MyModel(models.Model):
    _name = 'my.model'

    # ❌ 已棄用 - 約束不會生效！
    _sql_constraints = [
        ('name_unique', 'UNIQUE(name)', '名稱必須唯一！'),
        ('code_company_unique', 'UNIQUE(code, company_id)', '同一公司內代碼不可重複！'),
    ]
```

### 新的寫法（Odoo 19）

```python
from odoo import models, fields

class MyModel(models.Model):
    _name = 'my.model'

    name = fields.Char('名稱', required=True)
    code = fields.Char('代碼', required=True)
    company_id = fields.Many2one('res.company', string='公司')

    # ✅ 正確 - 使用 models.Constraint
    _constraints = [
        models.Constraint(
            'UNIQUE(name)',           # SQL 約束條件
            'name_unique',            # 約束名稱（用於資料庫）
            '名稱必須唯一！',          # 錯誤訊息
        ),
        models.Constraint(
            'UNIQUE(code, company_id)',
            'code_company_unique',
            '同一公司內代碼不可重複！',
        ),
    ]
```

### models.Constraint 參數說明

```python
models.Constraint(
    sql_condition,    # 第一參數：SQL 約束條件（如 UNIQUE, CHECK 等）
    constraint_name,  # 第二參數：約束名稱（在資料庫中的識別名稱）
    error_message,    # 第三參數：違反約束時顯示的錯誤訊息
)
```

### 常見約束類型範例

```python
_constraints = [
    # UNIQUE 約束 - 單一欄位
    models.Constraint(
        'UNIQUE(code)',
        'code_unique',
        '代碼必須唯一！',
    ),

    # UNIQUE 約束 - 複合欄位
    models.Constraint(
        'UNIQUE(security_id, price_date)',
        'security_date_unique',
        '同一證券在同一日期只能有一筆記錄！',
    ),

    # UNIQUE 約束 - 包含公司欄位（多公司環境）
    models.Constraint(
        'UNIQUE(code, company_id)',
        'code_company_unique',
        '同一公司內代碼不可重複！',
    ),

    # CHECK 約束 - 數值範圍
    models.Constraint(
        'CHECK(quantity >= 0)',
        'quantity_positive',
        '數量不能為負數！',
    ),

    # CHECK 約束 - 日期邏輯
    models.Constraint(
        'CHECK(end_date >= start_date)',
        'date_check',
        '結束日期必須大於或等於開始日期！',
    ),
]
```

### 轉換檢查清單

升級到 Odoo 19 時，請檢查並轉換所有使用 `_sql_constraints` 的模型：

1. 搜尋專案中所有 `_sql_constraints`：
   ```bash
   grep -r "_sql_constraints" custom_addons/
   ```

2. 將每個約束從舊格式轉換為新格式：
   - 舊：`('name', 'SQL', 'msg')` → 新：`models.Constraint('SQL', 'name', 'msg')`

3. 將屬性名稱從 `_sql_constraints` 改為 `_constraints`

4. 重新啟動 Odoo 並更新模組

---

## ORM 棄用方法

### read_group() 已棄用

**從 Odoo 19 開始**，`read_group()` 方法已被棄用，改用 `_read_group()` 進行後端處理，或使用 `formatted_read_group()` 作為格式化的公開 API。

#### 舊的寫法（已棄用）

```python
# ❌ 已棄用
result = self.env['sale.order'].read_group(
    domain=[('state', '=', 'sale')],
    fields=['partner_id', 'amount_total:sum'],
    groupby=['partner_id']
)
```

#### 新的寫法（Odoo 19）

```python
# ✅ 正確 - 後端使用 _read_group()
result = self.env['sale.order']._read_group(
    domain=[('state', '=', 'sale')],
    groupby=['partner_id'],
    aggregates=['amount_total:sum']
)

# ✅ 正確 - 公開 API 使用 formatted_read_group()
result = self.env['sale.order'].formatted_read_group(
    domain=[('state', '=', 'sale')],
    groupby=['partner_id'],
    aggregates=['amount_total:sum']
)
```

#### 新增方法：GROUPING SETS 最佳化

Odoo 19 新增 `_read_grouping_sets()` 和 `formatted_read_grouping_sets()` 方法，可將多個分組請求合併為單一 SQL 查詢，大幅提升效能：

```python
# ✅ 使用 GROUPING SETS 進行多維度分組
result = self.env['sale.order']._read_grouping_sets(
    domain=[('state', '=', 'sale')],
    groupby=[['partner_id'], ['partner_id', 'user_id']],
    aggregates=['amount_total:sum']
)
```

---

### name_get() 已棄用

**從 Odoo 17 開始**，`name_get()` 方法已被棄用，改為直接讀取 `display_name` 欄位。

#### 舊的寫法（已棄用）

```python
# ❌ 已棄用
class MyModel(models.Model):
    _name = 'my.model'

    def name_get(self):
        result = []
        for record in self:
            name = f'{record.code} - {record.name}'
            result.append((record.id, name))
        return result
```

#### 新的寫法（Odoo 17+）

```python
# ✅ 正確 - 覆寫 _compute_display_name()
from odoo import models, api

class MyModel(models.Model):
    _name = 'my.model'

    @api.depends('code', 'name')
    def _compute_display_name(self):
        for record in self:
            record.display_name = f'{record.code} - {record.name}'
```

#### 存取方式變更

```python
# ❌ 舊寫法
name = record.name_get()[0][1]

# ✅ 新寫法
name = record.display_name
```

---

### _flush_search() 已棄用

**從 Odoo 19 開始**，`_flush_search()` 方法已被棄用。欄位的 flush 操作現在由 `execute_query()` 自動處理，基於 `_search()` 及其他底層 ORM 方法在 SQL 物件中放置的元資料。

#### 舊的寫法（已棄用）

```python
# ❌ 已棄用 - 不需要手動呼叫
self._flush_search(domain)
```

#### 新的寫法（Odoo 19）

```python
# ✅ 正確 - 由 execute_query() 自動處理
# 不需要手動呼叫 flush，ORM 會自動處理
query = self._search(domain)
result = self.env.execute_query(query)
```

---

### inselect 運算子移除

**從 Odoo 19 開始**，內部運算子 `inselect` 已被移除，改用 `in` 搭配 Query 或 SQL 物件。

#### 舊的寫法（已移除）

```python
# ❌ 已移除
domain = [('id', 'inselect', subquery)]
```

#### 新的寫法（Odoo 19）

```python
# ✅ 正確 - 使用 in 搭配 Query 物件
from odoo.osv.expression import Query

subquery = self.env['other.model']._search([('active', '=', True)])
domain = [('id', 'in', subquery)]
```

---

### _check_recursion() 已棄用

**從 Odoo 18.0 開始**，`_check_recursion()` 方法已被棄用，改用 `_has_cycle()` 方法。

**重要差異：邏輯相反**

| 方法 | 返回 True 的情況 |
|------|-----------------|
| `_check_recursion()` (舊) | **沒有**循環時返回 True |
| `_has_cycle()` (新) | **有**循環時返回 True |

### 舊的寫法（已棄用）

```python
from odoo import models, api, _
from odoo.exceptions import ValidationError

class Category(models.Model):
    _name = 'my.category'

    parent_id = fields.Many2one('my.category', string='父級分類')

    @api.constrains('parent_id')
    def _check_category_recursion(self):
        """檢查是否有循環層級關係"""
        # ❌ 已棄用 - 會產生 DeprecationWarning
        if not self._check_recursion():
            raise ValidationError(_('錯誤！不能創建循環的分類層次。'))
```

### 新的寫法（Odoo 18+）

```python
from odoo import models, api, _
from odoo.exceptions import ValidationError

class Category(models.Model):
    _name = 'my.category'

    parent_id = fields.Many2one('my.category', string='父級分類')

    @api.constrains('parent_id')
    def _check_category_recursion(self):
        """檢查是否有循環層級關係"""
        # ✅ 正確 - 使用新的 _has_cycle() 方法
        if self._has_cycle():
            raise ValidationError(_('錯誤！不能創建循環的分類層次。'))
```

### 棄用警告訊息

如果繼續使用 `_check_recursion()`，會看到以下警告：

```
DeprecationWarning: Deprecated since 18.0, use _has_cycle() instead
```

### 適用場景

此方法主要用於具有父子層級關係（樹狀結構）的模型，例如：
- 分類（Categories）
- 部門（Departments）
- 帳戶科目（Chart of Accounts）
- 任何使用 `_parent_store = True` 的模型

---

## API 裝飾器棄用

### @api.multi 和 @api.one 已完全移除

**從 Odoo 13 開始**，`@api.multi` 和 `@api.one` 裝飾器已被移除。在 Odoo 19 中使用這些裝飾器會導致程式錯誤。

### 舊的寫法（已移除，會報錯）

```python
from odoo import models, api

class MyModel(models.Model):
    _name = 'my.model'

    # ❌ 已移除 - 會報錯！
    @api.multi
    def action_confirm(self):
        for record in self:
            record.state = 'confirmed'

    # ❌ 已移除 - 會報錯！
    @api.one
    def compute_total(self):
        self.total = self.quantity * self.price
```

### 新的寫法（Odoo 13+）

```python
from odoo import models, api

class MyModel(models.Model):
    _name = 'my.model'

    # ✅ 正確 - 不需要裝飾器，預設就是 multi 行為
    def action_confirm(self):
        for record in self:
            record.state = 'confirmed'

    # ✅ 正確 - 使用 @api.depends 進行計算欄位
    @api.depends('quantity', 'price')
    def _compute_total(self):
        for record in self:
            record.total = record.quantity * record.price
```

### 重要說明

| 裝飾器 | 移除版本 | 說明 |
|--------|----------|------|
| `@api.multi` | Odoo 13 | 不再需要，方法預設就是處理 recordset |
| `@api.one` | Odoo 13 | 改用 `for record in self:` 迴圈處理每筆記錄 |

### 錯誤訊息

使用已移除的裝飾器會看到類似以下錯誤：

```
AttributeError: module 'odoo.api' has no attribute 'multi'
AttributeError: module 'odoo.api' has no attribute 'one'
```

---

## track_visibility 棄用

### 參數名稱變更

**從 Odoo 13 開始**，欄位的 `track_visibility` 參數已改名為 `tracking`。

### 舊的寫法（已棄用）

```python
from odoo import models, fields

class MyModel(models.Model):
    _name = 'my.model'
    _inherit = ['mail.thread']

    # ❌ 已棄用
    name = fields.Char('名稱', track_visibility='always')
    state = fields.Selection([...], track_visibility='onchange')
    partner_id = fields.Many2one('res.partner', track_visibility='onchange')
```

### 新的寫法（Odoo 13+）

```python
from odoo import models, fields

class MyModel(models.Model):
    _name = 'my.model'
    _inherit = ['mail.thread']

    # ✅ 正確 - 使用 tracking 參數
    name = fields.Char('名稱', tracking=True)
    state = fields.Selection([...], tracking=True)
    partner_id = fields.Many2one('res.partner', tracking=True)
```

### 參數對照

| 舊參數 | 新參數 | 說明 |
|--------|--------|------|
| `track_visibility='always'` | `tracking=True` | 總是追蹤變更 |
| `track_visibility='onchange'` | `tracking=True` | 變更時追蹤 |
| `track_visibility=False` | `tracking=False` 或不設定 | 不追蹤 |

### 注意事項

1. 使用 `tracking` 參數的模型必須繼承 `mail.thread`
2. 追蹤的變更會顯示在記錄的聊天記錄（Chatter）中
3. `tracking=True` 統一取代了舊的 `'always'` 和 `'onchange'` 值

---

## ir.values 模型移除

### 重要變更

**從 Odoo 11 開始**，`ir.values` 模型已被完全移除。此模型原本用於定義：
- 預設值（Default values）
- 動作綁定（Action bindings）
- 選單項目（Menu items）

### 舊的用法（已移除）

```xml
<!-- ❌ 已移除 - 會報錯！ -->
<record id="action_my_report" model="ir.values">
    <field name="key2">client_print_multi</field>
    <field name="model">sale.order</field>
    <field name="name">My Report</field>
    <field name="value" eval="'ir.actions.report.xml,%d' % ref('my_report')"/>
</record>
```

```python
# ❌ 已移除 - 會報錯！
self.env['ir.values'].set_default('sale.order', 'note', 'Default note')
```

### 新的替代方案

#### 1. 動作綁定 - 使用 binding_model_id

```xml
<!-- ✅ 正確 - 使用 binding_model_id -->
<record id="action_my_report" model="ir.actions.report">
    <field name="name">My Report</field>
    <field name="model">sale.order</field>
    <field name="report_type">qweb-pdf</field>
    <field name="report_name">my_module.report_template</field>
    <field name="binding_model_id" ref="sale.model_sale_order"/>
    <field name="binding_type">report</field>
</record>

<!-- Server Action 綁定 -->
<record id="action_my_server_action" model="ir.actions.server">
    <field name="name">My Action</field>
    <field name="model_id" ref="sale.model_sale_order"/>
    <field name="binding_model_id" ref="sale.model_sale_order"/>
    <field name="binding_type">action</field>
    <field name="state">code</field>
    <field name="code">records.action_confirm()</field>
</record>
```

#### 2. 預設值 - 使用 default_get 或 context

```python
# ✅ 正確 - 使用 default_get 方法
class SaleOrder(models.Model):
    _inherit = 'sale.order'

    @api.model
    def default_get(self, fields_list):
        defaults = super().default_get(fields_list)
        if 'note' in fields_list:
            defaults['note'] = 'Default note'
        return defaults

# 或使用欄位的 default 參數
note = fields.Text('Note', default='Default note')
```

```xml
<!-- ✅ 正確 - 使用 context 設定預設值 -->
<record id="action_open_form" model="ir.actions.act_window">
    <field name="name">New Order</field>
    <field name="res_model">sale.order</field>
    <field name="view_mode">form</field>
    <field name="context">{'default_note': 'Default note'}</field>
</record>
```

### binding_type 值說明

| binding_type | 說明 | 顯示位置 |
|--------------|------|----------|
| `action` | Server Action 綁定 | 動作（Action）選單 |
| `report` | 報表綁定 | 列印（Print）選單 |

### 錯誤訊息

嘗試使用 `ir.values` 會看到以下錯誤：

```
KeyError: 'ir.values'
psycopg2.errors.UndefinedTable: relation "ir_values" does not exist
```

---

## res.partner.title 模型移除

### 重要變更

**Odoo 19 已移除 `res.partner.title` 模型。** 此模型原本用於儲存聯絡人的稱謂（如「先生」、「女士」、「博士」等）。

### 影響範圍

- 所有參照 `res.partner.title` 的程式碼和 XML 資料都需要移除或替換
- `res.partner` 上的 `title` 欄位（`Many2one` 至 `res.partner.title`）已不再可用

### 檢查方式

```bash
# 搜尋所有參照 res.partner.title 的程式碼
grep -rn "res.partner.title" --include="*.py" --include="*.xml" ./
```

### 錯誤訊息

```
KeyError: 'res.partner.title'
psycopg2.errors.UndefinedTable: relation "res_partner_title" does not exist
```

---

## 欄位屬性變更

### group_operator 重新命名為 aggregator

**從 Odoo 18 開始**，欄位的 `group_operator` 屬性已重新命名為 `aggregator`。

#### 舊的寫法（已棄用）

```python
from odoo import models, fields

class MyModel(models.Model):
    _name = 'my.model'

    # ❌ 已棄用
    amount = fields.Float('金額', group_operator='sum')
    quantity = fields.Integer('數量', group_operator='avg')
```

#### 新的寫法（Odoo 18+）

```python
from odoo import models, fields

class MyModel(models.Model):
    _name = 'my.model'

    # ✅ 正確 - 使用 aggregator
    amount = fields.Float('金額', aggregator='sum')
    quantity = fields.Integer('數量', aggregator='avg')
```

#### 可用的聚合運算子

| aggregator 值 | 說明 |
|--------------|------|
| `sum` | 加總 |
| `avg` | 平均 |
| `min` | 最小值 |
| `max` | 最大值 |
| `count` | 計數 |
| `count_distinct` | 不重複計數 |
| `bool_and` | 布林 AND |
| `bool_or` | 布林 OR |
| `array_agg` | 陣列聚合 |

### One2many / Many2many 移除 limit 參數

**從 Odoo 19 開始**，`One2many` 和 `Many2many` 欄位不再支援 `limit` 參數。

#### 舊的寫法（已移除）

```python
from odoo import models, fields

class MyModel(models.Model):
    _name = 'my.model'

    # ❌ 已移除 - limit 參數不再支援
    order_line_ids = fields.One2many(
        'sale.order.line', 'order_id',
        string='訂單明細',
        limit=80
    )

    tag_ids = fields.Many2many(
        'my.tag',
        string='標籤',
        limit=50
    )
```

#### 新的寫法（Odoo 19）

```python
from odoo import models, fields

class MyModel(models.Model):
    _name = 'my.model'

    # ✅ 正確 - 移除 limit 參數
    order_line_ids = fields.One2many(
        'sale.order.line', 'order_id',
        string='訂單明細',
    )

    tag_ids = fields.Many2many(
        'my.tag',
        string='標籤',
    )
```

#### 注意事項

如需限制顯示數量，應在視圖層級或查詢時處理，而非在欄位定義中設定。

---

## 外部 API 端點變更

### XML-RPC 和 JSON-RPC 端點已棄用

**重要變更：** Odoo 19 棄用了傳統的 `/xmlrpc`、`/xmlrpc/2` 和 `/jsonrpc` 端點，並以新的 `/json/2` 端點取代。

#### 棄用時程

| 版本 | 狀態 |
|------|------|
| Odoo 19.0 | 標記為棄用 |
| Odoo 19.1 (SaaS) | 移除 |
| Odoo 20 (on-prem/Odoo.sh) | 完全移除 |

#### 舊的端點（已棄用）

```python
# ❌ 已棄用 - XML-RPC
import xmlrpc.client
common = xmlrpc.client.ServerProxy('http://localhost:8069/xmlrpc/2/common')
uid = common.authenticate(db, username, password, {})

# ❌ 已棄用 - JSON-RPC
url = 'http://localhost:8069/jsonrpc'
```

#### 新的端點（Odoo 19）

```python
# ✅ 正確 - 使用 JSON-2 API
import requests

url = 'http://localhost:8069/json/2/res.partner/search_read'
headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_API_KEY',
    'X-Odoo-Database': 'your_database'  # 多資料庫時需要
}
payload = {
    'domain': [('is_company', '=', True)],
    'fields': ['name', 'email'],
    'limit': 10
}
response = requests.post(url, json=payload, headers=headers)
```

### JSON-2 API 主要變更

| 項目 | 舊 API | JSON-2 API |
|------|--------|------------|
| 端點格式 | `/xmlrpc/2/object` | `/json/2/<model>/<method>` |
| 認證方式 | 帳號/密碼 | API 金鑰 (Bearer Token) |
| 參數傳遞 | 位置參數 | 僅命名參數 |
| HTTP 狀態碼 | 總是 200 | 有意義的狀態碼 (404, 500 等) |
| 交易處理 | 可鏈接多個呼叫 | 每次呼叫獨立交易 |

### API 金鑰要求

- 一般使用者的 API 金鑰預設有 90 天期限
- 管理員可建立永久金鑰
- 解決雙因素驗證 (2FA) 在機器對機器場景的問題

### 自動生成文件

Odoo 19 在 `/doc` 路徑提供自動生成的 API 文件，可瀏覽欄位、公開方法，並複製範例程式碼。

---

## Controller 類型變更

### type='json' 重新命名為 type='jsonrpc'

**從 Odoo 19 開始**，Controller 的 `type='json'` 已重新命名為 `type='jsonrpc'`。

**注意：** 此變更僅影響自訂 Controller，不受外部 API 端點棄用影響。

#### 舊的寫法（已棄用）

```python
from odoo import http

class MyController(http.Controller):

    # ❌ 已棄用
    @http.route('/my/api/endpoint', type='json', auth='user')
    def my_endpoint(self, **kwargs):
        return {'status': 'success'}
```

#### 新的寫法（Odoo 19）

```python
from odoo import http

class MyController(http.Controller):

    # ✅ 正確 - 使用 type='jsonrpc'
    @http.route('/my/api/endpoint', type='jsonrpc', auth='user')
    def my_endpoint(self, **kwargs):
        return {'status': 'success'}
```

---

## 庫存模組變更

### procurement.group 已移除

**重要變更：** Odoo 19 移除了 `procurement.group` 模型，改用 `stock.reference`。

**警告：** 這不只是名稱變更，內部邏輯也有重大改變。

#### 影響範圍

| 項目 | Odoo 18 及更早版本 | Odoo 19 |
|------|-------------------|---------|
| 採購群組模型 | `procurement.group` | `stock.reference` |
| 補貨報表 UI | 可見「採購群組」欄位 | 已移除該欄位 |
| MTO 追蹤 | 透過 procurement.group | 透過 stock.reference |

#### 程式碼遷移

```python
# ❌ Odoo 18 及更早版本
procurement_group = self.env['procurement.group'].create({
    'name': self.name,
    'partner_id': self.partner_id.id,
})

# ✅ Odoo 19 - 需要根據實際情況調整
# stock.reference 的使用方式可能不同，請參考官方文件
```

#### 補貨規則變更

- 「傳播採購群組」設定在 Odoo 19 中已變更
- 建立新 RFQ（而非加入現有 RFQ）的設定方式不同

### 新增 mrp.production.group 模型

Odoo 19 新增 `mrp.production.group` 模型，用於組織製造訂單的父子結構：

```python
# ✅ Odoo 19 - 新模型
production_group = self.env['mrp.production.group'].create({
    'name': 'Production Group',
})
```

### 庫存估價變更

Odoo 19 對自動化/即時庫存估價進行了重大變更。如果您使用庫存估價功能，升級前務必完整了解其影響。

---

## 環境變數存取棄用

### record._cr, _context, _uid 已棄用

**從 Odoo 19 開始**，直接存取 `record._cr`、`record._context`、`record._uid` 已被棄用，應改用 `self.env` 屬性。

#### 舊的寫法（已棄用）

```python
# ❌ 已棄用
cursor = self._cr
context = self._context
user_id = self._uid
```

#### 新的寫法（Odoo 19）

```python
# ✅ 正確 - 透過 self.env 存取
cursor = self.env.cr
context = self.env.context
user_id = self.env.uid

# 或使用完整路徑
user = self.env.user
company = self.env.company
```

#### 環境屬性對照

| 舊屬性 | 新屬性 | 說明 |
|--------|--------|------|
| `self._cr` | `self.env.cr` | 資料庫游標 |
| `self._context` | `self.env.context` | 上下文字典 |
| `self._uid` | `self.env.uid` | 使用者 ID |
| N/A | `self.env.user` | 使用者記錄 |
| N/A | `self.env.company` | 當前公司 |
| N/A | `self.env.companies` | 允許的公司 |

---

## 匯入路徑變更

### 重要變更

**Odoo 19 多個匯入路徑已變更或移除**，升級時需要更新所有受影響的 `import` 語句。

### 匯入路徑對照表

| 舊匯入路徑 | 新匯入路徑 | 說明 |
|-----------|-----------|------|
| `from odoo import registry` | `from odoo.modules.registry import Registry` | Registry 類別移至 modules 子模組 |
| `from odoo.tools.misc import xlsxwriter` | `import xlsxwriter` | xlsxwriter 改為直接匯入第三方套件 |
| `from odoo.osv import expression` | `from odoo import Domain` | 改用新的 Domain API（見下一章節） |
| `from odoo.osv.expression import ...` | `from odoo.domain import ...` | expression 模組已棄用 |

### 舊的寫法（已棄用）

```python
# ❌ 已棄用
from odoo import registry
from odoo.tools.misc import xlsxwriter
from odoo.osv import expression
```

### 新的寫法（Odoo 19）

```python
# ✅ 正確
from odoo.modules.registry import Registry
import xlsxwriter
from odoo import Domain
```

### 檢查方式

```bash
# 搜尋需要更新的匯入路徑
grep -rn "from odoo import registry" --include="*.py" ./
grep -rn "from odoo.tools.misc import xlsxwriter" --include="*.py" ./
grep -rn "from odoo.osv" --include="*.py" ./
```

---

## Domain API 變更

### odoo.osv 已棄用

**從 Odoo 19 開始**，`odoo.osv` 模組已被棄用，改用新的 `odoo.domain` 和 `odoo.Domain` API。

#### 舊的寫法（已棄用）

```python
# ❌ 已棄用
from odoo.osv import expression

domain1 = [('active', '=', True)]
domain2 = [('state', '=', 'done')]
combined = expression.AND([domain1, domain2])
```

#### 新的寫法（Odoo 19）

```python
# ✅ 正確 - 使用新的 Domain API
from odoo.fields import Domain
# 或
from odoo import Domain

domain1 = Domain([('active', '=', True)])
domain2 = Domain([('state', '=', 'done')])
combined = domain1 & domain2  # AND 運算
combined_or = domain1 | domain2  # OR 運算
```

### Domain 最佳化

Odoo 19 在執行 `Fields.search` 方法前會套用 Domain 最佳化：

- `=` 運算子現在等同於 `in`（單一值時）
- Domain 條件會自動簡化

### _where_calc 變更

```python
# ❌ 舊寫法
query = self._where_calc(domain)

# ✅ 新寫法
query = self._search(domain, bypass_access=True)
```

### _apply_ir_rules 移除

`_apply_ir_rules` 已從 Odoo 19 移除，規則套用已整合進搜尋方法中。

---

## 新增功能與裝飾器

### @api.private 裝飾器

**Odoo 19 新增** `@api.private` 裝飾器，用於標記方法不可透過 RPC 呼叫。

```python
from odoo import models, api

class MyModel(models.Model):
    _name = 'my.model'

    # ✅ 此方法無法透過 XML-RPC 或 JSON-RPC 呼叫
    @api.private
    def internal_computation(self):
        # 僅供內部使用的邏輯
        return self._calculate_internal_value()

    # 傳統做法：使用底線前綴
    def _another_private_method(self):
        pass
```

#### 使用場景

- 將現有公開方法改為不可 RPC 呼叫
- ORM 內部方法
- 不適合重新命名（加底線）的方法

### PEP-420 原生命名空間

Odoo 19 採用 PEP-420 原生命名空間，允許 Odoo 模組分散在多個目錄中：

- 不再強制需要 `__init__.py` 於命名空間層級
- 模組可更靈活地分佈於 `PYTHONPATH` 中

### 新增 Domain 運算子

Odoo 19 新增了進階 Domain 運算子以支援複雜查詢邏輯：

```python
# 可以依據日期部分進行分組
domain = [('create_date:year', '=', 2025)]

# 支援相關欄位（無儲存）的分組/聚合/排序
result = self.env['sale.order']._read_group(
    domain=[],
    groupby=['partner_id.country_id'],  # 相關欄位
    aggregates=['amount_total:sum']
)
```

### GROUPING SETS 最佳化

Odoo 19 新增 `_read_grouping_sets()` 和 `formatted_read_grouping_sets()` 方法，可將多個分組請求合併為單一 SQL 查詢：

```python
# ✅ 使用 GROUPING SETS 進行多維度分組
result = self.env['sale.order']._read_grouping_sets(
    domain=[('state', '=', 'sale')],
    groupby=[['partner_id'], ['partner_id', 'user_id']],
    aggregates=['amount_total:sum']
)
```

---

## 其他常見問題

### 1. active_id 存取問題

在視圖中使用 `active_id` 可能導致權限錯誤，建議改用 `id`：

錯誤示範：
```xml
<button type="action" context="{'default_record_id': active_id}"/>
```

正確做法：
```xml
<button type="action" context="{'default_record_id': id}"/>
```

### 2. 欄位類型不一致

確保相關欄位（related fields）的類型與原始欄位一致：

```python
# 原始欄位
satisfaction_rating = fields.Selection([
    ('1', '非常不滿意'),
    ('5', '非常滿意'),
], string='滿意度')

# 相關欄位必須也是 Selection
related_satisfaction = fields.Selection(
    related='complaint_id.satisfaction_rating',
    string='滿意度'
)
```

### 3. Datetime 欄位的日期篩選

在搜尋視圖中，Datetime 欄位的日期篩選語法：

```xml
<!-- 正確的日期分組 -->
<filter name="group_by_date" string="日期" domain="[]"
        context="{'group_by': 'create_date:month'}"/>
```

### 4. XML 驗證工具

使用 Python 驗證 XML 檔案：

```python
from lxml import etree

rng_path = '/path/to/odoo/odoo/import_xml.rng'
rng_doc = etree.parse(rng_path)
rng = etree.RelaxNG(rng_doc)

xml_doc = etree.parse('/path/to/your/file.xml')
try:
    rng.assertValid(xml_doc)
    print('XML 驗證通過')
except etree.DocumentInvalid as e:
    print(f'驗證失敗: {e}')
```

### 5. 常見錯誤訊息對照表

| 錯誤訊息 | 原因 | 解決方案 |
|---------|------|---------|
| `Element odoo has extra content: data` | type="html" 與 CDATA 同時使用 | 移除 CDATA 或移除 type="html" |
| `Invalid field 'numbercall' in 'ir.cron'` | ir.cron 欄位已移除 | 移除 numbercall, doall, active 欄位 |
| `Invalid attribute expand for element group` | 搜尋視圖 group 不支援 expand | 移除 expand 屬性 |
| `Element search has extra content: field` | 搜尋視圖結構錯誤 | 檢查 group_by filter 是否有 domain="[]" |
| `_sql_constraints is no longer supported` | 使用舊的約束語法（約束不會生效！） | 改用 `_constraints = [models.Constraint('SQL', 'name', 'msg')]`，注意參數順序變更 |
| `Unknown field attribute 'states'` | 欄位定義中使用了已棄用的 states 參數 | 移除 states，改在視圖中使用 readonly/invisible 動態表達式 |
| `DeprecationWarning: Deprecated since 18.0, use _has_cycle() instead` | 使用已棄用的 `_check_recursion()` | 改用 `_has_cycle()`，注意邏輯相反 |
| `A <i> with fa class must have title` | FontAwesome 圖示缺少無障礙性描述 | 加入 `title` 屬性，如 `<i class="fa fa-phone" title="電話"/>` |
| `"<a>" tag with "btn" class must have "button" role` | 按鈕連結缺少 role 屬性 | 加入 `role="button"` 屬性 |
| `AttributeError: module 'odoo.api' has no attribute 'multi'` | 使用已移除的 `@api.multi` 裝飾器 | 直接移除裝飾器，方法預設就是處理 recordset |
| `AttributeError: module 'odoo.api' has no attribute 'one'` | 使用已移除的 `@api.one` 裝飾器 | 移除裝飾器，改用 `for record in self:` 迴圈 |
| `Unknown field attribute 'track_visibility'` | 使用已棄用的 `track_visibility` 參數 | 改用 `tracking=True` |
| `KeyError: 'ir.values'` | 使用已移除的 `ir.values` 模型 | 改用 `binding_model_id` 或 `default_get` |
| `relation "ir_values" does not exist` | 資料庫中找不到 ir_values 表 | 移除所有 ir.values 相關程式碼 |
| `DeprecationWarning: read_group is deprecated` | 使用已棄用的 `read_group()` 方法 | 改用 `_read_group()` 或 `formatted_read_group()` |
| `DeprecationWarning: name_get is deprecated` | 使用已棄用的 `name_get()` 方法 | 改用 `display_name` 欄位或覆寫 `_compute_display_name()` |
| `Unknown field attribute 'group_operator'` | 使用已重新命名的 `group_operator` 參數 | 改用 `aggregator` |
| `DeprecationWarning: odoo.osv is deprecated` | 使用已棄用的 `odoo.osv` 模組 | 改用 `odoo.fields.Domain` 或 `odoo.Domain` |
| `DeprecationWarning: _cr is deprecated` | 使用已棄用的 `record._cr` | 改用 `self.env.cr` |
| `type='json' is deprecated` | Controller 使用已棄用的 `type='json'` | 改用 `type='jsonrpc'` |
| `KeyError: 'procurement.group'` | 使用已移除的 `procurement.group` 模型 | 改用 `stock.reference`（注意：邏輯有變更） |
| `/xmlrpc endpoint is deprecated` | 使用已棄用的 XML-RPC API | 改用 `/json/2` API 端點 |
| `Unknown attribute 'attrs'` | 使用了已棄用的 `attrs` 屬性 | 將 attrs 拆分為獨立的 `readonly`/`invisible`/`required` 動態表達式 |
| `KeyError: 'res.partner.title'` | 使用已移除的 `res.partner.title` 模型 | 移除所有 `res.partner.title` 相關程式碼 |
| `ImportError: cannot import name 'registry'` | `from odoo import registry` 匯入路徑已變更 | 改用 `from odoo.modules.registry import Registry` |
| `ImportError: cannot import name 'xlsxwriter'` | `from odoo.tools.misc import xlsxwriter` 已移除 | 改用 `import xlsxwriter` 直接匯入 |
| `Unknown field attribute 'limit'` | `One2many`/`Many2many` 的 `limit` 參數已移除 | 移除欄位定義中的 `limit` 參數 |

---

## 開發建議

1. **在升級模組前，先驗證所有 XML 檔案**
2. **使用 Odoo 官方模組作為參考範本**
3. **注意 Odoo 版本變更日誌**
4. **測試環境中先進行升級測試**
5. **使用 `--log-handler=odoo.tools.convert:DEBUG` 取得詳細錯誤訊息**

## 參考資源

- [Odoo 19 Release Notes](https://www.odoo.com/documentation/19.0/developer/reference/release_notes.html)
- [Odoo 19 ORM Changelog](https://www.odoo.com/documentation/19.0/developer/reference/backend/orm/changelog.html)
- [Odoo 19 ORM API Reference](https://www.odoo.com/documentation/19.0/developer/reference/backend/orm.html)
- [Odoo 19 Views Reference](https://www.odoo.com/documentation/19.0/developer/reference/backend/views.html)
- [Odoo 19 External JSON-2 API](https://www.odoo.com/documentation/19.0/developer/reference/external_api.html)
- [Odoo 19 External RPC API (Legacy)](https://www.odoo.com/documentation/19.0/developer/reference/external_rpc_api.html)
- [Odoo Migration Guide (Ksolves)](https://www.ksolves.com/blog/odoo/how-to-migrate-from-odoo-18-to-odoo-19-step-by-step-guide)
