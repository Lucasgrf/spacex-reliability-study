# 🗺️ Project Roadmap - SpaceX Reliability Study

**Status**: 🟢 **Phase 1 Complete** | Entering **Phase 2: Statistical Analysis**  
**Date**: Junho 1, 2026  

---

## 📋 Project Overview

**Research Question**: Does rocket reusability affect mission reliability?

**Team**:
- **Lucas** (Data Engineer) — ✅ Data pipeline complete
- **Gabriel** (Analyst) — ⏳ Temporal trend analysis
- **Kaio** (Analyst) — ⏳ Reusability impact analysis
- **Izac** (Analyst) — ⏳ Booster experience analysis
- **Jonatas** (Analyst) — ⏳ Rocket family comparison

---

## 🎯 Phase 1: Data Engineering ✅ COMPLETE

### Tasks Completed

```
✅ 1. Project Planning
   - 33-section governance document
   - Team roles & responsibilities
   - Success criteria defined

✅ 2. Data Collection & Validation
   - SpaceX API v4 validated
   - 3 endpoints tested (launches, rockets, cores)
   - No authentication required

✅ 3. ETL Pipeline Development
   - lucas_data_extraction.ipynb built
   - Extract: 205 launches from API
   - Transform: Join on IDs, calculate derived fields
   - Load: Export to CSV

✅ 4. Bug Fixes & Refinement
   - Fixed reuse_count calculation (critical)
   - Validated multi-booster handling (Falcon Heavy)
   - Incremental testing (5 → 20 → 205 launches)

✅ 5. Data Quality Assurance
   - Null analysis: identified 23 + 19 null values
   - Root cause analysis: unexecuted 2022 missions
   - Dataset cleaning: removed nulls
   - Validation: 0 nulls remaining, 0 duplicates

✅ 6. Documentation & Release
   - Data dictionary created (9 columns defined)
   - Quality report signed off
   - Dataset frozen as v1.0
   - GitHub commit completed

✅ 7. Analyst Preparation
   - Individual analysis guides created (4 docs)
   - Dataset characteristics documented
   - Statistical methods suggested
   - Caveats noted for each analysis
```

### Deliverables from Phase 1

| Document | Location | Purpose |
|----------|----------|---------|
| Dataset v1.0 | `data/processed/processed_dataset_v1.csv` | Production data (192 rows, 9 cols) |
| Data Dictionary | `docs/data_dictionary.md` | Field definitions & usage |
| QA Report | `docs/data_quality_report.md` | Validation results & sign-off |
| Milestone Doc | `docs/MILESTONE_v1_COMPLETE.md` | Phase 1 completion summary |
| ETL Notebook | `notebooks/lucas_data_extraction.ipynb` | Reproducible pipeline |
| Project Plan | `docs/project-plan.md` | Overall governance |

---

## 🔬 Phase 2: Statistical Analysis ⏳ IN PROGRESS

### 2.1 Gabriel - Temporal Trends

**Document**: `docs/ANALYSIS_GABRIEL.md`  
**Timeline**: Week 1-2  
**Questions**:
1. How did success rate evolve 2006-2022?
2. Did launch frequency increase?
3. What's the impact of Falcon 1 vs Falcon 9 era?

**Key Variables**: `launch_year`, `success`, `rocket_name`  
**Methods**: Aggregation, trend analysis, stratification  
**Caution**: Falcon 1 (2006-2009) vs Falcon 9+ (2010+) confound

---

### 2.2 Kaio - Reusability Impact

**Document**: `docs/ANALYSIS_KAIO.md`  
**Timeline**: Week 1-2  
**Questions**:
1. Do reused boosters have higher success rates?
2. Is the advantage across all rocket types?
3. Controlling for temporal bias (year)?

**Key Variables**: `is_reused`, `reuse_count`, `success`, `launch_year`  
**Methods**: Cross-tabulation, chi-square, confidence intervals  
**⚠️ CRITICAL**: Check for temporal confounding!

**Preliminary Signal** (exploratory):
- Reused: 100.00% success (n=149)
- Virgin: 88.37% success (n=43)
- **Difference: +11.63 pp** ← Needs statistical validation!

---

### 2.3 Izac - Booster Experience

**Document**: `docs/ANALYSIS_IZAC.md`  
**Timeline**: Week 2-3  
**Questions**:
1. Does booster experience (reuse_count) correlate with success?
2. Is the relationship linear or non-linear?
3. Is there a "sweet spot" or wear-out effect?

