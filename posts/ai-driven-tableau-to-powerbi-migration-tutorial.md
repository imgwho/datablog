---
date: 2025-10-19
title: 如何用 AI 将 Tableau 看板转为 Power BI 看板
tags:
  - Power BI
  - Tableau
  - AI
description: 摘要：本文是一篇详细的复盘教程，记录了如何通过与 AI 编程助手协作，将一个完整的 Tableau 看板迁移到 Power BI 的全过程。文章重点展示了在不手写任何代码的情况下，如何指挥 AI 分析需求、生成 .pbip 项目源码、并协同调试，最终实现报表的完美复刻。这不仅是一个 BI 迁移案例，更是对“BI 即代码”（BI as Code）和未来 AI 驱动开发模式的一次深度探索。
---

# 如何用 AI 将 Tableau 看板转为 Power BI 看板

## 前言：当 BI 开发遇上“代码时代”

在商业智能（BI）领域，我们早已习惯了通过图形化界面（GUI）拖拽字段、调整格式来构建仪表板。这种方式直观易用，但当面临复杂的报表迁移、批量修改或版本控制时，其“手工作坊”式的弊端便暴露无遗。

但是，随着 Power BI 推出 `.pbip` 项目格式，一切都变得不同了。它将报表的定义、数据模型、布局等所有元素，都以人类可读的文本文件（如 JSON, TMDL）形式存储。这为我们打开了一扇全新的大门——**“BI 即代码” (BI as Code)**。

在这篇文章中，我将完整复盘一次前沿实验：**在不写一行 Power BI 代码的前提下，我如何通过与 AI 编程助手对话，指挥它将一个功能完善的 Tableau 看板，完美迁移为 Power BI 报表。**

这不仅是一次从 A 工具到 B 工具的简单复刻，更是一场关于未来 BI 开发模式的深度探索。

---

## 第一章：我们的目标与工具

### 1.1 迁移目标

我们的任务是将一个经典的 Tableau "Superstore" 销售看板，迁移到 Power BI 中。

**原始 Tableau 看板:**
![Tableau Superstore Dashboard](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/tableau%20superstore.jpg)

**最终 Power BI 成果:**
![Power BI Superstore Dashboard](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/powerbi%20superstore.jpg)

这个看板看似简单，但五脏俱全，涵盖了 BI 看板的核心元素：
- **KPI 指标卡**：7 个核心业务指标。
- **地理可视化**：按州/省展示销售额和利润率的气泡地图。
- **时间序列分析**：两个按月度展示销售趋势的面积图。
- **交互式筛选器**：四个不同维度的切片器。

### 1.2 核心工具

1.  **AI 编程助手**：Claude Code
2.  **Power BI Project (`.pbip`)**：我们的“画布”，它允许我们通过修改源代码来定义报表的一切。

---

## 第二章：人机协作的四幕剧

整个过程就像一出四幕剧，充满了规划、尝试、挫折和最终的成功。

### 第一幕：需求分析与 AI 的蓝图

一切始于一个清晰的指令。我将 Tableau 的源文件、截图和数据一并交给 AI，并提出了我的要求。AI 的第一步并非盲目编码，而是像一个资深项目经理一样，先输出了一份详尽的行动计划。

> 👤 **我：**
> “参考这个 Tableau 看板和截图，用给定的数据源，修改 Power BI 项目文件，新建一页看板，完全复刻 Tableau 的内容。先告诉我你的计划。”
>
> 🤖 **AI：**
> “完美！现在让我制定详细计划：
>
> **第一阶段：分析和准备**
> 1.  分析 Tableau 看板结构（从截图和 `.twb` 文件）：
>     - ✅ 顶部标题："Profitability Overview"
>     - ✅ 7个KPI卡片：Sales, Profit, Profit Ratio...
>     - ✅ 地图：按邮政编码显示 Profit Ratio（颜色），Sales（大小）
>     - ✅ 左下：Monthly Sales by Segment（面积图）
>     - ✅ 右下：Monthly Sales by Product Category（面积图）
>     - ✅ 右侧筛选器：Order Date, Region, State/Province...
>
> **第二阶段：创建新页面**
> 3.  在 Report 中新建页面 "Profitability Overview"...
>
> **第三阶段：添加可视化组件**
> 4.  添加 7 个 KPI 卡片...
> 5.  添加地图可视化...
> ...
>
> **第四阶段：样式和格式化**
> 9.  应用字体、颜色方案等...”

**复盘思考：** 这一步至关重要。让 AI 先出计划，确保了它完全理解了我的意图，避免了后续大量的返工。AI 的结构化思维能力在这里体现得淋漓尽致。

### 第二幕：初次尝试与“创造性的失败”

AI 根据计划，开始生成定义页面布局的 `page.json` 文件。它自信地将所有可视化组件的定义都写在了 `visualContainers` 属性里。然而，当我尝试在 Power BI Desktop 中打开时，立刻收到了一个 Schema 验证错误。

这是整个过程中最关键的转折点，真正的人机协作开始了。

