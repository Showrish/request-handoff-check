# 📋 Request Handoff Check

> **Where do hiring requests get stuck between approval and iCIMS?** An Excel mini project that reconciles two hiring systems, times every approval step and turns the findings into a dashboard. Built on synthetic data.

![Excel](https://img.shields.io/badge/Excel-365-217346?style=flat-square&logo=microsoftexcel&logoColor=white)
![Focus](https://img.shields.io/badge/Focus-HR%20Operations-1F3864?style=flat-square)
![Data](https://img.shields.io/badge/Data-Synthetic-7F7F7F?style=flat-square)

---

## ⚡ At a Glance

| ⏱️ **7.4 days** | 🔁 **6.0 days** | 📅 **56.1 days** | 🔍 **7 fields** |
|:---:|:---:|:---:|:---:|
| Average Finance approval, nearly 3x the other steps | Average wait from final approval to iCIMS | Average time from final approval to accepted offer | Fields that don't match between the two systems |

![Dashboard](screenshots/dashboard.png)

---

## ❓ The Problem

Hiring managers submit job requests on an internal request site. Each one goes through three approvals: Dept Head, Finance, then HR Business Partner. Once it's approved, someone retypes it into iCIMS by hand to post the job.

Manual handoffs like that lose time and data, so this project asks three questions:

1. **Did every approved request make it into iCIMS?**
2. **Does the data still match once it gets there?**
3. **Where does the time go?**

---

## 🧭 How I Approached It

| Step | What I did |
|---|---|
| **1. Reconcile** | Cleaned the request IDs, then matched 81 requests against 56 iCIMS jobs in both directions |
| **2. Check the fields** | Compared job title, hiring manager and department for every matched pair |
| **3. Time the process** | Measured days in each approval step, the handoff to iCIMS and the time to fill |
| **4. Summarize** | Built KPI cells, a chart and a PivotTable with a recruiter slicer |

---

## 🔍 What I Found

### Where time gets lost
- **Finance is the bottleneck.** Its decided approvals average 7.4 days, nearly 3x the 2.6 days of the other two steps, and 3 of its 7 pending approvals have waited 2+ weeks.
- **The handoff costs about 6 days.** That's how long approved requests sit before someone opens them in iCIMS.
- **Time to fill averages 56.1 days,** from final approval to accepted offer.

### What falls through the cracks
- **3 approved requests never reached iCIMS.** Five looked missing at first, but one was there under a typo'd ID and one was a duplicate.
- **3 more were approved within the last week,** which is normal lag, not a gap.
- **2 jobs skipped the request site entirely,** one with a blank reference and one marked "N/A."
- **1 job was opened before its approval finished,** while the HR Business Partner step was still pending.

### Where the data stops matching
- **7 fields disagree between the systems:** 4 job titles, 2 hiring managers and 1 department. Some are just formatting, like "Sr." vs "Senior." Others are real errors, like a request filed under the wrong hiring manager and a Marketing role filed under Events.

---

## 🧹 Data Issues I Caught

| Issue | How it showed up | How I handled it |
|---|---|---|
| Hidden spaces in 4 request IDs, including a non-breaking space | Valid jobs failed to match their requests | TRIM plus SUBSTITUTE for CHAR(160) |
| A letter O instead of a zero in one ID | A job looked like it had no request | Traced it back by matching title and hiring manager |
| The same request submitted twice, 2 days apart | One role approved twice | COUNTIFS on manager and title within 7 days |
| 3 dates stored as text | Broke date math | DATEVALUE |
| Trailing spaces in department names | One department split into two pivot rows | TRIM before pivoting |
| Different department names in each system | "IT" on one side, "Information Technology" on the other | A Dept_Map lookup table |
| 3 missing locations | Blank fields on the request | Flagged with COUNTBLANK |

---

## 💡 What I'd Recommend

1. **Connect the two systems.** Sending approved requests straight into iCIMS removes the 6-day handoff and the retyping behind the ID and field errors.
2. **Put Finance on a clock.** A reminder at 7 days and an escalation at 14 would target the slowest step.
3. **Validate request IDs.** A picklist instead of free text stops typos and hidden spaces at the source.
4. **Catch duplicates at submission.** Warn when the same manager submits the same title within a week.
5. **Share one department list** across both systems, and make location a required field.

---

## 🗂️ What's Inside the Workbook

| Sheet | What it does |
|---|---|
| About | Project summary and findings |
| Dashboard | KPIs, approval step breakdown and chart, all driven by one as-of date |
| ReqSite_Export | Requests from the approval site, plus checks for iCIMS status, gaps and duplicates |
| Approvals_Log | Every approval step, who handled it and how many days it sat there |
| iCIMS_Export | Jobs from iCIMS, plus columns that clean IDs and match them back to requests |
| Dept_Map | Translates department names between the two systems |
| Pivot | Jobs by department and request status, with a recruiter slicer |

Blue headers are fields from the system exports. Green headers are checks I added.

![Pivot](screenshots/pivot.png)

---

## 🛠️ Excel Techniques Used

| Area | Functions and features |
|---|---|
| Matching | XLOOKUP, COUNTIF, COUNTIFS |
| Cleaning | TRIM, SUBSTITUTE with CHAR(160), DATEVALUE |
| Timing | AVERAGEIFS, COUNTIFS with date criteria |
| Live lists | UNIQUE, SORT, HSTACK |
| Reporting | KPI cells, PivotTable, slicer, chart |

---

## 📁 Files

| File | What it is |
|---|---|
| `Req_Handoff_Check.xlsx` | The finished analysis, with every check, the dashboard and the pivot |
| `data/Req_Handoff_Raw.xlsx` | The untouched synthetic exports, before any checks were added |
| `screenshots/` | Dashboard and pivot screenshots |

---

## 🚀 Try It Yourself

- **See the finished analysis:** download `Req_Handoff_Check.xlsx` and start on the About tab. GitHub can't preview Excel files, so it has to be downloaded.
- **Rebuild it from scratch:** start with `data/Req_Handoff_Raw.xlsx` and see how many issues you can catch.
- **Needs Excel 365,** since it uses XLOOKUP, UNIQUE and HSTACK. Every age calculation points at one fixed as-of date, 9/30/2026, so the numbers never drift.

---

## ⚠️ About the Data

- **It's synthetic.** All names, requests and dates are made up, modeled on a typical request-to-iCIMS hiring process.
- **The issues are realistic on purpose.** The raw exports include hidden spaces, a typo'd ID, a duplicate and dates stored as text, put there to practice catching them.

---

## 👤 Author

**Showrish Reddy Sampath** · MPS Applied Machine Intelligence, Northeastern University · [LinkedIn](https://linkedin.com/in/showrish-reddy-sampath-b508a22a6)
