---
layout:     post                          
title:     '基于Jekyll与Github Pages搭建博客'          
date:       2019-09-30                   
author:     Max.C                    
header-img: 'assets/img/pro5.jpg'
catalog: true                           
tags:                                   
    - Github
    - jekyll
    - 搭建博客
---

## 引言

有搭建博客这个想法的原因是看到室友搭的博客，感觉很不错，暑假了也得搞点事情，于是就参考了室友的博客，查了不少教程学着自己也搭一个。

下面我就来说明一下自己的博客搭建过程，因为是第一次尝试，所以这篇博客可能出现一些错误，欢迎大家指出~

## 搭建过程
搭建博客的思路参考了吴坎师兄的博客，搭建的流程也基本遵从师兄的配置教程与网络上的教程。

首先通过知乎了解了一下基于 Github Pages 的博客搭建方式，选择了 Jekyll + Github Pages 的方式，下面简单说明下这种搭建方式。

> Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过一个转换器（如 Markdown）和我们的 Liquid 渲染器转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 GitHub Pages 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。

>GitHub Pages 是一个静态网站托管服务，直接从github仓库托管你个人、公司或者项目页面 ，并且不需要你写任何后端语言来支持。 

根据自己刚学到的知识，简单来说，GitHub Pages 可以被认为是用户编写的、托管在 GitHub 上的静态网页，即可以当作一个小服务器使用。

而 Jekyll 是一个生成静态网页的工具，在 Github 上绑定自己的域名后可以当作个人博客访问。

那么接下来就开始用这两个工具搭建一个属于我们自己的博客吧。


### 1. 创建一个 Github 库并开启 Github Pages
首先我们用自己的 Github 账号创建一个新的库（repository），这个库的名称有固定的格式： `username.github.io`，其中 `username` 必须是 Github 账户的用户名，`.github.io` 是固定的，这个地址将会成为个人站点的网站地址。另外，我们可以勾选`Initialize this repository with a README`，让仓库自动创建一个 `README.md` 文件。

![我创建的 Github 库](/assets/post_img/2019-08-08/1.png)

创建完成后，进入所创建的库，在`settings`页面找到`GitHub Pages`进行设置，如果你的库有按照上述方式进行命名，则它会自动进行设置，设置成功后会该页面出现绿色的提示，成功后可选择 `Choose a theme` 选择一个主题（默认的主题比较少，我们暂时先选一个）。

![GitHub Pages设置](/assets/post_img/2019-08-08/2.png)

到这一步，我们就成功完成了 Github Pages 的配置，接下来我们就需要安装 Jekyll ，上网找一个 Jekyll 的博客模板，再将自己修改后的模板上传至这个库中就可以完成我们的个人博客了。

