# OpenClaw 本地部署记录（VMware + Ubuntu 22.04）

## 一、环境信息

| 项目        | 内容                       |
| ----------- | -------------------------- |
| 虚拟化      | VMware                     |
| 系统        | Ubuntu 22.04               |
| Node.js     | v22                        |
| OpenClaw    | 2026.3.2                   |
| 模型        | openai-codex/gpt-5.3-codex |
| 用户        | 凶猫                       |
| Gateway端口 | 18789                      |

------

# 二、安装流程

## 1 更新系统

```
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl git ca-certificates lsof
```

------

## 2 安装 OpenClaw

官方安装脚本：

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

安装脚本会自动：

- 检查 Node.js
- 如果没有 Node.js ≥22 自动安装
- 安装 OpenClaw CLI
- 修复 npm 权限

------

## 3 PATH 修复

安装完成后提示：

```
PATH missing npm global bin dir
```

解决：

```
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

验证：

```
which openclaw
```

------

# 三、Onboarding 配置

## 1 安全提示

选择：

```
Yes
```

表示理解 OpenClaw 仍处于 beta。

------

## 2 Onboarding 模式

选择：

```
QuickStart
```

原因：

- 自动配置 Gateway
- 自动初始化 agent
- 适合第一次部署

------

## 3 模型选择

使用 OpenAI：

```
openai-codex/gpt-5.3-codex
```

最终保持默认：

```
Keep current
```

------

# 四、Skills 配置

初始选择：

| Skill     | 作用       | 是否推荐 |
| --------- | ---------- | -------- |
| clawhub   | skill管理  | 推荐     |
| github    | GitHub集成 | 推荐     |
| summarize | 文本总结   | 推荐     |
| nano-pdf  | PDF处理    | 推荐     |

其他技能暂未启用。

------

# 五、依赖问题

部分技能安装失败：

```
github -> brew not installed
summarize -> brew not installed
nano-pdf -> uv not installed
```

原因：

OpenClaw 很多 skills 依赖 Homebrew 或 uv。

解决方法（可选）：

### 安装 Homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

------

### 安装 uv

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

------

# 六、Gateway 服务

安装后自动创建 systemd 用户服务：

```
~/.config/systemd/user/openclaw-gateway.service
```

查看状态：

```
systemctl --user status openclaw-gateway
```

监听端口：

```
127.0.0.1:18789
```

------

# 七、访问 Dashboard

默认地址：

```
http://127.0.0.1:18789/#token=<token>
```

说明：

- token 为控制台认证
- WebSocket 连接必须带 token

如果缺少 token 会出现：

```
unauthorized: gateway token missing
```

------

# 八、127.0.0.1 与 IP 访问问题

### 可以访问

```
http://127.0.0.1:18789
```

### 无法访问

```
http://虚拟机IP:18789
```

原因：

Gateway 默认绑定：

```
127.0.0.1
```

即：

```
Bind: loopback
```

只允许本机访问。

------

## 如果需要局域网访问

修改绑定地址：

```
openclaw config set gateway.bind 0.0.0.0
systemctl --user restart openclaw-gateway
```

访问：

```
http://虚拟机IP:18789/#token=<token>
```

------

# 九、Doctor 检查

运行：

```
openclaw doctor
```

关键状态：

```
Plugins loaded
Agents active
No critical errors
```

------

# 十、Memory Search 问题

doctor 提示：

```
Memory search enabled but no embedding provider
```

原因：

需要 API key：

- OPENAI_API_KEY
- GEMINI_API_KEY
- VOYAGE_API_KEY
- MISTRAL_API_KEY

如果暂时不用记忆功能，可以关闭：

```
openclaw config set agents.defaults.memorySearch.enabled false
```

------

# 十一、性能优化（VM 推荐）

doctor 推荐设置：

```
mkdir -p /var/tmp/openclaw-compile-cache
```

写入：

```
echo 'export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache' >> ~/.zshrc
echo 'export OPENCLAW_NO_RESPAWN=1' >> ~/.zshrc
source ~/.zshrc
```

作用：

- 减少 Node 重编译
- 降低 VM CPU 消耗

------

# 十二、关键目录

程序：

```
~/.npm-global/lib/node_modules/openclaw
```

CLI：

```
~/.npm-global/bin/openclaw
```

配置与数据：

```
~/.openclaw
```

systemd 服务：

```
~/.config/systemd/user/openclaw-gateway.service
```

------

# 十三、常用命令

状态

```
openclaw status
```

健康检查

```
openclaw doctor
```

检测 gateway

```
openclaw gateway probe
```

查看日志

```
journalctl --user -u openclaw-gateway -n 80
```

重启 gateway

```
systemctl --user restart openclaw-gateway
```

------

# 十四、当前系统状态总结

当前部署状态：

- OpenClaw 安装成功
- Gateway 运行正常
- Dashboard 可访问
- Agent 默认配置完成

未配置项：

- skills 依赖（brew / uv）
- memory embeddings
- web search API
- 外部服务 API

------

# 十五、后续建议

优先配置：

1. Homebrew（方便安装 skill）
2. uv（nano-pdf依赖）
3. Brave Search API（web_search）
4. memory embeddings

建议逐步扩展 skills，而不是一次全部安装。

# 十六、安全启动、关闭与日常使用

## 1 启动 OpenClaw

OpenClaw 在 Linux 上通过 **systemd 用户服务**运行。

查看状态：

```
systemctl --user status openclaw-gateway
```

如果服务没有运行，可以启动：

```
systemctl --user start openclaw-gateway
```

如果需要重启：

```
systemctl --user restart openclaw-gateway
```

查看是否正常监听端口：

```
ss -lntp | grep 18789
```

正常情况下会看到类似：

```
127.0.0.1:18789
```

------

# 2 关闭 OpenClaw

安全关闭 Gateway：

```
systemctl --user stop openclaw-gateway
```

确认关闭：

```
systemctl --user status openclaw-gateway
```

状态应为：

```
inactive (dead)
```

如果只想临时停止而不影响开机自动启动，可以只执行 `stop`。

如果想禁用自动启动：

```
systemctl --user disable openclaw-gateway
```

重新启用：

```
systemctl --user enable openclaw-gateway
```

------

# 3 获取 Dashboard Token

OpenClaw 的 Web UI 使用 **token 进行访问认证**。

Dashboard 地址格式：

```
http://127.0.0.1:18789/#token=<TOKEN>
```

例如：

```
http://127.0.0.1:18789/#token=<TOKEN>
```

Token 可以通过以下方式获得：

### 方法一：使用 status 命令

```
openclaw status
```

输出中通常会显示 Dashboard 地址。

------

### 方法二：查看 Gateway 日志

```
journalctl --user -u openclaw-gateway -n 50
```

日志中通常会包含 dashboard URL。

------

### 方法三：重新生成访问状态

如果 token 丢失，可以重启 gateway：

```
systemctl --user restart openclaw-gateway
```

然后再次查看：

```
openclaw status
```

------

# 4 安全使用建议

## 1）不要公开 token

token 相当于控制权限。

避免：

- 在公开仓库分享
- 在截图中暴露
- 在聊天记录中长期保存

如果 token 泄露：

```
systemctl --user restart openclaw-gateway
```

即可刷新访问状态。

------

## 2）默认只允许本机访问

OpenClaw 默认绑定：

```
127.0.0.1
```

也就是：

```
Bind: loopback
```

优点：

- 只有本机可以访问
- 局域网设备无法连接
- 安全性更高

------

## 3）如果需要远程访问

可以通过 SSH 转发访问：

```
ssh -L 18789:127.0.0.1:18789 用户@服务器
```

然后在浏览器打开：

```
http://localhost:18789/#token=<TOKEN>
```

这种方式比直接开放端口更安全。

------

# 5 日常使用流程

推荐日常操作顺序：

## 启动服务

```
systemctl --user start openclaw-gateway
```

------

## 检查状态

```
openclaw status
```

------

## 打开控制面板

浏览器访问：

```
http://127.0.0.1:18789/#token=<TOKEN>
```

------

## 使用 Agent

在 Dashboard 中：

- 创建新会话
- 输入任务
- 查看 agent 输出

例如：

```
hello
```

或

```
explain openclaw architecture
```

------

## 查看日志（排错）

```
journalctl --user -u openclaw-gateway -n 50
```

实时日志：

```
journalctl --user -u openclaw-gateway -f
```

------

## 停止服务

```
systemctl --user stop openclaw-gateway
```

------

# 6 推荐维护命令

健康检查：

```
openclaw doctor
```

检查 gateway：

```
openclaw gateway probe
```

查看状态：

```
openclaw status
```

------

# 7 备份重要数据

OpenClaw 的主要数据目录：

```
~/.openclaw
```

建议定期备份：

```
tar -czvf openclaw-backup.tar.gz ~/.openclaw
```

------

# 十七、快速日常操作清单

启动：

```
systemctl --user start openclaw-gateway
```

查看状态：

```
openclaw status
```

打开控制台：

```
http://127.0.0.1:18789/#token=<TOKEN>
```

查看日志：

```
journalctl --user -u openclaw-gateway -f
```

关闭：

```
systemctl --user stop openclaw-gateway
```

# 十八、OpenClaw 故障排查速查表

本章节总结常见部署问题及解决方法。

适用于：

- VMware / Docker / 服务器部署
- Ubuntu / Linux 环境
- 本地 Gateway 运行模式

------

# 1 Dashboard 打不开

## 现象

浏览器访问：

```
http://127.0.0.1:18789
```

出现：

```
无法连接
connection refused
```

------

## 检查 Gateway 服务

```
systemctl --user status openclaw-gateway
```

如果没有运行：

```
systemctl --user start openclaw-gateway
```

------

## 检查端口监听

```
ss -lntp | grep 18789
```

正常应看到：

```
127.0.0.1:18789
```

------

# 2 Gateway unreachable

## 现象

运行：

```
openclaw status
```

显示：

```
Gateway unreachable (timeout)
```

------

## 检查 Gateway

运行：

```
openclaw gateway probe
```

正常输出：

```
Reachable: yes
Connect: ok
RPC: ok
```

------

## 查看日志

```
journalctl --user -u openclaw-gateway -n 50
```

------

# 3 unauthorized / token missing

## 现象

日志出现：

```
unauthorized
gateway token missing
```

------

## 原因

访问 Dashboard 时 **没有带 token**。

------

## 正确访问方式

```
http://127.0.0.1:18789/#token=<TOKEN>
```

例如：

```
http://127.0.0.1:18789/#token=<TOKEN>
```

------

## 获取 token

```
openclaw status
```

或查看日志：

```
journalctl --user -u openclaw-gateway
```

------

# 4 虚拟机 IP 无法访问

## 现象

访问：

```
http://虚拟机IP:18789
```

出现：

```
connection refused
```

但：

```
http://127.0.0.1:18789
```

可以访问。

------

## 原因

Gateway 默认绑定：

```
127.0.0.1
```

即：

```
Bind: loopback
```

只允许本机访问。

------

## 解决方法

修改绑定地址：

```
openclaw config set gateway.bind 0.0.0.0
systemctl --user restart openclaw-gateway
```

然后访问：

```
http://虚拟机IP:18789/#token=<TOKEN>
```

------

# 5 Skills 安装失败

## 现象

安装 skill 时出现：

```
brew not installed
uv not installed
```

------

## 原因

OpenClaw skills 依赖第三方工具。

例如：

| Skill     | 依赖   |
| --------- | ------ |
| github    | gh CLI |
| summarize | brew   |
| nano-pdf  | uv     |

------

## 安装 Homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

------

## 安装 uv

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

------

# 6 Memory search unavailable

## 现象

`openclaw doctor` 显示：

```
Memory search enabled but no embedding provider
```

------

## 原因

未配置 embedding provider API key。

支持：

- OpenAI
- Gemini
- Voyage
- Mistral

------

## 解决方法

### 方法一：设置 API key

例如：

```
export OPENAI_API_KEY="your_key"
```

------

### 方法二：关闭 memory search

```
openclaw config set agents.defaults.memorySearch.enabled false
```

------

# 7 OpenClaw command not found

## 现象

运行：

```
openclaw
```

出现：

```
command not found
```

------

## 原因

npm global bin 未加入 PATH。

------

## 解决方法

```
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

