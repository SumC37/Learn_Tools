 #fleeting 

[30分钟网络编程通俗讲解](https://www.bilibili.com/video/BV1yY4y1P7oz?spm_id_from=333.788.videopod.sections&vd_source=7cabdcd1ec9266743b06c0cdc385e80a)

# 快递和邮局

- 网关  路由器
- ip地址：计算机在互联网中的位置
  - 十进制 0.0.0.0 ~ 255.255.255.255
  - 十六进制 0.0.0.0 ~ ff .ff .ff .ff 
  - 二进制 0.0.0.0 ~ 1111 1111.1111 1111.1111 1111.1111 1111
  - 192.168.0.0 ~ 192.168.255.255 私有地址，家庭或公司的局域网

## 找计算机

| 网络区域                               | 机器序号                | 容纳机器数 |
| ---------------------------------- | ------------------- | ----- |
| xxxx xxxx.xxxx xxxx                | xxxx xxxx.xxxx xxxx | 2^16  |
| xxxx xxxx.xxxx xxxx.xxxx xxxx.xxxx | xxxx                | 2^4   |
| ……                                 | ……                  |       |
- IP地址中网络区域和计算机编号的位长不是固定的
- 192.168.1.0/24  ‘’/24‘’表示前24位是网络区域
- 子网掩码：255.255.255.0 == 11111111.1111111.11111111.00000000 表示网络区域的位标为1，表示计算机编号的位标为0

### 找程序

- 程序用编号表示
  - IP地址+端口`http://127.0.0.1:5000`
  - 网址(域名)
- 浏览器会把网址转换成IP地址+端口的形式

# 用flask编写网站

```
from flask import Flask #导入Flask类

app = Flask(__name__) #实例化一个对象

@app.route('/') #装饰器，指定路径
def index():
    return '这是网站的主页'

@app.route('/login/')
def login():
    return '<input type="text" placeholder="请输入用户名"/><button>提交</button>'
#字符串返给浏览器时，先被放入一个html文件，再把html文件传给浏览器

@app.route('/test/')#打开这个网页失败了
def test():
    return render_template('new.html')
app.run() #修改了端口号

#给出地址http://127.0.0.1:5000
```

- `bootstrap框架 jquery` 
- `django`
- 文件型数据库(`django`默认使用)  软件型数据库
- 数据库存数字、字符串、日期、时间 


