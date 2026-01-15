---
name: docx
description: "全面的文档创建、编辑和分析功能，支持修订模式（tracked changes）、批注、格式保留和文本提取。适用于处理专业文档（.docx 文件）的情况，包括：(1) 创建新文档，(2) 修改或编辑内容，(3) 处理修订痕迹，(4) 添加批注，或其他任何文档任务。触发关键词：docx, Word, 文档处理, 修订痕迹。"
license: Proprietary. LICENSE.txt has complete terms
---

# DOCX 创建、编辑与分析

## 概述

用户可能会要求你创建、编辑或分析 .docx 文件的内容。.docx 文件本质上是一个包含 XML 文件和其他资源的 ZIP 压缩包，你可以对其进行读取或编辑。针对不同的任务，有不同的工具和工作流可供选择。

## 工作流决策树

### 阅读/分析内容
请使用下方的“文本提取”或“原始 XML 访问”部分。

### 创建新文档
请使用“创建新的 Word 文档”工作流。

### 编辑现有文档
- **你自己的文档 + 简单的更改**
  使用“基础 OOXML 编辑”工作流。

- **他人的文档**
  使用 **“红线修订工作流”**（推荐的默认方式）。

- **法律、学术、商业或政府文档**
  必须使用 **“红线修订工作流”**。

## 阅读和分析内容

### 文本提取
如果你只需要读取文档的文本内容，应该使用 pandoc 将文档转换为 markdown。Pandoc 对保留文档结构提供了极佳的支持，并且可以显示修订痕迹：

```bash
# 将文档转换为带有修订痕迹的 markdown
pandoc --track-changes=all path-to-file.docx -o output.md
# 选项: --track-changes=accept/reject/all
```

## 使用 python-docx 创建文档

### 基础文档创建
```python
from docx import Document
from docx.shared import Inches, Pt, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.enum.style import WD_STYLE_TYPE

document = Document()

# 添加标题
document.add_heading('文档标题', 0)

# 添加段落（支持格式化）
p = document.add_paragraph('这是一个普通段落，包含 ')
p.add_run('粗体').bold = True
p.add_run(' 和 ')
p.add_run('斜体').italic = True
p.add_run(' 文本。')

# 添加各级标题
document.add_heading('一级标题', level=1)
document.add_heading('二级标题', level=2)

# 添加列表
document.add_paragraph('无序列表项 1', style='List Bullet')
document.add_paragraph('无序列表项 2', style='List Bullet')
document.add_paragraph('有序列表项 1', style='List Number')
document.add_paragraph('有序列表项 2', style='List Number')

# 添加图片
document.add_picture('image.png', width=Inches(4))

# 保存文档
document.save('output.docx')
```

### 表格操作
```python
from docx import Document
from docx.shared import Cm

document = Document()

# 创建表格
table = document.add_table(rows=1, cols=3)
table.style = 'Table Grid'

# 设置表头
hdr_cells = table.rows[0].cells
hdr_cells[0].text = '序号'
hdr_cells[1].text = '名称'
hdr_cells[2].text = '描述'

# 添加数据行
data = [
    ('1', '项目A', '描述A'),
    ('2', '项目B', '描述B'),
    ('3', '项目C', '描述C'),
]
for num, name, desc in data:
    row_cells = table.add_row().cells
    row_cells[0].text = num
    row_cells[1].text = name
    row_cells[2].text = desc

document.save('table_demo.docx')
```

### 页眉页脚设置
```python
from docx import Document

document = Document()
section = document.sections[0]

# 设置页眉
header = section.header
header_para = header.paragraphs[0]
header_para.text = "左侧文本\t中间文本\t右侧文本"
header_para.style = document.styles["Header"]

# 设置页脚
footer = section.footer
footer_para = footer.paragraphs[0]
footer_para.text = "第 X 页"

document.add_paragraph('正文内容...')
document.save('header_footer.docx')
```

### 自定义样式
```python
from docx import Document
from docx.shared import Pt, RGBColor
from docx.enum.style import WD_STYLE_TYPE

document = Document()

# 创建自定义段落样式
styles = document.styles
custom_style = styles.add_style('CustomHeading', WD_STYLE_TYPE.PARAGRAPH)
custom_style.font.name = 'Arial'
custom_style.font.size = Pt(16)
custom_style.font.bold = True
custom_style.font.color.rgb = RGBColor(0, 0, 128)

# 使用自定义样式
document.add_paragraph('使用自定义样式的标题', style='CustomHeading')

# 创建字符样式
char_style = styles.add_style('Highlight', WD_STYLE_TYPE.CHARACTER)
char_style.font.color.rgb = RGBColor(255, 0, 0)
char_style.font.bold = True

# 应用字符样式
p = document.add_paragraph('这段文字包含 ')
p.add_run('高亮文本', style='Highlight')
p.add_run(' 在中间。')

document.save('styled_doc.docx')