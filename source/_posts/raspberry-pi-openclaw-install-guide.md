---
title: 树莓派 × OpenClaw｜24 小时本地运行，一键安装超低成本方案
date: 2026-03-12 14:40:00
tags:
  - OpenClaw
  - 树莓派
  - RaspberryPi
  - AI
  - 1Panel
categories:
  - 技术教程
---

> **原文链接**：[树莓派 × OpenClaw｜24 小时本地运行，一键安装超低成本方案](https://my.feishu.cn/wiki/A8Wow5CM6ixihzkYqnjcZWBonWc)
> **说明**：本方案使用树莓派 5 作为硬件载体，通过 1Panel 实现 OpenClaw 的快速部署，打造 7×24 小时在线的本地 AI 助手。

## 一、硬件准备
推荐使用 **树莓派 5**，全套成本约 400-500 元，是 Mac Mini 的完美平替。组装时建议加装散热风扇以保证长期运行稳定。

## 二、安装操作系统
1. 下载 **Raspberry Pi Imager**。
2. 烧录 **Raspberry Pi OS (64-bit)**。
3. 在“Advanced options”中开启 SSH，配置 Wi-Fi 和账户信息。
4. 插入 microSD 卡，连接显示器和键盘，插电启动。

## 三、联网与远程连接
1. 获取树莓派 IP：`ifconfig`。
2. 开启接口：在 `Raspberry Pi Configuration` -> `Interfaces` 中打开 **SSH** 和 **VNC**。
3. 远程登录：`ssh pi@<树莓派IP>`。

## 四、一键安装 1Panel 与 OpenClaw

### 4.1 安装 1Panel
切换至 root 用户并执行：
```bash
bash -c "$(curl -sSL https://resource.fit2cloud.com/1panel/package/v2/quick_start.sh)"
```

### 4.2 安装 OpenClaw
1. 访问 1Panel 面板，进入“应用商店” -> “AI” 标签。
2. 找到 **OpenClaw** 点击安装。
3. **配置模型**：在“智能体”模块中创建模型账号，填入 API Key。
4. **创建智能体**：关联模型并开启端口外部访问。

## 五、外部访问配置
1. 在 `data/conf/openclaw.json` 中找到 `gateway` 下的 `auth token`。
2. 在 1Panel 应用参数中，将 token 粘贴到令牌字段。
3. 配置 Web 访问地址：`<树莓派IP>:18789?token=你的TOKEN`。

## 六、配置飞书插件
执行以下指令安装飞书官方 OpenClaw 插件：
```bash
npx -y https://sf3-cn.feishucdn.com/obj/open-platform-opendoc/8ab6e7a04c17db1becfcbda8ca35f091_1rCCFRWlRV.tgz install
```
按照提示扫码创建机器人或关联已有机器人。

## 七、进阶：使用海外模型 (Mihomo)
如需访问海外模型，可安装 Mihomo (原 Clash.Meta) 开启 TUN 模式实现透明代理。

### 常用命令汇总
- 查看日志：`tail -f ~/.openclaw/logs/gateway.log`
- 重启服务：`openclaw gateway restart`

---
*全部搞定后，拔掉键盘鼠标显示器，一个插电的树莓派就是你的 24 小时 AI 管家了！🦾*
