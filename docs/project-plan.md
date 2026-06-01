# Statistical Evidence of Reliability in Reusable Rocket Systems

## Team Members

- **Lucas Gabriel Rocha Ferreira**
- **Gabriel**
- **Kaio**
- **Izac**
- **Jonatas**

---

## 1. Project Overview

### Research Question

Does rocket reusability affect mission reliability?

### Hypothesis

Reusable boosters maintain or improve reliability over multiple launches, demonstrating that rocket reuse is a viable strategy for reducing costs without compromising mission success.

### Main Objective

Analyze SpaceX launch data to determine whether reusable rocket systems maintain high reliability throughout multiple launches.

---

## 2. Data Source

### Primary Source

**SpaceX API v4**

#### Documentation:
- [SpaceX API Documentation](https://docs.spacexdata.com/)
- [SpaceX API Repository](https://github.com/r-spacex/SpaceX-API)

### Reason for Selection

The SpaceX API provides structured information regarding:
- Launches
- Rockets
- Booster cores
- Payloads
- Mission outcomes
- Reuse information

This enables the team to investigate the relationship between booster reuse and launch reliability using real-world aerospace data.

### 2.1 References

- [SpaceX API Documentation](https://docs.spacexdata.com/)
- [SpaceX API Repository](https://github.com/r-spacex/SpaceX-API)
- [Python Documentation](https://docs.python.org/)
- [Pandas Documentation](https://pandas.pydata.org/)
- [Matplotlib Documentation](https://matplotlib.org/)

---

## 3. Research Scope

### Main Question

Does rocket reusability affect reliability?

### Supporting Questions

| Member | Research Question |
|--------|-------------------|
| Gabriel | How has SpaceX reliability evolved over time? |
| Kaio | Do reused boosters perform as reliably as new boosters? |
| Izac | Does booster experience improve reliability? |
| Jonatas | Which rocket family achieved the highest reliability? |

---

## 4. Expected Variables

The final processed dataset should contain at least the following variables:

| Variável | Descrição |
|----------|-----------|
| `launch_year` | Ano extraído da data de lançamento |
| `launch_name` | Nome oficial da missão |
| `flight_number` | Número de voo sequencial atribuído pela SpaceX |
| `success` | Indica se o lançamento foi bem-sucedido |
| `rocket_name` | Nome do foguete utilizado na missão |
| `core_id` | Identificador exclusivo do núcleo do propulsor |
| `reuse_count` | Número de vezes que o propulsor foi reutilizado antes da missão |
| `launch_date` | Data completa de lançamento (UTC) |
| `is_reused` | Boolean indicating whether the booster had been reused before the mission (`is_reused = reuse_count > 0`) |

### 4.1 Variable Ownership

| Variável | Principais Consumidores |
|----------|------------------------|
| `launch_year` | Gabriel |
| `launch_name` | Lucas |
| `flight_number` | Lucas |
| `success` | Gabriel, Kaio, Izac, Jonatas |
| `rocket_name` | Jonatas |
| `core_id` | Lucas, Kaio, Izac |
| `reuse_count`, `is_reused` | Kaio, Izac |
| `launch_date` | Gabriel |

---

## 5. Team Organization

### Lucas – Project Lead & Data Engineer

**Responsibilities:**
- API exploration
- Data extraction
- Data cleaning
- Dataset preparation
- Integration of analyses
- Final review
- Methodology section

**Deliverables:**
- Final processed dataset
- Data dictionary
- Master notebook
- Final conclusions

### Gabriel – Temporal Analysis

**Research Question:** How has SpaceX reliability evolved over time?

**Responsibilities:**
- Launches per year
- Success rate per year
- Reuse rate per year

**Expected Graphs:**
- Launches per Year
- Success Rate per Year
- Reuse Rate per Year

### Kaio – Reusability Analysis

**Research Question:** Do reused boosters perform as reliably as new boosters?

**Responsibilities:**
- Separate new and reused boosters
- Compare success rates
- Analyze failure distribution

**Expected Graphs:**
- Success Rate Comparison
- Booster Distribution

### Izac – Booster Experience Analysis

**Research Question:** Does booster experience improve reliability?

**Responsibilities:**
- Correlation analysis
- Experience analysis
- Trend identification

**Expected Graphs:**
- Scatter Plot
- Regression Trend

### Jonatas – Rocket Family Comparison

**Research Question:** Which rocket family achieved the highest reliability?

**Responsibilities:**
- Falcon 1 analysis
- Falcon 9 analysis
- Falcon Heavy analysis

**Expected Graphs:**
- Reliability by Rocket Family

### 5.1 Ownership Matrix

| Entrega | Responsável |
|---------|-------------|
| Coleta de Dados | Lucas |
| Limpeza de Dados | Lucas |
| Análise Temporal | Gabriel |
| Análise de Reutilização | Kaio |
| Análise de Experiência do Propulsor | Izac |
| Análise da Família de Foguetes | Jonatas |
| Integração do Notebook Mestre | Lucas |
| Criação de Pôster | Todos os Membros |
| Preparação da Apresentação | Todos os Membros |
| Revisão Final | Lucas |

---

## 6. GitHub Organization

**Repository Name:** `spacex-reliability-study`

### Repository Structure

```
README.md
data/
├── raw/
└── processed/
notebooks/
├── master_analysis.ipynb
├── lucas_data_cleaning.ipynb
├── gabriel_temporal_analysis.ipynb
├── kaio_reusability_analysis.ipynb
├── izac_booster_experience.ipynb
└── jonatas_rocket_comparison.ipynb
graphs/
poster/
report/
```

---

## 7. Branch Strategy

**Important:** Nobody commits directly to main.

Each member works on their own branch:

| Member | Branch |
|--------|--------|
| Lucas | `feature/data-cleaning` |
| Gabriel | `feature/temporal-analysis` |
| Kaio | `feature/reusability-analysis` |
| Izac | `feature/booster-experience` |
| Jonatas | `feature/rocket-comparison` |

---

## 8. Development Workflow

1. **Step 1:** Create branch
2. **Step 2:** Develop analysis
3. **Step 3:** Commit changes
4. **Step 4:** Open Pull Request
5. **Step 5:** Review
6. **Step 6:** Merge into main

---

## 9. Commit Standard

**Format:** `type: description`

**Examples:**
- `feat: add temporal analysis`
- `feat: create booster comparison chart`
- `fix: correct missing values`
- `docs: update methodology`

---

## 10. Dataset Governance

To guarantee consistency:
- Only **Lucas** creates the final processed dataset
- No team member may independently alter the processed dataset structure
- All analyses must use the same processed dataset version

### 10.1 Scope Freeze

After the dataset schema has been approved during the pre-analysis meeting, no new research questions, variables, or analytical objectives may be introduced without team approval.

**Purpose:**
- Prevent scope creep
- Reduce rework
- Maintain project consistency

---

## 11. Notebook Standard

Every notebook must follow the same structure:

1. Imports
2. Data Loading
3. Data Cleaning
4. Statistical Analysis
5. Visualizations
6. Conclusions

---

## 12. Visualization Standard

Every graph must include:
- Title
- Axis Labels
- Units (when applicable)
- Legend (when applicable)

All charts should maintain a consistent visual style.

---

## 13. Statistical Methods

### Descriptive Statistics

**Required:**
- Mean
- Median
- Standard Deviation
- Minimum
- Maximum
- Success Rate
- Correlation Coefficient (where applicable)

### Correlation Analysis

Required where applicable.

### Optional Inferential Statistics

If time allows:
- Chi-Square Test
- Two-Proportion Z-Test

**Goal:** Determine whether observed differences between new and reused boosters are statistically significant.

---

## 14. Risks and Mitigation

| Risk | Solution |
|------|----------|
| Different datasets being used | Single processed dataset maintained by Lucas |
| Different graph styles | Shared visualization standards |
| Last-minute integration issues | Each member must submit notebook, graphs (.png), and written interpretation before integration begins |

---

## 15. Project Timeline

### Phase 1 — Data Collection
- **Responsible:** Lucas
- **Duration:** 1 Day
- **Activities:** Explore API, Collect data, Prepare dataset

### Phase 2 — Individual Analysis
- **Responsible:** Gabriel, Kaio, Izac, Jonatas
- **Duration:** 3 Days
- **Activities:** Statistical analysis, Graph generation, Result interpretation

### Phase 3 — Integration
- **Responsible:** Lucas
- **Duration:** 1 Day
- **Activities:** Merge analyses, Validate results, Build master notebook

### Phase 4 — Poster Creation
- **Duration:** 1 Day
- **Distribution:**
  - Gabriel: Introduction
  - Kaio: Dataset Description
  - Izac: Statistical Analysis
  - Jonatas: Results
  - Lucas: Conclusion and Final Review

---

## 16. Poster Structure

1. **Introduction** – Background of SpaceX and reusable rockets
2. **Data Source** – Description of SpaceX API and dataset
3. **Methodology** – Data collection and statistical methods
4. **Results** – Main graphs and findings
5. **Conclusions** – Research conclusions and future implications

---

## 17. Deliverables Checklist

### Each Member Must Deliver:
- Notebook
- Statistical calculations
- Minimum of two visualizations
- Interpretation of results

### Final Team Deliverables:
- Google Colab Notebook
- Processed Dataset
- Statistical Analysis
- Poster
- Presentation
- GitHub Repository

---

## 18. Success Criteria

The project will be considered successful if:
- ✅ The research question is answered
- ✅ Statistical methods are correctly applied
- ✅ Results are supported by data
- ✅ Visualizations clearly communicate findings
- ✅ The final presentation demonstrates whether rocket reusability impacts reliability
- ✅ All team members contribute measurable work

---

## 19. Data Architecture and API Analysis

### Purpose

Before any statistical analysis begins, the team must fully understand the SpaceX API structure and define a common dataset schema.

This step is critical because inconsistent datasets can compromise the validity of the project's conclusions.

### Primary API Endpoints

The team will focus only on endpoints directly related to the research question.

#### Launches Endpoint
- **Purpose:** Provides information about each mission
- **Relevant Fields:**
  - Launch date
  - Mission name
  - Flight number
  - Success status
  - Rocket identifier
  - Booster core identifier
- **Research Applications:**
  - Launches per year
  - Success rate over time
  - Historical reliability analysis
- **Responsible:** Lucas and Gabriel

#### Cores Endpoint
- **Purpose:** Provides information about booster cores and their reuse history
- **Relevant Fields:**
  - Number of launches
  - Number of reuses
  - Landing information
  - Booster status
- **Research Applications:**
  - Reusability analysis
  - Booster experience analysis
  - Reliability comparison
- **Responsible:** Lucas, Kaio, and Izac

#### Rockets Endpoint
- **Purpose:** Provides information about rocket families
- **Relevant Fields:**
  - Rocket name
  - Rocket type
- **Research Applications:**
  - Falcon 1 analysis
  - Falcon 9 analysis
  - Falcon Heavy analysis
  - Reliability comparison between rocket families
- **Responsible:** Lucas and Jonatas

### Optional Endpoints

The following endpoints may be explored if additional analysis is desired:

- **Payloads** – Possible Question: Does payload mass affect mission success?
- **Launchpads** – Possible Question: Do launch locations affect mission reliability?

### Endpoints Excluded from Scope

The following endpoints will **not** be used because they do not directly contribute to answering the research question:
- Capsules
- Ships
- Landpads
- Crew

---

## 20. Data Architecture

The project dataset will be built using information from three primary entities:

```
Launches
│
├── success
├── date
├── flight_number
├── rocket_id
└── core_id
     │
     └─────────────────────┐
                           │
                           ▼
Cores (core_id)
├── reuse_count
├── launches
└── status

rocket_id
│
▼
Rockets
├── Falcon 1
├── Falcon 9
└── Falcon Heavy
```

### Data Integration Strategy

1. The **Launches** dataset will serve as the central source
2. Additional information will be joined from:
   - **Cores**
   - **Rockets**
3. Using their respective identifiers, this process will create a unified dataset for all statistical analyses

---

## 21. Final Dataset Schema

The team aims to generate a final dataset similar to the structure below:

| launch_year | launch_name | flight_number | success | rocket_name | core_id | reuse_count | launch_date |
|-------------|-------------|---------------|---------|-------------|---------|-------------|-------------|
| 2019 | Starlink-1 | 74 | True | Falcon 9 | B1048 | 2 | 2019-11-11 |
| 2020 | GPS III SV04 | 85 | True | Falcon 9 | B1060 | 0 | 2020-11-05 |
| 2021 | Transporter-1 | 96 | True | Falcon 9 | B1058 | 4 | 2021-01-24 |

Additional variables may be included if necessary.

### 21.1 Final Dataset Requirements

The final processed dataset must:
- Be stored in CSV format
- Be versioned through GitHub
- Contain all variables required by every analysis
- Use consistent data types
- Include documentation describing each variable

**Expected Output:** `processed_dataset.csv`

**Supporting Documentation:** `data_dictionary.md`

#### Data Dictionary Example

| Variable | Type | Description |
|----------|------|-------------|
| `reuse_count` | Integer | Number of times the booster was reused before the current mission |
| `success` | Boolean | Indicates whether the launch was successful |

---

## 22. Master Notebook Strategy

To simplify integration, the repository will contain a dedicated notebook: `master_analysis.ipynb`

**Purpose:**
- Load the final processed dataset
- Import visualizations produced by team members
- Consolidate statistical findings
- Generate final conclusions

**Important Rule:** The master notebook should **not** contain individual exploratory analyses. Each team member must maintain their own analysis notebook and contribute only finalized results to the master notebook.

### 22.1 Master Notebook Responsibilities

The `master_analysis.ipynb` notebook will serve exclusively as the final integration environment.

**Allowed Activities:**
- Load processed dataset
- Import final visualizations
- Present final statistics
- Present consolidated conclusions

**Not Allowed:**
- Exploratory analysis
- Data cleaning
- Experimental visualizations

**Purpose:** Ensure a clean and reproducible final analysis.

---

## 23. Pre-Analysis Meeting

Before implementation begins, the team must conduct a technical alignment meeting.

### Objectives:
- Review API documentation
- Validate selected endpoints
- Confirm dataset schema
- Define variable names
- Define data types
- Identify potential missing values

### Expected Duration:
30 to 60 minutes

### Success Criterion:
All team members understand the dataset structure before beginning analysis.

**This meeting is mandatory and should occur before any coding activities.**

---

## 24. Statistical Analysis Plan

This section defines the statistical responsibilities of each team member to avoid duplicated work and ensure that all analyses contribute directly to answering the research question.

### Lucas – Dataset Validation and Global Statistics

**Objectives:** Validate the final dataset and generate overall project statistics.

**Metrics:**
- Total number of launches
- Total number of successful launches
- Overall success rate
- Number of reusable boosters
- Number of rocket families analyzed

**Deliverables:**
- Dataset summary table
- Data quality report
- Global project statistics

### Gabriel – Temporal Reliability Analysis

**Research Question:** How has SpaceX reliability evolved over time?

**Metrics:**
- Number of launches per year
- Success rate per year
- Reuse rate per year
- Average launches per year
- Median launches per year
- Standard deviation of annual launches

**Visualizations:**
- Line Chart: Launches per Year
- Line Chart: Success Rate per Year
- Line Chart: Reuse Rate per Year

**Expected Outcome:** Identify long-term trends in reliability and operational growth.

### Kaio – Reusability Reliability Analysis

**Research Question:** Do reused boosters perform as reliably as new boosters?

**Metrics:**
- **Group 1:** New boosters
- **Group 2:** Reused boosters

For each group calculate:
- Number of launches
- Number of successful launches
- Success percentage
- Failure percentage

**Visualizations:**
- Bar Chart
- Pie Chart

**Optional Statistical Test:** Two-Proportion Z-Test

**Expected Outcome:** Determine whether reused boosters experience lower reliability than new boosters.

### Izac – Booster Experience Analysis

**Research Question:** Does booster experience improve reliability?

**Metrics:**
- Reuse count
- Success rate
- Correlation coefficient
- Trend analysis

**Visualizations:**
- Scatter Plot
- Regression Line

**Optional Statistical Test:** Pearson Correlation

**Expected Outcome:** Evaluate whether operational experience contributes to increased reliability.

### Jonatas – Rocket Family Reliability Analysis

**Research Question:** Which rocket family achieved the highest reliability?

**Metrics:**
For each rocket family:
- Number of launches
- Success rate
- Failure rate
- Mean reliability

**Rocket Families:**
- Falcon 1
- Falcon 9
- Falcon Heavy

**Visualizations:**
- Comparative Bar Chart
- Rocket Family Ranking

**Expected Outcome:** Identify the most reliable rocket family.

---

## 25. Integration Strategy

The project will follow a layered integration approach.

### Layer 1: Data Collection
- **Responsible:** Lucas
- **Output:** `processed_dataset.csv`

### Layer 2: Individual Analyses
- **Responsible:** Gabriel, Kaio, Izac, Jonatas
- **Output:** Notebook, Graphs, Statistical interpretations

### Layer 3: Project Integration
- **Responsible:** Lucas
- **Output:** `master_analysis.ipynb`
- **Responsibilities:** Consolidate findings, Review consistency, Validate conclusions

### 25.1 Integration Acceptance Criteria

An analysis will be accepted into the master notebook only if it includes:
- Notebook source
- Final visualizations
- Statistical calculations
- Written interpretation
- References to variables used

**Incomplete analyses will not be integrated until all requirements are satisfied.**

---

## 26. Interpretation Guidelines

To ensure consistency across the project, all interpretations must follow the same structure:

1. **Observation** – What does the graph show?
2. **Statistical Evidence** – What metric supports the observation?
3. **Conclusion** – How does the result contribute to answering the research question?

### Example:
- **Observation:** Mission success rates increased over time
- **Statistical Evidence:** Success rate increased from 62% to 98%
- **Conclusion:** Operational reliability improved significantly during the analyzed period

---

## 27. Definition of Success

The project will successfully answer the research question if the final analysis can clearly determine:
- Whether reusable boosters maintain high reliability
- Whether booster experience influences mission success
- Whether reliability improved over time
- Whether Falcon 9 and Falcon Heavy demonstrate consistent operational performance

**The final conclusion must be supported by statistical evidence and not by subjective interpretation.**

---

## 28. Final Deliverables Checklist

### Each Team Member Must Submit:
- ✅ Completed notebook
- ✅ Minimum two visualizations
- ✅ Statistical calculations
- ✅ Written interpretation

### Lucas Must Also Submit:
- ✅ Processed dataset
- ✅ Data dictionary
- ✅ Master notebook
- ✅ Final integration report

### Final Team Submission:
- ✅ GitHub Repository
- ✅ Google Colab Notebook
- ✅ Statistical Analysis
- ✅ Poster
- ✅ Presentation Slides
- ✅ Final Conclusions
- ✅ References

---

## 29. Repository Milestones

### Milestone 1 — Repository Setup
- **Owner:** Lucas
- **Deliverables:**
  - GitHub repository created
  - Branches created
  - Folder structure initialized

### Milestone 2 — Dataset Ready
- **Owner:** Lucas
- **Deliverables:**
  - Processed dataset
  - Data dictionary

### Milestone 3 — Individual Analyses Complete
- **Owners:** Gabriel, Kaio, Izac, Jonatas
- **Deliverables:**
  - Statistical analysis
  - Graphs
  - Interpretations

### Milestone 4 — Integration Complete
- **Owner:** Lucas
- **Deliverables:**
  - `master_analysis.ipynb`
  - Integrated results

### Milestone 5 — Poster and Presentation
- **Owners:** All Members
- **Deliverables:**
  - Poster
  - Slides
  - Final presentation

---

## 30. Project Success Metrics

The project will be considered successfully executed if:
- ✅ All planned analyses are completed
- ✅ All team members deliver their assigned work
- ✅ Results directly answer the research question
- ✅ Statistical evidence supports conclusions
- ✅ The final dataset is reproducible
- ✅ The GitHub repository contains complete project artifacts
- ✅ The poster clearly communicates the findings
- ✅ The final presentation can be delivered within the allotted time

---

## 31. Project Technology Stack

- **Programming Language:** Python 3
- **Environment:** Google Colab
- **Version Control:** GitHub
- **Libraries:**
  - Pandas
  - NumPy
  - Matplotlib
  - SciPy (optional)
- **Poster:** Canva or Google Slides
- **Presentation:** Google Slides

---

## 32. Data Quality Checklist

Before the processed dataset is approved:

- ✅ No duplicated rows
- ✅ No missing values in `success`
- ✅ No missing values in `launch_year`
- ✅ No missing values in `rocket_name`
- ✅ All data types validated
- ✅ Reuse count validated

---

## 33. Visualization Requirements

Each analysis must include:

- ✅ At least one statistical table
- ✅ At least two visualizations
- ✅ One written interpretation

All charts must be exportable as PNG.

---

**Last Updated:** June 1, 2026
