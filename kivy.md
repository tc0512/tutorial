# Kivy: 从`pip install`到`gh release create`全过程教程
## 1 安装kivy
MacOs64位, Linux64位和Windows\_amd64可以直接`pip install`到二进制; 对32位设备的用户, 你并不适合在它上面安装kivy; 对于安卓系统, 一般情况下不能安装kivy, 但如果你有Termux, 可以按下列步骤安装ubuntu以变成Linux系统, 从而获取二进制包
```bash
curl -O https://github.com/termux/termux-x11/releases/download/nightly/app-arm64-v8a-debug.apk && /system/bin/pm install ./*.apk #或手动安装
pkg install proot proot-distro termux-x11 termux-x11-nightly
mkdir -p /data/data/com.termux/files/tmp/ && chmod 777 /data/data/com.termux/files/tmp/
proot-distro install ubuntu
proot-distro login ubuntu
#如果命令行变成root@localhost~#, 就说明下载成功, 下面在ubuntu中安装Python
apt update && apt upgrade
apt install python3 python3-pip python-is-python3 python3-venv openbox xclip
#创建虚拟环境以安装kivy
python -m venv <虚拟环境名称>
source ~/<虚拟环境名称>/bin/activate #激活, 需要在ubuntu的家目录执行
pip install kivy
```
注意: 对于在Termux安装ubuntu的用户，写代码之前需执行下面几条命令
```bash
#~ $
export TMPDIR=/data/data/com.termux/files/usr/tmp
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
官方推荐使用buildozer构建, 但大量实验表明由于buildozer克隆的p4a的kivy配方有不存在的android库和兼容性极差的python3.14, 所以buildozer几乎无法完成构建, 真正的正确方案是在Actions中用p4a第2024.1.12版构建, 具体配置如下
```yaml
name: Build APK (Standalone)

on:
  push:
    branches: [main, master]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Free Space
        run: |
          sudo rm -rf /opt/ghc /usr/local/lib/android /usr/share/dotnet
          sudo rm -rf /opt/hostedtoolcache /usr/share/swift
          sudo apt remove -y '^llvm-.*' '^clang-.*' '^golang-.*'
          sudo apt autoremove -y
          sudo apt clean
          df -h

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          pip install --no-cache-dir Cython==0.29.36
          pip install --no-cache-dir python-for-android==2024.1.21
          sudo apt update -y
          sudo apt install -y default-jdk gcc g++ \
            make libffi-dev libssl-dev python3-dev \
            libncurses5-dev zlib1g-dev libbz2-dev \
            liblzma-dev sqlite3 libltdl-dev \
            automake autoconf libtool pkg-config \
            libgl1-mesa-dev libgles2-mesa-dev \
            libpulse-dev libasound2-dev

      - name: Install Android SDK and NDK manually
        run: |
          export ANDROID_SDK_ROOT=$HOME/android-sdk
          mkdir -p $ANDROID_SDK_ROOT
          wget -q https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -O cmdline-tools.zip
          unzip -q cmdline-tools.zip -d $ANDROID_SDK_ROOT
          mkdir -p $ANDROID_SDK_ROOT/cmdline-tools/latest
          mv $ANDROID_SDK_ROOT/cmdline-tools/* $ANDROID_SDK_ROOT/cmdline-tools/latest/ 2>/dev/null || true
          rmdir $ANDROID_SDK_ROOT/cmdline-tools 2>/dev/null || true
          echo "ANDROID_SDK_ROOT=$ANDROID_SDK_ROOT" >> $GITHUB_ENV
          echo "$ANDROID_SDK_ROOT/cmdline-tools/latest/bin" >> $GITHUB_PATH
          yes | $ANDROID_SDK_ROOT/cmdline-tools/latest/bin/sdkmanager --licenses > /dev/null 2>&1
          $ANDROID_SDK_ROOT/cmdline-tools/latest/bin/sdkmanager \
            "platforms;android-30" \
            "build-tools;30.0.3" \
            "ndk;25.2.9519653" > /dev/null 2>&1
          echo "✅ Android SDK and NDK installed successfully!"

      - name: Build with p4a
        run: |
          export ANDROID_HOME=$ANDROID_SDK_ROOT
          export ANDROID_SDK_ROOT=$ANDROID_SDK_ROOT
          export ANDROID_NDK_HOME=$ANDROID_SDK_ROOT/ndk/25.2.9519653
          export PATH=$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$PATH
          export CONFIGURE_HOST="aarch64-linux-android"
          export HOSTPYTHON_CFLAGS="-I$ANDROID_NDK_HOME/toolchains/llvm/prebuilt/linux-x86_64/sysroot/usr/include"
          export HOSTPYTHON_LDFLAGS="-L$ANDROID_NDK_HOME/toolchains/llvm/prebuilt/linux-x86_64/sysroot/usr/lib/aarch64-linux-android/21"
          echo "ANDROID_HOME=$ANDROID_HOME"
          echo "ANDROID_SDK_ROOT=$ANDROID_SDK_ROOT"
          echo "ANDROID_NDK_HOME=$ANDROID_NDK_HOME"
          echo "CONFIGURE_HOST=$CONFIGURE_HOST"
          which sdkmanager
          sdkmanager --version
          ls -la $ANDROID_NDK_HOME/toolchains/llvm/prebuilt/linux-x86_64/bin/clang || echo "NDK not found!"
          mkdir -p bin
          p4a apk \
            --private . \
            --package=org.phycalc.app \
            --name="PhyCalc" \
            --version=0.4 \
            --bootstrap=sdl2 \
            --requirements=python3==3.11.5,pip==23.3.1,setuptools==69.0.0,kivy==2.3.0 \
            --android-api=30 \
            --minsdk=21 \
            --arch=arm64-v8a \
            --permission=INTERNET

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: phycalc
          path: ~/.local/share/python-for-android/dists/*/build/outputs/apk/debug/*.apk
          retention-days: 30
```

## 4 成品
1. 登录github
2. 前往你的仓库的Actions
3. 找到Artifacts, 点击下载图标(需要30分钟且上不封顶, 请耐心等待)
4. 将下载的文件解压, 得到APK
5. 将APK转发到你的手机上, 测试安装(Termux用户别用termux-open), 如果效果符合预期则为成功
6. 使用`gh`创建release
7. 全部完成
