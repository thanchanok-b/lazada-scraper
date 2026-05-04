# 🛍️ Lazada Multi-Scraper (Playwright)

> **Version:** 2.0  
> **Last Updated:** 2026-05-04  
> **Platform:** Lazada Thailand  
> **Runtime:** Jupyter Notebook / VS Code Notebook  
> **Browser Engine:** Playwright Persistent Browser Context

A Python-based scraper for collecting Lazada Thailand data using **Playwright**.  
This project supports product listing scraping, product detail scraping, review scraping, and keyword search scraping.

Version 2 improves the project structure, checkpoint system, logging system, and resume workflow to reduce data loss and avoid Excel checkpoint freeze issues.

---

## 🚀 Main Features

This project includes four scraping workflows:

1. **Product by Shop Scraper**  
   Scrapes product listings from Lazada shop pages.

2. **Product Detail Scraper**  
   Scrapes product specifications, qualification information, product descriptions, and description images.

3. **Review Scraper**  
   Scrapes customer reviews from Lazada product review API.

4. **Search Keyword Scraper**  
   Scrapes product listings from Lazada search result pages by keyword.

---

## ⚙️ Setup and Installation

### 1. Prerequisites

- Python 3.8+
- Jupyter Lab or VS Code with Jupyter extension
- Playwright
- Chromium installed by Playwright

---

### 2. Install Required Libraries

You can install libraries directly:

```bash
pip install playwright pandas nest-asyncio openpyxl
playwright install chromium
```

Or install from `requirements.txt`:

```bash
pip install -r requirements.txt
playwright install chromium
```

Recommended `requirements.txt`:

```txt
pandas>=2.1.4
openpyxl>=3.1.0
playwright>=1.55.0
nest-asyncio>=1.5.0
```

---

## 🔐 Browser Session Setup

This project uses **Playwright Persistent Browser Context** to keep browser session, cookies, and login status.

The session folder is:

```text
my_session/
```

### Create or Refresh Lazada Session

In the notebook, set:

```python
CREATE_SESSION = True
```

Then run the session setup cell.

A browser window will open. Login to Lazada manually if required. After login is complete, close the browser window.

After creating the session, set it back to:

```python
CREATE_SESSION = False
```

> **Important:**  
> Do not upload `my_session/` to GitHub because it may contain cookies or login session data.

---

## 🛠️ Configuration

Main configuration variables:

```python
USER_DATA_DIR = "./my_session"

OUTPUT_DIR = "./collected_data"
TEMP_DIR = "./collected_data/temp"

LOG_DIR = "./log"
PRODUCT_LOG_DIR = "./log/product"
DETAIL_LOG_DIR = "./log/detail"
REVIEW_LOG_DIR = "./log/review"
SEARCH_LOG_DIR = "./log/search"

REQUEST_DELAY_MIN = 2.0
REQUEST_DELAY_MAX = 3.0

CREATE_SESSION = False

PRODUCT_CHECKPOINT_EVERY_PAGES = 10
DETAIL_CHECKPOINT_EVERY_URLS = 10
REVIEW_CHECKPOINT_EVERY_PAGES = 20
SEARCH_CHECKPOINT_EVERY_PAGES = 50

RESUME_FROM_BACKUP = True
```

| Variable | Description |
| :--- | :--- |
| `USER_DATA_DIR` | Browser session folder |
| `OUTPUT_DIR` | Final output folder |
| `TEMP_DIR` | Temp checkpoint folder |
| `LOG_DIR` | Main log folder |
| `PRODUCT_LOG_DIR` | Product by Shop log folder |
| `DETAIL_LOG_DIR` | Product Detail log folder |
| `REVIEW_LOG_DIR` | Review log folder |
| `SEARCH_LOG_DIR` | Search Keyword log folder |
| `REQUEST_DELAY_MIN` | Minimum random delay between requests |
| `REQUEST_DELAY_MAX` | Maximum random delay between requests |
| `CREATE_SESSION` | Set to `True` only when creating or refreshing session |
| `RESUME_FROM_BACKUP` | Load existing temp files and skip duplicated data |