**Key Variables**: `reuse_count` (0-13), `success`, `launch_year`  
**Methods**: Correlation, logistic regression, dose-response  
**Caution**: Reuse_count is recent phenomenon (temporal confound)

---

### 2.4 Jonatas - Rocket Families

**Document**: `docs/ANALYSIS_JONATAS.md`  
**Timeline**: Week 2-3  
**Questions**:
1. Which rocket family is most reliable?
2. Falcon 1 vs Falcon 9 vs Falcon Heavy?
3. How much is design vs operational maturity?

**Key Variables**: `rocket_name`, `success`, `launch_year`  
**Methods**: Comparison, chi-square, confidence intervals  
**Caution**: Falcon 1 is old (2006-09), Falcon Heavy is new (2018+)

---

## 📊 Preliminary Findings (Exploratory)

⚠️ **These are NOT conclusions yet** — Need proper statistical testing

### Signal 1: Reusability May Improve Reliability
```
Virgin boosters: 88.37% success (n=43)
Reused boosters: 100.00% success (n=149)
Difference: +11.63 percentage points
```

**Status**: PROMISING, but needs:
- Temporal control (are reused boosters more recent?)
- Statistical significance test
- Confidence intervals
- Selection bias investigation

### Signal 2: Falcon Heavy Perfect Record
```
Falcon Heavy: 5 launches, 5 successes (100%)
Falcon 9: 172 launches, 169 successes (98.26%)
Falcon 1: 5 launches, 4 successes (80.00%)
```

**Status**: Interesting, but Falcon Heavy sample is small (n=5) + very recent

### Signal 3: SpaceX Improving Over Time?
```
2006-2009 (Falcon 1): ~70% success
2010-2015 (Falcon 9 early): ~97% success
2016-2022 (Falcon 9 mature): ~98% success
```

**Status**: Unclear if design or operational learning — needs control

---

## 🔄 Interdependencies Between Analyses

### Gabriel → Kaio
Gabriel establishes timeline of improvements. Kaio checks: Is improvement due to reusability or just time?

### Gabriel ↔ Jonatas
Gabriel shows temporal trends. Jonatas stratifies by rocket family. Together: Separate rocket design improvements from operational learning.

### Izac ← Kaio & Gabriel
If Kaio finds reusability helps, Izac explores dose-response (how much reuse_count matters).

### All → Lucas (QA Check)
If results seem off, escalate to Lucas to verify data didn't introduce artifacts.

---

## 📚 Reading Order for Analysts

1. **Start here**: `docs/MILESTONE_v1_COMPLETE.md` (big picture)
2. **Then read**: `docs/data_dictionary.md` (field definitions)
3. **Then your analysis**: `docs/ANALYSIS_[YOUR_NAME].md`
4. **Reference**: `docs/data_quality_report.md` (if questions arise)

---

## 🎓 Statistical Reminders

**For All Analysts**:
- Always report `n` (sample size)
- Always report confidence intervals (not just point estimates)
- Always report p-values from formal tests
- Distinguish between exploratory and confirmatory findings
- Consider alternative explanations for observed patterns

**Specific Warnings**:
- ⚠️ Temporal confounding (more recent = more reusable & more reliable)
- ⚠️ Selection bias (broken boosters might not be reused)
- ⚠️ Small sample sizes (Falcon 1=5, Falcon Heavy=5, Falcon 9=172)
- ⚠️ Multiple comparisons (if doing many tests, p-values inflate)

---

## 📁 Directory Structure

```
spacex-reliability-study/
├── data/
│   └── processed/
│       ├── processed_dataset_v1.csv          ← USE THIS
│       ├── processed_dataset.csv             ← DEPRECATED
│       └── README.md                         ← Dataset guide
├── docs/
│   ├── project-plan.md                       ← Overall governance
│   ├── data_dictionary.md                    ← Field definitions
│   ├── data_quality_report.md                ← QA sign-off
│   ├── MILESTONE_v1_COMPLETE.md              ← Phase 1 summary
│   ├── ANALYSIS_GABRIEL.md                   ← His analysis guide
│   ├── ANALYSIS_KAIO.md                      ← His analysis guide
│   ├── ANALYSIS_IZAC.md                      ← His analysis guide
│   └── ANALYSIS_JONATAS.md                   ← His analysis guide
├── notebooks/
│   ├── lucas_data_extraction.ipynb           ← ETL pipeline
│   └── lucas_api_exploration.ipynb           ← Exploratory work
├── analyses/                                  ← Create for your notebooks
│   ├── gabriel_temporal_analysis.ipynb
│   ├── kaio_reusability_analysis.ipynb
│   ├── izac_experience_analysis.ipynb
│   └── jonatas_rocket_comparison.ipynb
├── LICENSE
├── README.md
└── .gitignore
```

