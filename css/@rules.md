# css```@```规则

## 概念

一个```@```规则是一个```CSS```语句,用来定义一些样式外的信息.  
如: 字符集, 外部导入样式表, 字体, 动画等.

## 常用规则

+ ```@charset```: 定义样式表(```css```文件)使用的字符集;  
  注意点:  
  + 大小写不敏感;
  + 只能使用双引号;
  + 如果有多个 @charset @规则被声明，只有第一个会被使用，而且不能在HTML元素或HTML页面的字符集相关 ```<style>``` 元素内的样式属性内使用。
  + 它必须是样式表中的第一个元素，而前面不得有任何字符。
  + 因为它不是一个嵌套语句，所以不能在@规则条件组中使用。
  示例:

  ```css
    /* 这儿不能有空格 */@charset/* 这儿只能有一个空格 */"UTF-8";
  ```

+ ```@import```: 引入一个外部样式表;  
  注意点:  
  + 必须先于所有其他类型的规则，@charset 规则除外;  
  + 因为它不是一个嵌套语句，```@import```不能在条件组的规则中使用。
  + 在没有任何媒体查询的情况下，导入是无条件的.  
  示例:  

  ```css
    @import url("./css");
    @import url("./css") print;/* 后面可以跟媒体查询规则 */
  ```

+ ```@namespace```: 是用来定义使用在CSS样式表中的XML命名空间的@规则。
  注意点:
  + 定义的命名空间可以把通配、元素和属性选择器限制在指定命名空间里的元素。
  + ```@namespace```规则通常在处理包含多个```namespaces```的文档时才有用，比如```HTML5```里内联的```SVG```、```MathML```或者混合多个词汇表的```XML```。
  + 任何 ```@namespace``` 规则都必须在所有的 ```@charset``` 和 ```@import``` 规则之后, 并且在样式表中，位于其他任何声明,规则,规则集之前。
  + ```@namespace``` 可以用来定义默认命名空间。当定义过默认命名空间后, 所有的通配选择器和类型选择器（但不包括属性选择器）都只应用在这个命名空间的元素中。
  + ```@namespace``` 规则也可以用于定义命名空间前缀。当一个通配、类型、属性选择器前面有命名空间前缀修饰时，这个选择器将只匹配那些命名空间与 元素名或属性匹配 的元素。
  示例:

  ```css
    /* 默认命名空间 */
    @namespace url(http://www.w3.org/1999/xhtml); /* html默认空间: html|选择器 */
    @namespace "http://www.w3.org/2000/svg"; /* svg默认空间: svg|选择器 */

    /* 命名空间前缀 */
    @namespace asd url(http://www.w3.org/1999/xhtml); /* 把html命名空间前缀改为asd: asd|选择器 */
    @namespace ddd "http://www.w3.org/2000/svg"; /* 把svg命名空间前缀改为ddd: ddd|选择器 */

    asd|a {
      color: #333333;
    } /* html下的a标签 */
    ddd|div {
      color: #444444;
    } /* svg下的a标签 */
    *|div {
      color: #666666;
    } /* 所有空间下的div */
  ```

## 嵌套规则

+ ```@media```: 如果满足媒介查询的条件则条件规则组里的规则生效;
  注意点:  
  + ```@media``` 规则可置于您代码的顶层或位于其它任何@条件规则组内。
  媒体类型:
  + ```all```: 适用所有;
  + ```print```: 适用于打印预览;
  + ```screen```: 屏幕查询;
  + ```speech```: 主要用于语音合成器;
  [媒体特性: 参考MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries);
  逻辑操作符:
  + ```and```: 操作符用于多个查询规则组合成单条媒体查询, 当每个查询都为真时应用;
    可用于组合媒体类型与媒体特性;

  ```css
    screen and (max-width: 500px) {
      .ex {
        color: #333333;
      }
    }
  ```

  + ```not```: ```not```关键字会反转整个媒体查询的含义,如果不满足这个条件则返回```true```.如果出现在以逗号分割的查询列表中,它将仅否定应用了该查询的特定查询.如果使用```not```运算符,则还必须指定媒体类型.
  + ```only```: 老版浏览器不支持媒体特性,使用```only```操作符可以在老版浏览器中排出当前规则;
  + ```,```: ```,```用于将多个媒体查询规则合并为一个规则,每个查询都与其他查询分开处理;任何一个查询为```true```,则整个列表都为```true```.

  媒体类型示例:  

  ```css
    /* 应用于屏幕类型 */
    @media screen {
      .ex {
        color: #333333;
      }
    }
    /* 应用于打印预览 */
    @media print {
      .ex {
        color: #333333;
      }
    }
  ```

  媒体特性示例:  

  ```css
  /* 主要输入机制支持悬停: 如: pc端支持悬停, 移动端不支持悬停 */
    @media (hover/* 媒体特性名 */: hover /* 媒体特性类型 */) {
      .ex {
        color: #666666;
      }
    }
  /* 主要输入机制不支持悬停: 如: pc端支持悬停, 移动端不支持悬停 */
    @media (hover/* 媒体特性名 */: none /* 媒体特性类型 */) {
      .ex {
        color: #666666;
      }
    }
  /* 视窗宽度为1080像素 */
    @media (width/* 媒体特性名 */: 1080px /* 媒体特性类型 */) {
      .ex {
        color: #666666;
      }
    }
  ```

  部分媒体特性支持范围查询:

  ```css
  /* 最大视窗宽度为1080像素 */
    @media (max-width/* 媒体特性名 */: 1080px /* 媒体特性类型 */) {
      .ex {
        color: #666666;
      }
    }
  ```

  复杂查询示例:

  ```css
  /* 屏幕最大宽度1080px或者不支持输入机制悬停,并且视窗最小高度720px */
    @media screen and (max-width: 1080px), (hover: none), not (min-height: 720px) {
      .ex {
        color: #888888;
      }
    }
  ```

