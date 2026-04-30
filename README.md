# 🛍️ Lazada Multi-Scraper (Playwright)

> **Last Updated:** 2026-04-30

A Python-based tool for scraping product listings, product details, customer reviews, and keyword search results from **Lazada Thailand** using **Playwright**.

This project is designed for market research, product monitoring, competitor analysis, customer review analytics, and e-commerce data collection workflows.

Unlike CDP-based scrapers, this project uses **Playwright Persistent Browser Context** to store browser sessions, cookies, and login state in a local session folder.

---

## 🚀 Main Features

This repository consists of four main scraping workflows:

1. **Product by Shop Scraper**  
   Extracts product listings from Lazada shop pages.

2. **Product Detail Scraper**  
   Extracts product specifications, qualification information, descriptions, and description images from product detail pages.

3. **Review Scraper**  
   Extracts customer reviews from Lazada product review API.

4. **Search Keyword Scraper**  
   Extracts product listings from Lazada search results based on keywords.

---

## ⚙️ Setup and Installation

### 1. Prerequisites

- Python 3.8+
- Jupyter Lab or VS Code with Jupyter extension
- Playwright
- Chromium browser installed by Playwright

---

### 2. Install Python Libraries

Install the required libraries using pip:

```bash
pip install playwright pandas nest-asyncio openpyxl
playwright install chromium
```

---

## 🔐 Browser Session Setup

This scraper uses a persistent browser session to store cookies and login status.

The browser session is saved in:

```text
my_session/
```

This allows the scraper to reuse login sessions without logging in every time.

---

### Create Lazada Session

In the notebook, set:

```python
CREATE_SESSION = True
```

Then run the session setup cell.

A browser window will open. Login to Lazada manually if required. After login is complete, close the browser window.

After the session is created, change back to:

```python
CREATE_SESSION = False
```

> **Important:**  
> Do not upload the `my_session/` folder to GitHub because it may contain cookies or login session data.

---

## 🛠️ Configuration

Main configuration variables:

```python
USER_DATA_DIR = "./my_session"
OUTPUT_DIR = "./collected_data"

REQUEST_DELAY_MIN = 2.0
REQUEST_DELAY_MAX = 3.0

CREATE_SESSION = False
```

| Variable | Description |
| :--- | :--- |
| `USER_DATA_DIR` | Folder used to store browser session and cookies |
| `OUTPUT_DIR` | Folder used to save exported Excel files and debug files |
| `REQUEST_DELAY_MIN` | Minimum random delay between requests |
| `REQUEST_DELAY_MAX` | Maximum random delay between requests |
| `CREATE_SESSION` | Set to `True` only when creating or refreshing Lazada login session |

---

## 🚀 Usage

Since the scripts are provided as **Jupyter Notebooks (.ipynb)**, it is recommended to run them in **VS Code** or **Jupyter Lab**.

---

### 1. 🏬 Product by Shop Scraper

**Objective:**  
Scrape all product listings from Lazada shop pages.

Edit the shop keys:

```python
SHOP_URL_KEYS = [
    "mizumi-bomi",
    "ing-on-official",
]
```

Run:

```python
df_products = await run_product_by_shop(SHOP_URL_KEYS)
```

Output example:

```text
collected_data/lazada_products_YYYYMMDD_HHMMSS.xlsx
```

---

### 2. 📦 Product Detail Scraper

**Objective:**  
Scrape product detail data from Lazada product pages.

Edit product detail URLs:

```python
DETAIL_PRODUCT_URLS = [
    "https://www.lazada.co.th/products/pdp-i5010918982-s21176047921.html",
    "https://www.lazada.co.th/products/pdp-i5430857467-s23060039894.html",
]
```

Run:

```python
df_details = await run_product_detail(DETAIL_PRODUCT_URLS)
```

Output example:

```text
collected_data/lazada_details_YYYYMMDD_HHMMSS.xlsx
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

Output example:

```text
collected_data/lazada_reviews_YYYYMMDD_HHMMSS.xlsx
```

---

### 4. 🔎 Search Keyword Scraper

**Objective:**  
Scrape product listings from Lazada search results by keyword.

Edit keywords:

```python
SEARCH_KEYWORDS = [
    "sunscreen",
    "lipstick",
]
```

Run:

```python
df_search = await run_search_keyword(
    SEARCH_KEYWORDS,
    max_pages=5,
    official_only=False
)
```

Output example:

```text
collected_data/lazada_search_YYYYMMDD_HHMMSS.xlsx
```

---

## 🗂️ Recommended Repository Structure

```text
lazada-scraper/
│
├── README.md
├── .gitignore
├── requirements.txt
│
├── lazada_scraper_playwright.ipynb
│
├── collected_data/          # ignored by git
│   ├── lazada_products_YYYYMMDD_HHMMSS.xlsx
│   ├── lazada_details_YYYYMMDD_HHMMSS.xlsx
│   ├── lazada_reviews_YYYYMMDD_HHMMSS.xlsx
│   ├── lazada_search_YYYYMMDD_HHMMSS.xlsx
│
└── my_session/              # ignored by git
    └── browser session files
