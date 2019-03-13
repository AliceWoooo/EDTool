# EDTool使用说明文档
文档访问地址：https://alicewoooo.github.io/EDTool/


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

[点击此处下载](https://github.com/AliceWoooo/EDTool/releases/download/v1.0/EDTool.exe)


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
> 例如：原文件中第x行为[A, B, C, D, **E**]， 新文件中第y行为[A, B, C, D, **F**]。 在容忍度高的情况下，x，y行可以被认定为是相同的一行，从而修改的内容为**单元格E更改为单元格F**； 而在容忍度低的情况下，x，y将被视作不同的一行，从而修改的内容为**删除了x行，增加了y行**。

![toolbar view](https://github.com/AliceWoooo/EDTool/blob/master/image/tool_view.png)

### 展示面板
展示面板用于标识差异内容所在位置，方便用户定位。其主要分为两个模式：
#### 上传模式
此模式主要用于上传要比较的文件。在此模式下，用户可以通过拖拽文件到指定区域来上传，或者点击按钮打开文件选择框指定文件。新旧文件上传顺序先后不影响比对结果，但是需要放在指定区域，即：原文件需放在左边，新文件需放在右边。此外，用户也可以通过点击工具栏按钮或者展示面板中的按钮来上传文件。

文件上传后，区域内将显示所选文件的路径。在没有指定完两个文件前，同一个区域内的文件可重复上传。一旦两个区域新旧文件都指定完毕，系统将会自动开始计算两个文件的差异并展示。
![display drag](https://github.com/AliceWoooo/EDTool/blob/master/image/display_drag.png)
#### 展示模式
此模式主要用于展示计算后的差异。整个展示面板按Sheet名区分，可以在上方选择切换要展示的Sheet。其中，左边区域展示的为原文件的内容，右边区域展示的为新文件的内容。若原文件有删除的行（列），标记为粉红色；若新文件有增加的行（列），则标记为浅蓝色。若有单元格进行改动，则标记为浅黄色。左右两块区域可以进行联动，在其中一块中选定某区域，另一块区域会实时显示，被选定的区域将会标记为深蓝色。
![display excel](https://github.com/AliceWoooo/EDTool/blob/master/image/display_excel.png)

### 差异面板
差异面板用于展示具体差异情况，提供点击导航功能。其按照列、行、单元格进行分类。其中，行（列）增删将会显示总共增加删除的行列数，以及具体更改的行（列）号。新增的部分会用蓝色标记，删除的部分则会用红色进行标。单元格改动则会把改动的坐标和具体改动的原值和新值一并显示，显示格式为 [原坐标][新坐标]。

同时，三个面板中的坐标均可与展示面板进行联动。用户点击任意需要展示的坐标，该坐标将会在展示面板中选定并跳转，便于用户进行差异内容导航。
![diff view](https://github.com/AliceWoooo/EDTool/blob/master/image/diff_view.png) ![cell view](https://github.com/AliceWoooo/EDTool/blob/master/image/cell_view.png)


## 其他

### 导出文件格式
差异比对结果导出为JSON文件，格式如下：
```
{
    "timestamp": "2019-02-29 25:61:61",                     // 时间戳
    "old file": "C:/Alice/Excel/Diff/Tool/oldFile.xlsx",    // 原文件路径
    "new file": "C:/Alice/Excel/Diff/Tool/newFile.xlsx",    // 新文件路径
    "threshold": [0.5],                                     // 结果对应的差异容忍度

    "Alice": {                                              // 根据 Sheet 名进行分组

        "column": {                                         // 列增删情况
            "add count": 5,                                 // add count 表示新文件中新增的列的数量
            "add": ["A", "L", "I", "C", "E"],               // add 中储存了具体新增的列号
            "delete count": 2,                              // delete count 表示原文件中删除的号的数量
            "delete": ["W", "U"]                            // delete 中储存了具体删除的列号
        },

        "row": {                                            // 行增删情况
            "add count": 4,                                 // add count 表示新文件中新增的行的数量
            "add": ["2", "0", "1", "9"],                    // add 中储存了具体新增的行号
            "delete count": 4,                              // delete count 表示原文件中删除的行的数量
            "delete": ["0", "2", "2", "9"]                  // delete 中储存了具有删除的行号
        },

        "cell": {                                           // 单元格改动情况
            "change count": 2,                              // change count 表示改动的单元格的数量
            "changes": {                                    // changes 中储存了所有改动的单元格的信息，格式为：
                "[0,E],[0,D]": ["ED","Tool"],               // [在原文件中的坐标][在新文件中的坐标]: [原数值，新数值]
                "[1,A],[1,W]": ["Alice","Wu"]
            }
        }
    },

    "Wu": {                                                 // 下一个Sheet表的情况
        ...
    }
    ...
}
```


### 致谢
* Bootstrap Theme Made By [www.w3schools.com](https://www.w3schools.com)
* Icons Made By [Freepik](https://www.freepik.com/) From [www.flaticon.com](https://www.flaticon.com/) Is Licensed By [CC 3.0 BY](http://creativecommons.org/licenses/by/3.0/)
* Icons made by [Pixel perfect](https://www.flaticon.com/authors/pixel-perfect) From [www.flaticon.com](https://www.flaticon.com/) Is Licensed By [CC 3.0 BY](http://creativecommons.org/licenses/by/3.0/)