------

# 8 Node.js 版本不正确

## 现象

安装 OpenClaw 时出现：

```
Node.js version too old
```

------

## 要求

```
Node >= 22
```

------

## 检查版本

```
node -v
```

------

# 9 Gateway 日志查看

排查问题最重要的命令：

```
journalctl --user -u openclaw-gateway -f
```

实时日志输出。

------

# 10 重置 OpenClaw

如果配置混乱，可以清空配置：

⚠ 会删除所有 agent 数据

```
rm -rf ~/.openclaw
```

然后重新初始化：

```
openclaw onboard
```

------

# 十九、最常用排查命令

```
openclaw status
```

查看整体状态。

------

```
openclaw doctor
```

系统健康检查。

------

```
openclaw gateway probe
```

检测 Gateway 是否正常。

------

```
journalctl --user -u openclaw-gateway -f
```

实时查看日志。

------

```
systemctl --user restart openclaw-gateway
```

重启 Gateway。

------

# 二十、部署完成检查清单

确认以下项目正常：

- Gateway service running
- Dashboard 可以访问
- Agent 可以创建 session
- 模型可以正常响应
- `openclaw doctor` 无 critical 错误

# 二十一、OpenClaw 架构说明（简化版）

OpenClaw 是一个 **本地 AI Agent 平台**，主要由以下几个核心组件组成：

