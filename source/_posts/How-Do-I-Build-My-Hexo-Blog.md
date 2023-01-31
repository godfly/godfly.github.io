---
title: 建站
date: 2023-01-31 10:18:19
toc: true
categories: 
- Tech
- Web
tags: 
- [Blog,Hexo]
---

这里记录下这个[Hexo Blog](https://hexo.io/)的搭建过程。上个Blog是基于LNMP在阿里云上搭建的，服务器+域名+没有备份显然没有免费的github香啊。整个搭建过程熟练的话半小时就能搞定，托管在github上基本0成本，用于学习的输出足够了。这里主要记录Hexo搭建-Icarus主题安装-主题修改-Hexo博客撰写-上传github，共五部分。

<!-- more -->

## Blog方案

如你所见的Blog是使用[Hexo Blog](https://hexo.io/) (version 6.3.0, hexo-cli 4.3.0)搭建的，使用了[Icarus](https://github.com/ppoffice/hexo-theme-icarus)(release 5.1.1)主题。我使用的是Mac系统，[nodejs](https://nodejs.org/)for Mac version 14.17.0。

选用Hexo理由有两个：用户基数大，官方更新频繁。避免搭建时或日后修改时踩坑。相对于Hexo另一个主流的静态博客框架是基于Ruby的JekyII，不熟悉Ruby的话折腾成本要大一些，Pass。

## Hexo搭建

[Hexo的官方文档](https://hexo.io/zh-cn/docs/index.html)写的非常详细，以下只是记录。

### 1.环境安装
使用HomeBrew安装NodeJS

``` shell
# 安装
brew install node
# 验证
node -v
npm -v
```

### 2.Hexo安装
使用npm安装Hexo

``` shell
# 安装
npm install -g hexo-cli
# 进阶安装（没有使用）
npm install hexo
```

### 3.Hexo建站
[Hexo的官方文档](https://hexo.io/zh-cn/docs/setup)中有详细内容，下面记录命令。

1.Hexo项目初始化
``` shell
hexo init <folder>
cd <folder>
npm install
```

2.项目结构
新建完成后文件夹目录如下：
``` 
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
其中比较重要的是：
- `_config.yml`: 站点配置文件，包括站点标题、作者、时区、URL、语言、主题等[配置](https://hexo.io/zh-cn/docs/configuration)
- `package.json`:` 应用程序信息，包括Hexo版本、依赖等
- scaffolds: [模板](https://hexo.io/zh-cn/docs/writing#%E6%A8%A1%E7%89%88%EF%BC%88Scaffold%EF%BC%89) 文件夹，新建文章时的模板
- source: 用户资源 文件夹。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽
- themes: [主题](https://hexo.io/zh-cn/docs/themes) 文件夹。Hexo 会根据主题来生成静态页面

3.验证
``` shell
# 启动服务
hexo server
```

到此就完成了hexo的搭建

## Icarus主题安装
[Icarus用户指南](https://ppoffice.github.io/hexo-theme-icarus/tags/Icarus%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97/) 中有详细内容，下面记录命令。

1.Icarus下载
我比较习惯用源码安装，方便后面修改。此外我没有采用submodule的方法：

``` shell
git clone https://github.com/ppoffice/hexo-theme-icarus.git themes/icarus -b <version number> --depth 1
```
可以省略`-b <version number>`来获取Icarus的最新开发版本。 如果你想同时下载Git仓库的完整提交历史，请同时省略`--depth 1`

2.Icarus配置启用

在站点配置文件`_config.yml`中的开启Icarus：
```
{% codeblock _config.yml lang:yaml %}
theme: icarus
{% endcodeblock %}
```

或使用`hexo`命令修改主题为Icarus:
```
{% codeblock "命令行" %}
hexo config theme icarus
{% endcodeblock %}
```

到此就完成了Icarus主题安装

## 主题自定义
[Icarus用户指南](https://ppoffice.github.io/hexo-theme-icarus/tags/Icarus%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97/) 中有详细内容，[Icarus社区](https://github.com/ppoffice/hexo-theme-icarus/discussions) 中有很多已解决的问题。下面记录我修改过的部分和遇到的坑，有些是Hexo的技巧也写在这里。

### 1.首页文章折叠
使用`<!-- MORE -->` 来设置简介，首页的文章预览超过一定长度时就自动折叠，并有一个`Read More`按钮

### 2.Icarus样式修改

1. Footer高度调整
   `themes/icarus/include/style/base.styl`中添加 `$footer-padding`，例如`$footer-padding ?= 2rem 1.5rem 2rem`

2. 加宽正文布局 [引用](https://blog.mchook.cn/2021/07/22/icarus%E4%B8%BB%E9%A2%98%E8%87%AA%E5%AE%9A%E4%B9%89/)
   `themes/icarus/include/style/base.styl`修改已有配置:
   ``` javascript
   $gap ?= 64px
   $tablet ?= 1104px
   $desktop ?= 1400px
   $widescreen ?= 1600px
   $fullhd ?= 1920px
   ```

3. 缩小边栏宽度 [引用](https://github.com/ppoffice/hexo-theme-icarus/issues/696)
   `themes/icarus/layout/common/widgets.jsx`中修改widge宽度`is-4-widescreen`变小为`is-3-widescreen`。
   ``` javascript
   function getColumnSizeClass(columnCount) {
       switch (columnCount) {
        case 2:
            return 'is-4-tablet is-4-desktop is-3-widescreen';
        case 3:
            return 'is-4-tablet is-4-desktop is-3-widescreen';
        }
        return '';
    }
    ```

    页面宽度使用了[bulma](https://bulma.io/documentation/columns/sizes/)关于宽度的定义，所有栏加起来要等于12，因此主页面也需要修改：

    `themes/icarus/layout/layout.jsx`中修改widge宽度`is-4-widescreen`变小为`is-3-widescreen`。
    ``` javascript
    <div class={classname({
        column: true,
        'order-2': true,
        'column-main': true,
        'is-12': columnCount === 1,
        'is-8-tablet is-8-desktop is-9-widescreen': columnCount === 2,
        'is-8-tablet is-8-desktop is-6-widescreen': columnCount === 3
    })} dangerouslySetInnerHTML={{ __html: body }}></div>
    ```
4. 首页更改为两栏，文章页一栏
   单独文章页的配置涉及到[icarus的优先级知识](https://ppoffice.github.io/hexo-theme-icarus/Configuration/icarus%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97-%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE/)，简单说有四个配置方法
   - 站点配置文件：`_config.yml`
   - 主题配置文件：`_config.icarus.yml`
   - 布局配置文件：`_config.post.yml`中的配置对所有文章生效，而`_config.page.yml`中的配置对所有自定义页面生效，这两个文件将覆盖主题配置文件中的配置。
   - 文章/页面的front-matter：会覆盖所有上面的配置源
   
   那么我们可以如此配置：

   在`_config.post.yml`中把所有文章变为一栏布局：

   ``` yml
   widgets:
   ```

    在`_config.post.yml`中，配置所有其他页面仍保持两栏布局：

   ``` yml
    widgets:
    -
        position: right
        type: toc
        index: true
        collapsed: true
        depth: 3
    -
        position: right
        type: recent_posts
    -
        position: right
        type: categories
    -
        position: right
        type: tags
        order_by: name
        amount: 10
        show_count: true
   ```

5. 单独的代码折叠
   ```
   {% codeblock "可选文件名" lang:代码语言 >folded %}
    ...代码块内容...
    {% endcodeblock %}
   ```

6. 分享文章
   默认的`sharethis`大陆用不了，更换为`sharejs`（太杂了，TODO精简）
   `_config.icarus.yml`更改`share`的`type`为sharejs

7. To be Continue

当然，如果你觉得还不错也可以直接clone我的git，欢迎star~
最后感谢Icarus作者

## Hexo博客撰写
[Hexo官方文档](https://hexo.io/docs/writing.html) 比较详细，常用的命令：
``` bash
# 创建新文章
hexo new "my post"

# 也可以更复杂一点
hexo new [layout] <title>
# layout post等同于hexo new 
hexo new post <title>
# layout page 用于页面
hexo new page <title>

# layout draft 为草稿，可在把 render_drafts 参数设为 true 来预览草稿 或 执行时加上 --draft 参数
hexo new draft <title>
# 草稿发布
hexo publish [layout] <title>

# 根据 scaffolds 文件夹内相对应的模板文件（photo.md）来建立文件
hexo new photo "My Gallery"
```

## 上传github
[Hexo官方文档](https://hexo.io/zh-cn/docs/github-pages) 比较详细，常用的命令：


