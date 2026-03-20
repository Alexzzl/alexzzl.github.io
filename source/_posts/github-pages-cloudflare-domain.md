---
title: GitHub Pages 绑定 Cloudflare 域名
date: 2026-03-20 13:40:00
tags:
  - GitHub
  - Cloudflare
  - DNS
categories:
  - 技术教程
---

给 GitHub Pages 绑定 Cloudflare 域名主要分为两部分：在 **Cloudflare** 端配置 DNS 解析，以及在 **GitHub** 端设置自定义域名。

以下是 2026 年最稳妥的配置步骤：

### 第一步：在 Cloudflare 中配置 DNS 记录
登录 Cloudflare 控制台，进入你的域名管理页面，点击 **DNS** -> **Records**，添加以下记录：

1. **添加 A 记录（解析根域名）**
你需要添加 4 条 A 记录，指向 GitHub Pages 的官方服务器 IP：
  - **Type:** A
  - **Name:** @ (代表根域名)
  - **IPv4 address:** 分别添加以下四个：
    - 185.199.108.153
    - 185.199.109.153
    - 185.199.110.153
    - 185.199.111.153
  - **Proxy status:** 建议先设为 **DNS Only** (灰色云朵)，等 GitHub 验证通过后再开启开启小黄云（Proxy）。

2. **添加 CNAME 记录（解析 www 二级域名）**
  - **Type:** CNAME
  - **Name:** www
  - **Target:** 你的用户名.github.io
  - **Proxy status:** 同上，建议先设为 **DNS Only**。

### 第二步：在 GitHub 中绑定域名
1. 打开你的 GitHub 仓库，点击 **Settings** (设置)。
2. 在左侧菜单栏找到 **Pages** 选项。
3. 在 **Custom domain** 下方的输入框中，输入你的域名（例如 alexzzl.com），点击 **Save**。
4. GitHub 会开始进行 **DNS check**。如果第一步配置正确，通常几分钟内就会显示 "DNS check successful"。

### 第三步：开启 HTTPS (关键)
在 GitHub Pages 设置页面的同一位置：
1. 找到 **Enforce HTTPS** 选项。
2. **勾选它**。这会确保你的网站通过加密连接访问。
  - *注意：如果勾选框是灰色的，说明证书还在申请中，请等待 15 分钟到 24 小时不等。*

### 💡 进阶：如何处理 Cloudflare 的“小黄云” (Proxy)？
很多同学在开启 Cloudflare 的 Proxy（橙色云朵）后，GitHub 会报错“域名未经验证”或 SSL 证书冲突。
- **最佳实践：**
  a. **初期配置：** 保持 **DNS Only**（灰色云朵），直到 GitHub 成功颁发证书并显示 HTTPS 已开启。
  b. **开启 Proxy：** 之后如果你想利用 Cloudflare 的 CDN 和防护功能，可以把云朵点亮。
  c. **SSL 设置：** 开启 Proxy 后，请确保 Cloudflare 的 **SSL/TLS** 模式设置为 **Full** 或 **Full (Strict)**。不要设为 *Flexible*，否则会导致重定向循环错误。

### 常见问题排查
- **404 错误：** 检查仓库根目录下是否自动生成了一个名为 CNAME 的文件，内容应仅包含你的域名。
- **解析不生效：** DNS 修改可能需要 1-10 分钟生效，可以使用终端命令 `dig 你的域名` 或 `ping 你的域名` 来确认 IP 是否已指向 GitHub。