---

## 🗂️ Recommended Project Structure

```text
lazada_scraper/
│
├── README.md
├── requirements.txt
├── .gitignore
├── lazada_scraper_playwright.ipynb
│
├── collected_data/
│   │
│   ├── lazada_product_YYYYMMDD_HHMMSS.xlsx
│   ├── lazada_detail_YYYYMMDD_HHMMSS.xlsx
│   ├── lazada_review_YYYYMMDD_HHMMSS.xlsx
│   └── lazada_search_YYYYMMDD_HHMMSS.xlsx
│   │
│   └── temp/
│       ├── temp_lazada_products.csv
│       ├── temp_lazada_details.csv
│       ├── temp_lazada_reviews.csv
│       └── temp_lazada_search.csv
│
├── log/
│   │
│   ├── product/
│   │   ├── product_scraper.log
│   │   └── product_error.jsonl
│   │
│   ├── detail/
│   │   ├── product_detail_scraper.log
│   │   ├── product_detail_error.jsonl
│   │   └── debug_empty_detail_*.html
│   │
│   ├── review/
│   │   ├── review_scraper.log
│   │   └── review_error.jsonl
│   │
│   └── search/
│       ├── search_keyword_scraper.log
│       └── search_keyword_error.jsonl
│
└── my_session/
```

---

## 🚀 Usage

The project is designed to run in **Jupyter Notebook** or **VS Code Notebook**.

---

### 1. 🏬 Product by Shop Scraper

**Objective:**  
Scrape product listings from Lazada shop pages.

`shop_key` comes from the Lazada shop URL.

Example:

```text
https://www.lazada.co.th/shop/mizumi-bomi/  →  mizumi-bomi
```

You can input either shop keys or full shop URLs:

```python
SHOP_URL_KEYS = [
    "mizumi-bomi",
    "ing-on-official",
    "https://www.lazada.co.th/shop/mizumi-bomi/",
]
```

Run:

```python
df_products = await run_product_by_shop(SHOP_URL_KEYS)
```

Final output:

```text
collected_data/lazada_product_YYYYMMDD_HHMMSS.xlsx
```

Temp checkpoint:

```text
collected_data/temp/temp_lazada_products.csv
```

Log files:

```text
log/product/product_scraper.log
log/product/product_error.jsonl
```

---

### 2. 📦 Product Detail Scraper

**Objective:**  
Scrape product specifications, qualification information, product description, and description images.

Edit product detail URLs:

```python
DETAIL_PRODUCT_URLS = [
    "https://www.lazada.co.th/products/pdp-i5010918982-s21176047921.html",
    "https://www.lazada.co.th/products/pdp-i4599133168-s18943395331.html",
]
```

Run:

```python
df_details = await run_product_detail(DETAIL_PRODUCT_URLS)
```

Final output:

```text
collected_data/lazada_detail_YYYYMMDD_HHMMSS.xlsx
```

Temp checkpoint:

```text
collected_data/temp/temp_lazada_details.csv
```

Log files:

```text
log/detail/product_detail_scraper.log
log/detail/product_detail_error.jsonl
log/detail/debug_empty_detail_*.html
```

---

### 3. 💬 Review Scraper

**Objective:**  
Scrape customer reviews from Lazada product review API.

Edit product URLs:

```python
REVIEW_PRODUCT_URLS = [
    "https://www.lazada.co.th/products/pdp-i5592245338.html",
    "https://www.lazada.co.th/products/pdp-i5592124928.html",
]
```

Run:

```python
df_reviews = await run_reviews(REVIEW_PRODUCT_URLS)
```

Final output:

```text
collected_data/lazada_review_YYYYMMDD_HHMMSS.xlsx
```

Temp checkpoint:

```text
collected_data/temp/temp_lazada_reviews.csv
```

Log files:

```text
log/review/review_scraper.log
log/review/review_error.jsonl
```

---

### 4. 🔎 Search Keyword Scraper

**Objective:**  
Scrape product listings from Lazada search results by keyword.

Edit keywords:

```python
SEARCH_KEYWORDS = [
    "ครีมกันแดด",
    "มาสก์หน้า",
    "รองพื้น",
    "อายไลเนอร์",
]
```

Run normally:

```python
df_search = await run_search_keyword(
    SEARCH_KEYWORDS,
    official_only=False
)
```

Run only a page range for error recovery:

```python
df_search = await run_search_keyword(
    ["อายไลเนอร์"],
    official_only=False,
    start_page_by_keyword={"อายไลเนอร์": 59},
    end_page_by_keyword={"อายไลเนอร์": 102}
)
```

Final output:

```text
collected_data/lazada_search_YYYYMMDD_HHMMSS.xlsx
```

Temp checkpoint:

```text
collected_data/temp/temp_lazada_search.csv
```

Log files:

```text
log/search/search_keyword_scraper.log
log/search/search_keyword_error.jsonl
```

---

## 🔁 Resume and Checkpoint Behavior

Version 2 uses CSV temp checkpoint files.

If `RESUME_FROM_BACKUP = True`, the scraper will load existing temp files and skip duplicated rows using dedupe keys.

| Workflow | Temp File | Resume Logic |
| :--- | :--- | :--- |
| Product by Shop | `temp_lazada_products.csv` | Skip duplicated `product_id + sku_id + product_url` |
| Product Detail | `temp_lazada_details.csv` | Skip duplicated `product_id + product_url` |
| Review | `temp_lazada_reviews.csv` | Skip duplicated `product_id + user_name + review_date_raw + comment_text` |
| Search Keyword | `temp_lazada_search.csv` | Skip duplicated `search_keyword + product_id + sku_id + product_url` |

---

## 🧾 Logging System

Version 2 separates logs by workflow.

| Workflow | General Log | Error Log |
| :--- | :--- | :--- |
| Product by Shop | `log/product/product_scraper.log` | `log/product/product_error.jsonl` |
| Product Detail | `log/detail/product_detail_scraper.log` | `log/detail/product_detail_error.jsonl` |
| Review | `log/review/review_scraper.log` | `log/review/review_error.jsonl` |
| Search Keyword | `log/search/search_keyword_scraper.log` | `log/search/search_keyword_error.jsonl` |

### Example `.log` Record

```text
2026-05-04 13:18:12 | INFO | search_keyword | Search keyword checkpoint saved | {"keyword": "ครีมกันแดด", "page_no": 5, "start_page": 1, "end_page": 5, "collected_rows": 200}
```

### Example `.jsonl` Error Record

```json
{"logged_at": "2026-05-04 13:20:00", "scraper_name": "search_keyword", "level": "ERROR", "message": "Page check timeout", "keyword": "อายไลเนอร์", "page_no": 59}
```

---

## 📊 Output Files

| Output File | Description |
| :--- | :--- |
| `lazada_product_*.xlsx` | Product listings scraped from Lazada shop pages |
| `lazada_detail_*.xlsx` | Product detail data scraped from product pages |
| `lazada_review_*.xlsx` | Customer reviews scraped from Lazada review API |
| `lazada_search_*.xlsx` | Product listings scraped from Lazada search keywords |

---

# 📊 Data Dictionary

## 1. Product by Shop Data

