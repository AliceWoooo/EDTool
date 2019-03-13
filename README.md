# EDTool使用说明文档
访问地址：https://alicewoooo.github.io/EDTool/

## 概况
EDTool 是一个基于 Python 开发的 Excel 表格差异比对工具， 主要通过计算新旧表格之间的列、行、单元格之间的差异，来定位 Excel 文件中修改的内容。 用户可以在交互界面中指定两个用于比较的 Excel 文件，系统将会自动对比名字一致的 Sheet 表， 得出的内容差异在将会在交互界面中标识显示。同时，用户也可以将比对结果导出到本地中进行查看。
### 详细信息
| 名称 | 信息 |
| --- | --- |
| 工具格式 | EXE |
| 导出文件格式 | JSON |
| 运行环境 | Windows |
| 编程语言 | Python 2.7 |
| GUI框架 | PyQT4 |
| Excel表格读取工具 | xlrd |
| 打包工具 | PyInstaller |

## 运行
本工具已打包成EXE格式，在Windows环境下下载后可直接点击运行。

下载链接：https://github.com/AliceWoooo/EDTool/releases/download/v1.0/EDTool.exe

## 界面
本工具交互界面主要包含三个模块：
### 工具栏
工具栏包含打开、重置、导出等基本操作，支持快捷键的使用。主要操作有：
* 打开：导入需要比较的Excel文件，可使用快捷键 `CTRL + O`
* 重新选择：重置已选择的Excel文件，可使用快捷键 `CTRL + R`
* 导出：导出比对后的差异结果，导出的文件格式为JSON文件，可使用快捷键 `CTRL + E`
* 帮助：打开说明文档，可使用快捷键 `CTRL + H`
* 关闭：关闭程序，可使用快捷键 `CTRL + Q`
* 行列相同容忍度：由于系统判断的局限性，考虑到得出的结果可能与用户期望得到的结果相差较大，这里添加了一个更改判定容忍度的设置，用于调整差异对比中对于行（列）相同的判定。其中，容忍度越高，则表示在行（列）判定中，被认定为相同的两行（列）里允许不同的单元格个数越多；反之，如果容忍度越低，则表示两个相同行（列）中允许不同的单元格个数越少。
> 例如：原文件中第x行为[A, B, C, D, E]， 新文件中第y行为[A, B, C, D, F]。 在容忍度高的情况下，x，y行可以被认定为是相同的一行，从而修改的内容为单元格E更改为单元格F； 而在容忍度低的情况下，x，y将被视作不同的一行，从而修改的内容为删除了x行，增加了y行。

![toolbar view](https://github.com/AliceWoooo/EDTool/blob/master/image/tool_view.png)
