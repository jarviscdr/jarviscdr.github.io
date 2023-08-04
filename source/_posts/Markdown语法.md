---
title: Markdown语法
date: 2023-08-03 08:00:17
index_img: https://s3.bmp.ovh/imgs/2023/08/04/0561aa8ca3e4e565.jpg
categories:
- 笔记
tags:
- Markdown
- 常用
---

## 标题
```Markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```


## 字体

*斜体文字* `*斜体文字*`

**粗体文字** `**粗体文字**`

***粗斜体文字*** `***粗斜体文字***`

~~删除线~~ `~~删除线~~`

<u>下划线</u> `<u>下划线</u>`


## 链接与图片

### 链接
[JarvisBlog](https://jarviscdr.github.io) `[JarvisBlog](https://jarviscdr.github.io)`


<https://jarviscdr.github.io> `<https://jarviscdr.github.io>`

[引用方式][2]

[2]: https://jarviscdr.github.io
```Markdown
# 引用方式
[JarvisBlog][1]

[1]: https://jarviscdr.github.io
```
### 图片

#### 默认图片
![图片描述](https://s2.loli.net/2023/08/03/PrGmgCE1qacUylv.png)
`![图片描述](https://s2.loli.net/2023/08/03/PrGmgCE1qacUylv.png)`

#### 图文混排
<div align=right>
    图文混排
    <img src="https://s2.loli.net/2023/08/03/PrGmgCE1qacUylv.png" alt="图片描述" width="30" height="30" style="background-color:transparent;" />
</div>
```html
<!-- 使用img标签可进行精细控制 -->
<div align=right>
    图文混排
    <img src="https://s2.loli.net/2023/08/03/PrGmgCE1qacUylv.png" alt="图片描述" width="30" height="30" style="background-color:transparent;" />
</div>
```


## 分隔线
***
`*** # 需要占据一行`


## 表格
| 默认  | 左对齐 | 居中对齐 | 右对齐 |
| ----- | :----- | :------: | -----: |
| value | value  |  value   |  value |
| value | value  |  value   |  value |
```
| 默认  | 左对齐 | 居中对齐 | 右对齐 |
| ----- | :----- | :------: | -----: |
| value | value  |  value   |  value |
| value | value  |  value   |  value |
```


## 列表

### 无序列表
- 列表项1 `- 列表项1`
- 列表项2 `- 列表项2`
- 列表项3 `- 列表项3`

### 有序列表
1. 列表项1 `1. 列表项1`
2. 列表项2 `2. 列表项2`
3. 列表项3 `3. 列表项3`

### 嵌套列表
1. 有序列表1 `1. 有序列表1`
   - 列表项1 `- 列表项1`
2. 有序列表2 `2. 有序列表2`
   - 列表项2 `- 列表项2`

### 待办列表
 - [ ] 这里是列表文本 `- [ ] 这里是列表文本`
 - [x] 这里是列表文本 `- [x] 这里是列表文本`


## 引用
> 1. 最外层
>> 2. 第一层嵌套
>>> 3. 第二层嵌套
```Markdown
> 1. 最外层
>> 2. 第一层嵌套
>>> 3. 第二层嵌套
```


## 脚注
右侧是注对应的编号. [^1]

[^1]: 脚注链接

```Markdown
右侧是注对应的编号. [^1]

[^1]: 脚注链接
```


## 代码块
```
​```语言
代码块
​```
```

