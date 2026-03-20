---
title: 三星 TV Seller Office 上架全攻略：流程、模板、加签名命令与 UI 说明大纲
date: 2026-03-19 14:50:00
tags:
  - 三星TV
  - SamsungTV
  - App上架
  - Tizen
  - 技术教程
categories:
  - 技术教程
---

> **前言**：本文整合了 [FitPulse](https://github.com/Alexzzl/FitPulse) 项目在上架三星 TV Seller Office 过程中的核心技术文档。内容涵盖完整的上架流程、后台字段填写模板、加签名发布命令，以及提交审核所需的 UI Description PPT 中文大纲。

---

## 一、三星 TV 应用上架完整流程

### 1. 确认卖家资格
- **Public Seller**：只能发布至美国商店。
- **Partner Seller**：要发布至美国以外（如欧洲、东南亚、中国等）地区，必须申请并获得此资格。建议先开通 Seller Office 账号，再进行 Partner 申请。

### 2. 提审准备清单
在正式进入 [seller.samsungapps.com](https://seller.samsungapps.com/) 之前，请务必准备好以下素材：
- **安装包**：已签名的 `.wgt` 文件。
- **核心文件**：`config.xml`（需确认 ID 和版本号）。
- **商店图**：`1920x1080` 商店展示图、`512x423` PNG 图标。
- **截图**：至少 4 张 `JPG` 格式的真机截图（建议涵盖 Home, Library, Programs, Player）。
- **文案**：中英文标题、简短描述、详细描述、搜索关键词。
- **合规**：隐私政策 URL（需公网可访问）、客服邮箱。
- **文档**：App UI Description `PPTX` 文件。

### 3. 核心步骤拆解
1. **Create App**：进入 `Applications > Create App`，填写内部管理名并选定默认语言。
2. **App Package**：上传 `.wgt` 包，系统会自动运行 `Pre-Test`。若不通过，需根据报错修改 `config.xml` 或签名。
3. **App Images**：上传 1920x1080 图片、PNG 图标及 4 张截图。
4. **Title/Description on TV**：填写展示给用户的标题和描述。**注意：描述不要包含当前未实现的功能（如搜索、支付等）**。
5. **Service Info**：配置分类（Health/Sports）、年龄分级及卖家联系方式。
6. **Verification Information**：提供测试步骤、是否需要测试账号、遥控器操作说明。
7. **UI Description**：上传审核专用的 PPT 文档。
8. **Distribute**：选择目标机型组，预览无误后提交。

---

## 二、关键命令：加签名与打包

在提审前，需要使用 Tizen 证书对项目进行签名和打包。

**项目路径示例**：`E:\code\workspace\FitPulse0`

**执行打包与加签名命令**：
```bash
# 进入项目目录并执行签名打包
# -t: 类型 (wgt)
# -s: 证书 Profile 名称 (需在 Tizen Certificate Manager 中创建)
# --: 后面接源码目录
tizen package -t wgt -s SamsungTV -- E:\code\workspace\FitPulse0\dist
```

**建议的提审前置脚本**：
```powershell
npm run build         # 构建 Web 资源
npm run capture:store  # 自动生成商店截图
npm run capture:ui     # 生成 UI 说明补充截图
npm run ppt:fill       # 自动填充 PPT 大纲内容
```

---

## 三、Seller Office 字段填写模板 (以 FitPulse 为例)

### 1. Title/Description 默认语言 (English)
| 字段 | 建议值 |
| :--- | :--- |
| **App Title** | `FitPulse` |
| **Short Description** | `A remote-friendly fitness app for Samsung Smart TV with guided home workouts, structured plans, and easy focus-based navigation.` |
| **Full Description** | `FitPulse is a fitness experience designed for Samsung Smart TV. Users can browse workout recommendations, explore body-part-based training in Library, open structured plans in Classic Programs, review workout history, and start guided daily workout sessions with the TV remote. The app is optimized for large-screen viewing, directional focus navigation, and quick access to home fitness content.` |
| **Keywords** | `fitness, workout, home workout, exercise, smart tv` |

### 2. Test Steps (验证步骤)
可直接复制到后台：
```text
1. Launch the app.
2. The app opens with the onboarding flow and then reaches the Home screen.
3. Use the remote directional keys to move focus across the sidebar and workout cards.
4. Open Library and browse body-part workout categories.
5. Open Classic Programs and select a workout plan.
6. Open the plan calendar and select an available day.
7. Start the workout from the Day Detail page.
8. Verify the workout player opens correctly and can be controlled with the remote.
9. Press Back to return to previous screens.
10. Open Me and History to verify the history list page.
```

---

## 四、App UI Description PPT 中文大纲

三星审核员通过此文档理解应用的交互逻辑。建议结构如下：

1. **封面 (Cover)**：应用名称、当前版本号、所属分类、卖家名称。
2. **应用概览 (App Overview)**：一句话说明核心功能。强调为大屏优化及遥控器操作。
3. **页面流转图 (Screen Flow)**：
   - `Launch -> Welcome -> Home -> Library/Programs -> Player -> History`。
4. **核心页面详解**：
   - **Home**: 主仪表盘，展示最近活动和每日推荐。
   - **Library**: 按身体部位分类的训练库浏览。
   - **Classic Programs**: 结构化训练计划与日历展示。
   - **Workout Player**: 核心播放界面，展示动图引导和进度。
5. **遥控器按键映射 (Remote Control Behavior)**：
   - `Up/Down/Left/Right`: 焦点移动（边缘不循环）。
   - `Enter`: 进入/确认。
   - `Back`: 返回上一级或退出。
6. **特别注意事项**：
   - 本版本不含账号体系（无需登录）。
   - 本版本不含内购功能。

---

## 五、避坑指南
1. **焦点丢失**：这是 TV 端审核失败的第一大原因。必须保证每个页面进去都有焦点，且焦点移动符合预期。
2. **包名一致性**：`config.xml` 的 `<name>` 必须与后台 `App Title` 高度一致，否则会有审核警告。
3. **隐私政策**：URL 必须是直接可见的 HTML 页面，且需包含数据收集及注销的相关说明（即便不要求登录）。

---
> **项目源码**：[GitHub - Alexzzl/FitPulse](https://github.com/Alexzzl/FitPulse)
