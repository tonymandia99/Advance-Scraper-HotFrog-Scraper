# Hotfrog Multithreaded Scraper

## Overview
This is a robust, multithreaded web scraper designed to extract business information (name, phone, address) from Hotfrog listings using Python, Selenium (undetected-chromedriver), and proxy rotation. It is built for stability, speed, and reliability on VPS environments.

---

## Features

### Core Scraping
- Scrapes business name, phone number, and address from multiple start URLs.
- Multithreaded scraping with configurable max threads (default 3).
- Avoids duplicate entries using thread-safe data structures.

### Proxy Management
- Uses a configurable list of HTTP proxies.
- Measures proxy latency with fast `requests.get()` calls.
- Filters out proxies with latency over 3000 ms.
- Rotates proxies after every 5 pages or on failure.
- Tries all proxies before skipping a problematic page.
- Logs proxy latency and usage timestamps for easy debugging.
- Prioritizes fastest proxies.

### Stability & Efficiency
- Headless Chrome with undetected-chromedriver to avoid detection.
- Disables images, CSS, and fonts to speed up loading.
- Smart throttling and reduced sleep times to improve scraping speed and stability.
- Refreshes pages automatically on Google vignette (interstitial ads).
- Retries pages up to 5 times before skipping.
- Keeps ChromeDriver instances open between pages for efficiency.
- Prevents “Too many open files” errors by properly closing browsers and collecting garbage.

### Data Persistence
- Saves scraped data in CSV format with headers.
- Auto-saves every 50 entries.
- Uses locks for thread-safe writes.
- Tracks last scraped page URLs per start URL in checkpoint files.
- On restart or crash, resumes scraping from last saved pages without data loss or duplication.

### Logging & Monitoring
- Logs all important events with timestamps and tags like `[INFO]`, `[ERROR]`, `[PROXY]`, `[DATA]`, `[NAV]`.
- Saves logs to `scraper.log` for debugging.
- Real-time log monitoring supported (`tail -f scraper.log`).

### Auto-Restart & Resource Management
- Automatically restarts scraping every 3 hours or after N pages.
- Saves checkpoints and data before restart.
- Cleanly closes all browsers on restart.
- Resumes scraping seamlessly after restart.
- Limits max concurrent threads to avoid overloading VPS.
- Compatible with VPS specs like 6 vCPU, 12 GB RAM, and NVMe or SSD storage.

---

## Requirements
- Python 3.8+
- Selenium-related packages: `undetected-chromedriver`
- `requests` library
- Chrome/Chromium installed on VPS

---

## Setup & Running

1. Install dependencies:
   ```bash
   pip install undetected-chromedriver requests selenium