> 👤 **我：**
> “有报错：'pages/ProfitabilityOverview/page.json': Property 'visualContainers' has not been defined and the schema does not allow additional properties...”
>
> 🤖 **AI：**
> “我明白了，看来 Power BI 的 `page.json` schema 不支持直接在内部定义 `visualContainers`。让我检查一下正确的结构... 原来如此！**可视化组件需要放在独立的子文件夹中，每个组件都有自己的 `visual.json` 文件。** 让我研究一下你提供的另一个范例项目 `Retail Analysis Sample`，学习正确的格式。”

**复盘思考：** AI 犯错了，但这并非失败。**错误信息成为了最宝贵的“提示词”**。我扮演的角色不是编码者，而是测试者和反馈者。AI 则展现了强大的学习和纠错能力，它能通过分析错误和学习范例，迅速掌握正确的开发范式。

### 第三幕：学习、适应与精确执行

在理解了正确的 `.pbip` 结构后，AI 的工作模式发生了根本性的改变。它不再生成一个大而全的 `page.json`，而是开始像一个严谨的工程师一样，按部就班地创建目录和文件。

> 🤖 **AI：**
> “好的，我已经理解了正确的结构。现在，我将为每个可视化组件创建独立的文件夹和 `visual.json` 文件。首先创建第一个 KPI 卡片（Sales）：”
>
> `> Bash(mkdir -p ".../visuals/CardSales")`
>
> `> Write(.../visuals/CardSales/visual.json)`
>
> ```json
> {
>   "$schema": "[https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.3.0/schema.json](https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.3.0/schema.json)",
>   "name": "CardSales",
>   "position": {
>     "x": 10,
>     "y": 60,
>     "z": 1001,
>     "height": 70,
>     "width": 145
>   },
>   "visual": {
>     "visualType": "card",
>     "query": {
>       "queryState": {
>         "Values": {
>           "projections": [
>             { "queryRef": "Orders.Total Sales" }
>           ]
>         }
>       }
>     }
>   }
> }
> ```
> “完美！现在创建第2个卡片 - Profit...”

这个过程被重复了 13 次，AI 精确地为 7 个 KPI 卡片、2 个面积图和 4 个筛选器分别创建了配置文件，并严格遵守了第一步规划好的坐标和尺寸。

### 第四幕：攻克难关与完美收官

当创建面积图时，我们遇到了最后一个难题：Tableau 看板是按“月”聚合数据的，而 AI 最初生成的图表是按“天”展示的。AI 最初尝试使用 Power BI 的内置日期层次结构，但再次遇到了 Schema 错误。

这一次，它展现了更高阶的解决问题的能力：**修改数据模型**。

> 🤖 **AI：**
> “我明白了，直接引用日期层级的方式在当前 Schema 下行不通。最稳妥的方案是，在数据模型中创建一个新的计算列，专门用于表示年月。让我来修改 `Orders.tmdl` 文件。”
>
> `> Update(superstore dashboard.SemanticModel\definition\tables\Orders.tmdl)`
>
> ```dax
> column 'Order Year-Month' = FORMAT(Orders[Order Date], "YYYY-MM")
>   dataType: string
>   lineageTag: c-order-year-month-001
>   summarizeBy: none
>   dataCategory: Months
> ```
> “很好！现在我已经添加了新的计算列。接下来，我将更新两个面积图的 `visual.json` 文件，让它们的X轴使用这个新的 `Order Year-Month` 列。”

最终，AI 成功地修改了数据模型（添加 DAX 计算列）和报表定义，完美实现了按月聚合的功能。至此，整个看板的迁移工作宣告完成。

---

## 第三章：复盘与展望

这次实验给我的震撼是巨大的，它预示着 BI 开发的一种全新可能。

### 1. 关键收获

* **开发者的角色转变**：我的角色从一个“执行者”（报表开发）转变为一个“指挥官”（提出需求、提供反馈、验证结果）。我负责定义“做什么”，而 AI 负责“怎么做”。
* **AI 是伙伴，而非工具**：AI 不再是被动执行命令的工具。它能够规划、学习、试错、纠错，成为了一个真正的开发伙伴。我们之间的交互是双向的、富有成效的对话。
* **“BI 即代码”的威力**：通过操作pbip的代码，我们获得了前所未有的精确性、可复用性和自动化潜力。想象一下，未来我们可以用脚本批量生成上百个风格统一、布局一致的报表，这将是效率的巨大飞跃。

### 2. 未来的展望

我们正站在 BI 开发新纪元的门口。随着 AI 能力的不断增强和 Power BI 开发工具链的日益成熟，我们可以预见：

- **高度自动化的 BI 运维**：通过 Git 进行版本控制，通过 CI/CD 流水线实现报表的自动化测试和部署将成为常态。
- **自然语言驱动的开发**：未来，我们可能只需要用一句话描述想要的报表，AI 就能为我们生成完整的、可交互的 `.pbip` 项目。
- **技能要求的演变**：BI 专业人士的价值将更多地体现在业务理解、数据建模、需求定义和 AI 指挥能力上，而非单纯的工具操作熟练度。

如果你也对这场正在发生的变革感到兴奋，不妨现在就打开一个 `.pbip` 项目，尝试用代码去“绘制”你的下一个数据故事吧！

<Comment />