```
浏览器 Dashboard
        │
        │ HTTP / WebSocket
        │
OpenClaw Gateway
        │
        │ Agent Runtime
        │
Agent (LLM)
        │
 ┌───────────────┬───────────────┬───────────────┐
 │               │               │               │
Skills         Memory         Plugins        Tools
 │               │               │               │
CLI / API      Embeddings     Extensions     System actions
```

------

# 1 Gateway（核心服务）

Gateway 是 OpenClaw 的 **核心服务器**。

主要职责：

- 接收 Dashboard 请求
- 管理 Agent
- 管理 session
- 调用模型 API
- 调用 skills
- 维护 memory

运行方式：

```
systemd user service
```

服务名称：

```
openclaw-gateway
```

默认端口：

```
18789
```

查看服务：

```
systemctl --user status openclaw-gateway
```

日志：

```
journalctl --user -u openclaw-gateway
```

------

# 2 Dashboard（Web 控制台）

Dashboard 是 OpenClaw 的 **管理界面**。

作用：

- 创建 Agent session
- 发送任务
- 查看输出
- 调试 tools
- 管理 skills
- 查看 logs

访问方式：

```
http://127.0.0.1:18789/#token=<TOKEN>
```

通信方式：

```
WebSocket
```

浏览器 → Gateway

