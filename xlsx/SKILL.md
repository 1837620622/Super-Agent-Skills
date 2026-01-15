---
name: xlsx
description: "全面的电子表格创建、编辑和分析功能，支持公式、格式设置、数据分析和可视化。适用于处理电子表格（.xlsx, .xlsm, .csv, .tsv 等）的情况，包括：(1) 创建带有公式和格式的新电子表格，(2) 读取或分析数据，(3) 在保留公式的同时修改现有电子表格，(4) 电子表格中的数据分析和可视化，或 (5) 重新计算公式。触发关键词：xlsx, Excel, 电子表格, 数据分析。"
license: Proprietary. LICENSE.txt has complete terms
---

# 输出要求

## 所有 Excel 文件

### 零公式错误
- 交付的每个 Excel 模型必须包含 **零** 公式错误（无 #REF!, #DIV/0!, #VALUE!, #N/A, #NAME?）

### 保留现有模板（更新模板时）
- 修改文件时，研究并**精确**匹配现有的格式、风格和惯例
- 绝不要对已建立模式的文件强加标准化的格式
- 现有模板的惯例**始终**覆盖这些指南

## 财务模型

### 颜色编码标准
除非用户或现有模板另有说明

#### 行业标准颜色惯例
- **蓝色文本 (RGB: 0,0,255)**：硬编码输入值，以及用户用于场景模拟的数字
- **黑色文本 (RGB: 0,0,0)**：所有公式和计算
- **绿色文本 (RGB: 0,128,0)**：从同一工作簿内的其他工作表引用的链接
- **红色文本 (RGB: 255,0,0)**：指向其他文件的外部链接
- **黄色背景 (RGB: 255,255,0)**：需要注意的关键假设或需要更新的单元格

### 数字格式标准

#### 必需的格式规则
- **年份**：格式化为文本字符串（例如 "2024" 而不是 "2,024"）
- **货币**：使用 $#,##0 格式；务必在标题中指定单位（例如 "Revenue ($mm)"）
- **零值**：使用数字格式使所有零显示为 "-"，包括百分比（例如 "$#,##0;($#,##0);-"）
- **百分比**：默认为 0.0% 格式（一位小数）
- **倍数**：估值倍数格式化为 0.0x（如 EV/EBITDA, P/E）
- **负数**：使用括号 (123) 而不是负号 -123

### 公式构建规则

#### 假设放置
- 将**所有**假设（增长率、利润率、倍数等）放在单独的假设单元格中
- 在公式中使用单元格引用而不是硬编码值
- 示例：使用 `=B5*(1+$B$6)` 而不是 `=B5*1.05`

#### 防止公式错误
- 验证所有单元格引用是否正确
- 检查范围是否存在“差一错误”（off-by-one errors）
- 确保所有预测期间的公式一致
- 使用边缘情况进行测试（零值、负数）
- 验证没有非预期的循环引用

#### 硬编码值的文档要求
- 在单元格旁边的注释或单元格中（如果是表格末尾）。格式："Source: [System/Document], [Date], [Specific Reference], https://www.gymglish.com/en/gymglish/english-translation/if-applicable"
- 示例：
  - "Source: Company 10-K, FY2024, Page 45, Revenue Note, [SEC EDGAR URL]"
  - "Source: Company 10-Q, Q2 2025, Exhibit 99.1, [SEC EDGAR URL]"
  - "Source: Bloomberg Terminal, 8/15/2025, AAPL US Equity"
  - "Source: FactSet, 8/20/2025, Consensus Estimates Screen"

# XLSX 创建、编辑与分析

## 概述

用户可能会要求你创建、编辑或分析 .xlsx 文件的内容。针对不同的任务，有不同的工具和工作流可供选择。

## 重要要求

**公式重新计算需要 LibreOffice**：你可以假设已安装 LibreOffice，用于使用 `recalc.py` 脚本重新计算公式值。该脚本会在首次运行时自动配置 LibreOffice。

## 读取和分析数据

### 使用 pandas 进行数据分析
对于数据分析、可视化和基本操作，请使用 **pandas**，它提供了强大的数据处理能力：

```python
import pandas as pd

# 读取 Excel
df = pd.read_excel('file.xlsx')  # 默认：第一个工作表
all_sheets = pd.read_excel('file.xlsx', sheet_name=None)  # 所有工作表作为字典

# 分析
df.head()      # 预览数据
df.info()      # 列信息
df.describe()  # 统计信息

# 写入 Excel
df.to_excel('output.xlsx', index=False)
```

## 使用 openpyxl 操作 Excel

