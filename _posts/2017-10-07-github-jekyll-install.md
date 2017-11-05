---
layout: single
category: "read"
title:  "Github jekyll 简易安装"
tags: [Github,jekyll]
toc: true
toc_label: "My Table of Contents"
toc_icon: "gear"
---
### 前言

发现能在Github上建立个人博客，于是注册了github帐号；准备写博客发现github需要配置jekyll来生成静态网页，并不像dokuwiki这类的知识库软件有现成的编辑，发布。在反反复复地测试后，发现github和jekyll配置似乎并不像很多网页说的那么简单，而且费时；离最初的只是用一个网站来记录一些自己的博客想法越来越远。最后发现不如采用现成的模版和方法来快速建立，减少在配置方面的花销，节省更多的精力在文章上。于是有了如下文章。  


### 主题的选择

只是想记录一些内容，而不想太花哨，也不想费更多的精力在配置上日，最后选择了[Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/)。

Minimal Mistakes 有如下好处
- 兼容Github，这个很重要，很多主题Github是不支持的
- 完整的描述文档，这个很棒，不像其他的很多Theme（好看，但是你不知道如何修改或者花很大的代价来修改）
- 功能很丰富，当然前提你要花精力去配置。如果只是简单（就像我这个），30分钟基本就能完成整体配置。

### 安装

- 首先需要github帐号，且登录
- 复制 [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes/fork)，会提示你要复制到什么帐号，确认后复制
- 建立本地jekyll环境，这个在官方文档有详细说明，参照安装即可（我安装在树莓派，树莓派好东西啊）
- 修改 _config.yml文件内容，改成你想要的设置
- 手动建立 _posts文件夹，在文件夹下建立Markdown格式文件（即博客内容）；yekyll有一定的格式要求，可以参照如下格式
```
---
layout: single # 这篇博客的布局，采用single即可
category: "read" # 分类
title:  "Github jekyll 简易安装" # 这篇博客标题
tags: [Github,jekyll] # tag
toc: true # 是否有目录，如果不需要就删除
toc_label: "My Table of Contents" # 目录标题 
toc_icon: "gear" # 目录显示图标，这里我选的齿轮
---
下面就是你自己的MarkDown的内容
```

### 最后
官方文档很丰富、完全。有什么问题你完全可以在[官方文档](https://mmistakes.github.io/minimal-mistakes/docs/helpers/)找到答案
