# 🏆 番茄风向标 · Fanqie Rank Tracker

> 每日自动追踪番茄小说女频新书榜数据，AI 生成趋势分析，部署为精美的在线看板。
>
> Daily automated tracking of Fanqie Novel's new book rankings with AI-powered trend analysis, deployed as a premium online dashboard.

---

## ✨ 功能概览 | Features

| 功能 | Feature | 说明 |
|------|---------|------|
| 🕷️ 自动爬取 | Auto Scraping | 每日定时抓取番茄小说全分类新书榜 Top 30 |
| 📊 趋势对比 | Trend Analysis | 自动对比相邻两天数据：新上榜 / 掉榜 / 排名变化 / 阅读量增长 |
| 🤖 AI 风向分析 | AI Summary | 接入 OpenAI 兼容 API，按分类生成市场趋势速评 |
| 🖥️ 精美看板 | Dashboard | 暗色编辑风格仪表盘，带打字机动画和瀑布流书籍卡片 |
| 📱 移动适配 | Responsive | 完整的移动端适配，侧边栏抽屉式菜单 |
| ⚡ 全自动化 | Fully Automated | GitHub Actions + GitHub Pages，零服务器运维 |

---

## 🚀 食用指南 | Getting Started

### 前置条件 | Prerequisites

- **Python 3.9+**
- **Git**
- 一个 GitHub 账号
- （可选）一个 OpenAI 兼容 API 的密钥，用于 AI 分析

### 第一步：Fork 仓库 | Step 1: Fork the Repository

点击 GitHub 页面右上角的 **Fork** 按钮，将项目 Fork 到你自己的账号下。

> Click the **Fork** button on the top-right corner of the GitHub page to fork this repository to your own account.

### 第二步：开启 GitHub Pages | Step 2: Enable GitHub Pages

1. 进入你 Fork 后的仓库 → **Settings** → **Pages**
2. Source 选择 **Deploy from a branch**
3. Branch 选择 `main`，目录选择 `/ (root)`
4. 点击 **Save**

> 1. Go to your forked repo → **Settings** → **Pages**
> 2. Under Source, select **Deploy from a branch**
> 3. Select `main` branch and `/ (root)` directory
> 4. Click **Save**

稍等几分钟，你的看板就会上线：`https://<你的用户名>.github.io/FanqieRankTracker/`

> After a few minutes, your dashboard will be live at: `https://<your-username>.github.io/FanqieRankTracker/`

### 第三步：配置 Secrets（可选，开启 AI 分析）| Step 3: Configure Secrets (Optional, for AI Analysis)

进入仓库 → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**，添加以下三个 Secret：

> Go to repo → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**, add the following three secrets:

| Secret 名称 | 说明 | 示例 |
|---|---|---|
| `API_BASE_URL` | OpenAI 兼容 API 的地址 | `https://api.openai.com/v1` |
| `API_KEY` | API 密钥 | `sk-xxxxxxxxxxxxx` |
| `API_MODEL` | 模型名称 | `gpt-4o-mini` |

> **💡 提示 / Tip:** 任何 OpenAI 兼容接口均可使用（如 Moonshot / DeepSeek / 自建服务等）。如果不配置这三个 Secret，系统将自动使用基于规则的摘要替代 AI 分析，**不影响核心功能**。
>
> Any OpenAI-compatible API works (e.g., Moonshot / DeepSeek / self-hosted). If these secrets are not configured, the system will automatically fall back to rule-based summaries — **core functionality is unaffected**.

### 第四步：手动触发首次运行 | Step 4: Trigger the First Run Manually

1. 进入仓库 → **Actions** → 左侧选择 **Daily Fanqie Rank Scraper**
2. 点击右上角 **Run workflow** → **Run workflow**
3. 等待 Workflow 运行完成（约 3–5 分钟）

> 1. Go to repo → **Actions** → Select **Daily Fanqie Rank Scraper** on the left
> 2. Click **Run workflow** → **Run workflow** on the top-right
> 3. Wait for the workflow to complete (~3–5 minutes)

运行成功后，`data/` 目录下会自动生成数据文件，打开 GitHub Pages 链接即可看到看板。

> After a successful run, data files will be generated in the `data/` directory. Open the GitHub Pages link to view your dashboard.

### 第五步：坐等自动更新 | Step 5: Sit Back and Relax

GitHub Actions 已配置为 **每天 UTC 00:00（北京时间 08:00）** 自动运行。之后无需任何手动操作，数据和看板会每天自动更新。

