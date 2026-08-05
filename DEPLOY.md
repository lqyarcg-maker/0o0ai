# 0o0ai.com 部署指南

## 域名信息
- 域名注册商: 阿里云（万网/hichina）
- DNS 服务商: 阿里云云解析 DNS（dns3.hichina.com / dns4.hichina.com）
- 当前状态: 未配置解析记录，需要添加

---

## 方案 A: Vercel 部署（推荐）

### 第1步：注册 Vercel 账号
1. 打开 https://vercel.com
2. 点击 Sign Up，用 GitHub/Google/邮箱注册（免费）

### 第2步：创建 GitHub 仓库
1. 打开 https://github.com/new
2. Repository name 填 `0o0ai`
3. 选择 Public
4. 不要勾选 README/.gitignore/license
5. 点击 Create repository

### 第3步：推送代码到 GitHub
在本项目目录（`C:\Users\Admin\WorkBuddy\Claw\0o0ai`）打开终端，执行：

```bash
git remote add origin https://github.com/你的用户名/0o0ai.git
git branch -M main
git push -u origin main
```

如果提示需要登录，用 GitHub 用户名和 Personal Access Token（不是密码）。
Token 生成地址: https://github.com/settings/tokens （勾选 repo 权限）

### 第4步：在 Vercel 导入项目
1. 打开 https://vercel.com/new
2. 选择刚才创建的 `0o0ai` 仓库
3. Framework Preset 选 Other（纯静态站）
4. 点击 Deploy
5. 等待 30 秒，部署完成，获得 `0o0ai.vercel.app` 类似的 URL

### 第5步：绑定自定义域名
1. 在 Vercel 项目页面 → Settings → Domains
2. 输入 `0o0ai.com`，点击 Add
3. Vercel 会显示需要添加的 DNS 记录（通常是 CNAME）
4. 同时添加 `www.0o0ai.com`

---

## 方案 B: Netlify 拖拽部署（最简单，无需 Git）

### 第1步：拖拽部署
1. 打开 https://app.netlify.com/drop
2. 将 `C:\Users\Admin\WorkBuddy\Claw\0o0ai` 文件夹拖到页面上
3. 等待 10 秒，获得 Netlify URL

### 第2步：认领站点
1. 注册 Netlify 账号（免费）
2. 在 Sites 列表中认领刚才拖拽的站点

### 第3步：绑定自定义域名
1. 进入站点 → Domain management → Add custom domain
2. 输入 `0o0ai.com`
3. Netlify 会显示需要添加的 DNS 记录

---

## DNS 配置（在阿里云控制台操作）

无论用 Vercel 还是 Netlify，绑定域名后平台会告诉你需要添加什么记录。
通常是一个 CNAME 记录：

### 操作步骤
1. 登录阿里云控制台: https://dc.console.aliyun.com
2. 左侧菜单 → 云解析 DNS → 解析管理
3. 找到 `0o0ai.com`，点击 解析设置
4. 点击 添加记录

### 需要添加的记录（以 Vercel 为例）

| 记录类型 | 主机记录 | 记录值 | TTL |
|---------|---------|--------|-----|
| CNAME | @ | cname.vercel-dns.com | 600 |
| CNAME | www | cname.vercel-dns.com | 600 |

> 注意：@ 记录如果已存在 A 记录需要先删除。具体记录值以 Vercel/Netlify 提示为准。

### 验证
- 添加后等待 10 分钟 - 2 小时（DNS 生效时间）
- 访问 https://0o0ai.com 确认上线
- SSL 证书由 Vercel/Netlify 自动签发，无需手动配置

---

## 更新网站内容

### Vercel 方案
```bash
# 修改文件后
git add -A
git commit -m "更新内容描述"
git push
# Vercel 自动重新部署，1分钟内生效
```

### Netlify 方案
- 重新拖拽文件夹到 Netlify，或
- 连接 GitHub 仓库后自动部署