------

# 3 Agent（AI 执行体）

Agent 是 **真正执行任务的 AI 实体**。

每个 Agent 包含：

```
Model
Prompt
Memory
Tools
Skills
```

默认 agent：

```
main
```

存储目录：

```
~/.openclaw/agents/main
```

结构示例：

```
~/.openclaw/
 ├─ agents
 │   └─ main
 │       ├─ sessions
 │       ├─ agent
 │       └─ memory
```

------

# 4 Sessions（会话）

每次对话都会创建一个 session。

session 用于：

- 保存上下文
- 保存对话历史
- 保存工具调用

文件：

```
sessions.json
```

位置：

```
~/.openclaw/agents/main/sessions
```

------

# 5 Models（模型）

OpenClaw 支持多个模型 provider。

例如：

| Provider  | 示例    |
| --------- | ------- |
| OpenAI    | GPT     |
| Anthropic | Claude  |
| Google    | Gemini  |
| Mistral   | Mistral |

本次配置使用：

```
openai-codex/gpt-5.3-codex
```

调用方式：

```
OAuth
```

或：

```
API KEY
```

------

# 6 Skills（技能系统）

Skills 是 **Agent 的扩展能力**。

例如：

| Skill     | 功能        |
| --------- | ----------- |
| github    | 操作 GitHub |
| nano-pdf  | PDF 解析    |
| summarize | 文档摘要    |
| clawhub   | skill 管理  |

skills 本质是：

