# Walmart 价格跟踪器

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://bright.cn)
[![Walmart Price Tracker](https://img.shields.io/badge/Walmart%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://bright.cn/products/insights/price-tracker/walmart)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://bright.cn/products/insights/price-tracker/walmart)

实时 Walmart 价格跟踪——面向全球最大的零售连锁之一。两种入门方式：**全托管**情报平台，或用于构建您自有 pipeline 的**自助式 API**。

---

## 选项 1：Bright Insights - AI 驱动的价格跟踪（推荐）

**[Bright Insights](https://bright.cn/products/insights/price-tracker/walmart)** 是 Bright Data 的全托管零售情报平台。无需构建 scraper，无需维护基础设施——只需将结构化、可直接用于分析的价格数据交付到仪表板、数据源或您的 BI 工具中。

**团队选择 Bright Insights 的原因：**
- 🚀 **零配置** - 使用开箱即用的仪表板和数据源，几分钟内即可上线
- 🤖 **AI 驱动的推荐** - 对话式 AI 助手可将数百万数据点即时转化为可执行洞察
- ⚡ **实时监控** - 支持每小时到每天的刷新频率，并提供即时告警（email、Slack、webhook）
- 🌍 **无限扩展** - 任意网站、任意地区、任意刷新频率
- 🔗 **即插即用集成** - AWS、GCP、Databricks、Snowflake 等
- 🛡️ **全托管** - Bright Data 自动处理 schema 变更、站点更新和数据质量问题

**关键用例：**
- ✅ **监控 Walmart 价格**，覆盖所有产品类别
- ✅ **实时跟踪库存水平和可用性**
- ✅ **为您关注的产品设置价格提醒**
- ✅ 监控 MAP 政策合规性并检测定价违规
- ✅ 跟踪竞争对手促销及促销动态
- ✅ 将干净、统一的数据直接输入动态定价算法或 AI 模型

> **每月 $250 起 - [获取定制报价 →](https://bright.cn/products/insights/price-tracker/walmart)**

---

## 选项 2：通过 Web Scraper API 自助使用

更希望构建您自己的 pipeline？Bright Data 的 **Web Scraper API** 为您提供对 Walmart 产品数据的编程访问——包括价格、可用性、评论等——无需管理代理或抓取基础设施。

### 前置条件

- Python 3.9 或更高版本
- 一个 [Bright Data account](https://bright.cn)（提供免费试用）
- 一个 Bright Data **API token**（[如何获取](https://docs.bright.cn/general/account/account-settings#api-token)）
- 一个用于 Walmart 产品的 Bright Data **Web Scraper ID**（[在 Web Scrapers 控制面板中找到您的 ID](https://bright.cn/cp/scrapers)）

### 设置

1. **克隆此 repository**

   ```bash
   git clone https://github.com/bright-cn/walmart-price-tracker.git
   cd walmart-price-tracker
   ```

2. **安装依赖**

   ```bash
   pip install -r requirements.txt
   ```

3. **配置凭据**

   将 `.env.example` 复制为 `.env` 并填写您的值：

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **查找您的 Web Scraper ID**
   > 登录 [Bright Data Control Panel](https://bright.cn/cp/scrapers)，进入 **Web Scrapers**，
   > 搜索 “Walmart”，然后复制 Web Scraper ID（格式：`gd_xxxxxxxxxxxx`）。

---

## 用法

### 1. 按 URL 跟踪特定产品

传入 Walmart 产品 URL 列表以获取结构化价格数据：

```python
from price_tracker import track_prices

urls = [
    "https://www.walmart.com/ip/sample-product/123456789",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

或直接运行：

```bash
python price_tracker.py
```

### 2. 按关键词发现产品

查找与关键词搜索匹配的产品：

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. 按分类 URL 浏览产品

从 Walmart 分类页面采集所有产品：

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://walmart.com/category/example",
    limit=100,
)
```

---

## 输出字段

每条结果记录包含以下字段：

| Field | Description |
|-------|-------------|
| `url` | 产品页面 URL |
| `title` | 产品名称 / 标题 |
| `brand` | 品牌或制造商 |
| `initial_price` | 原价 / 标价 |
| `final_price` | 当前售价 |
| `currency` | 货币代码（例如 USD、EUR） |
| `discount` | 折扣金额或百分比 |
| `in_stock` | 商品是否可用 |
| `rating` | 平均星级评分 |
| `reviews_count` | 评论总数 |
| `seller_name` | 卖家名称 |
| `images` | 产品图片 URL 数组 |
| `description` | 产品描述文本 |
| `timestamp` | 数据采集时间戳 |

### 示例输出

```json
[
  {
    "url": "https://www.walmart.com/ip/sample-product/123456789",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://walmart.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高级选项

`trigger_collection()` 函数接受可选参数来控制数据采集：

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返回记录的最大数量 |
| `include_errors` | boolean | `true` | 在结果中包含错误报告 |
| `notify` | string (URL) | - | 当快照准备就绪时调用的 webhook URL |
| `format` | string | `json` | 输出格式：`json`、`csv` 或 `ndjson` |

选项示例：

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.walmart.com/ip/sample-product/123456789"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## 资源

- 🌟 [Walmart Price Tracker - Bright Insights (Managed)](https://bright.cn/products/insights/price-tracker/walmart)
- 🔧 [Walmart Scraper API](https://bright.cn/products/web-scraper/walmart)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.bright.cn/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://bright.cn/cp/scrapers)
- 🔑 [How to get an API token](https://docs.bright.cn/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://bright.cn)

---

*由 [Bright Data](https://bright.cn) 构建 - 行业领先的网络数据平台。*