> GitHub Actions is configured to run automatically at **UTC 00:00 (08:00 Beijing Time)** every day. No further manual action is needed — data and dashboard will auto-update daily.

---

## 🔧 本地开发 | Local Development

如果你想在本地运行爬虫或调试前端：

> If you want to run the scraper locally or debug the frontend:

```bash
# 1. 克隆仓库 | Clone the repo
git clone https://github.com/<your-username>/FanqieRankTracker.git
cd FanqieRankTracker

# 2. 创建虚拟环境（推荐）| Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 3. 安装依赖 | Install dependencies
pip install -r requirements.txt
playwright install chromium

# 4. 运行爬虫 | Run the scraper
python scrape_fanqie_ranks.py

# 5. 构建看板数据（可选，带 AI 分析需设置环境变量）
#    Build dashboard data (optional, set env vars for AI analysis)
pip install openai
export API_BASE_URL="https://your-api-endpoint/v1"
export API_KEY="your-api-key"
export API_MODEL="your-model-name"
python scripts/build_latest.py

# 6. 本地预览前端 | Preview frontend locally
#    使用任意静态服务器 | Use any static server
python -m http.server 8000
#    然后打开 http://localhost:8000
#    Then open http://localhost:8000
```

---

## 📁 项目结构 | Project Structure

```
FanqieRankTracker/
├── .github/workflows/
│   └── scrape.yml              # GitHub Actions 自动化工作流
├── css/
│   └── style.css               # 暗色编辑风格主题样式
├── js/
│   └── app.js                  # 前端渲染逻辑（瀑布流 + 打字机动画）
├── scripts/
│   └── build_latest.py         # 趋势对比 + AI 分析构建脚本
├── data/
│   ├── fanqie_female_new_ranks_YYYYMMDD.json  # 每日原始快照
│   ├── latest_ranks.json       # 最新聚合数据（看板数据源）
│   └── trends/
│       └── YYYY-MM-DD.json     # 趋势归档
├── index.html                  # 仪表盘入口页
├── scrape_fanqie_ranks.py      # 番茄小说爬虫（Playwright）
├── requirements.txt            # Python 依赖
└── README.md                   # 本文件
```

---

## ⚙️ 工作流程 | How It Works

```
┌─────────────────────────────────────────────────────────────┐
│                   GitHub Actions (每日 08:00)                │
│                                                             │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│  │  Playwright   │───▶│  build_latest │───▶│  git commit  │  │
│  │  爬取榜单数据  │    │  趋势对比      │    │  自动提交     │  │
│  │              │    │  + AI 分析     │    │  到 main     │  │
│  └──────────────┘    └──────────────┘    └──────────────┘  │
│                                                             │
└────────────────────────────┬────────────────────────────────┘
                             │
                             ▼
                    GitHub Pages 自动部署
                    用户访问在线看板 🌐
```

---

## 📝 常见问题 | FAQ

<details>
<summary><b>Q: Workflow 运行失败怎么办？| What if the workflow fails?</b></summary>

检查 Actions 日志中的错误信息。常见原因：
- 番茄小说页面结构变更 → 需要更新爬虫选择器
- Playwright 安装超时 → 尝试重新运行

> Check the error message in the Actions log. Common causes:
> - Fanqie Novel page structure changed → Update the scraper selectors
> - Playwright installation timeout → Try re-running the workflow

</details>

<details>
<summary><b>Q: 不配置 AI Secret 也能用吗？| Can I use it without AI secrets?</b></summary>

可以！系统会自动 fallback 到基于规则的摘要（如"新增3本上榜；《XX》排名上升+5位"）。只是没有 AI 自然语言分析而已。

> Yes! The system will automatically fall back to rule-based summaries (e.g., "3 new entries; Book X rose +5 ranks"). You just won't have the AI natural language analysis.

</details>

<details>
<summary><b>Q: 可以换成男频或其他榜单吗？| Can I track other rankings?</b></summary>

可以，修改 `scrape_fanqie_ranks.py` 中的 `init_url` 变量，将 URL 改为目标榜单的地址即可。

> Yes, modify the `init_url` variable in `scrape_fanqie_ranks.py` to point to the desired ranking page URL.

</details>

---

## 📜 License

MIT

---

<p align="center">
  <sub>Made with ☕ and 🤖 — 数据每日自动更新，无需手动维护</sub>
  <br>
  <sub>Data updates daily via automation — zero manual maintenance required</sub>
</p>