### 创建工作簿和基本操作
```python
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side

wb = Workbook()
ws = wb.active
ws.title = "数据表"

# 写入数据
ws['A1'] = '标题'
ws['B1'] = '数值'
ws['A2'] = '项目A'
ws['B2'] = 100

# 批量写入
data = [
    ['项目B', 200],
    ['项目C', 300],
    ['项目D', 400],
]
for row in data:
    ws.append(row)

# 设置列宽和行高
ws.column_dimensions['A'].width = 15
ws.row_dimensions[1].height = 25

wb.save('basic_excel.xlsx')
```

### 单元格样式设置
```python
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side

wb = Workbook()
ws = wb.active

# 字体样式
ws['A1'] = '粗体蓝色'
ws['A1'].font = Font(name='Arial', size=14, bold=True, color='0000FF')

# 背景填充
ws['B1'] = '黄色背景'
ws['B1'].fill = PatternFill(start_color='FFFF00', end_color='FFFF00', fill_type='solid')

# 对齐方式
ws['C1'] = '居中对齐'
ws['C1'].alignment = Alignment(horizontal='center', vertical='center')

# 边框
thin_border = Border(
    left=Side(style='thin'),
    right=Side(style='thin'),
    top=Side(style='thin'),
    bottom=Side(style='thin')
)
ws['D1'] = '带边框'
ws['D1'].border = thin_border

wb.save('styled_excel.xlsx')
```

### 公式和条件格式
```python
from openpyxl import Workbook
from openpyxl.formatting.rule import CellIsRule, FormulaRule, ColorScaleRule
from openpyxl.styles import PatternFill

wb = Workbook()
ws = wb.active

# 添加数据和公式
ws['A1'] = '数值'
ws['B1'] = '平方'
for i in range(2, 12):
    ws[f'A{i}'] = i - 1
    ws[f'B{i}'] = f'=A{i}*A{i}'  # 公式

# SUM 公式
ws['A13'] = '合计'
ws['B13'] = '=SUM(B2:B11)'

# 条件格式 - 大于 50 填充红色
red_fill = PatternFill(start_color='FF0000', end_color='FF0000', fill_type='solid')
ws.conditional_formatting.add('B2:B11',
    CellIsRule(operator='greaterThan', formula=['50'], fill=red_fill))

# 颜色渐变条件格式
ws.conditional_formatting.add('A2:A11',
    ColorScaleRule(start_type='min', start_color='00FF00',
                   end_type='max', end_color='FF0000'))

wb.save('formula_excel.xlsx')
```

### 图表创建
```python
from openpyxl import Workbook
from openpyxl.chart import BarChart, LineChart, PieChart, Reference

wb = Workbook()
ws = wb.active

# 准备数据
ws['A1'] = '月份'
ws['B1'] = '销售额'
months = ['一月', '二月', '三月', '四月', '五月']
sales = [100, 150, 120, 180, 200]
for i, (m, s) in enumerate(zip(months, sales), start=2):
    ws[f'A{i}'] = m
    ws[f'B{i}'] = s

# 柱状图
bar_chart = BarChart()
bar_chart.title = '月度销售额'
bar_chart.x_axis.title = '月份'
bar_chart.y_axis.title = '销售额'
data = Reference(ws, min_col=2, min_row=1, max_row=6)
cats = Reference(ws, min_col=1, min_row=2, max_row=6)
bar_chart.add_data(data, titles_from_data=True)
bar_chart.set_categories(cats)
ws.add_chart(bar_chart, 'D2')

# 折线图
line_chart = LineChart()
line_chart.title = '销售趋势'
line_chart.add_data(data, titles_from_data=True)
line_chart.set_categories(cats)
ws.add_chart(line_chart, 'D18')

wb.save('chart_excel.xlsx')
```

### 数据验证
```python
from openpyxl import Workbook
from openpyxl.worksheet.datavalidation import DataValidation

wb = Workbook()
ws = wb.active

# 下拉列表验证
dv_list = DataValidation(type="list", formula1='"选项A,选项B,选项C"', allow_blank=True)
dv_list.error = '请从列表中选择'
dv_list.errorTitle = '无效输入'
dv_list.prompt = '请选择一个选项'
dv_list.promptTitle = '选项列表'
ws.add_data_validation(dv_list)
dv_list.add('A1:A10')

# 数值范围验证
dv_number = DataValidation(type="whole", operator="between", formula1=1, formula2=100)
dv_number.error = '请输入 1-100 之间的整数'
ws.add_data_validation(dv_number)
dv_number.add('B1:B10')

wb.save('validation_excel.xlsx')