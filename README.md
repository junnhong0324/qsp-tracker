# QSP Model Tracker

每日自动检索 PubMed，追踪最新 QSP 模型发表动态。

## 部署步骤（10分钟）

### 第一步：上传到 GitHub

1. 登录 [github.com](https://github.com)，点击右上角 **+** → **New repository**
2. 仓库名填 `qsp-tracker`，选 **Public**，点 **Create repository**
3. 上传两个文件：
   - `index.html` → 拖入仓库页面上传
   - `.github/workflows/daily-check.yml` → 需要先创建 `.github/workflows/` 文件夹

   **创建文件夹的方法**：点 **Add file** → **Create new file**，文件名输入 `.github/workflows/daily-check.yml`，粘贴 yml 内容，提交。

### 第二步：开启 GitHub Pages

1. 进入仓库 → **Settings** → 左侧 **Pages**
2. Source 选 **Deploy from a branch**
3. Branch 选 **main**，目录选 **/ (root)**
4. 点 **Save**

等约 1 分钟，你的工具地址就是：
```
https://你的用户名.github.io/qsp-tracker/
```

### 第三步：配置每日邮件通知（可选）

需要一个免费的 SendGrid 账号来发邮件：

1. 注册 [sendgrid.com](https://sendgrid.com)（免费额度 100 封/天）
2. 创建 API Key，复制

在 GitHub 仓库里配置 Secrets：
进入仓库 → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

添加以下三个 secret：

| Name | Value |
|------|-------|
| `NOTIFY_EMAIL` | 你的邮箱地址 |
| `SENDGRID_API_KEY` | SendGrid API Key |
| `SITE_URL` | https://你的用户名.github.io/qsp-tracker/ |

完成后，每天北京时间 9:00 自动运行，有新 QSP 模型时发邮件通知。

### 不需要邮件通知？

跳过第三步。工具本身每次打开自动从 PubMed 拉取最新数据（2小时缓存），直接使用即可。

## 功能说明

| 功能 | 说明 |
|------|------|
| 多策略检索 | 4条并行 PubMed 检索策略，覆盖全量 QSP 模型 |
| 疾病分类 | 肿瘤免疫、自身免疫、代谢、心血管、神经、肾脏、感染等 8 类 |
| 综述过滤 | 默认隐藏综述类文献，可手动勾选显示 |
| 代码筛选 | 快速找到有代码发布的模型 |
| 全文搜索 | 按标题、期刊、疾病标签搜索 |
| 本地缓存 | 2小时缓存，避免重复请求 |
| 每日自动更新 | GitHub Actions 每天 9:00 检索，有新文章发邮件 |

## 文件结构

```
qsp-tracker/
├── index.html                        # 主工具页面（单文件，无依赖）
├── README.md                         # 本文件
└── .github/
    └── workflows/
        └── daily-check.yml           # 每日自动检索脚本
```
