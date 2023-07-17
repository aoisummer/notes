---
tags:
  - frontend
---

# 前端学习指南

## 基础 {#basic}

要进行开发，应准备一个使用比较友好的编辑器（如 VSCode），以及确定一个兼容的最低浏览器版本，这将决定你能使用到哪些语法、功能、API 等等。

:::info

- HTML 无法被 [Polyfill](<https://en.wikipedia.org/wiki/Polyfill_(programming)>)，但部分可以退化为依靠 CSS 和 JavaScript 实现
- CSS 无法被 Polyfill
- JavaScript 部分标准可以被 Polyfill

:::

### HTML、CSS、JavaScript {#html-css-javascript}

可以从[附录](#appendix)里的 MDN 和 W3Schools 进行基础的了解。

### 布局模式 {#layout-mode}

随着各项标准的发展，布局模式也经历了多次演变。根据当时浏览器的兼容性，产生了如下几种布局模式：

#### table 布局 {#table-layout}

早期的开发是使用 **Microsoft FrontPage** 和 **Macromedia Dreamweaver** 等所见即所得编辑器居多，但生成的代码过于冗余，甚至部分不符合规范标准。这个时期的网页功能很少，以展示类的居多，同时屏幕分辨率也相对较小（`800 * 600`），产生了以 table 为主的布局方式。

<details>
<summary>示例</summary>

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
  <head>
    <title>Title</title>
  </head>
  <body>
    <table width="780" border="0" align="center" cellspacing="0">
      <tr>
        <td colspan="2">
          <center>header</center>
        </td>
      </tr>
      <tr>
        <td width="75%">
          <font color="red" size="4">content</font>
        </td>
        <td width="25%" valign="top">side</td>
      </tr>
      <tr>
        <td colspan="2" bgcolor="#eeeeee">footer</td>
      </tr>
    </table>
  </body>
</html>
```

</details>

#### float 布局 {#float-layout}

随着 CSS 的引入，语义化 HTML 的要求，页面上逻辑变得越来越多，老旧的所见即所得编辑变得不太好用，合适的文本编辑器流行起来。同时为了解决跨浏览器兼容性问题，jQuery 作为这个时代最主要的解决方案遍布各个地方。主流屏幕分辨率提高到了 `1024 * 768`，栅格式布局也来到了 `80 * 12 = 960` 的宽度。

<details>
<summary>示例</summary>

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title>Title</title>
    <style type="text/css">
      .container {
        width: 960px;
        margin: 0 auto;
      }
      .header {
        text-align: center;
      }
      .row:after {
        content: ' ';
        display: block;
        height: 0;
        clear: both;
      }
      .content {
        width: 75%;
        float: left;
        color: red;
        font-size: 1.25em;
      }
      .side {
        width: 25%;
        float: right;
      }
      .footer {
        background-color: #eee;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="header">header</div>
      <div class="row">
        <div class="content">content</div>
        <div class="side">side</div>
      </div>
      <div class="footer">footer</div>
    </div>
  </body>
</html>
```

</details>

#### flex 布局 {#flex-layout}

标准的进一步发展，逻辑的进一步复杂，对 UI 的统一性需求，催生出了 Bootstrap 这款响应式设计优先的框架。在 float 也渐渐不能满足需求的情况下，flex 作为新的标准诞生了，对于现代浏览器来说有着足够好的兼容性（IE 10+），也是目前的主要布局模式。

<details>
<summary>示例</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Title</title>
    <style>
      .container {
        width: 960px;
        margin: 0 auto;
      }
      .row {
        display: flex;
      }
      .content {
        flex: 1;
        color: red;
        font-size: 1.25rem;
      }
      .side {
        width: 25%;
      }
      .footer {
        background-color: #eee;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="header">header</div>
      <div class="row">
        <div class="content">content</div>
        <div class="side">side</div>
      </div>
      <div class="footer">footer</div>
    </div>
  </body>
</html>
```

</details>

了解更多信息：

- [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

## 开发模式 {#development-mode}

下面的部分小节列出了一些成熟的常用工具。

### 传统模式 {#traditional-mode}

针对仅静态内容，使用编辑器编写原生内容然后浏览器刷新查看更改。

### 静态生成（SSG） {#static-site-generator}

在前一节的基础上引入了 Node.js 工具链，同时使用模板引擎等提高代码复用率。

- [Harp](https://github.com/sintaxi/harp)
- [Eleventy](https://github.com/11ty/eleventy)
- [Hexo](https://github.com/hexojs/hexo)
- [Hugo](https://github.com/gohugoio/hugo) （Go 语言）

### 客户端渲染（CSR） {#client-side-rendering}

针对前后端分离并有交互的内容，大量使用了 Node.js 工具链，当前主要开发模式。同时也分为了多个框架流派。

#### React {#csr-react}

- [Create React App](https://create-react-app.dev/)

#### Vue {#csr-vue}

- [Vite](https://vitejs.dev/)
- [Vue CLI](https://cli.vuejs.org/)

#### Angular {#csr-angular}

- [Angular CLI](https://angular.io/guide/setup-local)

### 服务器端渲染（SSR） {#server-side-rendering}

在前一节的基础上发展而来，将前后端整合开发，主要用于全栈，SEO 优化等。（严格来说已超出前端范畴）

#### React {#ssr-react}

- [Next.js](https://nextjs.org/)
- [Remix](https://remix.run/)

#### Vue {#ssr-vue}

- [Nuxt.js](https://nuxtjs.org/)

## 附录 {#appendix}

参考资料：

- MDN: https://developer.mozilla.org/zh-CN/docs/Web<br />
  可以说是最全的参考手册
- W3Schools: https://www.w3schools.com/<br />
  学习基础的好网站
- Developer Roadmaps: https://roadmap.sh/frontend<br />
  你可以关注哪方面内容的汇总、路线图

工具类：

- Can I use: https://caniuse.com/<br />
  兼容性查询的主要工具
- Browserslist: https://browsersl.ist/<br />
  查询浏览器的市场份额占比

段子：

- [在 2016 年学 JavaScript 是一种什么样的体验？](https://zhuanlan.zhihu.com/p/22782487)
