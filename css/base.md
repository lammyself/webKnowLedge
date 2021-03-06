# ```CSS```基础概念

## 声明

```CSS```的核心功能是为```CSS```属性赋予特定的值.将值赋予属性并获得一个键值对,被称为```声明```.  
在我的理解中```声明```是一个将值赋予属性的行为,得到的结果也可以被称为```声明```.[参考MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Syntax#CSS_%E5%A3%B0%E6%98%8E%E5%9D%97)
每个属性都有对应的合法值, 如果为一个属性赋一个非法的值,那么这个```声明```将会被```CSS```引擎忽略.
示例:  

  ```css
    color: #000000; /* 合法值 */
    color: 1px; /* 非法值 */
  ```

## 声明块

```声明```应该按照块的形式被组合,每个声明块被```{}```包裹.  
每个```声明```都应以```;```结尾,```声明块```中的最后一项可以不用分号.

示例:  

  ```css
    {
      width: 1px;
    }
    /* 或 */
    {
      width: 1px;
      height: 1px;
    }
  ```

## 规则

```规则```就是通过```选择器```为元素选择```css```的声明块,一对```选择器```与```声明块```被称为规则集.  

示例:  

  ```css
    body {
      color: #000000;
    }
  ```

## 语句

```语句```是用来描述```css```内容的文本信息, 以非空格的字符开头, 以第一个```}```或者```;```结束.

合法```语句```类型有: ```规则```, ```@规则```.  
其余```语句```为非法语句,将会被忽略.

## 优先级

当相同的```属性```被应用到一个```元素```上时, 会根据```优先级```决定哪一个生效.  
```优先级```主要由```规则集```的```选择器```决定, 或者通过```内联样式```或```!important```提升```优先级```.  
其中```!important```优先级为最高, ```内联样式```低于```!important```高于选择器.

  ```mermaid
  graph LR
  B(高) --> b(低)
  ```

| 等级 | 1 | 2 | 3 | 4 |
| ---- | ---- | ---- | ---- | ---- |
| 选择器| id | 类, 属性, 伪类 | 标签, 伪元素 | 通配符 |

根据```组合选择器```, 优先级可叠加.
低等级```选择器```与高等级```选择器```叠加, 因进制过大低等级```选择器```相对高等级```选择器```可以忽略.(相关文章中提到进制为```65536```)
可以先判断高等级```选择器```叠加权重,如果高等级```选择器```权重相等再判断低等级```选择器```.
最后应用```优先级```高的样式.

## 层叠

```css```中```规则```与```声明```的顺序很重要, 当相同的```属性```被应用到同一个``元素``上并且```优先级```相同时,实际生效的是后面那个.  

## 继承

一些在```父元素```上的```css```属性会被应用到子元素上.

如: 文字属性与文本属性.