```
CLI 工具 + wrapper
```

例如：

```
gh CLI
```

------

# 7 Plugins（插件）

Plugins 是 OpenClaw 内部模块。

例如：

```
memory-core
web_search
canvas
```

查看：

```
openclaw doctor
```

输出示例：

```
Plugins
Loaded: 4
Disabled: 34
```

------

# 8 Memory（长期记忆）

Memory 用于：

- 存储历史信息
- 支持 semantic search

需要 embedding provider。

例如：

```
OpenAI embeddings
Voyage embeddings
Gemini embeddings
```

如果没有配置 API key：

```
Memory unavailable
```

可以关闭：

```
openclaw config set agents.defaults.memorySearch.enabled false
```

------

# 9 Tools（工具）

Tools 是 Agent 可以调用的系统操作。

例如：

| Tool       | 功能       |
| ---------- | ---------- |
| web_search | 搜索互联网 |
| canvas     | UI 操作    |
| filesystem | 读写文件   |
| exec       | 执行命令   |

安全机制：

```
allowCommands
denyCommands
```

用于限制 Agent 权限。

------

# 10 Canvas（UI 渲染系统）

Canvas 用于：

- UI 输出
- 图形界面
- 可视化结果

日志示例：

```
canvas host mounted
```

路径：

```
~/.openclaw/canvas
```

------

# 11 Hooks（自动化）

Hooks 用于自动触发操作。

例如：

```
/new
/reset
```

触发：

```
session-memory
command-logger
```

------

# 12 配置文件

主配置：

```
~/.openclaw/openclaw.json
```

包含：

```
gateway
agents
skills
tools
security
```

修改配置：

```
openclaw config set
```

或：

```
openclaw configure
```

------

# 13 安全模型

OpenClaw 默认是：

```
single operator trust model
```

也就是：

```
单用户可信环境
```

如果要开放给多人：

需要：

```
access control
sandbox
tool restrictions
```

------

# 14 数据目录

最重要的数据目录：

```
~/.openclaw
```

包含：

```
agents
sessions
canvas
config
logs
```

备份方法：

```
tar -czvf openclaw-backup.tar.gz ~/.openclaw
```

------

# 15 完整系统流程

OpenClaw 执行任务流程：

```
用户输入
   │
Dashboard
   │
WebSocket
   │
Gateway
   │
Agent
   │
LLM 推理
   │
Tools / Skills
   │
返回结果
   │
Dashboard 显示
```

------

# 二十二、总结

完整系统包含：

| 组件      | 作用      |
| --------- | --------- |
| Gateway   | 核心服务  |
| Dashboard | Web UI    |
| Agent     | AI 执行体 |
| Skills    | 扩展能力  |
| Memory    | 长期记忆  |
| Tools     | 系统操作  |

运行方式：

```
systemd user service
```

访问方式：

```
浏览器 Dashboard
```

数据存储：

```
~/.openclaw
```

# 二十三、OpenClaw 迁移与打包

当需要：

- 更换电脑
- 迁移虚拟机
- 打包带走环境
- 服务器迁移

只需要备份 **两个核心部分**：

```
OpenClaw 数据
Node 环境
```

------

# 1 OpenClaw 数据目录

最重要的数据目录：

```
~/.openclaw
```

里面包含：

```
agents
sessions
memory
canvas
config
logs
```

结构示例：

```
~/.openclaw
 ├─ agents
 │   └─ main
 │       ├─ sessions
 │       ├─ memory
 │       └─ agent
 │
 ├─ canvas
 ├─ logs
 └─ openclaw.json
```

------

# 2 打包 OpenClaw 数据

在 Linux 运行：

```
tar -czvf openclaw-backup.tar.gz ~/.openclaw
```

生成：

```
openclaw-backup.tar.gz
```

这个文件包含：

- agent
- session
- memory
- 配置

------

# 3 恢复 OpenClaw 数据

在新机器：

解压：

```
tar -xzvf openclaw-backup.tar.gz -C ~/
```

恢复目录：

```
~/.openclaw
```

然后重启 gateway：

```
systemctl --user restart openclaw-gateway
```

------

# 4 迁移 Node.js 环境

