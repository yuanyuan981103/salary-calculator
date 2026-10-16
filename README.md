# 工资计算器

纯前端的工资计算器：**单文件 HTML + 无任何外部依赖**，支持手机端、可安装到主屏幕、离线可用。

## 功能

- **岗位工资**输入后自动按比例拆分基础工资 / 基础绩效（默认 70% / 30%，可调）
- **绩效**按考核等级浮动：胜任 100%、良好 110%、优秀 120%、合格 90%
- **加班费**：平时 ×1.5、周末 ×2、节假日 ×3（时薪 = 基础工资 ÷ 当月工时）
- **缺勤**先用平时加班 1:1 抵扣，抵不完才扣款
- **社保**：基数 = max(岗位工资 × 系数, 最低基数)，个人扣养老 + 医疗 + 失业
- **公积金**：个人比例（从工资扣）与公司比例（单位缴存，仅显示）分开设置
- **个税**：国家标准累计预扣预缴法（支持填入截至上月的累计数据）
- **其他代扣**：水电气 / 宿舍费等，不计个税
- 输出应发明细、扣款明细、个税计算过程，自动保存输入（存各自浏览器）

## 文件说明

| 文件 | 作用 |
|------|------|
| `index.html` | 计算器本体（唯一需要维护的文件） |
| `manifest.webmanifest` | PWA 清单（应用名、图标、主题色） |
| `service-worker.js` | 离线缓存，断网也能打开 |
| `icon.svg` / `icon-*.png` | 应用图标（浏览器标签页 + 手机主屏图标） |
| `deploy.ps1` | 一键部署脚本（方式 A 用） |
| `README.md` | 本说明文档 |

---

## 部署到 GitHub Pages

> 部署后得到一个网址，手机浏览器打开后可「添加到主屏幕」，像 App 一样使用，**不依赖电脑开机**。

### 方式 A：一键脚本（本机已装 Git，推荐）

1. 在 GitHub 网页创建**空仓库**
   - 入口：https://github.com/new
   - Repository name 填 `salary-calculator`（或自定）
   - 选 **Public**
   - ⚠️ **不要勾选** Add a README file / .gitignore / license，否则推送会冲突
2. 在本文件夹打开 PowerShell，执行：

   ```powershell
   .\deploy.ps1 -User 你的GitHub用户名
   ```

   脚本自动完成：配置提交身份（用 GitHub 免回复邮箱 `用户名@users.noreply.github.com`，不暴露真实邮箱）→ 提交文件 → 关联远程仓库 → 推送。
   推送时会弹出浏览器/登录窗口，**请自行登录 GitHub 授权**（凭证由 Git 本地保存，不经过第三方）。

   如果想换仓库名：` .\deploy.ps1 -User 用户名 -RepoName 其他名字`
3. 按脚本末尾提示开启 Pages：**Settings → Pages** → Source 选 `Deploy from a branch` → Branch 选 `main` + `/ (root)` → Save

### 方式 B：网页点点点（不用命令行）

1. 打开 [github.com](https://github.com) 注册 / 登录
2. 右上角 **+** → **New repository**
   - Repository name 填 `salary-calculator`（随便起）
   - 选 **Public**（免费版 Pages 需要公开仓库）
   - 点 **Create repository**
3. 进入新仓库，点 **uploading an existing file** 链接
4. 把本文件夹里的**所有文件**拖进去（`index.html`、`manifest.webmanifest`、`service-worker.js`、各个图标文件）
   - ⚠️ 不要拖整个文件夹，要把文件放在仓库**根目录**
5. 下方 Commit changes 点 **Commit changes**
6. 仓库页面点 **Settings** → 左侧 **Pages**
   - **Source** 选 `Deploy from a branch`
   - **Branch** 选 `main`，目录选 `/ (root)`，点 **Save**
7. 等 1~2 分钟刷新页面，会看到网址：

   ```
   https://你的用户名.github.io/salary-calculator/
   ```

   打开就能用了 🎉

### 方式 C：命令行手动（想看每一步做了什么）

在本文件夹打开终端，依次执行：

```bash
git init
git add .
git commit -m "add salary calculator"
git branch -M main
git remote add origin https://github.com/你的用户名/salary-calculator.git
git push -u origin main
```

然后按方式 B 的第 6~7 步开启 Pages 即可。

---

## 手机添加到主屏幕

**Android（Chrome / Edge）**
1. 浏览器打开部署好的网址
2. 右上角菜单 **⋮** → **添加到主屏幕 / 安装应用**
3. 确认后桌面出现图标，点开是全屏应用（离线也能用）

**iPhone（Safari）**
1. Safari 打开网址（要用 Safari，微信内置浏览器不行）
2. 底部 **分享** 按钮 → 下滑找到 **添加到主屏幕**
3. 命名为「工资计算器」→ 添加

> 部署到 https 网址后，`service-worker.js` 会自动缓存页面，**断网也能打开**。

---

## 其他托管方式

GitHub Pages 在国内访问可能偏慢，也可以用：

- **Cloudflare Pages** / **Vercel** / **Netlify**：拖拽文件夹即可部署，同样免费、自带 https
- **腾讯云 COS / 阿里云 OSS**：开通静态网站托管，上传文件即可

> 只要是静态托管，把这几个文件传上去就能用，无需任何构建步骤。

---

## 隐私说明

- 文件本身是**空白模板**，不含任何人的工资数据
- 你填的数据保存在**你自己浏览器的 localStorage** 里，**不会上传**、也不会随文件传给他人
- 多个人用同一个网址，各存各的数据，互不影响
- 要清空自己的数据：点页面下方 **🗑️ 清空全部数据**
- 如果想彻底不公开：把 GitHub 仓库设为 Private（此时 Pages 需要付费版；或改用仅本地/局域网方式）

## 本地直接打开（不上网）

双击 `index.html` 即可在浏览器使用。此方式下 Service Worker（离线缓存）不会生效，其他功能完全正常。
