---
title:	"使用GithubPages和Jekyll搭建个人主页"
layout: post
categories: Config_Log
---

### 1. 基于Github的个人主页搭建

Github Pages是面向用户、组织和项目开放的公共静态页面搭建托管服务，站点可以被免费托管在Github上，你可以选择使用Github Pages默认提供的域名github.io或者自定义域名来发布站点，更便利地是你直接从你的GitHub存储库托管。只需编辑和推送你的blog，并且你的更改是实时的。

#### 1.1 创建Repo

进入[Jekyll的主题模板网站](https://jekyllthemes.io/jekyll-blog-themes)找到想要的模板（我这里选择的是blog themes下的Contrast模板）

![image-20230429134509914](/images/ConfigLog/2023-04-29-使用GithubPages和Jekyll搭建个人主页_images/image-20230429134509914.png)

进入后点击`Get Contrast on Github`进入Github仓库，选择`Fork`到自己仓库

![image-20230429134816602](/images/ConfigLog/2023-04-29-使用GithubPages和Jekyll搭建个人主页_images/image-20230429134816602.png)

在自己的仓库中选择`Settings`-`General`，将仓库重命名为“你的Github用户名.github.io”，注意这里必须和你的用户名一致，我的是“UstcZHZhang.github.io”，点击`Rename`重命名

![image-20230429135623039](/images/ConfigLog/2023-04-29-使用GithubPages和Jekyll搭建个人主页_images/image-20230429135623039-1682747789333-13.png)

现在就可以用[ustczhzhang.github.io](https://ustczhzhang.github.io/)这个网址来访问自己的主页了。

#### 1.2 个性化自己的主页

可以在右上角的`设置齿轮`修改下此仓库的信息（About）

![image-20230429140134833](/images/ConfigLog/2023-04-29-使用GithubPages和Jekyll搭建个人主页_images/image-20230429140134833.png)

通过修改文件配置来个性化自己的主页，可以用过Github Desktop将仓库导入到本地，也可以直接在github上直接修改。

我这里给出对于仓库中一些文件的大致介绍

| 文件名         | 说明                                            |
| -------------- | ----------------------------------------------- |
| assets         | 存放CSS、JavaScript和其他资源文件的目录         |
| _data          | 存放YAML、JSON或CSV格式的数据文件，以供模板使用 |
| _includes      | 存放可以在布局和页面中重复使用的HTML片段的目录  |
| _layouts       | 存放模板布局文件的目录                          |
| _posts         | 存放博客文章文件的目录                          |
| _sass          | 存放Sass样式文件的目录                          |
| 404.html       | 定义404错误页面的HTML文件                       |
| archive.html   | 文章归档页面文件                                |
| Gemfile        | 定义Ruby依赖项（包括Jekyll插件）的文件          |
| index.md       | 网站首页文件（Markdown格式）                    |
| README.md      | 项目的自述文件（Markdown格式）                  |
| _config.yml    | Jekyll的配置文件，包含网站设置和选项            |

具体的一些修改可以参考视频[How to make a personal website with github pages - YouTube](https://www.youtube.com/watch?v=qZsgPgGdOzQ)

---

### 2. 基于Ruby的本地编写和调试主页内容

由于我们使用Jekyll建站，如果想在本地预览的话，就需要安装Ruby。如果是把代码提交到GitHub上发布后，通过浏览器预览，则不需要安装。

#### 2.1 下载和安装Ruby

windows系统，直接[下载Ruby+Devkit(x64)](https://rubyinstaller.org/downloads/)，我下载的版本是2.7.8，注意安装时安装路径的文件夹不能含有空格，其他都选择默认的即可

![image-20230429105737197](/images/ConfigLog/2023-04-29-使用GithubPages和Jekyll搭建个人主页_images/image-20230429105737197.png)

安装结束后会弹出如图的界面，输入`3`点击`Enter`等待安装，如果安装失败可以用

```powershell
ridk install
```

来重新进行安装。

![image-20230429110011979](/images/ConfigLog/2023-04-29-使用GithubPages和Jekyll搭建个人主页_images/image-20230429110011979.png)

安装完成后，应该已经有ruby、gem（包管理器）、bundle了，可以用

```powershell
ruby -v
gem -v
bundle -v
```

进行查看。

![image-20230429110544058](/images/ConfigLog/2023-04-29-使用GithubPages和Jekyll搭建个人主页_images/image-20230429110544058-1682737546053-1.png)

#### 2.2 安装jekyll

使用gem安装jekyll，我这里安装的是3.8.5版本：

```powershell
gem install jekyll -v 3.8.5
```

最后安装github-pages，这部分时间可能较长：

```powershell
gem install github-pages
```

#### 2.3 进行本地调试

首先进入自己博客所在目录：

```powershell
cd E:\Program\Repository\UstcZHZhang.github.io\
```

在这里我遇到一些bug。正常来说`bundle`应该在我的当前目录下去寻找Gemfile文件，但是我运行时确总是去C盘根目录下寻找，所以我在这里需要修改下环境变量：

```powershell
$env:BUNDLE_GEMFILE = "E:\Program\Repository\UstcZHZhang.github.io\Gemfile"
echo $env:BUNDLE_GEMFILE
```

此时可以正确找到Gemfile了，确认下Gemfile中有如下内容，没有的话手动添加上：

```
gem 'github-pages', group: :jekyll_plugins
```

然后下载Gemfile中的对应文件：

```powershell
bundle install
```

这里仍然有bug，正常来说我使用`jekyll serve`时应该在当前目录下，但是系统仍然跑到了C盘根目录下，所以只能通过`sourse`手动指定路径：

```powershell
bundle exec jekyll serve -P 5555 --watch --source "E:\Program\Repository\UstcZHZhang.github.io"
```

`--watch`表示这个本地网页是实时刷新的，当你更改网页的内容时它能实时的变化，而不用不断重启和加载网页。`-P 5555`参数是指定端口号为`5555`，Jekyll默认的端口号是4000，会与福昕阅读器的端口号冲突（如果你的电脑安装了福昕阅读器），所以还是指定端口号最佳。正常情况下你能看到类似下图的启动界面了，此时在浏览器的地址栏输出 `localhost:5555`就能看到你的主页预览了。

![image-20230429112547010](/images/ConfigLog/2023-04-29-使用GithubPages和Jekyll搭建个人主页_images/image-20230429112547010.png)

---

### 3. 一些修改记录

#### 3.1 修改网页的icon图标

去一些常见的网站寻找想使用的icon图（如[icon-icons.com](https://icon-icons.com/)），下载对应.ico文件，重命名为favicon.ico，并放在个人主页的根目录

![image-20230523144223719](/images/ConfigLog/2023-04-29-使用GithubPages和Jekyll搭建个人主页_images/image-20230523144223719.png)

修改`_layouts`文件夹内的`default.html`文件，向`<head>`节点内加入子节点，具体如下

```html
<head>
  <link rel="shortcut icon" type="image/x-icon" href="favicon.ico?">
<head>
```

---

主要参考

1. [如何在Windows平台上基于github搭建个人博客平台 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/102346113)
2. [使用 Jekyll 设置 GitHub Pages 站点 - GitHub 文档](https://docs.github.com/zh/pages/setting-up-a-github-pages-site-with-jekyll)
3. [How to make a personal website with github pages - YouTube](https://www.youtube.com/watch?v=qZsgPgGdOzQ)



