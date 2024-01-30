---
title: "HTML标签"
date: 2021-11-26T15:23:42+08:00
draft: false
summary: "常用标签总结"
---

# HTML常用标签

[MDN官网](https://developer.mozilla.org/zh-TW/docs/orphaned/Web/Guide/HTML/HTML5)
### 一、所有标签都有的属性
- 标记 class
- 格式 style
- 使元素被编辑 contenteditable
- 让一个东西看不到 hidden
- id 在html中是唯一的，可以作为链接锚，通过js或者css改变样式
- 控制tab键的顺序 tabindex
- 显示完整内容 title

### 二、章节标签

- 标题 h1-h6
- 章节 section
- 文章 article
- 段落 p
- 头部 header
- 脚步 footer
- 主要内容 main
- 旁支内容 aside
- 划分 div

### 三、内容标签

- ol+li
- ul+li
- dl+dt+dd
- pre 保留空格回车tab
- code 代码
- hr 分割线
- br 换行
- a 链接
```
1.href
  （1）超链接
  （2）href的取值
      网址
      路径
      伪协议（javascript：代码；、mailto：邮箱、tel：电话）
      id
2.target
  （1）_blank 在空白页面打开
  （2）_self 在当前页面打开
  （3）_top 在顶部打开页面
  （4）_parent 在父页面打开页面
  （5）程序猿的命名 固定窗口xxx（windows的name、iframe的name）
3.download 下载
4.rel=noopener 防止一个bug
  
```

- em 强调
- strong 加粗
- quote 引用
- blockquote 块引用

### 四、iframe标签
内嵌窗口

### 五、table

1.相关标签
- thead
- tbody
- tfoot
- tr
- td
- th

2.相关样式

- table-layout
- border-spacing
- boder-collapse

### 六、img

1.作用：发出一个get请求，展示一个图片

2.属性
- alt
- src
- height
- width

3.事件
- onload
- onerror

4.响应式

max-width：100%

### 七、form标签

1.作用：发一个get或post请求，然后刷新页面

2.属性
- action
- method
- autocomplete
- target

3.事件：onsubmit

### 八、input标签
1.作用：让用户输入内容

2.属性
> 类型
> - button
> - checkbox
> - email
> - file
> - hidden
> - number
> - password
> - radio
> - search
> - submit
> - tel
> - text
> 
> 其他
> - name
> - autofocus
> - checked
> - disabled
> - maxlength
> - pattern
> - value
> - placeholder

3.事件
- onchange
- onfocus
- onblur

4.验证器：HTML5新增功能