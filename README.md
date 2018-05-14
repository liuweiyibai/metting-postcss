---
title: 很大一个疑问：PostCSS 是什么
tags:
  - css
abbrlink: 20170816
date: 2017-08-16 14:18:16
---

首先，PostCSS 和styl、scss、以及less等预处理语言，差别在哪里。

### css预处理语言

众所周知，css是没有变量和作用域的，所以看起来不像一门语言，所以便有了css预处理器的出现。旨在帮助开发者更爽的写 css。

### PostCSS

有人说：PostCSS 就是 CSS 界的 babel。
也有人说：PostCSS 是用来处理你的css，让你的css更加健壮的。
说的都是很对，PostCSS就是用来处理css，比如，自动给你的css代码增加浏览器前缀等等。

### 你写的代码，不是最终的代码。

学过编译型语言的人都知道，代码会被转化成机器码或者中间码，你写的代码，不是最终运行在机器上的代码（二进制）。而只学的 JS 的人，没有这种体会。PostCSS 就是一种类似编译的过程（应该叫转译）。它的目的是让你的代码更可靠（Bug更少、兼容性更高甚至功能更强大）。PostCSS 就可以认为是 CSS 的「编译器」。


### PostCSS的配置以及使用，不使用webpack

你使用 PostCSS 是为了用里面的插件来强化你的css

安装 PostCSS 以及插件

```bash
npm i --save-dev postcss
npm i --save-dev postcss-cli
npm i --save-dev autoprefixer
```

新建一个 `style/index.css` 文件 随便写点css样式放进去

```css
body{
  margin: 0;padding: 0;
  transform: translate(3d);
}
```

然后在项目根目录中跑命令

```bash
npx postcss ./style/index.css -o ./style/dist.css -u autoprefixer
```

将会在`style/`中新建一个`dist.css`文件来保存处理后的css

```css
body{
  margin: 0;padding: 0;
  -webkit-transform: translate(3d);
          transform: translate(3d);
}
```

但是为了避免我们每次使用该插件都要在shell中添加配置，所以在项目根目录要添加一个PostCSS的配置文件`.postcssrc.js`

```js
module.exports = {
  plugins: {
    autoprefixer: {
      browsers: ['last 7 iOS versions', 'last 3 versions', '> 1%']
    }
  }
};
```

然后直接执行如下命令，能得到和上面处理一样的结果

```bash
npx postcss ./style/index.css -o ./style/dist.css
```

### 在webpack中使用PostCSS

使用预处理语言scss or less等,webpack的loader是从右向左加载的，所以要先处理预处理语言，然后在使用postCss来处理

```js
{
  test: /\.less$/,
  use: [
    'style-loader',
    'css-loader',
    'postcss-loader',
    'less-loader'
  ]
},{
  test: /\.scss$/,
  use: [
    'style-loader',
    'css-loader',
    'postcss-loader',
    'scss-loader'
  ]
},{
    test: /\.scss|css$/,
    use: [
        "style-loader",
        "css-loader",
        "postcss-loader?sourceMap",
        "resolve-url-loader",
        "sass-loader?sourceMap"
    ]
}
```