---
title: neovim折腾
date: 2024/09/09
desc: 🎉neovim
tags: ['#全部','#neovim']
cover: https://cdn.jsdelivr.net/gh/RookieOHY/wallpaper/blogneovim%20%E6%8A%98%E8%85%BE-RookieOHY.png
---

[[toc]]

<p align="center">
<img alt="封面" src="https://cdn.jsdelivr.net/gh/RookieOHY/wallpaper/blogneovim%20%E6%8A%98%E8%85%BE-RookieOHY.png" width=800 />
</p>

## 预备知识

### 安装

在 [neovim](https://github.com/neovim/neovim/releases) 直接下载 `nvim-win64.msi`，直接安装（无需手动配置环境变量）。

### 配置

不同系统的配置文件目录：

- Unix：~/.config/nvim/init.vim (or init.lua)
- Windows：C:\Users\20413\AppData\Local\nvim\init.vim (or init.lua)

  > 推荐使用init.lua来配置neovim

使用 [oh-my-nvim](https://github.com/hardhackerlabs/oh-my-nvim) 来配置neovim。

1. 在配置文件目录即 `nvim\` 下，克隆仓库。

```shell
git clone https://github.com/hardhackerlabs/oh-my-nvim.git C:\Users\20413\AppData\Local\nvim\init.lua
```

2. 移除 `init.vim` 文件（和 `init.lua` 会冲突，保留其中一个即可）

3. 打开终端模拟器 `wezterm`, 输入 `nvim`, 会启动和自动安装配置的neovim插件，等待下载和安装结束。下载完毕后，可以在目录：C:\Users\20413\AppData\Local\nvim-data 查看安装的插件

4. 最终效果

![](https://cdn.jsdelivr.net/gh/RookieOHY/wallpaper/blogSnipaste_2024-09-09_22-31-17.png)
![](https://cdn.jsdelivr.net/gh/RookieOHY/wallpaper/blogSnipaste_2024-09-09_22-30-08.png)
![](https://cdn.jsdelivr.net/gh/RookieOHY/wallpaper/blogSnipaste_2024-09-09_22-30-41.png)

5. 可能遇到的问题：因网络代理问题无法无法下载 `lazy.vim`，导致报错。解决方式：解决网络问题，重新打开 nvim 下载 `lazy.vim`。

```shell
E5113: Error while calling lua chunk: C:\Users\20413\AppData\Local\nvim\init.lua:21: module 'lazy' not found:
        no field package.preload['lazy']
        no file '.\lazy.lua'
        no file 'D:\neovim\bin\lua\lazy.lua'
        no file 'D:\neovim\bin\lua\lazy\init.lua'
        no file '.\lazy.dll'
        no file 'D:\neovim\bin\lazy.dll'
        no file 'D:\neovim\bin\loadall.dll'
```

### 命令

- 查看版本信息 nvim -v 或者 nvim --version
- 打开/创建文件 nvim filename.txt
- 编辑模式 i
- 返回正常模式 esc
- 保存文件 :w
- 退出 :q

### 关联文章
