---
title: Hexo 博客搭建与历史项目恢复完全指南
top: false
cover: false
toc: true
mathjax: true
date: 2026-03-07 01:08:24
password:
summary:
tags: 
    - 博客
categories: 
    - 问题解决
---

## 一、环境准备

### 1.1 安装 Node.js
- **作用**：Hexo 的运行环境
- **下载地址**：[Node.js 官网](https://nodejs.org/)
- **选择版本**：下载 **LTS（长期支持）版本**
- **安装提示**：Windows 用户保持默认设置即可，确保添加到系统 PATH

### 1.2 安装 Git
- **作用**：版本控制和部署工具
- **下载地址**：[Git 官网](https://git-scm.com/downloads)
- **安装提示**：Windows 用户可选择默认选项

### 1.3 验证安装
打开命令行（Git Bash / 终端 / CMD），输入以下命令验证：

```bash
# 验证 Node.js
node -v
# 应显示 v18.x.x 或更高版本

# 验证 npm
npm -v
# 应显示 9.x.x 或更高版本

# 验证 Git
git --version
# 应显示 git version 2.x.x
```

---

## 二、从零搭建新博客

### 2.1 安装 Hexo

```bash
# 全局安装 Hexo 命令行工具
npm install -g hexo-cli

# 验证安装
hexo -v
```

### 2.2 初始化博客

```bash
# 初始化一个名为 myblog 的博客（名字可自定义）
hexo init myblog

# 进入博客目录
cd myblog

# 安装依赖
npm install
```

### 2.3 目录结构说明

```
myblog/
├── node_modules/      # 依赖模块
├── scaffolds/         # 模板文件
├── source/            # 源文件
│   └── _posts/        # 文章存放处（.md 文件）
├── themes/            # 主题文件夹
├── _config.yml        # 博客配置文件（重要！）
├── package.json       # 项目依赖配置
└── .gitignore         # Git 忽略文件
```

### 2.4 本地运行与查看（重要）

这是开发过程中最常用的步骤，用于在本地预览博客效果：

```bash
# 1. 生成静态文件
hexo generate
# 或使用简写
hexo g

# 2. 启动本地服务器
hexo server
# 或使用简写
hexo s

# 3. 命令行会显示：
# INFO  Hexo is running at http://localhost:4000
# 按住 Ctrl 键点击该链接，或在浏览器中手动输入 http://localhost:4000

# 4. 本地预览功能：
# - 实时查看博客效果
# - 修改文章后刷新浏览器即可看到更新
# - 修改配置文件后需要重启服务

# 5. 停止本地服务器
# 在终端中按 Ctrl + C
```

**本地运行的好处**：
- 写文章时实时预览效果
- 测试主题修改
- 检查排版和样式
- 确认无误后再部署到线上

### 2.5 写作流程

```bash
# 1. 创建新文章
hexo new "我的第一篇文章"
# 会在 source/_posts/ 生成 "我的第一篇文章.md"

# 2. 编辑文章（用 VS Code、Typora 等编辑器）
# 文章头部格式（Front-matter）：
---
title: 我的第一篇文章
date: 2024-01-01 12:00:00
tags: [Hexo, 教程]
categories: 技术
---

这里是文章内容，使用 Markdown 格式编写。

# 3. 本地预览效果
hexo clean  # 清理缓存（有时需要）
hexo g      # 重新生成
hexo s      # 启动服务查看

# 4. 满意后部署到线上
hexo clean && hexo g && hexo d
```

---

## 三、部署到 GitHub Pages

### 3.1 创建 GitHub 仓库
1. 登录 [GitHub](https://github.com)
2. 点击 "+" → "New repository"
3. **仓库名必须为**：`你的用户名.github.io`
   - 例如用户名为 `john`，则仓库名为 `john.github.io`
4. 选择 Public（免费），点击 Create repository

### 3.2 配置 SSH 密钥

```bash
# 1. 生成 SSH 密钥（替换为你的 GitHub 邮箱）
ssh-keygen -t rsa -C "your_email@example.com"
# 连续回车三次

# 2. 查看公钥并复制
cat ~/.ssh/id_rsa.pub
# Windows 用户可用：type C:\Users\用户名\.ssh\id_rsa.pub
```

**添加到 GitHub**：
- GitHub 头像 → Settings → SSH and GPG keys → New SSH key
- 粘贴公钥，添加标题，点击 Add SSH key

**测试连接**：
```bash
ssh -T git@github.com
# 显示 "Hi 用户名! You've successfully authenticated" 即成功
```

### 3.3 修改配置文件

编辑博客根目录下的 `_config.yml`，找到 `deploy` 配置（通常在文件末尾）：

```yaml
# 部署配置
deploy:
  type: git
  repo: git@github.com:你的用户名/你的用户名.github.io.git
  branch: main  # 或 master，取决于你的默认分支
```

### 3.4 一键部署

```bash
# 安装部署插件（只需安装一次）
npm install hexo-deployer-git --save

# 完整的部署流程（先预览再部署）
hexo clean      # 清除缓存
hexo generate   # 生成静态文件
hexo server     # 本地预览确认
# 确认无误后，按 Ctrl+C 停止服务，然后：
hexo deploy     # 部署到 GitHub

# 或使用组合命令（跳过预览步骤）
hexo clean && hexo generate && hexo deploy
```

部署完成后，访问 `https://你的用户名.github.io` 即可看到博客！

---

## 四、历史项目找回指南

### 4.1 找回前的准备
1. 找到你的 GitHub 仓库地址
2. 确保已配置 SSH 密钥
3. 准备一个空目录用于恢复

### 4.2 Git 分支查看命令详解

```bash
# 1. 克隆仓库（先看看有什么）
git clone git@github.com:你的用户名/你的用户名.github.io.git
cd 你的用户名.github.io

# 2. 查看本地分支
git branch

# 3. 查看远程分支
git branch -r

# 4. 查看所有分支（最重要！）
git branch -a

# 输出示例：
#   master
#   remotes/origin/HEAD -> origin/master
#   remotes/origin/hexo          # ⭐ 如果有这个，恭喜！
#   remotes/origin/hexo-source    # ⭐ 备份分支
#   remotes/origin/source         # ⭐ 备份分支
#   remotes/origin/master
```

### 4.3 场景一：找到备份分支（理想情况）

如果你看到类似 `remotes/origin/hexo`、`remotes/origin/source` 等非 master 分支：

```bash
# 1. 查看所有远程分支，找到备份分支
git branch -a

# 2. 假设找到 remotes/origin/hexo-source
# 基于远程分支创建本地分支
git checkout -b hexo-source origin/hexo-source
# 或使用新版命令
git switch -c hexo-source origin/hexo-source

# 3. 查看文件列表
ls -la
# 应该能看到：
# - source/_posts/  📁 你的文章！
# - _config.yml     ⚙️ 配置文件
# - themes/         🎨 主题文件夹
# - package.json    📦 依赖列表

# 4. 安装依赖
npm install

# 5. 本地预览验证
hexo clean && hexo generate
hexo server
# 访问 http://localhost:4000 确认博客恢复成功

# 6. 恢复完成！可以继续写文章了
```

### 4.4 场景二：只有 master/main 分支

如果只有 master/main 分支，需要手动找回文章：

```bash
# 1. 确保在 master 分支
git checkout master

# 2. 查看目录结构
ls -la
# 会看到类似：
# - 2023/     📁 年份文件夹
# - 2024/     📁 年份文件夹
# - archives/ 📁 归档
# - css/      📁 样式文件

# 3. 寻找文章（HTML 格式）
# 进入年份/月份/日期/文章标题/ 目录
cd 2024/03/15/hello-world/

# 4. 打开 index.html
# 用浏览器打开，复制文章内容

# 5. 在新博客中重建
# 创建一个新目录，重新初始化
cd ~
mkdir my-new-blog
cd my-new-blog
hexo init
npm install

# 6. 手动创建文章
# 将复制的文章内容保存为 Markdown 文件
# 放到 source/_posts/ 目录下

# 7. 本地预览确认
hexo clean && hexo generate
hexo server
# 访问 http://localhost:4000 查看文章是否成功恢复
```

**手动找回文章的步骤**：
1. 逐个打开 `年份/月份/日期/文章标题/index.html`
2. 复制文章正文内容
3. 在 `source/_posts/` 下创建同名的 `.md` 文件
4. 粘贴内容，保留 Markdown 格式
5. 重复直到找回所有文章
6. 每次恢复一篇就本地预览确认一下

---

## 五、日常写作与维护

### 5.1 完整的日常写作流程

```bash
# 1. 创建新文章
hexo new "文章标题"

# 2. 编辑文章（用 Markdown 编辑器）

# 3. 本地预览（反复调整直到满意）
hexo clean    # 清理缓存（推荐每次都执行）
hexo g        # 生成静态文件
hexo s        # 启动本地服务
# 访问 http://localhost:4000 查看效果
# 修改文章后，刷新浏览器即可看到更新

# 4. 确认无误后部署
# 先停止本地服务 (Ctrl+C)
hexo clean && hexo g && hexo d

# 5. 访问线上地址确认
# https://你的用户名.github.io
```

### 5.2 双保险备份策略（避免再次丢失）

**避免再次丢失文件的完整方案：**

```bash
# 1. 在 GitHub 仓库创建备份分支
# 仓库 → Branches → New branch
# 分支名：hexo-source （或你喜欢的名字）

# 2. 在本地博客目录初始化 Git（如果还没有）
git init

# 3. 关联远程仓库
git remote add origin git@github.com:你的用户名/你的用户名.github.io.git

# 4. 创建并切换到备份分支
git checkout -b hexo-source

# 5. 配置 .gitignore（确保不提交无关文件）
cat > .gitignore << EOF
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
EOF

# 6. 第一次提交
git add .
git commit -m "初始备份：完整 Hexo 源文件"

# 7. 推送到远程备份分支
git push -u origin hexo-source

# 8. 日常写作后的双重备份
# 写完文章并部署后，立即执行：
git add .
git commit -m "备份：更新文章《文章标题》"
git push origin hexo-source
```

**最终效果**：
- `main/master` 分支：存放生成的静态网页（用于展示）
- `hexo-source` 分支：存放完整的源文件（文章、配置、主题）

**换电脑时恢复环境**：
```bash
# 只需克隆备份分支
git clone -b hexo-source git@github.com:你的用户名/你的用户名.github.io.git
cd 你的用户名.github.io
npm install
hexo s  # 本地预览，一切恢复如初！
```

---

## 六、常见问题解决

### 6.1 本地运行看不到文章

```bash
# 1. 检查文章是否在正确位置
ls source/_posts/
# 应该能看到 .md 文件

# 2. 检查文章头部格式
# ---
# title: 标题
# date: 日期
# ---

# 3. 清理缓存后重新生成
hexo clean
hexo g
hexo s
```

### 6.2 部署后样式错乱或 404

```bash
# 1. 清除缓存后重新生成部署
hexo clean
hexo generate
hexo deploy

# 2. 检查 _config.yml 中的 url 配置
# 应该设置为你的 GitHub Pages 地址
url: https://你的用户名.github.io
root: /

# 3. 检查仓库名称是否正确
# 必须是 用户名.github.io
```

### 6.3 deploy 时提示权限错误

```bash
# 1. 检查 SSH 密钥是否正确添加
ssh -T git@github.com

# 2. 重新添加 SSH 密钥
cat ~/.ssh/id_rsa.pub
# 复制后到 GitHub 重新添加

# 3. 检查仓库地址是否正确
git remote -v
# 应为 git@github.com:用户名/用户名.github.io.git
```

### 6.4 想绑定自己的域名

1. 购买域名
2. 在 `source` 目录创建 `CNAME` 文件（无后缀）
3. 文件内容：`yourdomain.com`
4. 重新部署：`hexo clean && hexo g && hexo d`
5. 在域名服务商添加解析记录

---

## 七、结语

现在你拥有了完整的 Hexo 博客搭建和恢复指南！记住最重要的几点：

1. **本地预览**：写文章时用 `hexo s` 实时查看效果
2. **完整流程**：`hexo clean → hexo g → hexo s（预览）→ hexo d（部署）`
3. **备份源文件**：创建独立分支保存完整的 Hexo 工程
4. **双保险策略**：部署网站的同时提交源文件到备份分支
