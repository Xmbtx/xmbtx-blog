# Linux 虚拟机启用 Clash TUN 记录

本文记录在 Linux 虚拟机中使用 Clash GUI 客户端时启用 **TUN 模式**的完整过程，以及排查和解决权限问题的方法。

------

# 1. 背景

在虚拟机中使用代理时，最初通过设置环境变量使用 HTTP 代理：

```
export http_proxy=http://<host-ip>:<port>
export https_proxy=http://<host-ip>:<port>
```

这种方式存在几个问题：

- 只对部分程序有效
- 某些 CLI 工具不会自动使用代理
- OAuth 登录等流程可能失败
- 需要手动设置/取消代理

因此决定启用 **Clash TUN 模式（透明代理）**。

TUN 模式开启后：

- 所有网络流量都会自动走代理
- 不需要 `http_proxy`
- 对 CLI 工具更加友好

------

# 2. 确认系统支持 TUN

首先检查 Linux 系统是否支持 TUN 设备：

```
ls /dev/net/tun
```

如果输出：

```
/dev/net/tun
```

说明系统已经支持。

如果不存在：

```
sudo modprobe tun
```

------

# 3. 查找 Clash 内核程序

GUI 程序本身并不是代理核心，真正处理流量的是 **Clash 内核**。

通过进程查找：

```
ps aux | grep -i clash
```

输出类似：

```
/home/<user>/.../cfw
/home/<user>/.../clash-linux
```

其中：

```
clash-linux
```

就是实际的代理核心。

示例路径：

```
/home/<user>/path/to/clash/resources/.../clash-linux
```

------

# 4. 给 Clash 内核添加 TUN 权限

Clash 创建 TUN 设备需要 Linux capability。

执行：

```
sudo setcap cap_net_admin,cap_net_bind_service+ep <clash-core-path>
```

示例：

```
sudo setcap cap_net_admin,cap_net_bind_service+ep /path/to/clash-linux
```

------

# 5. 验证权限

运行：

```
getcap <clash-core-path>
```

如果成功会看到：

```
clash-linux cap_net_admin,cap_net_bind_service+ep
```

说明权限已正确添加。

------

# 6. 重启 Clash

关闭 GUI 再重新启动 Clash。

然后在设置中启用：

```
TUN Mode
Auto Route
```

------

# 7. 取消旧代理环境变量

启用 TUN 后不再需要环境变量代理：

```
unset http_proxy https_proxy HTTP_PROXY HTTPS_PROXY ALL_PROXY
```

------

# 8. 测试代理是否生效

测试公网 IP：

```
curl ipinfo.io
```

如果返回：

```
country: <proxy-country>
```

说明 TUN 已成功接管网络。

------

# 9. 效果

开启 TUN 后：

以下程序都会自动使用代理：

- curl
- apt
- git
- pip
- docker
- CLI AI 工具

无需额外配置代理环境变量。

------

# 10. 常见问题

## clash not found

说明 Clash 没有加入 PATH，需要查找真实路径：

```
ps aux | grep clash
```

------

## setcap 报错

常见原因：

- 使用了符号链接
- 没有对真正的可执行文件操作

需要对 **真实二进制文件**执行 `setcap`。

------

# 11. 总结

启用 Clash TUN 的关键步骤：

1. 确认 `/dev/net/tun`
2. 找到 `clash-linux`
3. 使用 `setcap` 添加 capability
4. 开启 TUN + Auto Route
5. 删除 `http_proxy`

最终实现 **透明代理**。

# 2026.3.7更新

虚拟机挂起后再启动，TUN 模式会自动失效。

需要重新开关 TUN 模式才能恢复。