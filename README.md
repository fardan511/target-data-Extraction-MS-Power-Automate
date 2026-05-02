# 🎯 Target.com Review Scraper — Power Automate Desktop

An automated desktop flow built with **Microsoft Power Automate Desktop** that scrapes product reviews, ratings, and recommendation percentages from [Target.com](https://www.target.com/) using DPCI codes, and writes the structured data into an Excel report.

---

## 📌 What This Project Does

Given a list of **DPCI product codes** in an Excel file, this automation:

1. Opens Target.com in Chrome
2. Searches each product by its DPCI code
3. Navigates to the reviews section
4. Extracts:
   - ⭐ Average product rating
   - 💬 Individual review text
   - 📅 Review date (relative & formatted)
   - 👍 Recommendation percentage
5. Writes all extracted data into a structured Excel output file
6. Handles unavailable/out-of-stock products gracefully (marks as `Item Not Available`)

---

## 🖥️ Demo

> 📹 Watch the full automation in action:

[![Demo Video](https://img.shields.io/badge/Watch-Demo%20Video-blue?style=for-the-badge&logo=youtube)](./demo/demo-video.mp4)

---

## 🔁 Flow Architecture

### Main Flow

| Step | Action |
|------|--------|
| 1–2 | Launch Excel & open DPCI Codes Test + DPCI Codes files |
| 3 | Read all cells from worksheet → `ExcelData` |
| 4 | Close Excel instance |
| 5 | Open Reviews_Target.xlsx (output file) |
| 6 | Set `FirstRow = 2` (skip header) |
| 7 | Get current date/time |
| 8 | Launch Chrome → navigate to Target.com |
| 9 | Wait 3 seconds |
| 10 | Run **Subflow_1** for each DPCI item |

### Subflow_1 — Per Product Logic

- Iterates over each `CurrentItem` in `ExcelData`
- Searches product on Target.com using DPCI
- **If** product not found → logs `Item Not Available` and skips
- **If** found → waits for rating element to load, clicks review section
- Extracts `RecommendationPercent` and `ProductAvgRating`
- Loops through individual reviews, comparing dates against a threshold
- Writes each valid review row to Excel with: DPCI, Rating, Review Text, Date, Avg Rating, Recommendation %

---

## 📊 Sample Output

| DPCI | Total Rating | Period | Extraction Date | Reviews | Total Reviews | Individual Ratings |
|------|-------------|--------|----------------|---------|--------------|-------------------|
| 064-15-7032 | 4.5 | 11 days ago | 30/04/2026 | I want to acknowledge... | 4.5 out of 5 stars | 3 out of 5 stars |
| 064-03-0021 | 4.6 | 4 days ago | 30/04/2026 | They are soft and feels great | 4.6 out of 5 stars | 5 out of 5 stars |
| 064-03-0023 | 4.6 | 2 days ago | 30/04/2026 | Not the quality I thought... | 4.6 out of 5 stars | 2 out of 5 stars |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| Microsoft Power Automate Desktop | Core automation engine |
| Microsoft Excel (.xlsx) | Input (DPCI codes) & Output (reviews data) |
| Google Chrome | Web browser for Target.com |
| JavaScript (inline) | Date parsing inside PAD JS action |
| Target.com | Data source |

---

## ⚙️ Setup & Requirements

- Windows 10/11
- Microsoft Power Automate Desktop (free with Windows)
- Microsoft Excel
- Google Chrome

---

## 📸 Screenshots 

### Input Data
![Main Flow](screenshots/input-data.jpeg)

### Main Flow
![Main Flow](screenshots/01-main-flow.png)

### Product Search & Availability Check
![Subflow Search](screenshots/02-subflow-search.png)

### Review Data Extraction
![Data Extraction](screenshots/03-data-extraction.png)

### Date Filtering & Excel Write
![Date Condition](screenshots/04-date-condition.jpeg)

---

## 🚧 Known Limitations

- Target.com UI changes may break UI element selectors
- Requires stable internet connection
- Review pagination handled via `NextPage` variable logic
- Hardcoded file paths (update before use)

---

## 👤 Author

**Fardan**
> Built as a personal automation project to streamline product review tracking from Target.com

---

## 📄 License

This project is for educational and personal use only. Scraping websites may violate their Terms of Service — use responsibly.