OpenClaw 是通过 npm 安装的。

查看安装：

```
which openclaw
```

通常在：

```
~/.npm-global/bin/openclaw
```

重新安装即可：

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

恢复 `.openclaw` 数据即可继续使用。

------

# 5 VMware 直接迁移（推荐）

如果使用 VMware：

最简单的方法是 **直接复制虚拟机**。

复制文件：

```
vmname.vmdk
vmname.vmx
```

新电脑导入即可。

优点：

- Node 环境完整
- OpenClaw 已配置
- 模型认证保留
- 无需重新部署

------

# 6 OpenClaw 完整迁移步骤

推荐流程：

### 1 备份数据

```
tar -czvf openclaw-backup.tar.gz ~/.openclaw
```

------

### 2 新机器安装 OpenClaw

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

------

### 3 恢复数据

```
tar -xzvf openclaw-backup.tar.gz -C ~/
```

------

### 4 启动服务

```
systemctl --user start openclaw-gateway
```

------

# 7 验证迁移成功

运行：

```
openclaw status
```

检查：

```
Gateway running
Agent loaded
Sessions available
```

然后打开：

```
http://127.0.0.1:18789/#token=<TOKEN>
```

------

# 二十四、OpenClaw 性能优化

如果运行环境是：

- 虚拟机
- 小服务器
- 低配电脑

建议进行以下优化。

------

# 1 Node 启动优化

OpenClaw 官方推荐：

```
NODE_COMPILE_CACHE
OPENCLAW_NO_RESPAWN
```

设置环境变量：

```
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1
```

加入：

```
~/.zshrc
```

或：

```
~/.bashrc
```

------

# 2 降低内存占用

关闭 memory search：

```
openclaw config set agents.defaults.memorySearch.enabled false
```

适合：

- 不使用长期记忆
- 仅使用 LLM 推理

------

# 3 减少 Plugins

查看：

```
openclaw doctor
```

禁用不需要的插件。

------

# 4 减少 Skills

Skills 越多：

```
启动越慢
```

只保留常用：

推荐：

```
github
summarize
nano-pdf
clawhub
```

------

# 5 限制 Agent 工具

在配置中限制：

```
allowCommands
```

避免 Agent 频繁调用系统命令。

------

# 6 虚拟机资源建议

推荐 VMware 配置：

```
CPU: 4 core
RAM: 8GB
Disk: 40GB+
```

最低：

```
CPU: 2 core
RAM: 4GB
```

------

# 7 日志清理

OpenClaw 日志在：

```
/tmp/openclaw
```

定期清理：

```
rm -rf /tmp/openclaw/*
```

------

# 8 定期更新

更新 OpenClaw：

```
openclaw update
```

或：

```
npm update -g openclaw
```

------

# 二十五、完整部署总结

OpenClaw 本地部署结构：

```
Ubuntu VM
   │
Node.js 22
   │
OpenClaw Gateway
   │
Agent Runtime
   │
OpenAI / Claude / Gemini
```

核心目录：

```
~/.openclaw
```

核心服务：

```
openclaw-gateway
```

访问方式：

```
http://127.0.0.1:18789/#token=<TOKEN>
```

------

# 二十六、常用命令速查

查看状态：

```
openclaw status
```

健康检查：

```
openclaw doctor
```

检查 gateway：

```
openclaw gateway probe
```

查看日志：

```
journalctl --user -u openclaw-gateway -f
```

启动：

```
systemctl --user start openclaw-gateway
```

停止：

```
systemctl --user stop openclaw-gateway
```

重启：

```
systemctl --user restart openclaw-gateway
```

# 二十七、OpenClaw 进阶玩法（Agent 自动化）

OpenClaw 不只是聊天 AI，它的真正用途是 **自动执行任务的 AI Agent**。

核心能力：

```
任务自动化
代码辅助
工具调用
数据处理
文件操作
```

------

# 1 Agent 自动执行任务

普通 AI：

```
问答
```

OpenClaw：

```
规划任务 → 调用工具 → 执行 → 返回结果
```

示例：

输入：

```
analyze this project and summarize the architecture
```

Agent 会：

```
读取代码
分析文件结构
生成总结
```

------

