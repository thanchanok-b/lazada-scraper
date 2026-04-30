# Lazada Scraper

A Python-based Lazada scraper for collecting product listings, product details, and customer reviews from Lazada Thailand.  
This project is designed for research, market monitoring, product analysis, and review analytics workflows.

---

## Features

This scraper currently supports:

- Scrape products by Lazada shop URL key
- Scrape product detail pages
- Scrape customer reviews from product review API
- Scrape products by search keyword
- Export results to Excel
- Save failed logs for debugging
- Save debug HTML when product detail extraction fails
- Use persistent browser session for manual login
- Convert Lazada review dates into standard `YYYY-MM-DD` format
- Parse sold count such as `1.2K`, `25.7K`, and Thai text formats
- Extract product price, original price, discount amount, coupon promotion, rating, review count, and product URLs

---

## Project Structure

```text
lazada-scraper/
│
├── notebooks/
│   └── lazada_scraper.ipynb
│
├── outputs/
│   ├── lazada_products_YYYYMMDD_HHMMSS.xlsx
│   ├── lazada_details_YYYYMMDD_HHMMSS.xlsx
│   ├── lazada_reviews_YYYYMMDD_HHMMSS.xlsx
│   ├── lazada_search_YYYYMMDD_HHMMSS.xlsx
│   └── failed_logs_YYYYMMDD_HHMMSS.xlsx
│
├── my_session/
│   └── browser session files
│
├── README.md
└── .gitignore
