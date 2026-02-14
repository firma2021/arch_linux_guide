## git配置

```shell
git config --list # 查看当前的git配置
git config --global user.name USER_NAME
git config --global user.email USER_EMAIL
git config --global init.defaultBranch main
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%ci %cr) %C(bold blue)<%an>%Creset'"
git config --global alias.br "branch --format='%(HEAD) %(color:yellow)%(refname:short)%(color:reset) - %(contents:subject) %(color:green)(%(committerdate:relative)) [%(authorname)]' --sort=-committerdate"
```

## 包管理器配置

如果发行版不是`CachyOS`，编辑`/etc/pacman.conf `，在 `[options]`下, 添加`Color`、`ILoveCandy`、`VerbosePkgLists`、`ParallelDownloads`，注释掉`NoProgressBar`。

编辑`/etc/paru.conf`，添加`BottomUp`。

如果发行版是`manjaro`, 请为`pamac`启用`aur`和`flatpak`。

> [!TIP]
>
> 执行 `pacman -Si make`，输出中 **Repository** 字段会明确显示即将安装的包来自哪个仓库。
>
> 如果你想强制安装某个特定仓库的版本，可以在命令中指定仓库名，例如：`pacman -S cachyos-core-znver4/make`

## 终端选择

kitty。

> [!TIP]
>
> 在一个kitty终端中，可以打开多个标签页，在一个标签页中可以打开多个窗口
>
> 打开/关闭 标签页的快捷键为 ctrl + shift + t  /  ctrl + shift + q
>
> 打开/关闭 窗口的快捷键为 ctrl+shift+enter /  ctrl+shift+w

查看并设置kitty主题：`kitty +kitten themes`

查看并设置kitty字体：`kitty +list-fonts`

为什么不选择以下终端？

- KDE自带的Konsole：配置繁琐，不够直观
- Alacritty：不支持在终端内原生显示图片，维护者认为要专注于显示文本
- Ghostty：linux下不支持滚动条；有bug，打开后不能居中显示

## shell选择

fish，因为它的语法简洁优美，配置方便。

> [!TIP]
>
> bash/zsh有.profile, .rc, .logout等配置文件，将登录/非登录、交互/非交互的配置分离。
>
> fish简化配置模型，一个`config.fish`文件管所有。
>
> fish 的原始版本是用 C++ 编写的，现在已经迁移到了 Rust。

> [!TIP]
>
> 设置root用户的shell为fish：`sudo -i；chsh`

> [!NOTE]
>
> 在 Fish Shell 中，按 **Alt+S** 自动在当前命令前添加 `sudo` ，以替代 Bash 等 Shell 中常用的 `sudo !!` 操作。

安装fish插件管理器：`paru fisher`

安装fish主题：

```bash
fisher install catppuccin/fish
fisher install vitallium/tokyonight-fish
```

设置fish主题：

```bash
fish_config theme save "TokyoNight Day"
```

### starship

starship是一个通用、跨 Shell 的命令行提示符生成器，用 Rust 写的，主打快、
跨平台、跨 Shell（bash、zsh、fish、nushell、PowerShell、xonsh 等）。它的配置统一写在 starship.toml，不同 的shell 可以共享同一提示符。

## 输入法配置

- **fcitx5**：输入法框架，统一管理各种输入法、键盘布局。完全离线，不上传任何输入数据。
- **Rime**：输入法引擎，支持拼音、双拼等方案。
- **雾凇拼音**：基于 Rime 的一套**拼音输入方案**（schema + 词库 + 配置），为 Rime 写好的一套“拼音规则 + 词库 + 配置”打包方案。

> [!TIP]
>
> Rime 在不同平台上都有各自的别称：Windows 版叫“小狼毫”，Linux 版叫“中州韵”，而 macOS 版则以“鼠须管”命名。

安装：`sudo pacman -S fcitx5 fcitx5-configtool fcitx5-gtk fcitx5-qt fcitx5-rime rime-ice-git`

在 系统设置-键盘-虚拟键盘 中，选择Fcitx 5。

编辑` ~/.local/share/fcitx5/rime/default.custom.yaml`，添加

````
patch:
    __include: rime_ice_suggestion:/
````

作为雾凇拼音的默认配置，你还可以在`~/.local/share/fcitx5/rime/rime_ice.custom.yaml`中添加自定义配置。

在系统设置-输入法中，

- 点击中州韵的齿轮图标，打开其设置界面，将“预编辑模式”修改为“提交预览”，取消勾选“将嵌入式预编辑文本的光标固定在开头”；将切换输入法时的行为修改为“提交原始字符串”。
- 点击左下角的 配置附加组件，点击经典用户界面右侧的齿轮，设置你喜欢的字体，设置字体大小为14号，将主题和深色主题改为KDE Plasma。

vscode支持：在`~/.config/code-flags.conf`中，添加`--ozone-platform=wayland`。

> [!TIP]
>
> 按左shift键临时切换中英文输入法，
>
> 按上/下键切换上一个/下一个候选词，
>
> 以上快捷键可以在 系统设置-输入法-左下角的配置全局选项 中设置。
>
> 按鼠标滚轮翻页，即切换上一页/下一页候选词。

## 字体选择

> [!NOTE]
>
> 字体需要支持emoji和noto-fonts图标，才不会出现乱码。

> [!TIP]
>
> - Noto Fonts 是 Google和Adobe合作开发的一个开源字体家族，意为 ‍“No Tofu”‍（没有豆腐块），目标是消除因缺少字体而显示的方框（□）乱码。
> - Adobe 将其称为 Source Han Sans（思源黑体）和 Source Han Serif（思源宋体）。
> - Noto Fonts 按语言或字符集被拆分为多个子包，如noto-fonts：基本拉丁字母等；noto-fonts-cjk：简体中文、繁体中文、日文、韩文；noto-fonts-emoji：彩色 Emoji。

安装：

- `sudo pacman -S noto-fonts-cjk noto-fonts-emoji` 
- `paru ttf-lxgw-bright-code-git`

运行 `fc-cache -fv `更新字体缓存。

> [!TIP]
>
> 字体后缀的含义：
>
> - Sans 无衬线
> - Mono 等宽
> - NL 无连字

> [!TIP]
>
> 你可以通过`pacman -Qs font` 命令查找安装的字体，卸载不需要的字体。

## 命令行工具

`bat`和`bat-extras`：cat的现代替代。配置主题：执行`bat --list-themes` 查看可用的主题，编辑`~/.config/bat/config`，添加`--theme="ThemeName"`

 `eza`：ls的现代替代

`fd`：find的现代替代

`procs`：ps的现代替代

`ripgrep`：rg的现代替代

`dust`：du的现代替代

`duf`：df的现代替代

## 开发工具

python：`uv`

c++：

```
gcc
clang
gdb
make
cmake
ninja
libc++
boost
```

shell：`shellcheck shfmt`

代码行数统计：`cloc`

> [!WARNING]
>
> `man-pages-zh_cn`翻译不完整且滞后，不要使用。

## 系统清理

清理两周前的系统日志：`sudo journalctl --vacuum-time=2weeks`

清理包缓存：`sudo pacman -Scc`

删除孤儿包：` sudo pacman -Rns \$(pacman -Qdtq)`

手动清理以下目录的数据:`~/.config/`，`~/.cache/`，`~/.local/share/`

查找并删除无效的符号链接
