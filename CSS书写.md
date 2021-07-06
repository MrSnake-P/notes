[TOC]

## 书写

### 垂直文本

* `writing-mode`

> `horizontal-tb`：从上到下的块流动方向。句子水平排列。
>
> `vertical-rl`：从右到左的块流动方向。句子垂直排列。
>
> `vertical-lr`：从左到右的块流动方向。句子垂直排列。

在垂直书写过程中，用`inline-size`（内联尺寸的大小）代替 `width`，`block-size`替代`height`，用到的就是逻辑值映射。	

```css
.box {
  inline-size: 15px;
  writing-mode: vertical-rl;
}
```

 ```html
  <div class="box vertical">
    <h2>Heading</h2>
    <p>A paragraph. Demonstrating Writing Modes in CSS.</p>
    <p>These boxes have inline-size.</p>
  </div>
 ```

[物理值和逻辑值的映射](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Logical_Properties)

**物理值会被绑定，当写作模式发生变化时，盒子不会变。而逻辑映射值会根据写作模式切换而相应切换，如margin-top，就会变成margin-right**

------

**note：用逻辑边距值来确保无论书写方式是什么，边距都位于正确的位置。**

------

