## 常用的标签以及属性

### 1.table 标签鼠标悬停显示内容

在td标签中添加title属性 `<td title='鼠标悬停显示'>mrsnake</td>`

### 2.table 标签表单内容过多省略号显示

```html
td {
    text-align: center;		# 字体居中
    white-space: nowrap;	# 不换行 
    overflow: hidden;		# 隐藏滚动行
    text-overflow: ellipsis;	# 内容过长省略显示
}
```

