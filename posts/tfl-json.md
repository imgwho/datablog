---
date: 2025-06-19
title: TFL 文件处理器 - 用户手册
tags:
  - Tableau
  - code
description: 摘要：一款轻量级的桌面应用程序，旨在帮助用户快速、方便地从 Tableau 流程文件 (`.tfl`) 中提取核心的流程定义数据Json格式，方便进一步处理或者询问AI。
---



-----

好的，我们再次精简文档，将重点完全集中在核心的 JSON 文件解读上。

在这个版本中，**“功能特性”**和**“使用指南”**部分将被压缩到最简短的描述，以便让读者能直接进入最具技术价值的章节。

---

# TFL 文件处理器 - 产品文档

## 1. 简介

**TFL 文件处理器**是一款专为处理 Tableau 流程文件 (`.tfl`) 而设计的轻量级桌面应用程序。它的核心功能是帮助用户从一个或多个 `.tfl` 文件中快速、准确地提取出核心的流程定义数据，并将其保存为通用的 `.json` 格式，以便进行深入的自动化分析、文档生成或流程比对。

## 2. 核心功能

本工具是一个独立的 Windows 可执行程序 (`.exe`)，提供了一个简洁的图形界面。用户可通过拖放操作，快速地将 `.tfl` 文件批量转换为结构化的 `.json` 数据定义文件。

## 3. 系统要求

* **操作系统**：Windows 10 或 Windows 11 (64位版本)。

## 4. 使用方法

1.  **运行程序**：直接双击 `TFL_Processor.exe` 文件启动应用。
2.  **处理文件**：将一个或多个 `.tfl` 文件拖动到程序窗口中。处理过程和结果将显示在底部状态栏，生成的 `.json` 文件会自动保存在与原文件相同的目录中。

## 5. JSON 文件结构解析

本程序的核心产出物是一个 `.json` 文件，它是 Tableau Prep 流程的完整“设计蓝图”。下面我们将通过具体的（已脱敏的）示例代码来深入解读其中包含的关键信息。

### 5.1 数据源与连接信息

JSON 文件详细记录了数据从何而来，无论是直连数据库表还是通过自定义SQL查询。

#### 示例 1：直连数据库表
```json
"1d4b3866-e59f-41eb-8e40-4b9ce0fa462e" : {
  "nodeType" : ".v1.LoadSql",
  "name" : "source_admin_table",
  "connectionId" : "b688372d-1ab9-414f-9e5b-e213bfb55643",
  "connectionAttributes" : {
    "dbname" : "my_database_admin"
  },
  "fields" : [ {
    "name" : "id",
    "type" : "real",
    "ordinal" : 1
  }, {
    "name" : "username",
    "type" : "string",
    "ordinal" : 6
  } ],
  "relation" : {
    "type" : "table",
    "table" : "[source_admin_table]"
  }
}
```
* **解读**：此代码段定义了一个名为 `source_admin_table` 的输入节点。它连接到 `my_database_admin` 数据库，并直接从 `source_admin_table` 表中读取数据。`fields` 数组详细列出了该表中的所有字段及其数据类型。

#### 示例 2：使用自定义 SQL 查询
```json
"f79b83bb-f8f1-4254-a8ac-fb7ed99dadb1" : {
  "nodeType" : ".v1.LoadSql",
  "name" : "contact_with_wechat_info",
  "description" : "微信+中间表 拿客户添加日期",
  "relation" : {
    "type" : "query",
    "query" : "SELECT c.Company, c.Depart, DATE(FROM_UNIXTIME(c.AddTime)) AS 添加日期 FROM source_contact_table"
  }
}
```
* **解读**：此节点展示了更复杂的数据输入方式。它的 `relation.type` 是 `query`，并且 `query` 字段中包含了完整的、用于提取源数据的 SQL 语句。这使得数据血缘分析可以追溯到具体的查询逻辑。

### 5.2 数据转换步骤

JSON 文件记录了从数据输入到输出之间的每一个处理步骤，如联接、聚合、筛选、重命名等。

#### 示例 3：数据联接 (Join)
```json
"02da700c-ede1-436c-9811-2c7b3c44e0f9" : {
  "nodeType" : ".v2018_2_3.SuperJoin",
  "name" : "联接 订单与产品",
  "actionNode" : {
    "nodeType" : ".v1.SimpleJoin",
    "conditions" : [ {
      "leftExpression" : "[订单ID]",
      "rightExpression" : "[order_id]",
      "comparator" : "=="
    } ],
    "joinType" : "left"
  }
}
```
* **解读**：这是一个联接节点。`actionNode` 详细定义了联接的配置：`joinType` 为 `left`（左联接），`conditions` 指明了联接条件是左表的 `[订单ID]` 字段等于右表的 `[order_id]` 字段。

#### 示例 4：数据清理步骤序列
```json
"3fdc4e61-ba64-4395-83fb-38fc9889b20c" : {
  "nodeType" : ".v1.Container",
  "name" : "清理步骤 9",
  "loomContainer" : {
    "nodes" : {
      "9f29222b-661d-41aa-8556-fc4fc8379bf8" : {
        "nodeType" : ".v2019_2_2.KeepOnlyColumns",
        "name" : "只保留指定列",
        "columnNames" : [ "order_id", "goods_id", "goods_name" ]
      },
      "ecfd3ccf-f96d-451d-9519-6bc26cd2945e" : {
        "nodeType" : ".v1.ChangeColumnType",
        "name" : "更改 order_id 类型",
        "fields" : {
          "order_id" : { "type" : "integer" }
        }
      }
    }
  }
}
```
* **解读**：`Container` 节点通常包含了一系列连续的清理操作。本例中，数据首先经过 `KeepOnlyColumns` 节点，只保留了 `order_id` 等三个字段；紧接着，数据流向 `ChangeColumnType` 节点，将 `order_id` 字段的数据类型更改为整数。

