# Ubuntu 虚拟机黑屏问题排查记录（VMware + Ubuntu 22.04）

## 一、问题背景

环境信息：

- 虚拟化平台：VMware
- 系统：Ubuntu 22.04
- 桌面环境：GNOME
- 显卡：VMware SVGA II Adapter
- GPU 驱动：vmwgfx
- 最近操作：系统更新（升级到 6.8.0-101-generic 内核）

出现问题：

- 重启后进入系统黑屏
- GRUB 可以进入
- 旧内核可以进入系统
- 再次重启后系统恢复正常（问题具有偶发性）

---

# 二、问题排查过程

## 1 通过 GRUB 进入系统

进入：

Advanced options for Ubuntu

选择旧内核或 Recovery Mode。

确认：

- 系统文件没有损坏
- 可以进入终端

---

## 2 查看 GPU 信息

检查虚拟机显卡：

lspci | grep -i vga

输出：

VMware SVGA II Adapter

说明：

系统使用 VMware 虚拟显卡。

---

## 3 检查加载的显卡驱动

lsmod | grep -E 'nvidia|amdgpu|nouveau|vmwgfx|i915'

结果：

vmwgfx

说明：

Ubuntu 使用 VMware 官方内核驱动：

vmwgfx

---

## 4 查看系统日志

查看上一次启动日志：

journalctl -b -1

筛选 GPU 相关信息：

journalctl -b -1 | grep -Ei "drm|gpu|vmwgfx"

发现关键错误：

vmwgfx warning  
vmwgfx_dri.so  
GL_OUT_OF_MEMORY

以及：

gnome-shell: Running GNOME Shell as a Wayland display server

说明：

系统运行在 Wayland 会话。

---

## 5 检查 GNOME 会话类型

理论检查：

echo $XDG_SESSION_TYPE

但更可靠的是日志：

Running GNOME Shell as a Wayland display server

确认：

当前桌面 = Wayland

---

## 6 对照实验（关键步骤）

在登录界面选择：

Ubuntu on Xorg

重新登录。

结果：

Xorg 登录完全正常

说明：

问题只在 Wayland 会话下出现。

---

# 三、最终问题定位

问题原因：

Wayland + VMware vmwgfx 图形驱动兼容性问题

渲染链路：

Wayland：

GNOME Shell (Wayland compositor)  
↓  
Mesa / OpenGL  
↓  
vmwgfx_dri.so  
↓  
VMware SVGA II  

Xorg：

GNOME Shell  
↓  
Xorg  
↓  
vmwgfx  

Wayland 对虚拟 GPU 的兼容性较差，因此会出现：

- GL_OUT_OF_MEMORY
- vmwgfx warning
- gnome-shell 渲染异常
- 偶发黑屏

---

# 四、解决方案

## 方案一（推荐）：永久使用 Xorg

编辑配置：

sudo nano /etc/gdm3/custom.conf

修改为：

WaylandEnable=false

重启：

sudo reboot

效果：

系统默认使用 Xorg 登录。

优点：

- 更稳定
- VMware 兼容性更好

---

## 方案二：关闭 VMware 3D 加速

VMware 设置：

VM Settings  
Display  
取消勾选 Accelerate 3D Graphics

可能减少：

vmwgfx / OpenGL 渲染问题

但会降低图形性能。

---

## 方案三：更新 VMware Tools

安装官方工具：

sudo apt install open-vm-tools open-vm-tools-desktop

重启系统：

sudo reboot

---

# 五、未来可能出现的问题

## 1 Wayland 黑屏

可能表现：

- 登录后黑屏
- 桌面无法渲染
- 鼠标存在但界面不显示

解决：

改用 Xorg

---

## 2 GNOME Shell 崩溃

日志可能出现：

gnome-shell crashed  
GL_OUT_OF_MEMORY

解决：

关闭 3D 加速  
或  
使用 Xorg

---

## 3 VMware 图形驱动警告

日志可能出现：

vmwgfx warning  
vmw_bo_free

通常属于驱动兼容问题，一般无需处理。

---

## 4 Xwayland 相关错误

日志可能出现：

Xwayland glamor GL error

原因：

Wayland 运行 X11 程序时的兼容层问题

解决：

使用 Xorg

---

# 六、推荐配置（虚拟机）

虚拟机运行 Linux 桌面推荐配置：

桌面协议：Xorg  
3D 加速：关闭或谨慎开启  
驱动：vmwgfx  
VMware Tools：已安装

原因：

Wayland 在虚拟机中稳定性较差

---

# 七、排查 Linux 黑屏问题的通用流程

Step 1

确认系统是否可进入：

GRUB  
Recovery Mode  
旧内核  

---

Step 2

检查 GPU：

lspci

---

Step 3

检查驱动：

lsmod

---

Step 4

查看系统日志：

journalctl -b -1

---

Step 5

确认桌面协议：

Wayland / Xorg

---

Step 6

做对照实验：

Wayland ↔ Xorg

---

# 八、本次问题总结

问题本质：

Wayland 与 VMware vmwgfx 图形栈兼容性问题

解决方法：

改用 Xorg

排查关键点：

journalctl  
GPU 驱动  
桌面协议  
对照实验

最终系统状态：

Ubuntu 正常运行  
Xorg 会话稳定

---

# 九、经验总结

虚拟机环境建议：

优先使用 Xorg

原因：

- Wayland 对虚拟 GPU 支持仍不完善
- Xorg 在虚拟机环境更成熟稳定