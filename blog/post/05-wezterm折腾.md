---
title: wezterm折腾
date: 2024/09/14
desc: 🎉wezterm
tags: ['#全部','#wezterm']
cover: https://cdn.jsdelivr.net/gh/RookieOHY/wallpaper/blogWezterm%20%E6%8A%98%E8%85%BE-RookieOHY.png
---

[[toc]]

<p align="center">
<img alt="封面" src="https://cdn.jsdelivr.net/gh/RookieOHY/wallpaper/blogWezterm%20%E6%8A%98%E8%85%BE-RookieOHY.png" width=800 />
</p>


### wezterm 配置

在 [wezterm](https://wezfurlong.org/wezterm/index.html) ，下载 [windows安装包](https://wezfurlong.org/wezterm/install/windows.html)。

- 安装目录 `D:\WezTerm`

![](https://cdn.jsdelivr.net/gh/RookieOHY/wallpaper/blogSnipaste_2024-09-14_15-46-53.png) 

- 配置 wezterm ,在用户目录之下新建 `.wezterm.lua` ，写入配置（因为是lua脚本的关系，配置写入后会实时渲染），配置：颜色、字体、窗口、默认shell、启动菜单、快捷键、IP显示等。其中字体需要预先下载和安装，下载地址：[nerd-fonts](https://www.nerdfonts.com/)
> lua 是脚本语言，语法参见：[lua 语法](https://www.runoob.com/lua/lua-tutorial.html)
``` lua
-- 加载 wezterm API
local wezterm = require 'wezterm'
-- 获取 config 对象
local config = wezterm.config_builder()

-------------------- 颜色配置 --------------------
-- 设置颜色方案
config.color_scheme = 'tokyonight_moon'
-- 设置窗口装饰
config.window_decorations = "RESIZE"
-- 设置是否使用漂亮的标签栏
config.use_fancy_tab_bar = false
-- 设置是否显示标签栏
config.enable_tab_bar = true
-- 设置是否在标签栏中显示标签索引
config.show_tab_index_in_tab_bar = true
-- 设置是否在只有一个标签时隐藏标签栏
config.hide_tab_bar_if_only_one_tab = false
-- 设置是否在改变字体大小时调整窗口大小
config.adjust_window_size_when_changing_font_size = false
-- 设置非活动窗口的颜色
config.inactive_pane_hsb = {
  saturation = 0.9,
  brightness = 0.8,
}

-- 设置指定字体，需要先安装后再设置
config.font = wezterm.font("CaskaydiaCove Nerd Font")
-- 设置字体大小
config.font_size = 12
-- 设置初始列数
config.initial_cols = 130
-- 设置初始行数
config.initial_rows = 30
-- 设置窗口填充
config.window_padding = {
  left = 20,
  right = 20,
  top = 20,
  bottom = 5,
}
-- 设置默认的启动shell
config.set_environment_variables = {
  -- COMSPEC = 'C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe',
  COMSPEC = 'D:\\nu\\bin\\nu.exe',
}

-------------------- 键盘绑定 --------------------
local act = wezterm.action
-- 设置 leader 键
config.leader = { key = 'a', mods = 'CTRL', timeout_milliseconds = 1000 }
-- 设置键盘绑定
config.keys = {
  -- 退出应用
  { key = 'q',          mods = 'LEADER',         action = act.QuitApplication },
  -- 水平分割
  { key = 'h',          mods = 'LEADER',         action = act.SplitHorizontal { domain = 'CurrentPaneDomain' } },
  -- 垂直分割
  { key = 'v',          mods = 'LEADER',         action = act.SplitVertical { domain = 'CurrentPaneDomain' } },
  -- 关闭当前窗格
  { key = 'q',          mods = 'CTRL',           action = act.CloseCurrentPane { confirm = false } },
  -- 移动到左侧窗格
  { key = 'LeftArrow',  mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Left' },
  -- 移动到右侧窗格
  { key = 'RightArrow', mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Right' },
  -- 移动到上方窗格
  { key = 'UpArrow',    mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Up' },
  -- 移动到下方窗格
  { key = 'DownArrow',  mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Down' },
  -- CTRL + T 创建默认的Tab 
  { key = 't', mods = 'CTRL', action = act.SpawnTab 'DefaultDomain' },
  -- CTRL + W 关闭当前Tab
  { key = 'w', mods = 'CTRL', action = act.CloseCurrentTab { confirm = true } },
  -- CTRL + SHIFT + 1 创建新Tab - WSL
  {
    key = '!',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      domain = 'DefaultDomain',
      args = {'wsl', '-d', 'Ubuntu-20.04'},
    }
  },
}
-- ctrl + 1-8 激活第1-8个tab
for i = 1, 8 do
  table.insert(config.keys, {
    key = tostring(i),
    mods = 'CTRL',
    action = act.ActivateTab(i - 1),
  })
end

-------------------- 鼠标绑定 --------------------
config.mouse_bindings = {
  -- 复制选中的文本
  {
    event = { Up = { streak = 1, button = 'Left' } },
    mods = 'NONE',
    action = act.CompleteSelection 'ClipboardAndPrimarySelection',
  },
  -- 打开超链接
  {
    event = { Up = { streak = 1, button = 'Left' } },
    mods = 'CTRL',
    action = act.OpenLinkAtMouseCursor,
  },
}

-- 设置窗口透明度
config.window_background_opacity = 0.9
-- 设置窗口背景模糊
config.macos_window_background_blur = 10

-- 窗口右上角显示IP
wezterm.on('update-status', function(window)
  -- 设置左箭头
  local SOLID_LEFT_ARROW = utf8.char(0xe0b2)
  -- 获取当前窗口的配置，并从中获取调色板（这是您选择的配色方案，包括任何覆盖）
  local color_scheme = window:effective_config().resolved_palette
  -- 获取背景色
  local bg = color_scheme.background
  -- 获取前景色
  local fg = color_scheme.foreground
  -- 获取公网IP
  local success, stdout, stderr = wezterm.run_child_process { "curl", "-s", "https://realip.cc/simple" }
  local public_ip=stdout
  -- 获取当前时间
  local date = wezterm.strftime("%Y-%m-%d %H:%M:%S")
  -- 休眠6秒
  wezterm.sleep_ms(6000)
  -- 如果获取不到公网IP，则设置为N/A

  if not public_ip then
    public_ip = 'N/A'
  end
  -- 设置到右状态栏
  window:set_right_status(wezterm.format({
    { Background = { Color = 'none' } },
    { Foreground = { Color = bg } },
    { Text = SOLID_LEFT_ARROW },
    { Background = { Color = bg } },
    { Foreground = { Color = fg } },
    { Text = ' ' .. wezterm.hostname() .. ' | IP: ' .. public_ip .. ' | ' .. date ..' ' },
  }))
end)


return config


```


### starship 配置

- [statrship] (https://starship.rs/zh-CN/installing/) 安装。配置地址：https://starship.rs/zh-CN/presets/pastel-powerline#%E5%89%8D%E7%BD%AE%E8%A6%81%E6%B1%82

```shell
starship preset pastel-powerline -o ~/.config/starship.toml
```
- 编辑配置文件：`~/.config/starship.toml`
``` toml
format = """
[](#9A348E)\
$os\
$username\
[](bg:#DA627D fg:#9A348E)\
$directory\
[](fg:#DA627D bg:#FCA17D)\
$git_branch\
$git_status\
[](fg:#FCA17D bg:#86BBD8)\
$c\
$elixir\
$elm\
$golang\
$gradle\
$haskell\
$java\
$julia\
$nodejs\
$nim\
$rust\
$scala\
[](fg:#86BBD8 bg:#06969A)\
$docker_context\
[](fg:#06969A bg:#33658A)\
$time\
[ ](fg:#33658A)\
"""

# Disable the blank line at the start of the prompt
# add_newline = false

# You can also replace your username with a neat symbol like   or disable this
# and use the os module below
[username]
show_always = true
style_user = "bg:#9A348E"
style_root = "bg:#9A348E"
format = '[$user ]($style)'
disabled = false

# An alternative to the username module which displays a symbol that
# represents the current operating system
[os]
style = "bg:#9A348E"
disabled = true # Disabled by default

[directory]
style = "bg:#DA627D"
format = "[ $path ]($style)"
truncation_length = 3
truncation_symbol = "…/"

# Here is how you can shorten some long paths by text replacement
# similar to mapped_locations in Oh My Posh:
[directory.substitutions]
"Documents" = "󰈙 "
"Downloads" = " "
"Music" = " "
"Pictures" = " "
# Keep in mind that the order matters. For example:
# "Important Documents" = " 󰈙 "
# will not be replaced, because "Documents" was already substituted before.
# So either put "Important Documents" before "Documents" or use the substituted version:
# "Important 󰈙 " = " 󰈙 "

[c]
symbol = " "
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[docker_context]
symbol = " "
style = "bg:#06969A"
format = '[ $symbol $context ]($style)'

[elixir]
symbol = " "
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[elm]
symbol = " "
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[git_branch]
symbol = ""
style = "bg:#FCA17D"
format = '[ $symbol $branch ]($style)'

[git_status]
style = "bg:#FCA17D"
format = '[$all_status$ahead_behind ]($style)'

[golang]
symbol = " "
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[gradle]
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[haskell]
symbol = " "
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[java]
symbol = " "
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[julia]
symbol = " "
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[nodejs]
symbol = ""
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[nim]
symbol = "󰆥 "
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[rust]
symbol = ""
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[scala]
symbol = " "
style = "bg:#86BBD8"
format = '[ $symbol ($version) ]($style)'

[time]
disabled = true
time_format = "%R" # Hour:Minute Format
style = "bg:#33658A"
format = '[ ♥ $time ]($style)'

```
### nushell 配置

- [nushell](https://www.nushell.sh/zh-CN/) 下载可执行文件安装。
- 执行 `starship init nu | save -f ~/.cache/starship/init.nu` 初始化 init.nu
- 执行 `$nu.config-path` 获取 nushell 配置文件的路径，追加内容 `use ~/.cache/starship/init.nu`

### 配置结果

![](https://cdn.jsdelivr.net/gh/RookieOHY/wallpaper/blogSnipaste_2024-09-14_17-45-16.png)

### 快捷键 
- wezterm 

- nushell


### 参考
- [在Windows中使用并配置 Wezterm + Nushell + Starship](https://www.bilibili.com/video/BV1h2WEeoEZT/?spm_id_from=333.1296.top_right_bar_window_default_collection.content.click&vd_source=b9396f42fd476ece9fcafd189b49977d)