# 2 代码项目分析

OpenClaw 非常适合：

```
代码仓库分析
```

示例任务：

```
scan this repository and explain how it works
```

Agent 会：

```
读取代码
分析结构
总结模块
```

适用于：

```
陌生项目
开源代码
大型仓库
```

------

# 3 自动文档生成

可以让 Agent 自动生成文档。

示例：

```
generate README for this project
```

或者：

```
create documentation for all modules
```

Agent 会：

```
扫描代码
总结 API
生成文档
```

------

# 4 文件批处理

OpenClaw 可以处理本地文件。

例如：

```
summarize all pdf files in this folder
```

Agent 会：

```
读取 PDF
提取内容
生成摘要
```

适合：

```
论文整理
资料总结
```

------

# 5 GitHub 自动化

如果启用 `github skill`。

Agent 可以：

```
clone repo
create issue
create PR
review code
```

示例：

```
clone the repository and explain the main modules
```

------

# 6 Coding Agent

OpenClaw 可以当 **本地编程助手**。

示例：

```
write a python script that downloads images
```

或者：

```
refactor this code
```

Agent 会：

```
生成代码
解释代码
优化代码
```

------

# 7 自动执行 Shell

OpenClaw 可以调用系统命令。

例如：

```
list files
analyze system
run scripts
```

示例：

```
check disk usage and summarize
```

Agent 会：

```
df -h
分析结果
生成报告
```

------

# 8 多步骤任务

OpenClaw 支持 **任务链**。

例如：

```
download a dataset
analyze it
generate charts
write a report
```

Agent 会：

```
step1 下载
step2 处理
step3 分析
step4 生成结果
```

------

# 9 自动化工作流

可以让 Agent 执行完整流程。

例如：

```
scan new blog posts and summarize them daily
```

配合：

```
blogwatcher skill
```

Agent 自动：

```
监控 RSS
生成摘要
```

------

# 10 本地 AI 助手

OpenClaw 可以作为：

```
本地 AI 助手
```

例如：

```
organize my documents
summarize notes
analyze logs
```

------

# 11 Canvas 可视化

Canvas 可以显示：

```
图表
UI
HTML
可视化结果
```

例如：

```
create a chart of this dataset
```

Agent 会生成：

```
图形
表格
可视化
```

------

# 12 Hooks 自动执行

Hooks 可以自动执行任务。

例如：

```
/new
/reset
```

自动：

```
保存 session
记录日志
```

------

# 13 Web Search

如果配置：

```
BRAVE_API_KEY
```

Agent 可以：

```
搜索互联网
```

示例：

```
search latest AI news
```

------

# 14 多 Agent 系统

OpenClaw 支持多个 Agent。

例如：

```
coding-agent
research-agent
analysis-agent
```

每个 Agent 可以使用：

```
不同模型
不同 tools
不同 skills
```

------

# 15 实用任务示例

以下任务非常适合 OpenClaw：

### 项目分析

```
analyze this project architecture
```

------

### 代码解释

```
explain this code
```

------

### PDF 总结

```
summarize this research paper
```

------

### 日志分析

```
analyze these logs and find errors
```

------

### 自动报告

```
generate weekly report from these files
```

------

# 二十八、推荐技能组合

推荐启用：

```
github
summarize
nano-pdf
clawhub
blogwatcher
```

用途：

```
代码分析
文档处理
PDF解析
自动更新
```

------

# 二十九、最佳实践

推荐：

```
一个 VM 一个 Agent
```

避免：

```
多个用户共享 agent
```

安全性更高。

------

# 三十、完整使用流程

日常使用：

```
启动 Gateway
↓
打开 Dashboard
↓
创建 Session
↓
输入任务
↓
Agent 执行
↓
查看结果
```

------

# 三十一、OpenClaw 使用建议

OpenClaw 适合：

```
开发者
研究人员
自动化任务
本地 AI 助手
```

不适合：

```
多用户公共服务
高安全场景
```

------

# 三十二、文档结束

至此，本指南完成：

内容包含：

```
安装
配置
调试
安全
迁移
优化
架构
进阶使用
```

OpenClaw 本地部署已经可以 **稳定运行并长期使用**。