# css选择器

## 基本选择器

+ 元素选择器: 通过元素标签名选择元素;
+ 类选择器: 通过```.类名```的方式选择元素;
+ ID选择器: 通过```#ID```的方式选择元素;
  > 同一个文档内每个元素只能绑定一个```ID```,每个```ID```也只能绑定一个元素;
+ 通配符选择器: 通过```*```选择所有元素;
+ 属性选择器: 通过```[attr]```或者```[attr=value]```的方式选择元素.并且可以模糊匹配属性值:[参考MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Attribute_selectors)

## 组合选择器

+ [相邻兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Adjacent_sibling_combinator): ```+```
+ [普通兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/General_sibling_combinator): ```~```
+ [子代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Child_combinator): ```>```
+ [后代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Descendant_combinator): ```空格```

## [伪类选择器 & 伪元素选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference#%E9%80%89%E6%8B%A9%E5%99%A8)
