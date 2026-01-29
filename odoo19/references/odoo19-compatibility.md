# Odoo 19 相容性問題與視圖注意事項

本文件記錄 Odoo 19 的重要變更、不相容語法、以及視圖開發時的注意事項。

## 目錄

1. [XML 資料檔案格式](#xml-資料檔案格式)
2. [ir.cron 排程任務](#ircron-排程任務)
3. [欄位定義 states 參數棄用](#欄位定義-states-參數棄用)
4. [搜尋視圖 (Search View)](#搜尋視圖-search-view)
5. [列表視圖 (List View)](#列表視圖-list-view)
6. [看板視圖無障礙性要求 (Kanban Accessibility)](#看板視圖無障礙性要求-kanban-accessibility)
7. [郵件範本 (Mail Template)](#郵件範本-mail-template)
8. [權限群組 (Security Groups)](#權限群組-security-groups)
9. [SQL 約束](#sql-約束)
10. [ORM 棄用方法](#orm-棄用方法)
11. [API 裝飾器棄用](#api-裝飾器棄用)
12. [track_visibility 棄用](#track_visibility-棄用)
13. [ir.values 模型移除](#irvalues-模型移除)
14. [其他常見問題](#其他常見問題)

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

---

## 開發建議

1. **在升級模組前，先驗證所有 XML 檔案**
2. **使用 Odoo 官方模組作為參考範本**
3. **注意 Odoo 版本變更日誌**
4. **測試環境中先進行升級測試**
5. **使用 `--log-handler=odoo.tools.convert:DEBUG` 取得詳細錯誤訊息**

## 參考資源

- [Odoo 19 Release Notes](https://www.odoo.com/documentation/19.0/developer/reference/release_notes.html)
- [Odoo Views Reference](https://www.odoo.com/documentation/19.0/developer/reference/backend/views.html)
- [Odoo ORM Reference](https://www.odoo.com/documentation/19.0/developer/reference/backend/orm.html)
