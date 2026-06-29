# Manim: 手把手教你作出网上的数学动画效果
## 1 安装
### Windows64位: 先装ffmpeg再`pip install`
### Linux: 
```bash
apt install python3-manimpango python3-cairo ffmpeg
pip install manim
```
如果是Termux上的ubuntu: 
```bash
apt install python3-manimpango python3-cairo #可能需要从主环境mv
# mv /usr/lib/python3/dist-packages/manimpango/ /root/<你的虚拟环境名称>/lib/python3.xx/site-packages
# mv /usr/lib/python3/dist-packages/ManimPango-0.6.0.dist-info/ /root/<你的虚拟环境名称>/lib/python3.xx/site-packages/
# mv /usr/lib/python3/dist-packages/*cairo*/ /root/<你的虚拟环境名称>/lib/python3.xx/lib/python3.xx/site-packages/
apt install ffmpeg
pip install manim
```
### MacOs64位: 同Windows64位
32位设备建议放弃治疗
