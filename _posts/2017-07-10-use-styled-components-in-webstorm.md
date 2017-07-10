---
layout: post
comment: false
title:  "在 Webstorm 中使用 styled-components"
date:   2017-07-10 10:00 +0800
categories: Other
---

styled-components 是目前比较火的 css-in-js 方案，最近在做一些组件和工作的工作中也尝试去用它。不过 WebStorm 原生对他的支持并不好，在不加任何配置之前，styled-components 在 WebStorm 中是这样的：

![](/assets/images/2017-07-10-1.png)

css 部分完全没有被编辑器识别并且进行语法高亮，对于开发来说非常不方便。而 styled-components 目前还没有提供相应的 WebStorm 插件，所以我们只能借助 WebStorm 自己的工具来进行 styled-components 的支持。

#### 添加 Language Injections
首先，我们需要借助 WebStorm 的 [Language Injections] 这一项设置，来帮助编辑器认出在 JS 代码中的 CSS 代码。JetBrains 官方对 Language Injections 的说明如下：

> You can inject a language (such as HTML, CSS, XML, RegExp, etc.) into a string literal in your code and, as a result, get comprehensive coding assistance when editing that literal.

它允许我们将任意一段代码识别为你需要的语言，这样你就可以使用相应的高亮以及代码补全等特性。我们可以添加如下配置，将 JS 代码中符合 styled.+任意字符串以及以 // styled 或 /* styled */ 作为开头的代码段识别为 less，

![](/assets/images/2017-07-10-2.png)

![](/assets/images/2017-07-10-3.png)

```
+ jsLiteralExpression().withText(string().matchesBrics("styled.+"))
+ jsLiteralExpression().withText(string().startsWith("`/* styled */"))
+ jsLiteralExpression().withText(string().startsWith("`// styled"))
(方便复制)
```
来让 WebStorm 识别出如下格式的 styled-components 代码，

```
const Page = styled.div`// styled
  & {
    padding: 16px 32px;
  }
`;

or

const Page = styled.div`/* styled */
  & {
    padding: 16px 32px;
  }
`;
```
效果如图，同时支持了语法高亮和代码补全，

![](/assets/images/2017-07-10-4.png)

这样我们每次在使用到 styled-components 时，只需要按照上述的格式编写即可。

g#### 添加 Live Template
添加完识别之后，我发现 styled-components 定义的代码相对繁琐，要写组件定义，还要加 styled 的注释，所以我为他还添加了对应的 [Live Template]，这一功能可以参见 JetBrains 的官方文档。具体配置如下，

![](/assets/images/2017-07-10-5.png)

```
const $COMP_NAME$ = styled.$COMP_TYPE$`// styled
  & {
    $END$
  }
`;
```

使用的时候只需要在输入 styled 后点击 Tab键即可，效果如下，

![](/assets/images/2017-07-10-6.png)

这样就可以愉快地在 WebStorm 中使用 styled-components 啦~

[Language Injections]: https://www.jetbrains.com/help/webstorm/2017.1/using-language-injections.html  "Language Injections"
[Live Template]: https://www.jetbrains.com/help/webstorm/2017.1/live-templates-2.html  "Live Template"