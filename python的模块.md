
[30分钟通俗讲解python的模块](https://www.bilibili.com/video/BV1FP4y1M7cu?spm_id_from=333.788.videopod.sections&vd_source=7cabdcd1ec9266743b06c0cdc385e80a)

# 同声传译

- 写代码-->写纯文本的文本文件
- 代码--*转换器*-->计算机

| 解释器        | 编译器                 |
| ---------- | ------------------- |
| 翻译+执行      | 只翻译不执行              |
| 一句一句翻译     | 整体翻译                |
| 语言和机器之间的交互 | 语言和机器间的交互,任两种语言间的转换 |
| 直译         | 编译                  |

# 标准库和模块

- 库  模块  包

# 摆弄对象的工具

| 元组         | 列表         | 集合         | 字典                  |
| ---------- | ---------- | ---------- | ------------------- |
| (a1,a2,a3) | [a1,a2,a3] | {a1,a2,a3} | {A1:a1,A2:a2,A3:a3} |
| 有序号,不能修改   | 有序号,能修改    | 不重复,无序号    | 键值对                 |

# time时间模块

- ISO国际标准，Greenwich 1970-01-01 00:00:00
- 
```python
import time

print(time.time(),'\n') #ISO国际标准总秒数
print(time.localtime(),'\n') #计算机中的9值结构体:年月日时分秒，星期，一年中的第几天，夏令时
print(time.asctime(time.localtime())) #人习惯的格式
```

- `time.sleep()`