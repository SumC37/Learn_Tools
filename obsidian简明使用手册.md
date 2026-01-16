---
tags:
  - permanent
  - obsidian
---
## 参考
[obsidian中文手册](https://publish.obsidian.md/help-zh/)
[pkmer](https://pkmer.cn/)

## 属性
属性以[YAML](https://yaml.org/)格式存储在文件的顶部。
也可以使用[JSON 语法](https://www.json.org/)定义属性，JSON 块将被读取、解释和保存为YAML。

## 嵌入网页
`<iframe src="在这里插入你的URL"></iframe>`

## 块链接[[]]
[[obsidian简明使用手册#示例标题 456]] #
[[obsidian简明使用手册#^123]] #^
[[obsidian简明使用手册#^123|示例文本]] |
[obsidian中文手册](https://publish.obsidian.md/help-zh/)

>块链接和块引用并非 Markdown 的标准语法，而是带有 Obsidian 风格的 Markdown 语法。这意味着这些链接和引用将在其他软件中失效。


___
## 块引用![[]]
![[obsidian简明使用手册#示例标题 456]]

![[obsidian简明使用手册#^123]]

---
## 手动创建块ID
手动创建的块 ID 仅支持字母、数字、破折号

---
## 标注 > [!类型标识符]
> [!info]
> > [!tip] 
>hint,important
> 
> >[!Note]
> 
> >[!abstract]
>summary,tldr，摘要
>
> >[!Todo]
>
>
> > [!Success]
>check,done
>
> > [!Question]
>help,faq
>
> > [!Warning]
>caution,attention
> 
> > [!Failure]
>fail,missing
>
> > [!Danger]
>error
>
> > [!Bug]
>
> > [!Example]
>
> > [!quote]
>cite

---
## 支持表格、mermaid、MathJax和LaTeX、HTML

| header1 | header2 |
| ------- | ------- |
| 1       | 2       |
$$
\begin{vmatrix}a & b\\
c & d
\end{vmatrix}=ad-bc
$$


---
## 多光标模式alt
alt添加,esc或退格键退出
shift+alt，选择多行

---
### 示例标题 ^456
示例文本 ^123







