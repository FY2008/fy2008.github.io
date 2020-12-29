---
title: Hexo 配置笔记
date: 2020-12-28 14:45:03
tags: Hexo
category: 计算机
index_img: /img/fluid-favicon.png
banner_img: /img/fluid.png
---

这是一篇介绍 `hexo` 配置的文章。

## 选择一款主题
在我有意的搜索中，发现了这款漂亮的主题 `fluid`，这是一款 `Material Design` 风格的主题，通过测试发现这款主题还是比较完美的。

### 截图
![fluid](/img/fluid.png)

### 官网
* [https://hexo.fluid-dev.com/](https://hexo.fluid-dev.com/)
* [https://github.com/fluid-dev/hexo-theme-fluid](https://github.com/fluid-dev/hexo-theme-fluid)

该主题对中文用户非常友好，配置中的注释都是中文的，所有功能开箱即用。

## 开始使用
快速开始使用 Hexo 写文章并且发布到 `Github Page` 上，前提是还要安装个插件：

```$ npm install hexo-deployer-git --save```

这里贴上配置代码：

```yml
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: 'git'
  repo: 'https://github.com/FY2008/fy2008.github.io.git' # https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
  branch: 'main'
  message: "站点更新: {{ now('YYYY-MM-DD HH:mm:ss') }}"
```

## hexo 常用命令

### init 初始化

```$ hexo init [folder]```

新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。

### new 新建命令
```$ hexo new [layout] <title>```

新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。


| 参数 | 描述 |
|:----|:----|
|-p, --path|	自定义新文章的路径|
|-r, --replace|	如果存在同名文章，将其替换|
|-s, --slug|	文章的 Slug，作为新文章的文件名和发布后的 URL|

默认情况下，Hexo 会使用文章的标题来决定文章文件的路径。对于独立页面来说，Hexo 会创建一个以标题为名字的目录，并在目录中放置一个 index.md 文件。你可以使用 --path 参数来覆盖上述行为、自行决定文件的目录：

```hexo new page --path about/me "About me"```

以上命令会创建一个 source/about/me.md 文件，同时 Front Matter 中的 title 为 "About me"

注意！title 是必须指定的！如果你这么做并不能达到你的目的：

```hexo new page --path about/me```

```D:\dev\blog>hexo new post -p stm32-for-vscode "推荐一款 STM32 的 vscode 插件，stm32-for-vscode"```

这条命令是新建一个文章，并且指定路径名和文章标题名，路径名通过 `-p` 指定，文章标题名在最后面，并且用双引号括起来，以为文章标题包含空格。

### generate 生成命令（g）

```$ hexo generate```
生成静态文件。

| 选项 | 描述 |
|:----|:----|
|-d, --deploy|文件生成后立即部署网站|
|-w, --watch|	监视文件变动|
|-b, --bail|	生成过程中如果发生任何未处理的异常则抛出异常|
|-f, --force|	强制重新生成文件Hexo 引入了差分机制，如果 public 目录存在，那么 hexo g 只会重新生成改动的文件。使用该参数的效果接近 hexo clean && hexo generate|
|-c, --concurrency|	最大同时生成文件的数量，默认无限制|

该命令可以简写为:

```$ hexo g```

### 建立文章草稿

```$ hexo new draft <title>```

`Hexo` 另外提供 `draft` 机制，它的原理是新文章将建立在 `source/_drafts` 目录下，因此 `hexo generate` 并不会将其编译到 `public` 目录下，所以 `hexo deploy` 也不会将其部署到 `GitHub`。

### 将草稿发布为正式文章

```$ hexo publish [layout] <filename>```
```$ hexo P <filename>```

其中 `<filename>` 为不包含 `md` 后缀的文章名称。它的原理只是将文章从 `source/_drafts` 移动到 `source/_posts` 而已。

### server 启动本地服务器

```$ hexo server```

启动服务器。默认情况下，访问网址为： http://localhost:4000/。

|选项|	描述|
|:----|:----|
|-p, --port|重设端口|
|-s, --static|只使用静态文件|
|-l, --log|	启动日记记录，使用覆盖记录格式|

该命令可以简写为:

```$ hexo s```

### deploy 部署命令

```$ hexo deploy```

部署网站。

|参数|	描述|
|:----|:----|
|-g, --generate|部署之前预先生成静态文件|

该命令可以简写为：

```$ hexo d```

### clean

```$ hexo clean```

清除缓存文件 (`db.json`) 和已生成的静态文件 (`public`)。

在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

### list

```$ hexo list <type>```

list 命令参数
```
Arguments:
  type  Available types: page, post, route, tag, category
```

列出网站资料。

### version

```$ hexo version```

显示 Hexo 版本。

### 显示草稿

```$ hexo --draft```

显示 `source/_drafts` 文件夹中的草稿文章。

## Fluid 主题 tag 标签插件的颜色

```
{% note success %}
文字 或者 `markdown` 均可
{% endnote %}
```

或者使用 HTML 形式：

```<p class="note note-primary">标签</p>```

{% note success %}
文字 或者 `markdown` 均可
{% endnote %}



### 可选便签

{% note primary %}
primary
{% endnote %}

{% note secondary %}
secondary
{% endnote %}

{% note success %}
success
{% endnote %}

{% note secondary %}
secondary
{% endnote %}

{% note danger %}
danger
{% endnote %}

{% note warning %}
warning
{% endnote %}

{% note info %}
info
{% endnote %}

{% note light %}
light
{% endnote %}
  
{% note warning %}
**WARNING**
```使用时 {% note primary %} 和 {% endnote %} 需单独一行，否则会出现问题```
{% endnote %}

### 行内标签
在 `markdown` 中加入如下的代码来使用 `Label`：

```{% label primary @text %}```

或者使用 HTML 形式：

```<span class="label label-primary">Label</span>```

{% label primary @text %}
{% label default @text %}
{% label info @text %}
{% label success @text %}
{% label warning @text %}
{% label danger @text %}

### 勾选框
在 markdown 中加入如下的代码来使用 Checkbox：
```{% cb text, checked?, incline? %}```

text：显示的文字
checked：默认是否已勾选，默认 false
incline: 是否内联（可以理解为后面的文字是否换行），默认 false

{% cb text, true, true %}
{% cb text, false, true %}
{% cb text, false, true %}
{% cb false %} text

### 按钮
你可以在 markdown 中加入如下的代码来使用 Button：

```{% btn url, text, title %}```

或者使用 HTML 形式：

```<a class="btn" href="url" title="title">text</a>```

{% btn https://www.github.com/fy2008, Github, Github %}

