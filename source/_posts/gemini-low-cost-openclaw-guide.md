---
title: OpenClaw Token 省大钱｜谷歌 Gemini 白嫖 / 低成本使用全攻略
date: 2026-03-11 14:50:00
tags:
  - OpenClaw
  - Gemini
  - Google
  - AI
  - 省钱攻略
categories:
  - 技术教程
---

> **原文链接**：[OpenClaw Token 省大钱｜谷歌 Gemini 白嫖 / 低成本使用全攻略](https://my.feishu.cn/wiki/OoGAwapfPipV1Ekt9I4ciZpHnNg)
> **说明**：OpenClaw 对 Token 消耗较高。本教程教你如何通过 Antigravity 代理将 Google 的 Gemini 免费额度接入 OpenClaw，极大降低运行成本。

## 一、前置获取
1. **Google One 会员** 或 **学生认证**：可以获得更高额度的 Gemini 免费使用权限。
2. **账号获取**：如果没有上述条件，可以去闲鱼或小红书搜索相关认证教程或成品号。

## 二、安装 Antigravity 代理

### 2.1 全局安装
```bash
npm install -g antigravity-claude-proxy@latest
```

### 2.2 添加账号授权 (无头模式)
```bash
antigravity-claude-proxy accounts add --no-browser
```
复制输出的 URL 到本地浏览器，登录 Google 账号授权，并将获取的授权码粘贴回终端。

### 2.3 设置 Systemd 持久运行
创建 `/etc/systemd/system/antigravity-proxy.service`：
```ini
[Unit]
Description=Antigravity Claude Proxy
After=network.target

[Service]
Type=simple
User=root
Environment=HOST=127.0.0.1
Environment=PORT=8080
ExecStart=/usr/bin/env antigravity-claude-proxy start
Restart=always

[Install]
WantedBy=multi-user.target
```
启动服务：`systemctl enable --now antigravity-proxy`。

## 三、配置 OpenClaw 使用 Gemini 3 Flash

### 3.1 修改 openclaw.json
在 `models.providers` 中添加代理指向：
```json
"antigravity-proxy": {
  "baseUrl": "http://127.0.0.1:8080",
  "apiKey": "test",
  "api": "anthropic-messages",
  "models": [
    {
      "id": "gemini-3-flash",
      "name": "Gemini 3 Flash",
      "reasoning": true,
      "contextWindow": 1048576
    }
  ]
}
```

### 3.2 验证配置
```bash
openclaw models list
# 确认 antigravity-proxy/gemini-3-flash 的 auth 为 yes
```

## 四、终端代理配置 (以 7890 端口为例)
国内服务器访问 Google API 需要终端代理。
- **临时生效**：`export HTTP_PROXY=http://127.0.0.1:7890`
- **永久生效**：将上述内容写入 `~/.bashrc` 或 `~/.bash_profile` 并 `source`。

---
*参加 OpenClaw 虾神争霸赛，如果觉得文章对你有帮助，求各位点个赞支持下！感谢大家🙏*