+ ```@page```: 用于在打印文档时修改某些特定属性: ```margin```, ```orphans```,```widow```, ```page-break-before```, ```page-break-after```.
  描述符:
  + [size](https://developer.mozilla.org/en-US/docs/Web/CSS/@page/size)
  + [marks](https://developer.mozilla.org/en-US/docs/Web/CSS/@page/marks)
  + [bleed](https://developer.mozilla.org/en-US/docs/Web/CSS/@page/bleed)
  
```css
  @page {
    margin: 10px;
  }
```

+ ```@font-face```: 描述引用的外部字体;
  注意点:
  + 线上字体文件引用受跨域限制;
  + 不能在```css```规则集中使用;
  + 参数说明:  

  ```css
  @font-face {
    font-family: "Example Font"; /* 自定义字体名称可以作为css属性 font-family 的值;*/
    src: url(./example.woff) format(woff), url(./example.ttf); /* 字体文件路径.如果前面的路径找不到字体文件,会按照url顺序依次查找. format是可选的,用来什么字体文件类型*/
    font-variant: none; /* 字型, 取值可以参考mdn文档,建议不使用 */
    font-stretch: normal; /* 字体拉伸, 兼容性较差.大部分浏览器都不支持, 建议不使用 */
    font-style: italic; /* 指定字体样式,建议不使用 */
    unicode-range:  U+26; /* 只下载字体文件指定的部分,可以节省流量 */
  }
  ```

  + 字型取值:
    + ```small-caps```: 显示小号字体;
    + ```normal```: 显示标准字体;
    + ```inherit```: 从父元素继承;
  + 字体拉伸取值:
  
  | 取值 | 拉伸百分比 |
  |---|---|
  | ultra-condensed | 50 |
  | extra-condensed | 62.5 |
  | condensed | 75 |
  | semi-condensed | 87.5 |
  | normal | 100 |
  | semi-expanded | 112.5 |
  | expanded | 125 |
  | extra-expanded | 150 |
  | ultra-expanded | 200 |

  + 字体样式取值:  
    + ```normal```: 标准样式;
    + ```italic```: 斜体;
    + ```oblique```: 人工倾斜;
      ```oblique```是一个css函数,参数是角度;

      ```css
        @font-face {
        font-family: "hanyi";
        src: url(./HYSuZeLiXingKaiOriginalW.ttf);
        font-style: oblique(45deg);
      }
      ```

  + ```unicode-range```: 参考[MDN文档](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/unicode-range)或[腾讯开发者手册](https://cloud.tencent.com/developer/section/1072045)

+ ```@keyframes```: 定义一个动画的关键帧;
  示例:  

  ```css
  @keyframes ex/* 动画名,在animation中使用 */ {
    0% {
      top: 0;
      left: 10px;
    }/* 起始关键帧 */
    10% {
      top: 10px;
    }
    20% {
      top: 30px;
    }
    100% {
      top: 100px;
      left: 100px;
    } /* 结束关键帧 */
  }

  @keyframes ex2/* 动画名,在animation中使用 */ {
    from {
      top: 0;
      left: 10px;
    }/* 起始关键帧 */
    10% {
      top: 10px;
    }
    20% {
      top: 30px;
    }
    to {
      top: 100px;
      left: 100px;
    } /* 结束关键帧 */
  }
  ```
  
  注意点:  
  + 如果一个关键帧规则没有指定动画的开始或结束状态（也就是，0%/from 和100%/to，浏览器将使用元素的现有样式作为起始/结束状态。这可以用来从初始状态开始元素动画，最终返回初始状态。

  ```css
  @keyframes ex {
    10% {
      top: 10px;
    }
    20% {
      top: 30px;
    }
  }
  ```

  + 如果在关键帧的样式中使用了不能用作动画的属性，那么这些属性会被忽略掉，支持动画的属性仍然是有效的。
  + 如果同一帧多次定义了相同的属性,那么只有最后一次定义的生效;
  + 关键帧中出现的```!important```将会被忽略.
+ ```@supports```: 测试浏览器对```css```的支持;
  + 简单使用:

  ```css
  @supports (display: flex)/* 如果括号内的声明有效就返回true */ {
    .ex {
      display: flex;
    }
  }
  ```

  + 使用函数:  

  ```css
  @supports selector(A > B) {
    .ex > div {
      color: #888;
    }
  };
  ```

  + 使用操作符:  

  ```css
  @supports not selector(A > B) {
    .ex div {
      color: #888;
    }
  };
  ```

  操作符类型有:
  1. ```not```: 放在任何表达式之前就能否定一条表达式;
  2. ```and```: 用来将两个原始的表达式做逻辑与后生成一个新的表达式，如果两个原始表达式的值都为真，则生成的表达式也为真.
  3. ```or```: 用来将两个原始的表达式做逻辑或后生成一个新的表达式，如果两个原始表达式的值有一个或者都为真，则生成的表达式也为真.

+ ```@document```: 匹配一个```URL```规则, 限制当前样式只在相匹配的```URL```中生效;
  示例:  
  
  ```css
  @document url("https://www.example.com/") {
    h1 {
      color: green;
    }
  }
  ```

  除```url()```函数外还可以使用:
  1. ```url-prefix()```: 匹配文档的```URL```是否以参数指定的值开头.
  2. ```domain()```, 匹配文档的```域名```是否为参数中指定的```域名```或者为它的```子域名```.
  3. ```regexp()```,匹配文档的 ```URL``` 是否和参数中指定的正则表达式匹配, 该表达式必须匹配整个 ```URL```.
