[TOC]

## 常用标签和属性

### 标签

> `<em></em> `将内容变成斜体
>
> `<strong></strong>` 加粗
>
> `<img src="">`	引入图片
>
> `<span>` 标签被用来组合文档中的行内元素,无特殊语义
>
> > `示例<p>日语实例: <span lang="jp">ご飯が熱い。</span>.</p>`
> >
> > `<span style="font-size: 32px; margin: 21px 0;">这是顶级标题吗？</span>`
>
> `<code>`代码块
>
> `<sup>`、`<sub`上标和下标
>
> `<nav>`导航栏
>
> `<main><article><section><div>`主内容和内容区段
>
> `<aside>`侧边栏
>
> `<footer>`页脚
>
> `<header>`全局页眉
>
> `<hr>`水平分割线 

### 属性

>`<a href=''></a>` 声明超链接
>
>`<a title='鼠标悬停将显示的内容'></a>`
>
>`<a target="_blank"></a>` 将会在新标签页打开超链接
>
>块级链接
>
>> `<a href="https://mrsnake.top/">  <img src="naruto.jpg" alt="链接至博客主页的">
>> </a>
>
>链接到文章片段
>
>> 1. `<h2 id="set_id">邮寄地址</h2>`
>>
>> 2. `<a href="index.html#set_id">链接到index的片段</a>`
>> 3. `<a href="#set_id">链接到此页面的片段</a>`
>
>下载链接使用download属性
>
>> `<a href="https://download"
>>    download="software-64bit-installer.exe">
>>   下载软件- Windows（64位）
>> </a>`

### 常用操作

>`<input type="text" disabled>` 禁止输入的表单
>
>`<link rel="shortcut icon" href="favicon.ico" type="image/x-icon">` 增加自定义图标
>
>`<ul><li>无序</li></ul>`
>
>`<ol><li>有序</li></ol>`

### CSS 和 JavaScript

>`<link rel="stylesheet" href="my-css-file.css">`
>
>`<!-- rel="stylesheet" 表示文档样式表 -->`
>
>`<script src="my-js-file.js" defer></script>`
>
>`<!-- defer文档加载完毕了再执行脚本 -->`

------

**块级元素	会自动换行**

**内联元素	不会换行**

------

## 实体引用

| 原义字符 | 等价字符引用 |
| :------- | :----------- |
| <        | `&lt;`       |
| >        | `&gt;`       |
| "        | `&quot;`     |
| '        | `&apos;`     |
| &        | `&amp;`      |

## 描述列表

```html
<dl>
  <dt>番茄牛腩</dt>
    <dd>酸甜多汁，美味可口</dd>
  <dt>麻婆豆腐</dt>
    <dd>麻辣爽口，入口即化</dd>
  <dt>红烧肉</dt>
    <dd>肥而不腻，软糯香甜</dd>
</dl>
```

## 缩略语

```html
<p><abbr title="just a name">MrSnake</abbr> 会飞。</p>
```

