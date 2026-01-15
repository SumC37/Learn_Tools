#fleeting

[7分中钟通俗讲解python虚拟环境](https://www.bilibili.com/video/BV1Nq4y1b762?spm_id_from=333.788.videopod.sections&vd_source=7cabdcd1ec9266743b06c0cdc385e80a)

- win+r --> cmd --> where python
- python可以在一台机器上安装多次

文件activate  批量修改路径

```cmd
C:\Projects> python -m venv myenv           # 创建环境,不会修改系统路径
C:\Projects> myenv\Scripts\activate.bat     # 激活环境
(myenv) C:\Projects> python --version       # 验证
Python 3.13.2
(myenv) C:\Projects> pip install requests   # 安装包（只在虚拟环境内）
(myenv) C:\Projects> deactivate             # 退出环境
C:\Projects>                               # 返回系统环境
```

下一节：[16分钟通俗讲解程序中的算法](算法.md)
