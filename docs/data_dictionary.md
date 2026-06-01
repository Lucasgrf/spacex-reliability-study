# Data Dictionary - SpaceX Reliability Study

## Dataset Overview
- **File**: `data/processed/processed_dataset.csv`
- **Format**: 192 rows × 9 columns
- **Structure**: 1 row per booster (multi-booster launches like Falcon Heavy generate multiple rows)
- **Date Range**: 2006-2022
- **Success Rate**: 97.40% (187 successful launches out of 192 executed)
- **Data Quality**: 100% complete (0 null values)

---

## Column Definitions

### `launch_year` (Integer)
- **Type**: `int64`
- **Nullable**: No
- **Description**: The year in which the launch occurred
- **Range**: 2006-2022
- **Source**: Derived from `launch_date` field in SpaceX API
- **Usage**: Temporal grouping, year-over-year trend analysis

### `launch_name` (String)
- **Type**: `object` (string)
- **Nullable**: No
- **Description**: Official mission name assigned by SpaceX or customer
- **Examples**: "CRS-8", "SES-9", "JCSAT-2B", "Starlink 3-1", "Falcon 9 Test Flight"
- **Source**: SpaceX API `/launches` endpoint
- **Usage**: Mission identification, customer segmentation

### `flight_number` (Integer)
- **Type**: `int64`
- **Nullable**: No
- **Description**: Sequential mission number in SpaceX's flight history
- **Range**: 1-203
- **Source**: SpaceX API `/launches` endpoint
- **Usage**: Chronological ordering, mission sequencing analysis
- **Note**: In case of multi-booster missions, all boosters from the same launch share the same `flight_number`

### `success` (Boolean)
- **Type**: `bool`
- **Nullable**: No
- **Description**: Mission outcome - whether the primary objective was achieved
- **Values**: `True` (successful) or `False` (failure)
- **Source**: SpaceX API `/launches` endpoint (`success` field)
- **Usage**: Primary dependent variable for reliability analysis
- **Note**: This reflects the overall mission success, not individual booster performance

### `rocket_name` (String)
- **Type**: `object` (string)
- **Nullable**: No
- **Description**: Name of the rocket family used for this launch
- **Values**: "Falcon 1", "Falcon 9", "Falcon Heavy"
- **Source**: SpaceX API `/rockets` endpoint (joined on `rocket_id`)
- **Usage**: Rocket family comparison, booster type classification
- **Distribution**:
  - Falcon 1: 5 rows
  - Falcon 9: 172 rows
  - Falcon Heavy: 15 rows (5 launches × 3 boosters each)

### `core_id` (String)
- **Type**: `object` (string)
- **Nullable**: No
- **Description**: Unique identifier for the booster/core used in this launch
- **Format**: MongoDB ObjectID (24-character hexadecimal string)
- **Source**: SpaceX API `/launches` endpoint → `cores[].core` field → matched to `/cores` endpoint
- **Usage**: Individual booster tracking, reusability analysis
- **Note**: Each core_id represents a unique physical booster

### `reuse_count` (Integer)
- **Type**: `int64`
- **Nullable**: No
- **Description**: Number of times the booster has been reused (launched) prior to this mission
- **Range**: 0-13
- **Mean**: 4.6 launches per booster
- **Source**: SpaceX API `/cores` endpoint (`reuse_count` field)
- **Derivation**: Direct field from API, NOT calculated from launches list
- **Usage**: Booster experience measure, primary independent variable for reusability analysis
- **Interpretation**:
  - 0 = First-time launch (virgin booster)
  - 1+ = Reused booster (previously launched)
- **Quality Note**: Corrected during development (was initially calculated incorrectly as list length)

### `is_reused` (Boolean)
- **Type**: `bool`
- **Nullable**: No
- **Description**: Binary indicator of whether this booster has previous flight history
- **Values**: `True` (reused) or `False` (first-time)
- **Source**: Derived from `reuse_count` (is_reused = reuse_count > 0)
- **Usage**: Simplified comparison: new vs reused boosters
- **Distribution**: ~60% reused boosters in dataset