补充：如果没有 Github 账号、不熟悉 Github 的使用的，可以参考下面的教程，其实我也是这几天才开始慢慢学习使用 Github 的。
- [github新手使用指南](https://blog.csdn.net/qq_37512323/article/details/80693445)
- [真小白入门之Github](https://blog.csdn.net/nmjuzi/article/details/82184818)


### 2. Jekyll 运行环境的配置与安装
事实上，在搭建博客的过程中，配置这个安装环境我花的时间是最久的也是最懵逼的，最后也是~~不知道为什么~~才终于配置成功。

那首先，根据教程，运行 Jekyll 所需的环境如下：

- Ruby
- Ruby Gems
- NodeJS或其他 JavaScript 运行环境
- Python2.7(或2.7以上版本）

由于网络上大部分教程都是在 Linux 上配置并安装 Jekyll ，看起来操作也比较简单，也比较推荐大家用 Linux 作为配置环境。而我作为一个强迫症，已经将自己 Github 的库克隆在 Windows 的本地文件里，不想再改位置，于是我硬着头皮一边查资料，一边尝试将 Jekyll 安装在 Window 系统上。

首先我们要安装的是 Ruby 和 Ruby DevKit，安装具体流程可以参考[Windows上Jekyll的安装](http://jekyll-windows.juthilo.com/1-ruby-and-devkit/)。什么，都是英文看不懂？Chrome网页翻译了解一下。

下载 Ruby 时，如果选择的是 WITH DEVKIT 版本，可以直接安装下载的文件，不必执行上面的教程里 **安装Ruby DevKit** 这一项。

![](/assets/post_img/2019-08-08/3.png)

安装 Ruby 时，一开始因为不想装在C盘我就更改了安装路径，结果不知道出于什么原因，执行`gem install jekyll`指令时总会出错，最后还是老老实实安装在了默认安装路径。

安装完成后可以用命令行执行`ruby -v`和`gem -v`检测是否安装成功。

接下来我们可以安装NodeJS，这个比较简单，在 NodeJS 官网就可以[下载](https://nodejs.org/en/download/)，找到适合自己系统的版本并安装，安装过程一路选择默认选项，安装后可以用命令行执行`node -v `和`npm -v`检测是否安装成功。

我的 Windows 在之前就已经安装好了 Python 环境，所以这一次没有进行安装，没有安装的朋友可以参考[Python安装教程](https://baijiahao.baidu.com/s?id=1606573927720991570&wfr=spider&for=pc)。

如果完成了上面环境的配置，打开命令行，执行`gem install jekyll`，~~然后保佑安装过程一切正常，~~安装后执行`jekyll -v`检测是否安装成功，如果成功显示版本，那么恭喜你，搭建博客过程中最让人云里雾里的一部分终于完成了QAQ。

```C
C:\Users\34961>jekyll -v
jekyll 3.8.6
```


### 3. 个人博客的搭建
为了让我们的博客看起来更有逼格，我们最好选择下载一个模板，在 http://jekyllthemes.org/上 可以找一个合适的模板。

我的博客选择的是在 Github 上找的博客主题，它对目录中各个文件的内容、功能都有很详细地说明，适合我这样的小白学习使用 Jekyll 。我在对这个模板进行改造的过程中也慢慢熟悉了 jekyll 的目录结构和操作方式。
- [我使用的博客模板](https://github.com/kaeyleo/jekyll-theme-H2O#%E6%A0%87%E7%AD%BE)

 jekyll 的目录结构大概是这样的：
```
.
├── assets # 存放用于线上环境的静态资源，比如我们想放在博客上的图片之类
├── _config.yml # 配置文件，我们通过修改这里的参数改造博客
├── _data
|   └── members.yml
├── _drafts # 未发布的文章。这些文件的格式中都没有title.MARKUP数据。
|   ├── begin-with-the-crazy-ideas.md
|   └── on-simplicity-in-technology.md
└── index.html # 模板首页
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts # 用于存放博客文章的文件夹，文件格式必须符合: 年-月-日-文章标题.md
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.md
|   └── 2009-04-26-barcamp-boston-4-roundup.md
├── _sass
|   ├── _base.scss
|   └── _layout.scss
├── _site
├── .jekyll-metadata
```

几个主要文件的参数在上面的 Github 页面上有很清楚的说明，想直接用这个模板的朋友可以根据说明修改。在修改模板中，我暂时只改了`_config.yml`、`index.html`，在`../assets/img`里面加上了一些图片，将`../_posts`里的文章整理了一下。

顺便一提，我修改到最后才发现这个模板使用的代码高亮库 Prism.js 里面不支持 C++ 语言，作为一个学 C++ 的人突然感到非常绝望，于是又花了不少时间查资料学着重新配置 Prism。（配置其实也挺简单的）

首先我们打开[这个网址](https://prismjs.com/download.html#themes=prism-okaidia&languages=markup+css+clike+javascript)进行 Prism 的下载，在这里可以选择想要的主题、支持的语言和一些特性，选择完成后在网址最下方选择下载`Prism.js`和`Prism.css`，将`.css`覆盖到`\assets\css`文件夹中，将`.js`文件覆盖到`\assets\js`文件夹中就完成了配置。

为了看到博客呈现出来的效果，就要用上我们上一步安装的 Jekyll，先打开命令行，将路径修改至**博客模板所在路径**，执行命令` jekyll server`，复制 http://127.0.0.1:4000/ 到浏览器打开，就能看见本地的博客了。

```
PS E:\Github\437436999.github.io> jekyll server
Configuration file: E:/Github/437436999.github.io/_config.yml
            Source: E:/Github/437436999.github.io
       Destination: E:/Github/437436999.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 2.391 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'E:/Github/437436999.github.io'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

通过查看本地博客和修改文件中的参数，我们就可以慢慢完善出一个属于自己的博客了。当你对自己本地的博客满意后，就可以开始将博客文件上传到第一步创建的 Github 库中了。


### 4. 博客文件的上传
将文件上传至 Github 上的方法就不多赘述，我使用的是 GitHub 桌面版，操作起来比较简单，具体操作可以参考[这里](https://zhuanlan.zhihu.com/p/28321740)。

文件成功上传之后，在 Github 库页面就可以看到你的博客文件了，这时只要访问`username.github.io`，你就能看到自己搭建的个人博客了。

![我的博客所在的Github库](/assets/post_img/2019-08-08/4.png)

### 5. 博客高亮代码设置（2019.9.30）
根据博客`_config.yml`的内容，本博客利用了`rouge`作为语法高亮插件。`_config.yml`里相关的参数如下：
```yml
markdown: kramdown
highlighter: rouge
kramdown:
  input: GFM 
```

根据上面的教程，我们已经配置好 Ruby 环境，我们可以用命令配置rouge文件，步骤如下：

1. 打开命令行，输入命令安装rouge：`gem install rouge`
2. 安装之后，使用以下命令查看自带的样式有哪些：`rougify help style`。样式主题预览可以参考下面的参考资料。
3. 在本地博客文件的**根目录**里打开命令行，使用命令`rougify style github > css/syntax.css`生成一个 github 风格的样式到`css/syntax.css`文件中。
4. 由于我们的博客原来已经引用了rouge插件，所以`_includes/head.html`中已经引用了该css文件。如果博客里原来没有引用，注意需要在`head.html`中加入`<link rel="stylesheet" href="{{ "/css/syntax.css" | prepend: site.baseurl }}">`

添加css文件后，高亮代码设置完成了，上传至GitHub就可以在博客里查看效果了。

同时，由于感觉代码字号太小，我还试着修改博客的字号大小，首先，根据`head.html`的信息，博客参数引用自`hux-blog.min.css`文件中:
```html
    <!-- Custom CSS -->
    <link rel="stylesheet" href="{{ "/css/hux-blog.min.css" | prepend: site.baseurl }}">
```

由于.min文件没有缩进，为了便于我们修改，我们先将上面的引用文件改为`hux-blog.css`，这样我们就可以直接修改`hux-blog.css`文件来修改参数了。

打开`hux-blog.css`，查找（Ctrl+F）到code相关的信息如下：
```css
pre code {
  display: block;
  width: auto;
  white-space: pre;
  word-wrap: normal;
}
```

发现没有字号大小相关的信息，我们尝试在大括号中加上`font-size: 17px;`，上传至GitHub，发现代码的字号大小成功改变。


## 参考资料
本次博客的搭建主要参考了以下内容，感谢作者们~
- [基于Jekyll搭建个人博客](https://wu-kan.github.io/posts/%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA/%E5%9F%BA%E4%BA%8EJekyll%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2)
- [jekyll-theme-H2O博客主题](https://github.com/kaeyleo/jekyll-theme-H2O)
- [个人网站的搭建（基于GitHub和Jekyll主题 ）](https://blog.csdn.net/qq_19799765/article/details/80869363)
- [Jekyll + Github Pages 博客搭建入门](https://www.jianshu.com/p/9f198d5779e6)
- [语法高亮不够漂亮？这里有你想要的 Rouge 主题](https://cloud.tencent.com/developer/article/1341156)
