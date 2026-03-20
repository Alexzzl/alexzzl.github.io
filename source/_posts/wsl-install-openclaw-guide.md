---
title: Windows 10/11 WSL 安装 OpenClaw 指南
date: 2026-03-13 14:35:00
tags:
  - OpenClaw
  - WSL
  - Windows
  - AI
categories:
  - 技术教程
---

> **原文链接**：[Windows 10/11 WSL 安装 OpenClaw 指南](https://my.feishu.cn/wiki/V6XVwmqtfi3bRvkpOOKc6JjCngQ)
> **说明**：本教程提供在 Windows 10/11 WSL 环境下安装和配置 OpenClaw 的完整流程，涵盖从环境初始化到国内 API 配置的全部步骤。

## 一、WSL 安装与网络配置

### 1.1 WSL 版本说明
WSL（适用于 Linux 的 Windows 子系统）目前有两个主要版本：
- **WSL 1**：早期版本，采用系统调用转换层。
- **WSL 2**：最新版本，运行完整的 Linux 内核，性能更佳。
**强烈建议使用 WSL 2**。

### 1.2 一键安装 WSL
在具有管理员权限的 PowerShell 或 Windows 终端中执行：
```powershell
wsl --install
```
安装完成后，必须**重启计算机**。

### 1.3 验证安装状态
```powershell
wsl --status
wsl --version
```

### 1.4 Ubuntu 初始化配置
首次启动 Ubuntu 时，根据提示创建 UNIX 用户名和密码。如果需要切换默认系统为 Ubuntu：
```powershell
wsl --set-default Ubuntu
wsl
```

### 1.5 系统更新与软件源配置
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget git
```

## 二、WSL 网络架构详解与配置

### 2.1 镜像模式 (推荐)
镜像模式通过同步 Windows 宿主机的网络接口状态，解决了 NAT 模式下的 VPN 兼容性和局域网访问问题。

### 2.2 配置 .wslconfig 文件
在 Windows 用户目录 `C:\Users\<用户名>\` 下创建 `.wslconfig`：
```toml
[wsl2]
networkingMode=mirrored
dnsTunneling=true
autoProxy=true
firewall=true

[experimental]
autoMemoryReclaim=gradual
hostAddressLoopback=true
```
保存后执行 `wsl --shutdown` 并重启。

### 2.3 防火墙规则
以管理员身份运行 PowerShell：
```powershell
New-NetFirewallRule -DisplayName "OpenClaw-Service" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 18789
```

## 三、安装 OpenClaw

### 3.1 基础环境
建议开启 sudo 免密权限：`sudo visudo`，添加 `your_user ALL=(ALL) NOPASSWD: ALL`。

### 3.2 运行安装脚本
```bash
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git
sudo corepack enable
sudo corepack prepare pnpm@latest --activate
```

### 3.3 初始化向导
```bash
openclaw onboard --install-daemon
```

## 四、国内 API 配置

### 4.1 MiniMax 国内版
**端点**: `https://api.minimaxi.com/anthropic`
```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "minimax-cn": {
        "baseUrl": "https://api.minimaxi.com/anthropic",
        "apiKey": "YOUR_KEY",
        "api": "anthropic-messages",
        "models": [{"id": "MiniMax-M2.1", "name": "MiniMax M2.1 (China)", "reasoning": true}]
      }
    }
  }
}
```

### 4.2 智谱 (GLM)
**端点**: `https://open.bigmodel.cn/api/coding/paas/v4`
```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "zhipu-cn": {
        "baseUrl": "https://open.bigmodel.cn/api/coding/paas/v4",
        "apiKey": "YOUR_KEY",
        "api": "anthropic-messages",
        "models": [{"id": "glm-4.7", "name": "GLM-4.7 (China)", "reasoning": true}]
      }
    }
  }
}
```

重启生效：`openclaw gateway restart`。

## 五、局域网访问
编辑 `~/.openclaw/openclaw.json`，将 `bind` 改为 `lan`：
```json
"gateway": {
  "bind": "lan",
  "controlUi": { "allowInsecureAuth": true }
}
```

---
*参加 OpenClaw 虾神争霸赛，如果觉得文章对你有帮助，求各位点个赞支持下！感谢大家🙏*
