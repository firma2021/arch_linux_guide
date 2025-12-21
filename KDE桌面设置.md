## limine bootloader主题

编辑/boot/limine.conf,
添加

```
# Limine theme
# Terminal colors (Tokyo Night palette)
# Terminal colors (Tokyo Night Day palette)
term_palette: b4b5b9;f52a65;587539;8c6c3e;2e7de9;9854f1;007197;6172b0
term_palette_bright: a1a6c5;ff4774;5c8524;a27629;358aff;a463ff;007ea8;3760bf
# Text colors
term_foreground: 3760bf
term_background: e1e2e7
term_foreground_bright: 3760bf
term_background_bright: d0d5e3
```
删除
wallpaper: boot():/limine-splash.png

## KDE系统设置

> [!IMPORTANT]
>
> KDE以其高度的可定制性、现代化的界面和丰富的功能而著称，缺点是配置繁琐复杂。

### 快速设置

- 动效速度：中等


### 键盘

- 键盘
  - NumLock在开机时的状态：打开
  - 按键重复：延迟250毫秒，频率75次重复/秒
  - 布局：关闭
- 虚拟键盘
  - 选择Fcitx 5


### 声音

右上角配置音量控制，音量调整步长：1%

### 显示和监视器
- 屏幕边缘：点击显示器示意图左上角的正方形，右键设置为无操作，替代原来的桌面概览；可以使用快捷键 meta + w打开桌面概览

### 无障碍辅助

- 抖动后放大光标：按需开启

### Wifi和互联网

- 防火墙：可关闭，按需开启

### 壁纸

- 按需删除/usr/share/wallpapers/ 下的多余壁纸：通过`pacman -Qo 壁纸绝对路径`，获取壁纸所属的包，然后执行`sudo pacman -Rsn 包名`删除
- 安装插件：`paru plasma6-wallpapers-blurredwallpaper`，将壁纸类型改为 Active blur

### 颜色和主题

#### 全局主题

安装：`paru catppuccin-plasma-colorscheme-latte`

将全局主题设置为`Catppuccin Latte Mauve`

#### 颜色

选择`Catppuccin Latte Mauve`

上方自定义强调色：按需选择

#### 应用程序外观样式

安装：`paru kvantum-theme-catppuccin-git`

打开Kvantum Manager，

1. 变更/删除主题：选择`catppuccin-latte-mauve`，然后应用此主题
2. 配置当前主题：在原有的基础上，
    1. 技巧：勾选透明化Dolphin视图、强制尺寸伸缩句柄、单一顶部工具栏；取消勾选无图标按钮、无选中着色；未启用的图标的不透明度设置为70%
    2. 合成 & 一般外观：勾选半透明窗口、模糊化半透明窗体；降低窗口的不透明度，设置为20%
    4. 杂项：取消勾选并排数值调节钮，勾选非可勾选的组合菜单

最后，在应用程序外观样式中选择kvantum

#### 窗口装饰元素

- 选择`Catppuccin Latte Modern`
- 把鼠标移动到当前选择的主题，点击右下角的编辑主题，将按钮大小设置为小
- 右上角-配置标题栏按钮：按需选择，拖拽编辑

#### 图标

- 安装主题：`paru papirus-icon-theme`

- 选择Papirus

#### 光标

- 安装主题：`paru bibata-cursor-theme-bin`

- 选择Bibata Modern Ice
- 上方的光标大小设置为 28

#### 欢迎屏幕

设置为`Catppuccin Latte Mauve`

#### 登录屏幕（SDDM）
> [!TIP]
>
> sddm主题保存在`/usr/share/sddm/themes`下，
> 通过`pacman -Qo /usr/share/sddm/themes/xxx`命令，查看`xxx`主题属于哪一个包；通过`pacman -Qs xxx`，查看`xxx`主题的包名。
> 如果不需要某个主题，可以卸载对应的包。
>
> 注意，不要卸载当前正在使用的主题，否则重启后会进不到桌面。
> 解决办法是，使用安装U盘，进入维护系统，编辑`/etc/sddm.conf`，添加
>
> ```
> [Theme]
> Current=breeze
> ```

右上角，点击应用Plasma设置

#### 启动屏幕 （plymouth）

- 安装主题：`paru plymouth-theme-catppuccin-latte-git`。

- 在颜色和主题-启动屏幕中，选择`catppuccin-latte`


> [!TIP]
>
> plymouth主题保存在在`/usr/share/plymouth/themes/`下。
>
> 如果想显示启动过程中的日志，不显示启动动画，则选择Details主题。

### 文字和字体

字体：大小设置为14pt。

字体管理：删除不需要的字体。运行` fc-cache -fv `更新字体缓存。

### 动效

- 窗口打开/关闭：淡入淡出动画
- 窗口最小化：神灯

### 默认应用程序

- PDF查看器：Okular
- 终端模拟器：Kitty

### 通知

显示捐款请求-取消勾选“显示弹窗消息”

- 右上角可以设置系统通知和应用程序通知，按需设置


### 窗口管理

- 任务切换器：将主窗口的大图标改为3D封面切换
- 桌面特效：
    - 安装`paru kwin-effect-rounded-corners-git`，进行如下设置：
      - 圆角：
        - 活动窗口圆角半径20,非活动窗口圆角半径20
      - 轮廓：
        - 主轮廓活动窗口轮廓粗细4.50,使用装饰颜色高亮；
        - 主轮廓非活动窗口轮廓粗细3.50,使用装饰颜色高亮；
        - 次轮廓活动窗口轮廓粗细0.25,使用自定义颜色白色；
        - 次轮廓非活动窗口轮廓粗细0.25,使用自定义颜色白色。
    - 勾选窗口惯性晃动，晃动程度设置为一格
    - 勾选窗口透明度
