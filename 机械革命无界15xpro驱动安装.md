## 安装`aur`驱动包

- 有线网卡驱动：`yt6801-dkms`

- 内核模块：`mechrevo-drivers-dkms`

- 控制台：`tuxedo-control-center-bin`

## 查看和修改电池充电策略

- 在控制台的settings中设置；

- `/sys/devices/platform/tuxedo_keyboard/charging_profile/`文件夹下的      
  - charging_profiles_available文件：保存着可选的充电策略
  - charging_profile文件：保存着当前的充电策略

  修改charging_profile文件，如`echo stationary | sudo tee /sys/devices/platform/tuxedo_keyboard/charging_profile/charging_profile`，可以设置电池充电策略。

> [!NOTE]
>
> “工作站模式”将电池电量限制在80%，降低电池的电压，减少电池的发热，从而延长电池的循环寿命。

## 获取显示器校色文件

在windows下，进入`C:\Program Files\OEM\机械革命控制中心`，搜索`*.icm`，找到校色文件，复制到linux下。

在linux KDE桌面下，将显示器配置-色彩特性文件，设置为你找到的校色文件。



> [!TIP]
>
> 查看声卡型号：`aplay -l`
