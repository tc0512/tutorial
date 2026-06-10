# Rich: 终端美化的轻量王者
## 1 安装rich
```bash
pip install rich #-i https://pypi.tuna.tsinghua.edu.cn/simple/
```

## 2 终端输出彩色文字
```python
from rich import print

# 普通文字
print("Hello World!")

# 彩色文字
print("[green]绿色文字[/green]")
print("[red]红色文字[/red]")
print("[blue]蓝色文字[/blue]")
print("[yellow]黄色文字[/yellow]")

# 组合样式
print("[bold red]红色粗体[/bold red]")
print("[italic green]绿色斜体[/italic green]")
print("[underline blue]下划线蓝字[/underline blue]")
print("[bold italic magenta]紫粗斜[/bold italic magenta]")
```

## 3 背景色
```python
from rich import print

print("[on red]红底[/on red]")
print("[on green]绿底[/on green]")
print("[on blue]蓝底[/on blue]")
print("[bold yellow on red]红底黄字粗体[/bold yellow on red]")
```

## 4 表格
```python
from rich.table import Table
from rich import print

table = Table(title="成绩单")

table.add_column("姓名", style="cyan")
table.add_column("数学", style="green")
table.add_column("语文", style="yellow")
table.add_column("英语", style="magenta")
table.add_column("满分", style="blue")

table.add_row("张三", "120", "80", "110", "120")
table.add_row("李四", "112", "90", "110", "120")
table.add_row("王五", "102", "90", "102", "120")

print(table)
```

## 5 进度条
```python
from rich.progress import track
import time

# 简单用法
for i in track(range(100), description="处理中..."):
    time.sleep(0.01)

# 自定义进度条
from rich.progress import Progress

with Progress() as progress:
    task = progress.add_task("[green]下载中...", total=100)
    for i in range(100):
        time.sleep(0.01)
        progress.update(task, advance=1)
```

## 6 目录树
```python
from rich.tree import Tree
from rich import print

tree = Tree("[blue]/root/nbmath/[/blue]")
tree.add("LICENSE")
tree.add("README.md")
tree.add("pyproject.toml")

sub1 = tree.add("[blue]nbmath/[/blue]")
sub1.add("__init__.py")
sub1.add("utils.py")
sub1.add("equation.py")
sub1.add("optimize.py")
sub1.add("const.py")
sub1.add("geo.py")

sub2 = sub1.add("[blue]plots/[/blue]")
sub2.add("__init__.py")
sub2.add("core.py")

sub3 = sub2.add("[blue]examples/[/blue]")
sub3.add("__init__.py")
sub3.add("mandelbrot.py")
sub3.add("random_walk.py")
sub3.add("lissajous.py")
sub3.add("barnsley.py")
sub3.add("julia.py")
sub3.add("cantor_stair.py")
sub3.add("sin.py")
sub3.add("heart.py")

print(tree)
```
## 7 语法高亮
```python
from rich.syntax import Syntax
from rich import print

code = """
#include<iostream>
using namespace std;
int main() {
    cout << "Hello world!" << endl;
    return 0;
}
"""

syntax = Syntax(code, "cpp", theme="monokai")
print(syntax)
```

## 8 注意事项
1. Windows cmd对彩色文字支持可能较弱
2. 大量输出可能卡顿
3. IDLE控制台可能不显示彩色, 建议使用PyCharm/VS Code/code-server