### `launch_date` (String)
- **Type**: `object` (string)
- **Nullable**: No
- **Description**: ISO 8601 formatted timestamp of launch time (UTC)
- **Format**: "YYYY-MM-DDTHH:MM:SS.000Z"
- **Examples**: "2016-04-08T20:43:00.000Z", "2022-03-15T12:34:00.000Z"
- **Source**: SpaceX API `/launches` endpoint
- **Usage**: Precise temporal analysis, time-series analysis
- **Note**: All times are in UTC; adjust for local time if needed

---

## Data Collection & Quality

### Data Source
- **SpaceX API v4**: https://api.spacexdata.com/v4
- **Endpoints Used**:
  - `/launches` - 205 total missions (190 executed before 2023-01-01)
  - `/rockets` - Rocket specifications and families
  - `/cores` - Individual booster telemetry and history

### Quality Assurance
- **Records**: 192 executed missions (removed 23 planned/future launches from 2022)
- **Completeness**: 100% (no null values)
- **Validation**: 
  - Schema verified (all 9 columns present)
  - Data types verified (correct types for all columns)
  - Multi-booster missions verified (Falcon Heavy correctly generates 3 rows)
  - Reuse count validation (first reusable booster: CRS-8 with reuse_count=1)

### Data Limitations
1. **Time Period**: Only includes launches up to early 2023 (API snapshot date)
2. **Multi-booster Missions**: Each booster generates a separate row
   - Example: Falcon Heavy with 3 cores → 3 rows with identical flight number but different core_id
3. **Success Definition**: Reflects primary mission objective only
   - Does not account for partial successes or secondary payload failures
4. **Reuse Count**: Historical from API; does not update in real-time

---

## Suggested Analyses by Researcher

### Gabriel - Temporal Trend Analysis
- **Variables**: `launch_year`, `success`, `launch_date`
- **Questions**:
  - How has success rate changed over time?
  - Is there a trend in launch frequency?
  - Which year had the most missions?

### Kaio - Reusability Impact
- **Variables**: `is_reused`, `reuse_count`, `success`, `rocket_name`
- **Questions**:
  - Do reused boosters have different success rates?
  - Does reuse count correlate with success?
  - How does reusability impact Falcon 9 vs Falcon Heavy?

### Izac - Booster Experience Analysis
- **Variables**: `reuse_count`, `success`, `launch_date`
- **Questions**:
  - What is the success rate for first-time boosters?
  - Does success rate improve with booster experience?
  - What is the maximum reuse_count in the dataset?

### Jonatas - Rocket Family Comparison
- **Variables**: `rocket_name`, `success`, `launch_year`, `is_reused`
- **Questions**:
  - Which rocket family has the highest reliability?
  - How does Falcon 1 compare to Falcon 9 and Falcon Heavy?
  - Has reliability improved across generations?

---

## Technical Notes for Data Users

### Data Format
- **Encoding**: UTF-8
- **Line Terminator**: LF (Unix-style)
- **Field Separator**: Comma (,)
- **Quote Character**: " (double quotes)
- **Boolean Representation**: "True"/"False" strings

### Loading the Data
```python
import pandas as pd
df = pd.read_csv('data/processed/processed_dataset.csv')
```

### Key Assumptions
1. Each row represents one booster in one mission
2. Multi-booster launches (Falcon Heavy) create multiple rows
3. `success` reflects overall mission outcome, not individual booster performance
4. `reuse_count` is the number of PREVIOUS launches (current launch not included)
5. Null values have been removed; dataset is analysis-ready

### Known Edge Cases
- **Falcon Heavy**: 5 launches generate 15 rows (3 boosters each)
- **Multi-core missions**: Some payloads use multiple boosters; each gets a separate row
- **Core reuse**: Some cores appear in multiple rows (different launches)

---

## Version History
- **v1.0** (2024-01-XX): Initial dataset - 192 clean records from SpaceX API
- **Cleaned**: Removed 23 rows with null `success` values (unexecuted 2022 missions)
- **Quality Status**: ✅ Ready for statistical analysis