---

## ⏱️ Timeline

### Week 1 (Weeks of June 1-7)
- ✅ Phase 1 complete & delivered
- Gabriel: Start temporal trend analysis
- Kaio: Start reusability impact analysis
- Lucas: Stand by for data questions

### Week 2 (Weeks of June 8-14)
- Gabriel: Complete temporal analysis
- Kaio: Complete reusability analysis
- Izac: Start booster experience analysis
- Jonatas: Start rocket family comparison

### Week 3 (Weeks of June 15-21)
- Izac: Complete experience analysis
- Jonatas: Complete family comparison
- All: Prepare draft results
- Lucas: Begin consolidation

### Week 4 (Weeks of June 22-28)
- All: Review & revise results
- Create visualization deck
- Draft final report
- Prepare presentation

### Week 5+ (Weeks of June 29+)
- Final report writing
- Presentation preparation
- Publication/sharing

---

## 🎯 Success Criteria for Phase 2

For **Each Analyst**:
- ✅ Answer your research question with data
- ✅ Report confidence intervals & p-values
- ✅ Identify possible confounders
- ✅ Propose alternative explanations
- ✅ Deliver reproducible code (notebook)
- ✅ Create publication-quality visualization

For **Team**:
- ✅ Integrate findings into cohesive narrative
- ✅ Create unified final report
- ✅ Identify key conclusions about SpaceX reliability
- ✅ Discuss implications for reusability
- ✅ Present to stakeholders

---

## 🤝 Communication Guidelines

**For Data Questions**:
1. Check `docs/data_dictionary.md` first
2. If still unclear, check `notebooks/lucas_data_extraction.ipynb`
3. If still confused, ask Lucas directly

**For Statistical Questions**:
1. Refer to caveats in your `ANALYSIS_[NAME].md` document
2. Discuss with other analysts (cross-validation of methods)
3. Document assumptions in your notebook

**For Git Commits**:
```bash
# Save your work regularly
git add analyses/yourname_analysis.ipynb
git commit -m "Add [question]: [brief description]"
git push

# Example:
git commit -m "Add Gabriel temporal trends: success rate by year 2006-2022"
```

---

## 🚀 Next Actions

### Immediate (Today)
- [ ] Gabriel, Kaio, Izac, Jonatas: Read your assigned `ANALYSIS_*.md` doc
- [ ] All: Load `processed_dataset_v1.csv` and verify shape (192 x 9)
- [ ] Create your notebook in `analyses/` folder

### This Week
- [ ] Gabriel & Kaio: Complete initial explorations
- [ ] Share preliminary findings for cross-check
- [ ] Identify if data looks as expected

### Next Week
- [ ] Complete formal analyses
- [ ] Run statistical tests
- [ ] Create publication-quality visualizations

---

## 📞 Key Contacts

| Role | Name | Responsibility |
|------|------|-----------------|
| Data Engineer | Lucas | Dataset creation, validation, support |
| Analyst 1 | Gabriel | Temporal trends |
| Analyst 2 | Kaio | Reusability impact |
| Analyst 3 | Izac | Booster experience |
| Analyst 4 | Jonatas | Rocket families |

---

## ✅ Checklist for Analysts

Before starting your analysis:
- [ ] Read this roadmap
- [ ] Read `data_dictionary.md`
- [ ] Load and explore the dataset (check shape, types, distributions)
- [ ] Read your specific `ANALYSIS_[NAME].md` document
- [ ] Create a new notebook in `analyses/` folder
- [ ] Run preliminary aggregations to familiarize yourself with data
- [ ] Identify potential confounders early
- [ ] Flag any data anomalies to Lucas

---

## 📝 Document Version History

- **v1.0** (Junho 1, 2026): Project roadmap created at Phase 1 → Phase 2 transition
- **v1.1** (TBD): Updated with Phase 2 progress

---

**Project Manager**: Lucas  
**Last Updated**: 2026-06-01  
**Status**: 🟢 Phase 1 Complete, Phase 2 Started

---

## Final Note

You have a clean, well-documented dataset and clear research questions. The preliminary findings are promising — there's a real signal to investigate. Execute your analyses carefully, control for confounders, and let the data speak.

**Good luck!** 🚀