- KWin脚本：勾选桌面变化突出显示

### 常规行为

拖动文件或文件夹时：改为“如果在同一设备，则移动”

### 搜索

文件搜索：启用文件索引，要索引的数据：仅文件名

### 用户反馈

完全禁用

### 电源管理

设置自动睡眠、自动降低亮度、关闭屏幕的时间

### 用户

设置用户头像

## 桌面设置

鼠标移动到桌面空白处，点击右键，

- 图标-排列方式：按行
- 桌面和壁纸-鼠标操作：中键改为程序启动器；（可选）添加操作-垂直滚动滚轮-切换桌面

### 任务栏设置

> [!NOTE]
>
> KDE中称任务栏为面板。

把鼠标放到面板的空白处，点击右键，点击显示面板配置，在右侧的面板设置中，进行如下设置：
- 不透明度：半透明
- 显示/隐藏：避开窗口

点击右下角向上的箭头（即“显示隐藏的图标”），弹出状态和通知栏，点击右上角的配置系统托盘，

- 常规：将面板图标大小改为：按面板高度缩放
- 项目：按需设置

把鼠标放到数字时钟上，点击右键，点击配置数字时钟，

- 显示日期：总是在时间旁边；
- 显示秒数：总是显示；
- 日期格式：长日期; 
- 日历-其它、其它历法系统：中国农历；
- 为数字时钟两侧添加“面板间隙”小组件，使其居中。


把鼠标放到面板的空白处，点击右键，点击显示面板配置，

- 删除左面“虚拟桌面切换器”的小部件
- 删除右边“暂时显示桌面”的小部件  

安装：`paru plasma6-applets-panel-colorizer` ，把这个小部件添加到任务栏，在预设中，选择Sleek，加载并应用

## 软件

### 火狐浏览器
隐私与安全-自动播放-所有网站的默认值：允许音频和视频
安装主题：Catppuccin Latte - Mauve

在地址栏输入about:config，搜索**`widget.non-native-theme.scrollbar.style`**，将值修改为4,效果是使用更宽的滚动条

### spectacle截图

选项-配置-截图之后：

勾选：保存文件到默认文件夹

选择：复制截图到剪贴板

### KRunner

屏幕上的位置：中间
取消勾选：在左面按下任意按键时激活

### Dolphin

> [!TIP]
>
> 按下 Ctrl + H 或 Alt + . 可以快速切换隐藏文件的显示状态

安装 dolphin-plugins，提供版本控制和云存储/磁盘镜像管理功能。

界面：

- 在文件夹与标签页下，勾选 标题栏显示完整目录；勾选 显示过滤栏。
- 在状态与位置栏下，勾选 位置栏默认在可编辑状态；勾选 位置栏显示完整路径。

视图：在常规下，勾选 拖放操作时打开文件夹。

右键菜单：勾选 删除

### KRunner

点击KRunner左上角的齿轮图标，

屏幕上的位置：改为中间

取消勾选：在桌面按下任意按键时激活

### 推荐安装

- mission-center：win11风格的任务管理器

- kcolorchooser：颜色选择工具

- ksystemlog：系统日志查看器

> [!NOTE]
>
> journald 是 systemd 系统套件中的日志守护进程（systemd-journald），负责统一收集和存储系统日志。它取代了传统的文本日志文件，将日志以二进制格式集中管理，日志来源包括内核消息、系统服务输出、用户程序消息等。
>
> journalctl 是用于查询和显示 journald 收集的日志的命令行工具。通过 journalctl，用户可以查看、过滤和分析系统日志。

## 快捷键

在系统设置-键盘-快捷键中进行设置。

- 终端：meta + z
- 浏览器：meta + b
- vscode：meta + c
- mission-center：meta + esc
- KRunner：alt + 空格

> [!TIP]
>
> meta键和super键，就是win键

> [!TIP]
>
> 一些你可能不知道的操作：
>
> 鼠标左键点击标题栏的放大按钮，可以最大化窗口，而中键点击是垂直最大化，水平点击是水平最大化。（在系统设置-窗口管理-窗口行为-标题栏操作中，进行配置）
>
>
> 按住meta键，左键点击窗口的任意位置，可以拖动窗口，不需要点击窗口的边框进行拖动；按住meta键和右键，可以调整窗口的水平宽度（在系统设置-窗口管理-窗口行为-窗口操作中，进行配置）

## 其它

> [!TIP]
>
> paru -Syu 的含义和 pacman -Syu 类似,同步仓库数据库,升级所有官方仓库包,再升级 AUR 包

> [!TIP]
>
> `paru pkg` 这种不带任何操作选项、只跟一个搜索词的命令，其行为是**交互式搜索并让用户选择安装**。这是 `paru` 作为一个 AUR 助手提供的便捷功能。
>
> `pacman` 命令本身并不直接提供这种交互式搜索安装功能。

> [!TIP]
>
> 在 Arch Linux 中，/bin 目录是 /usr/bin 的符号链接。这是“usr merge”运动的一部分，旨在通过将可执行二进制文件整合到一个位置来简化文件系统层次结构。因此，在 Arch 上，/bin、/sbin，有时还有 /usr/sbin 都链接到 /usr/bin。
