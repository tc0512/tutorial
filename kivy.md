# Kivy: 从`pip install`到`gh release create`全过程教程
## 1 安装kivy
MacOs64位, Linux64位和Windows\_amd64可以直接`pip install`到二进制; 对32位设备的用户, 你并不适合在它上面安装kivy; 对于安卓系统, 一般情况下不能安装kivy, 但如果你有Termux, 可以按下列步骤安装ubuntu以变成Linux系统, 从而获取二进制包
```bash
pm install https://github.com/termux/termux-x11/releases/download/nightly/app-arm64-v8a-debug.apk #或手动安装
pkg install proot proot-distro
proot-distro install ubuntu
proot-distro login ubuntu
#如果命令行变成root@localhost~#, 就说明下载成功, 下面在ubuntu中安装Python
apt update && apt upgrade
apt install python3 python3-pip python-is-python3
apt install python3-venv
apt install openbox
#创建虚拟环境以安装kivy
python -m venv <虚拟环境名称>
source ~/<虚拟环境名称>/bin/activate #激活, 需要在ubuntu的家目录执行
pip install kivy
```
注意: 对于在Termux安装ubuntu的用户，写代码之前需执行下面两条命令
```bash
#~ $
termux-x11 :0 &
export DISPLAY=:0
proot-distro login ubuntu --bind /data/data/com.termux/files/usr/tmp:/tmp
#root@localhost~#
export DISPLAY=:0
export XAUTHORITY=/tmp/termux-x11.xauth
openbox &
source ~/<虚拟环境名称>/bin/activate
```

## 2 编写代码
下面是一个最简单的helloworld示例
```python
from kivy.app import App
from kivy.uix.label import Label

class HelloApp(App):
    def build(self):
        return Label(text="Hello, World!")

if __name__ == "__main__":
    HelloApp().run()
```
这是按钮的创建方法
```python
from kivy.app import App
from kivy.uix.button import Button

class MyApp(App):
    def build(self):
        return Button(text="点我") #这里注意配置字体

if __name__ == "__main__":
    MyApp().run()
#点击"点我"后，按钮会闪一下蓝色
```
这是输入框的创建方法
```python
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.textinput import TextInput
from kivy.uix.button import Button
from kivy.uix.label import Label

class CalcApp(App):
    def build(self):
        layout = BoxLayout(orientation='vertical', spacing=10, padding=10)

        self.input_box = TextInput(text="0", input_filter='float', font_size=30)
        self.result_label = Label(text="结果：", font_size=30)

        btn = Button(text="计算平方", font_size=30)
        btn.bind(on_press=self.calc_square)

        layout.add_widget(self.input_box)
        layout.add_widget(btn)
        layout.add_widget(self.result_label)

        return layout

    def calc_square(self, instance):
        try:
            val = float(self.input_box.text)
            self.result_label.text = f"结果：{val * val}"
        except:
            self.result_label.text = "错误：无效数字"

if __name__ == "__main__":
    CalcApp().run()
```
一个kivy做的APP的基础建立在以上代码之上

## 3 构建
这是网上给的一个错误示范
```bash
pip install buildozer
buildozer android init
buildozer android debug
```
之所以说它错误，是因为`buildozer android debug`会出现很多问题(如网络, aidl, tty等)
真正正确的方案是Github Actions，但请注意，build.yml千万不要这么写
```yaml
name: Build Android APK

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Set up Python
      uses: actions/setup-python@v5
      with:
        python-version: '3.9'

    - name: Install system dependencies
      run: |
        sudo apt update
        sudo apt install -y \
          git zip unzip openjdk-17-jdk \
          python3-pip autoconf libtool pkg-config \
          zlib1g-dev libncurses5-dev libncursesw5-dev \
          libtinfo5 cmake libffi-dev libssl-dev

    - name: Install buildozer
      run: |
        pip install buildozer
        pip install cython==0.29.33

    - name: Configure buildozer.spec
      run: |
        if [ ! -f buildozer.spec ]; then
          buildozer init
        fi
        sed -i 's/^#\?android.accept_sdk_license =.*/android.accept_sdk_license = True/' buildozer.spec
        sed -i 's/^#\?android.ndk =.*/android.ndk = 23b/' buildozer.spec
        sed -i 's/^#\?android.sdk =.*/android.sdk = 24/' buildozer.spec

    - name: Build APK
      run: buildozer android debug

    - name: Upload APK
      uses: actions/upload-artifact@v4
      with:
        name: kivy-apk
        path: bin/*.apk
        retention-days: 30

    - name: Upload logs if build fails
      if: failure()
      uses: actions/upload-artifact@v4
      with:
        name: buildozer-logs
        path: .buildozer/logs/
```
这个yaml的问题: `apt install libtinfo5`在新版ubuntu会报错, 且在Github Actions这种CI环境中将无法输入y/n, 导致构建失败
真正的正解
```yaml
name: Build Android APK

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    container: kivy/buildozer:latest

    steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Build APK
      run: |
        # 自动回答 root 警告
        yes | buildozer android debug

    - name: Upload APK
      uses: actions/upload-artifact@v4
      with:
        name: kivy-apk
        path: bin/*.apk
        retention-days: 30

    - name: Upload logs if build fails
      if: failure()
      uses: actions/upload-artifact@v4
      with:
        name: buildozer-logs
        path: .buildozer/logs/
```
这个版本使用docker, 且自动回答了y, 避免了许多问题
**注:** 
**1 文件名为build.yml, 且位置在\<项目根目录\>/.github/workflows**
**2 主入口文件必须名为main.py, 否则需要在buildozer.spec中设置**
**3 这个模板需要提前编写buildozer.spec, 基本内容如下**
```ini
[app]
title = <显示在手机桌面上的名字>
package.name = <同上, 但是必须全小写>
package.domain = org.example
source.dir = .
source.include_exts = py,png,jpg,kv,atlas #如果有字体的话加ttc
version = 0.1
requirements = python3,kivy #按需添加
icon.filename = %(source.dir)s/<图标>.png

[android]
api = 34
minapi = 21
ndk = 23b
android.accept_sdk_license = True
android.permissions = INTERNET

[buildozer]
log_level = 2
warn_on_root = 1
```

## 4 成品
1. 登录github
2. 前往你的仓库的Actions
3. 找到Artifacts, 点击下载图标(需要30分钟且上不封顶, 请耐心等待)
4. 将下载的文件解压, 得到APK
5. 将APK转发到你的手机上, 测试安装(Termux用户别用termux-open), 如果效果符合预期则为成功
6. 使用`gh`创建release
7. 全部完成
