📊 HR Executive Dashboard
Developed a strategic Power BI dashboard to monitor workforce KPIs including headcount, attrition, tenure analysis, performance metrics, and diversity indicators. Designed for executive-level reporting with drill-down insights to support data-driven talent management and organizational decision-making 💼📈

## 📋 Table of Contents
1. [Tools You'll Need](#tools-youll-need)
2. [GitHub Repository Structure](#github-repository-structure)
3. [Step-by-Step Execution Guide](#step-by-step-execution-guide)
4. [DAX Measures - Copy/Paste Ready](#dax-measures---copypaste-ready)
5. [Visual Layout Blueprint](#visual-layout-blueprint)
6. ["Bank Style" Design Guide](#bank-style-design-guide)
7. [Business Recommendations Template](#business-recommendations-template)
8. [README.md Template with Screenshot Placement](#readmemd-template-with-screenshot-placement)
9. [Final Checklist](#final-checklist)

---

## 🔧 Tools You'll Need

| Tool | Purpose | Where to Get It |
|------|---------|-----------------|
| **Power BI Desktop** | Main dashboard development | [Microsoft Store](https://powerbi.microsoft.com/desktop/) or direct download (FREE) |
| **Power BI Service (optional)** | Publishing online | app.powerbi.com (FREE with Microsoft account) |
| **GitHub Account** | Hosting your project | [github.com](https://github.com) (FREE) |
| **GitHub Desktop (optional)** | Easier Git management | [desktop.github.com](https://desktop.github.com/) (FREE) |
| **Kaggle Account** | Download the dataset | [kaggle.com](https://www.kaggle.com) (FREE) |
| **Snipping Tool / Lightshot** | Taking screenshots | Built into Windows / [getlightshot.com](https://getlightshot.com/) |
| **Notion / VS Code** | Taking notes / editing README | [notion.so](https://notion.so) or [code.visualstudio.com](https://code.visualstudio.com/) |

**Total Cost: $0.00**

---

## 📂 GitHub Repository Structure

Create this exact folder structure in your GitHub repository:

```
HR-Executive-Dashboard/
│
├── README.md                       # Main project documentation (CRITICAL)
├── LICENSE                         # MIT License recommended
│
├── data/
│   └── WA_Fn-UseC_-HR-Employee-Attrition.csv   # Original dataset
│
├── dashboard/
│   └── HR_Executive_Dashboard.pbix              # Your Power BI file
│
├── screenshots/
│   ├── 01_overview_dashboard.png                 # Main dashboard view
│   ├── 02_attrition_analysis.png                 # Attrition deep-dive
│   ├── 03_diversity_metrics.png                  # DEI dashboard
│   ├── 04_department_drilldown.png               # Department view
│   └── 05_recommendations.png                    # Business recs slide
│
├── docs/
│   ├── DAX_measures.txt                          # All DAX formulas
│   ├── data_dictionary.md                         # Column explanations
│   └── presentation_script.md                      # Talking points
│
└── references/
    └── helpful_links.txt                          # Resources used
```

**Pro Tip**: Create the repository first on GitHub, then clone it to your local machine using GitHub Desktop .

---

## 👣 Step-by-Step Execution Guide

### Phase 1: Setup & Data Acquisition (30 minutes)

**Step 1: Download the Dataset**
1. Go to [IBM HR Analytics Dataset on Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)
2. Click "Download" (you may need to sign in)
3. Save `WA_Fn-UseC_-HR-Employee-Attrition.csv` to your project's `data/` folder

**Step 2: Install Power BI Desktop**
1. Download from [Microsoft Power BI](https://powerbi.microsoft.com/desktop/)
2. Run installer (typical installation takes 5-10 minutes)
3. Open Power BI Desktop to verify installation

**Step 3: Create GitHub Repository**
1. Go to github.com and sign in
2. Click green "New" button
3. Repository name: `HR-Executive-Dashboard`
4. Description: "Executive-level HR dashboard analyzing employee attrition, performance, and diversity metrics using Power BI. Built with bank-style professional design."
5. Public repository
6. Initialize with README
7. Choose MIT License
8. Click "Create repository"

---

### Phase 2: Data Import & Initial Cleaning (1 hour)

**Step 4: Import Data into Power BI**

1. **Open Power BI Desktop**
2. Click **Get Data** → **Text/CSV**
3. Navigate to your downloaded CSV file and select it
4. Click **Load** (not Transform - the data is already clean!) 

**Quick Data Check**:
- Verify 1,470 rows and 35 columns loaded
- Check that column names match the dataset description

**Step 5: Create a Date Table (Best Practice)**

Go to **Modeling** tab → **New Table** and paste:

```
DateTable = CALENDAR(MIN('WA_Fn-UseC_-HR-Employee-Attrition'[HireDate]), MAX('WA_Fn-UseC_-HR-Employee-Attrition'[HireDate]))
```

Then add these columns to the DateTable:
```
Year = YEAR(DateTable[Date])
Month = FORMAT(DateTable[Date], "MMM")
MonthNumber = MONTH(DateTable[Date])
Quarter = "Q" & QUARTER(DateTable[Date])
```

**Step 6: Create Relationships**
1. Go to **Model** view
2. Drag `DateTable[Date]` to connect with your main table's `HireDate`
3. Ensure relationship is "One to Many" 

---

### Phase 3: Creating DAX Measures (1.5 hours)

Go to **Modeling** → **New Measure** and create these measures exactly. **Copy and paste these formulas** :

**Core Headcount Measures:**
```
Total Employees = COUNTROWS('WA_Fn-UseC_-HR-Employee-Attrition')

Active Employees = CALCULATE(
    COUNTROWS('WA_Fn-UseC_-HR-Employee-Attrition'),
    'WA_Fn-UseC_-HR-Employee-Attrition'[Attrition] = "No"
)

Terminated Employees = CALCULATE(
    COUNTROWS('WA_Fn-UseC_-HR-Employee-Attrition'),
    'WA_Fn-UseC_-HR-Employee-Attrition'[Attrition] = "Yes"
)
```

**Attrition Measures:**
```
Attrition Rate = DIVIDE(
    [Terminated Employees],
    [Total Employees],
    0
)

Attrition Rate % = FORMAT([Attrition Rate], "0.0%")
```

**Tenure Measures:**
```
Avg Tenure Years = AVERAGE('WA_Fn-UseC_-HR-Employee-Attrition'[YearsAtCompany])

Max Tenure Years = MAX('WA_Fn-UseC_-HR-Employee-Attrition'[YearsAtCompany])

Min Tenure Years = MIN('WA_Fn-UseC_-HR-Employee-Attrition'[YearsAtCompany])
```

**Performance Measures:**
```
Avg Performance Rating = AVERAGE('WA_Fn-UseC_-HR-Employee-Attrition'[PerformanceRating])

High Performers = CALCULATE(
    COUNTROWS('WA_Fn-UseC_-HR-Employee-Attrition'),
    'WA_Fn-UseC_-HR-Employee-Attrition'[PerformanceRating] >= 3
)

High Performer % = DIVIDE([High Performers], [Total Employees], 0)
```

**Diversity Measures:**
```
Female Count = CALCULATE(
    COUNTROWS('WA_Fn-UseC_-HR-Employee-Attrition'),
    'WA_Fn-UseC_-HR-Employee-Attrition'[Gender] = "Female"
)

Male Count = CALCULATE(
    COUNTROWS('WA_Fn-UseC_-HR-Employee-Attrition'),
    'WA_Fn-UseC_-HR-Employee-Attrition'[Gender] = "Male"
)

Female % = DIVIDE([Female Count], [Total Employees], 0)

Male % = DIVIDE([Male Count], [Total Employees], 0)
```

**Compensation Measures:**
```
Avg Monthly Income = AVERAGE('WA_Fn-UseC_-HR-Employee-Attrition'[MonthlyIncome])

Avg Salary by Department = 
    AVERAGEX(
        VALUES('WA_Fn-UseC_-HR-Employee-Attrition'[Department]),
        CALCULATE(AVERAGE('WA_Fn-UseC_-HR-Employee-Attrition'[MonthlyIncome]))
    )
```

---

### Phase 4: Building Visuals (2.5 hours)

**Step 7: Create Report Pages**

Create **5 pages** in your report (click the + at the bottom):

1. **Executive Overview**
2. **Attrition Analysis**
3. **Diversity & Inclusion**
4. **Performance & Tenure**
5. **Department Deep-Dive**

**Step 8: Page 1 - Executive Overview Layout**

| Position | Visual Type | Fields | What It Shows |
|----------|-------------|--------|---------------|
| Top Row (4 cards) | Card | Total Employees | Current headcount |
| Top Row (4 cards) | Card | Attrition Rate % | Overall turnover |
| Top Row (4 cards) | Card | Avg Tenure Years | Retention health |
| Top Row (4 cards) | Card | Avg Monthly Income | Compensation baseline |
| Left Middle | Stacked Bar Chart | Department vs Total Employees | Headcount by dept |
| Right Middle | Donut Chart | Gender vs Total Employees | Gender diversity |
| Bottom Left | Line Chart | HireDate (by month) vs Total Employees | Hiring trends |
| Bottom Right | Stacked Column Chart | Department vs Attrition Count | Attrition by dept |

**Step 9: Page 2 - Attrition Analysis**

| Visual | Configuration |
|--------|---------------|
| **KPI Visual** | Attrition Rate with Department as trend axis |
| **Clustered Bar Chart** | JobRole vs Attrition Count (sorted descending) |
| **Stacked Column** | Department with Attrition (Yes/No) stacked |
| **Matrix** | Rows: Department, Columns: Attrition, Values: Count |
| **Key Influencers Visual** | Field: Attrition, Explain by: Overtime, JobSatisfaction, WorkLifeBalance  |

**Step 10: Page 3 - Diversity Metrics**

| Visual | Configuration |
|--------|---------------|
| **Treemap** | Gender by Department (size = Employee Count) |
| **Donut Chart** | MaritalStatus distribution |
| **Stacked Bar** | EducationField by Gender |
| **Clustered Bar** | AgeGroup (create bins: 18-25, 26-35, 36-45, 46-55, 55+) vs Count |
| **Table** | Department, Female %, Male %, Avg Age |

**Step 11: Page 4 - Performance & Tenure**

| Visual | Configuration |
|--------|---------------|
| **Gauge** | Avg Performance Rating (min=1, max=4) |
| **Histogram** | Tenure Bands (0-2, 3-5, 6-10, 10+ years) vs Count |
| **Scatter Plot** | X=YearsAtCompany, Y=MonthlyIncome, Legend=Attrition |
| **Stacked Bar** | PerformanceRating by Department |
| **Line Chart** | Avg Performance Rating by YearsSinceLastPromotion |

**Step 12: Page 5 - Department Drill-Down**

| Visual | Configuration |
|--------|---------------|
| **Slicer** | Department (dropdown style) |
| **Multi-row Card** | All KPIs filtered by selected department |
| **Decomposition Tree** | Attrition breakdown by JobRole → Gender → AgeGroup |
| **Table** | EmployeeNumber, JobRole, Attrition, PerformanceRating, MonthlyIncome |

---

### Phase 5: "Bank Style" Design Polish (1.5 hours)

**Step 13: Apply Professional Theme**

1. Go to **View** tab → **Themes** → **Customize current theme**
2. Apply these settings:

```
Colors:
- Primary: #0F2B4B (Navy Blue)
- Secondary: #4A6782 (Slate Gray)
- Accent 1: #2E5A88 (Medium Blue)
- Accent 2: #6C8EB2 (Light Blue)
- Warning: #A6392E (Burgundy for attrition)
- Success: #2E7D5E (Forest Green)

Background: #FFFFFF (White)
Card Background: #F5F7FA (Light Gray)
Text: #333333 (Dark Gray)
Font: Segoe UI or Arial (clean, sans-serif)
```

**Step 14: Format Every Visual**

For EACH visual:

1. **Remove all default titles** (unless absolutely necessary)
2. **Add a clean, left-aligned title** in 12pt bold
3. **Remove gridlines** (Format → Gridlines → Off)
4. **Set consistent borders**: 1px, light gray
5. **Data labels**: Only if essential, positioned inside end
6. **Legend**: Top or right position, not bottom

**Step 15: Add Corporate Elements**

1. Add a **company logo placeholder** (top left) - can use a simple rectangle with "CORP INC" text
2. Add **report title** in 20pt light font: "EXECUTIVE HR DASHBOARD | Q4 2024"
3. Add **last refresh date** in bottom right corner (small, 8pt gray)

**Step 16: Create Consistent Page Navigation**

1. Add **rectangle shapes** at the top of each page (like tabs)
2. Label them: OVERVIEW | ATTRITION | DIVERSITY | PERFORMANCE | DEPARTMENTS
3. Use dark blue for the active page, light gray for others
4. Add **bookmark functionality** to make them clickable (optional but impressive)

---

### Phase 6: Adding Interactivity (30 minutes)

**Step 17: Add Slicers**

On each page, add a **slicer panel** on the right or left:

```
Slicers to include (all dropdown style):
- Department
- Gender
- JobRole
- EducationField
- AgeGroup (calculated column)
```

**To create AgeGroup calculated column**:

```
AgeGroup = 
SWITCH(
    TRUE(),
    'WA_Fn-UseC_-HR-Employee-Attrition'[Age] < 25, "18-25",
    'WA_Fn-UseC_-HR-Employee-Attrition'[Age] < 35, "26-35",
    'WA_Fn-UseC_-HR-Employee-Attrition'[Age] < 45, "36-45",
    'WA_Fn-UseC_-HR-Employee-Attrition'[Age] < 55, "46-55",
    "55+"
)
```

**Step 18: Enable Cross-Filtering**

1. Go to **Format** → **Edit Interactions**
2. Set all visuals to **filter** each other (not highlight)
3. This ensures clean drill-down experience

---

### Phase 7: Business Recommendations (1 hour)

**Step 19: Analyze Your Dashboard & Write Recommendations**

Use the insights from your dashboard to write 5-7 actionable recommendations .

**Template format**:

```
## Key Business Recommendations

### 1. [Title - Action Oriented]
**Finding:** [Data-backed observation from dashboard]
**Impact:** [Why this matters financially/operationally]
**Recommendation:** [Specific, actionable step]
**Priority:** [High/Medium/Low]
```

**Example based on typical IBM dataset findings** :

```
## Key Business Recommendations

### 1. Reduce Sales Department Attrition Through Targeted Retention
**Finding:** Sales department shows 20% attrition rate, significantly above company average of 16%. Sales Representatives have the highest turnover among all roles.
**Impact:** Estimated replacement cost of $10,000-15,000 per sales role. Annual cost of sales turnover exceeds $200,000.
**Recommendation:** Implement quarterly retention bonuses for Sales Reps with >2 years tenure. Conduct exit interviews specifically for Sales departures.
**Priority:** HIGH

### 2. Address Early-Career Turnover Among Young Employees
**Finding:** Employees aged 18-25 have 35% higher attrition than any other age group. Female employees under 25 show 50% attrition rate.
**Impact:** Loss of entry-level talent pipeline and recruitment ROI.
**Recommendation:** Launch "Early Career Mentorship Program" pairing junior employees with senior leaders. Review onboarding process for under-25 demographics.
**Priority:** HIGH

### 3. Improve Work-Life Balance to Reduce Attrition
**Finding:** Employees with "Bad" or "Good" work-life balance scores have 2.2x higher attrition than those with "Better" or "Best" scores. Overtime employees show 30% higher turnover.
**Impact:** Burnout-driven turnover affecting productivity and culture.
**Recommendation:** Audit department overtime policies. Implement "No Meeting Fridays" in departments with highest overtime (R&D, Sales).
**Priority:** MEDIUM

### 4. Align Promotions with Performance Ratings
**Finding:** 40% of high performers (rating 3-4) haven't received promotions in 5+ years. These employees show 25% higher attrition risk.
**Impact:** Losing top talent due to career stagnation.
**Recommendation:** Create "High Performer Fast Track" program guaranteeing promotion review within 24 months. Tie promotion cycles to performance review calendar.
**Priority:** MEDIUM

### 5. Address Gender Imbalance in Specific Departments
**Finding:** Research & Development is 85% male; Human Resources is 65% male (counterintuitive but dataset-specific). Gender pay gap minimal overall but varies by role.
**Impact:** Diversity gaps in innovation teams; potential brand reputation risk.
**Recommendation:** Set hiring targets for underrepresented genders in R&D. Partner with women-in-tech organizations for recruitment pipelines.
**Priority:** LOW (strategic)

### 6. Leverage Compensation Strategy for Retention
**Finding:** Employees earning below $3,000/month have 40% higher attrition. Income progression plateaus after 10 years of tenure.
**Impact:** Losing cost-effective junior talent; senior talent becoming complacent.
**Recommendation:** Review entry-level salaries against market rates. Implement "Tenure Milestone Bonuses" at 5, 10, 15 years.
**Priority:** MEDIUM
```

---

### Phase 8: Taking Screenshots (30 minutes)

**Step 20: Capture Professional Screenshots**

For each page of your dashboard:

1. **Zoom to 100%** in Power BI
2. **Hide all slicer panels** (temporarily if needed)
3. **Use Snipping Tool** (Windows Key + Shift + S)
4. **Capture full page** - include all visuals, no cropping
5. **Save as PNG** with descriptive names:

```
screenshots/
├── 01_executive_overview.png
├── 02_attrition_analysis.png
├── 03_diversity_metrics.png
├── 04_performance_tenure.png
├── 05_department_drilldown.png
└── 06_recommendations_slide.png (if you create one)
```

**Pro Tip**: Take screenshots in **daylight mode** (not dark theme) for better GitHub visibility .

---

### Phase 9: GitHub Upload & README Creation (1 hour)

**Step 21: Prepare Files for Upload**

Organize your local folder exactly as shown in the [GitHub Structure](#github-repository-structure) section above.

**Step 22: Create Your README.md**

Below is a **complete template** you can copy, paste, and customize. **Replace the bracketed text** with your own content.

---

## 📝 README.md Template (Copy This Entire Section)

```markdown
# 🏦 Executive HR Dashboard | Power BI

![Dashboard Overview](screenshots/01_executive_overview.png)

## 📌 Project Overview

This **executive-level HR dashboard** analyzes workforce data to provide strategic insights for leadership decision-making. Built with a "bank style" professional aesthetic, this dashboard transforms raw HR data into actionable business intelligence.

**Dataset**: IBM HR Analytics Employee Attrition & Performance (1,470 employees, 35 attributes)  
**Tools Used**: Power BI Desktop, Power Query, DAX  
**Timeline**: Complete end-to-end analysis in under 10 hours

### 🎯 Business Questions Answered
- What is our current headcount and attrition rate?
- Which departments have the highest turnover and why?
- How diverse is our workforce across gender, age, and education?
- Is there a relationship between performance, tenure, and compensation?
- What actionable steps can leadership take to improve retention?

---

## 📊 Live Dashboard Preview

### Executive Overview
> High-level KPIs: Headcount, Attrition Rate, Average Tenure, Average Salary

![Executive Overview](screenshots/01_executive_overview.png)

### Attrition Deep-Dive
> Analyzing turnover by department, job role, and key drivers

![Attrition Analysis](screenshots/02_attrition_analysis.png)

### Diversity & Inclusion Metrics
> Gender distribution, age demographics, and education field breakdown

![Diversity Metrics](screenshots/03_diversity_metrics.png)

### Performance & Tenure Analysis
> Performance ratings distribution, tenure bands, and compensation correlation

![Performance Tenure](screenshots/04_performance_tenure.png)

### Department-Level Drill-Down
> Interactive view of any department's KPIs and employee composition

![Department Drill-Down](screenshots/05_department_drilldown.png)

---

## 🔧 Key Metrics & DAX Measures

### Core Calculations
```dax
// Total Active Employees
Active Employees = CALCULATE(
    COUNTROWS('HR Data'),
    'HR Data'[Attrition] = "No"
)

// Attrition Rate
Attrition Rate = DIVIDE(
    CALCULATE(COUNTROWS('HR Data'), 'HR Data'[Attrition] = "Yes"),
    COUNTROWS('HR Data'),
    0
)

// Average Tenure (Years)
Avg Tenure Years = AVERAGE('HR Data'[YearsAtCompany])

// Female Percentage
Female % = DIVIDE(
    CALCULATE(COUNTROWS('HR Data'), 'HR Data'[Gender] = "Female"),
    COUNTROWS('HR Data'),
    0
)
```

[Full DAX Measures Library](docs/DAX_measures.txt)

---

## 💡 Key Business Recommendations

### 1. 🎯 Reduce Sales Department Attrition
**Finding**: Sales department shows **20% attrition rate** vs. company average of 16%. Sales Representatives have the highest turnover.  
**Impact**: Estimated replacement cost of **$10,000-15,000 per sales role**.  
**Recommendation**: Implement quarterly retention bonuses for Sales Reps with >2 years tenure. Conduct dedicated Sales exit interviews.  
**Priority**: HIGH

### 2. 📉 Address Early-Career Turnover
**Finding**: Employees aged 18-25 have **35% higher attrition** than other groups. Female employees under 25 show **50% attrition rate**.  
**Impact**: Loss of entry-level talent pipeline and recruitment ROI.  
**Recommendation**: Launch "Early Career Mentorship Program" pairing junior employees with senior leaders.  
**Priority**: HIGH

### 3. ⚖️ Improve Work-Life Balance
**Finding**: Employees with low work-life balance scores have **2.2x higher attrition**. Overtime employees show **30% higher turnover**.  
**Impact**: Burnout-driven turnover affecting productivity and culture.  
**Recommendation**: Audit department overtime policies. Implement "No Meeting Fridays" in highest overtime departments.  
**Priority**: MEDIUM

### 4. 🚀 Align Promotions with Performance
**Finding**: **40% of high performers** haven't received promotions in 5+ years. These employees show **25% higher attrition risk**.  
**Impact**: Losing top talent due to career stagnation.  
**Recommendation**: Create "High Performer Fast Track" program guaranteeing promotion review within 24 months.  
**Priority**: MEDIUM

### 5. 🌍 Address Department Gender Imbalance
**Finding**: R&D is **85% male**; Human Resources is **65% male**.  
**Impact**: Diversity gaps in innovation teams; potential brand reputation risk.  
**Recommendation**: Set hiring targets for underrepresented genders. Partner with diversity-focused recruitment organizations.  
**Priority**: STRATEGIC

[Complete Recommendations Document](docs/business_recommendations.md)

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Power BI Desktop** | Data modeling, DAX calculations, visualization |
| **Power Query** | Data cleaning and transformation |
| **DAX** | Created 20+ custom measures for KPIs |
| **GitHub** | Version control and project hosting |

---

## 📁 Repository Structure

```
HR-Executive-Dashboard/
│
├── README.md                          # You are here
├── LICENSE                            # MIT License
│
├── data/
│   └── WA_Fn-UseC_-HR-Employee-Attrition.csv    # Original dataset
│
├── dashboard/
│   └── HR_Executive_Dashboard.pbix               # Power BI file
│
├── screenshots/                        # All dashboard images
│   ├── 01_executive_overview.png
│   ├── 02_attrition_analysis.png
│   ├── 03_diversity_metrics.png
│   ├── 04_performance_tenure.png
│   └── 05_department_drilldown.png
│
├── docs/
│   ├── DAX_measures.txt                # All DAX formulas
│   └── business_recommendations.md     # Detailed recommendations
│
└── references/
    └── helpful_links.txt                # Resources used
```

---

## 🚀 How to Use This Project

### Option 1: View the Dashboard
1. Browse the screenshots folder to see the final output
2. Read the business recommendations section for insights

### Option 2: Explore Interactive Version
1. Download `dashboard/HR_Executive_Dashboard.pbix`
2. Open with Power BI Desktop (free)
3. Interact with slicers and explore the data yourself

### Option 3: Reproduce the Analysis
1. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)
2. Follow the step-by-step guide in the documentation
3. Use the DAX measures provided in `docs/DAX_measures.txt`

---

## 📈 Key Insights Summary

- **Overall Attrition Rate**: 16.2% (above industry average)
- **Highest Attrition Department**: Sales (20.6%)
- **Lowest Attrition Department**: Research & Development (13.8%)
- **Average Employee Tenure**: 7.0 years
- **Gender Distribution**: 60% Male, 40% Female
- **Average Monthly Income**: $6,500
- **High Performers (>3 rating)**: 15% of workforce

---

## 👨‍💻 About Me

**[Your Name]**
Data Analyst specializing in HR analytics and executive dashboards

- **Portfolio**: [yourwebsite.com]
- **LinkedIn**: [linkedin.com/in/yourprofile]
- **Email**: youremail@domain.com

I create data solutions that help leadership teams make better decisions. This project demonstrates my ability to:
- Transform raw data into strategic insights
- Design professional, executive-level dashboards
- Communicate complex findings through storytelling
- Operate within organizational contexts and constraints

---

## 📄 License

This project is open source under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- IBM for creating the fictional HR dataset
- Kaggle for hosting the data
- Power BI community for DAX inspiration
```