| Column Name | Description |
| :--- | :--- |
| `product_id` | Lazada product ID |
| `sku_id` | SKU ID |
| `product_name` | Product name |
| `brand_name` | Brand name |
| `categories` | Product category IDs from Lazada |
| `shop_name` | Shop name |
| `seller_id` | Seller ID |
| `shop_key` | Shop key extracted from Lazada shop URL |
| `location` | Product shipping location |
| `current_price` | Current selling price |
| `original_price` | Original price before discount |
| `sold_count` | Parsed number of sold items |
| `sold_count_raw` | Original sold count text from Lazada |
| `rating_score` | Average product rating |
| `review_count` | Number of product reviews |
| `in_stock` | Product stock status |
| `is_sponsored` | Whether product is sponsored |
| `product_url` | Product URL |
| `image_url` | Product image URL |
| `source_platform` | Source platform, usually `Lazada` |
| `collected_at` | Timestamp of data collection |

---

## 2. Product Detail Data

| Column Name | Description |
| :--- | :--- |
| `product_id` | Lazada product ID |
| `product_name` | Product name |
| `shop_name` | Shop name |
| `product_url` | Product URL |
| `qualification_info` | Raw qualification information in JSON format |
| `all_specs` | Raw product specification information in JSON format |
| `description` | Product description text |
| `description_images` | Product description image URLs in JSON format |
| `source_platform` | Source platform, usually `Lazada` |
| `collected_at` | Timestamp of data collection |

---

## 3. Review Data

| Column Name | Description |
| :--- | :--- |
| `shop_id` | Lazada seller/shop ID |
| `product_id` | Lazada product ID |
| `shop_name` | Shop name |
| `product_name` | Product name |
| `user_name` | Reviewer username |
| `rating_score` | Rating given by customer |
| `review_date_raw` | Original review date text from Lazada |
| `review_date` | Converted review date in `YYYY-MM-DD` format |
| `product_option` | Product variation purchased by customer |
| `comment_text` | Customer review text |
| `source_platform` | Source platform, usually `Lazada` |
| `product_url` | Product URL |
| `collected_at` | Timestamp of data collection |

---

## 4. Search Keyword Data

| Column Name | Description |
| :--- | :--- |
| `product_id` | Lazada product ID |
| `sku_id` | SKU ID |
| `product_name` | Product name |
| `brand_name` | Brand name |
| `discount_price` | Current selling price from search results |
| `original_price` | Original price before discount |
| `sold_count` | Parsed number of sold items |
| `sold_count_raw` | Original sold count text from Lazada |
| `rating_score` | Average product rating |
| `review_count` | Number of reviews |
| `shop_name` | Shop name |
| `seller_id` | Seller ID |
| `is_mall` | Whether product is detected as LazMall |
| `location` | Product shipping location |
| `is_sponsored` | Whether product is sponsored |
| `product_url` | Product URL |
| `image_url` | Product image URL |
| `search_keyword` | Search keyword used |
| `source_platform` | Source platform, usually `Lazada` |
| `collected_at` | Timestamp of data collection |

---

## 🧩 Notes on JSON Columns

Some columns are stored as JSON strings to preserve the original Lazada structure.

| Column | Description |
| :--- | :--- |
| `qualification_info` | Raw qualification information from product detail page |
| `all_specs` | Raw product specification data |
| `description_images` | List of product description image URLs |

Example:

```json
{
  "License Type": "TH_FDA_Advertising",
  "License Code": ""
}
```

These raw JSON columns are useful for later reprocessing if additional fields are needed.

---

## ⚠️ Important Notes

- The scraper uses Playwright Persistent Browser Context to keep browser sessions.
- The scraper does not bypass CAPTCHA, verification, login, or security checks.
- If Lazada shows a verification page, the user must complete it manually in the browser.
- `shop_key` is available only in Product by Shop output because it is extracted from shop URLs.
- Product Detail, Review, and Search Keyword URLs normally do not contain `shop_key`.
- Temp checkpoint files are saved as CSV to reduce freezing issues from large Excel files.
- Final output files are saved as Excel for easier analysis.
- Review date conversion is approximate for relative dates such as `1 เดือนที่แล้ว` or `2 ปีที่แล้ว`.
- Lazada page structure and API responses may change over time.

---
