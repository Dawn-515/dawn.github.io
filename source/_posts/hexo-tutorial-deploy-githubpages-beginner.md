---
title: 在 GitHub Pages 上部署 Hexo
date: 2024-06-24 02:21:50
tags:

---

# 在 GitHub Pages 上部署 Hexo

## 1 前言

#### 1.1 github page

[GitHub Pages](https://docs.github.com/zh/pages/getting-started-with-github-pages)  是由 GitHub 官方提供的一种免费的静态站点托管服务，让我们可以在 GitHub 仓库里托管和发布自己的静态网站页面。

#### 1.2 hexo

[Hexo](https://hexo.io/zh-cn/) 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他标记语言）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

#### 1.3 总结

使用 GitHub Pages 来搭建 Hexo 静态博客网站，其最吸引人的莫过于完全免费使用，并且非常稳定（网络问题除外）。本文就将详细介绍 在windows 11系统下使用 Hexo + GitHub 搭建免费个人博客网站的过程。

## 2 创建github仓库

打开 [github](https://github.com/) 页面，点击右上角 + 号，再点击 New repository，在Repository name 中输入 用户名.github.io，最后点击 Create repository 即可。

## 3 本地部署hexo

本步骤主要参考 [官方文档](https://hexo.io/zh-cn/docs/setup)。

#### 3.1 首先使用npm安装 hexo-cli

```powershell
npm install -g hexo-cli
hexo -version
```

#### 3.2 然后进入代码目录初始化项目

```powershell
cd E:\workplace\github
hexo init hexo-blog
cd hexo-blog
npm install
```

其中，`hexo init [folder]` 包括两个步骤:  
  1 git clone [hexo-starter](https://github.com/hexojs/hexo-starter);  
  2 将 [hexo-theme-landscape](https://github.com/hexojs/hexo-theme-landscape) 主题移动到指定目录。  
至此，最基础的本地部署已经完成，可以在本地进行预览：

```powershell
hexo g   # 生成静态文件
hexo s   # 启动本地服务器，打开网页即可预览
```

#### 3.3 设置个性化主题

##### 3.3.1 [solitude](https://solitude.js.org/)

首先下载主题文件到 themes 文件夹内，并安装依赖；

```powershell
cd E:\workplace\github\hexo-blog
git clone -b main https://github.com/everfu/hexo-theme-solitude.git themes/solitude
cd themes/solitude
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

然后修改 hexo 根目录中的`_config.yml`，把主题改为 `solitude`，将语言修改为中文。

```yml
theme: solitude # 指定主题
language: zh-CN # 指定语言
```

将 solitude 中的配置文件 `_config.yml` 复制到 hexo 根目录，并改名为 `_config.solitude.yml`

```powershell
cd E:\workplace\github\hexo-blog
copy themes\solitude\_config.yml _config.solitude.yml
```

**注意**  
三个配置文件的优先级 `_config.yml`中的theme config  > `_config.my-theme.yml` > `themes/my-theme/_config.yml`
hexo 根目录中 `_config.solitude.yml` 的配置是高优先级，因此，渲染时会优先采用此文件的配置项内容。  
在更新主题时可能会存在配置变更，请注意更新说明，可能需要手动对 `_config.solitude.yml` 同步修改。  
想查看覆盖配置有没有生效，可以通过 `hexo g --debug` 查看命令行输出。  

最后重新启动

```
hexo s 
```

##### 3.3.2 [fluid](https://hexo.fluid-dev.com/)

首先下载主题文件到 themes 文件夹内,

```powershell
cd E:\workplace\github\hexo-blog
git clone -b master git@github.com:fluid-dev/hexo-theme-fluid.git themes/fluid
```

然后修改 hexo 根目录中的`_config.yml`，把主题改为 `fluid`，将语言修改为中文。

```yml
theme: fluid     # 指定主题
language: zh-CN  # 指定语言
```

```powershell
cd E:\workplace\github\hexo-blog
copy themes\fluid\_config.yml _config.fluid.yml
```

#### 4 上传到GitHub

首先安装依赖，

```powershell
npm install hexo-deployer-git --save
```

然后修改 `_config.yml` 

```yml
deploy:
  type: git
  repo: git@github.com:Dawn-515/Dawn-515.github.io.git
  branch: main
```

可以同时使用多个 deployer，Hexo 会依照顺序执行每个 deployer。

```yml
deploy:
- type: git
  repo: git@github.com:Dawn-515/Dawn-515.github.io.git
  branch: main
- type: git
  repo: git@gitee.com:mulin_cx/mulin_cx.gitee.io.git
  branch: master
```

最后执行推送 `hexo g && hexo c && hexo d`。

```powershell
hexo c # 清除缓存文件 (db.json) 和已生成的静态文件 (public)。
hexo d # 部署网站
```

当执行 `hexo deploy` 时，Hexo 会将 public 目录中的文件和目录推送至 _config.yml 中指定的远端仓库和分支中，并且完全覆盖该分支下的已有内容。  

浏览个人网站 [用户名.github.io](https://dawn-515.github.io/)。


