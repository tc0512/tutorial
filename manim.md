# Manim: 手把手教你作出网上的数学动画效果
## 1 安装
### Windows64位: 先装ffmpeg和LaTeX再`pip install`
### Linux: 
```bash
apt install python3-manimpango python3-cairo ffmpeg
pip install manim
apt install texlive texlive-latex-extra
```
如果是Termux上的ubuntu: 
```bash
apt install python3-manimpango python3-cairo #可能需要从主环境mv
# mv /usr/lib/python3/dist-packages/manimpango/ /root/<你的虚拟环境名称>/lib/python3.xx/site-packages
# mv /usr/lib/python3/dist-packages/ManimPango-0.6.0.dist-info/ /root/<你的虚拟环境名称>/lib/python3.xx/site-packages/
# mv /usr/lib/python3/dist-packages/*cairo*/ /root/<你的虚拟环境名称>/lib/python3.xx/lib/python3.xx/site-packages/
apt install ffmpeg
pip install manim
apt install texlive texlive-latex-extra
```
### MacOs64位: 同Windows64位
32位设备建议放弃治疗

## 2 HelloWorld
```python
from manim import*

class HelloWorld(Scene):
    def construct(self):
        self.play(Write(Text("Hello world!")), run_time=3)
```
渲染
```bash
manim -pql <文件名>.py HelloWorld
```
查看视频: 找到日志中带有 "预览文件" 的字样的一行, 复制那个地址, 在文件管理器中查看即可

## 3 常见的文本类型
| 类名 | 介绍 |
| ------ | ------ |
| `Text` | 最普通, 适合初学者 |
| `MarkupText` | 推荐, 可以避免文字太长溢出屏幕, 还可以做彩色字 |
| `Tex` | LaTeX通用但中文支持差 |
| `MathTex` | 数学公式专用 (不支持中文)  |

## 4 常见的图形
| 类名 | 用法 |
| `Circle` | 圆 |
| `Square` | 正方形 |
| `Rectangle` | 长方形 |
| `Triangle` | 等边三角形 |
| `RegularPolygon` | 正多边形 |
| `Arc` | 弧 |
| `Arrow` | 箭头 |
| `Curve` | 曲线 |
| `Dot` | 描点 |
| `Line` | 线段 |
| `DashedLine` | 虚线 |
| `Polygon` | 任意多边形 |
| `RoundedRectangle` | 圆角框 |
| `Star` | 正五角星 |
| `Axes` | 坐标系 |
| `NumberPlane` | 刻度坐标系 |
| `axes.plot()` | 在Axes上画曲线/函数 |