### 5.3 数据输出配置

文件同样清晰地定义了处理完成后的数据将流向何处。

#### 示例 5：发布数据源
```json
"04ec3e5b-9422-4f55-ab7d-03e80ea3d7d5" : {
  "nodeType" : ".v1.PublishExtract",
  "name" : "Product Activity Analysis",
  "projectName" : "Default Project",
  "datasourceName" : "Product Activity Analysis",
  "serverUrl" : "http://tableau.example.com"
}
```
* **解读**：这是一个输出节点，类型为 `PublishExtract`（发布数据提取）。它详细说明了输出的目标：在服务器 `http://tableau.example.com` 上的 `Default Project` 项目中，发布一个名为 `Product Activity Analysis` 的数据源。

## Python源代码
```
import tkinter as tk
from tkinter import ttk
from tkinterdnd2 import DND_FILES, TkinterDnD
import os
import shutil
import zipfile
import threading
import re

# ==============================================================================
# 核心处理函数 (无变化)
# ==============================================================================
def process_tfl_file(tfl_path, update_status_callback):
    """处理单个 .tfl 文件，并通过回调函数更新UI。"""
    base_name = os.path.basename(tfl_path)
    update_status_callback(f"▶️ 开始处理: {base_name}")

    if not tfl_path.lower().endswith('.tfl'):
        update_status_callback(f"❌ 错误: '{base_name}' 不是 .tfl 文件。")
        return False
    
    dir_name = os.path.dirname(tfl_path)
    file_stem = os.path.splitext(base_name)[0]
    temp_zip_path = os.path.join(dir_name, f"{file_stem}_temp.zip")
    json_output_path = os.path.join(dir_name, f"{file_stem}.json")

    try:
        shutil.copy2(tfl_path, temp_zip_path)
        with zipfile.ZipFile(temp_zip_path, 'r') as archive:
            if 'flow' not in archive.namelist():
                update_status_callback(f"❌ 错误: 在 '{base_name}' 中未找到 'flow' 文件。")
                return False
            flow_data = archive.read('flow')
            with open(json_output_path, 'wb') as json_file:
                json_file.write(flow_data)
            update_status_callback(f"✅ 处理成功: {base_name}")
            return True
    except Exception as e:
        update_status_callback(f"❌ 处理 '{base_name}' 时发生严重错误: {e}")
        return False
    finally:
        if os.path.exists(temp_zip_path):
            os.remove(temp_zip_path)

# ==============================================================================
# 图形用户界面 (GUI) 部分
# ==============================================================================
class App(TkinterDnD.Tk):
    def __init__(self):
        super().__init__()
        self.title("TFL 文件处理器")
        self.geometry("450x300")
        
        style = ttk.Style()
        style.configure("TLabel", padding=10, font=('Arial', 12))
        style.configure("Drop.TLabel", background="lightgrey", borderwidth=2, relief="dashed", anchor=tk.CENTER, font=('Arial', 14, 'bold'))
        style.configure("Status.TLabel", anchor=tk.W, font=('Arial', 9))

        self.drop_target_frame = ttk.Frame(self, relief="solid", borderwidth=0)
        self.drop_target_frame.pack(expand=True, fill="both", padx=20, pady=20)
        
        self.drop_label = ttk.Label(self.drop_target_frame, text="\n请将一个或多个 .tfl 文件\n\n拖到这里\n", style="Drop.TLabel")
        self.drop_label.pack(expand=True, fill="both")
        
        self.drop_label.drop_target_register(DND_FILES)
        self.drop_label.dnd_bind('<<Drop>>', self.handle_drop)

        self.status_label = ttk.Label(self, text="状态：等待文件...", style="Status.TLabel")
        self.status_label.pack(side="bottom", fill="x", padx=10, pady=5)
    
    def update_status(self, message):
        """线程安全地更新状态栏文本。"""
        self.status_label.config(text=f"状态：{message}")

    def handle_drop(self, event):
        """处理文件拖放事件。"""
        paths = re.findall(r'\{[^{}]+\}|[^{}\s]+', event.data)
        cleaned_paths = [p.strip('{}') for p in paths]

        if not cleaned_paths:
            self.update_status("未能识别拖入的文件。")
            return
        
        processing_thread = threading.Thread(
            target=self.run_file_processing, 
            args=(cleaned_paths,)
        )
        processing_thread.start()

    def run_file_processing(self, file_paths):
        """
        【此函数已修改】
        循环处理所有文件，并在结束后直接更新状态栏。
        """
        success_count = 0
        total_count = len(file_paths)
        for path in file_paths:
            if process_tfl_file(path, self.update_status):
                success_count += 1
        
        # --- 修改部分开始 ---
        # 处理完成后，在状态栏显示最终总结，而不是弹窗
        final_summary = f"任务完成：成功 {success_count} / 总计 {total_count} 个文件。等待新文件..."
        self.after(0, self.update_status, final_summary)
        # --- 修改部分结束 ---

# ==============================================================================
# 程序入口
# ==============================================================================
if __name__ == "__main__":
    app = App()
    app.mainloop()
```

---