```

---

## 📁 Repository Files

| Filename | Description |
| :--- | :--- |
| `README.md` | Project documentation |
| `.gitignore` | Specifies files and folders excluded from GitHub |
| `requirements.txt` | Python package dependencies |
| `lazada_scraper_playwright.ipynb` | Main Jupyter Notebook for Lazada scraping workflows |

---

## 📊 Output Files

| Output File | Description |
| :--- | :--- |
| `lazada_products_*.xlsx` | Product listings scraped from Lazada shop pages |
| `lazada_details_*.xlsx` | Product detail data scraped from product pages |
| `lazada_reviews_*.xlsx` | Customer reviews scraped from Lazada review API |
| `lazada_search_*.xlsx` | Product listings scraped from Lazada search keywords |

---

# 📊 Data Dictionary

## 1. Product by Shop Data

This file contains product listing data scraped from Lazada shop pages.

| Column Name | Description |
| :--- | :--- |
| `product_id` | Lazada product ID |
| `sku_id` | SKU ID |
| `product_name` | Product name |
| `brand_name` | Brand name |
| `categories` | Product category IDs from Lazada |
| `shop_name` | Shop name |
| `seller_id` | Seller ID |
| `shop_key` | Shop key used in Lazada URL |
| `location` | Product shipping location |
| `discount_price` | Current selling price after discount |
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

This file contains detailed information scraped from product detail pages.

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
| `collected_at` | Timestamp of data collection |

---

## 3. Review Data

This file contains customer reviews scraped from Lazada product review API.

| Column Name | Description |
| :--- | :--- |
| `shop_id` | Shop ID |
| `product_id` | Lazada product ID |
| `shop_name` | Shop name |
| `product_name` | Product name |
| `user_name` | Reviewer's username |
| `rating_score` | Rating given by the customer |
| `review_time_raw` | Original review date text from Lazada |
| `review_date` | Converted review date in `YYYY-MM-DD` format |
| `product_option` | Product variation purchased by customer |
| `comment_text` | Customer review text |
| `source_platform` | Source platform, usually `Lazada` |
| `product_url` | Product URL |
| `collected_at` | Timestamp of data collection |

---

## 4. Search Keyword Data

This file contains product listing data scraped from Lazada search results.

| Column Name | Description |
| :--- | :--- |
| `product_id` | Lazada product ID |
| `sku_id` | SKU ID |
| `search_keyword` | Search keyword used |
| `product_name` | Product name |
| `brand_name` | Brand name |
| `discount_price` | Current selling price after discount |
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
| `source_platform` | Source platform, usually `Lazada` |
| `collected_at` | Timestamp of data collection |

---

## 🧩 Notes on JSON Columns

Some columns are stored as JSON strings to preserve the original structure from Lazada.

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

- The scraper uses Playwright persistent browser context to keep login sessions.
- The scraper does not bypass CAPTCHA, verification, login, or security checks.
- If Lazada shows a verification page, the user must complete it manually in the browser.
- Some fields may be empty depending on Lazada API response.
- Product detail data may require scrolling before content is loaded.
- Review date conversion is approximate for relative dates such as `1 เดือนที่แล้ว` or `2 ปีที่แล้ว`.
- The structure of Lazada pages and APIs may change over time.

---

## ✅ Recommended `.gitignore`

Create a `.gitignore` file and add:

```gitignore
# Python
__pycache__/
*.pyc
*.pyo
*.pyd
.ipynb_checkpoints/

# Virtual environments
.env
.venv/
venv/
env/

# Browser session
my_session/
playwright_session/

# Output files
collected_data/
outputs/
*.xlsx
*.csv
*.json
*.html

# OS files
.DS_Store
Thumbs.db
```

---

## 📦 Recommended `requirements.txt`

Create a `requirements.txt` file and add:

```text
pandas
openpyxl
playwright
nest-asyncio
```

---

## 📌 Recommended Upload Files

Upload these files to GitHub:

```text
README.md
.gitignore
requirements.txt
lazada_scraper_playwright.ipynb
```

Do not upload:

```text
my_session/
collected_data/
*.xlsx
*.csv
*.json
*.html
.env
```

---

## ⚖️ Disclaimer

This project is intended for educational, analytical, and internal research purposes only.

Users should comply with Lazada’s terms of service, applicable platform policies, and relevant data privacy regulations.

This scraper does not bypass login, CAPTCHA, verification, or other security systems.

---

