# API Validation

## Purpose

This document records the validation process of the SpaceX API endpoints used in the project.

Project:

Statistical Evidence of Reliability in Reusable Rocket Systems

---

## API Version

SpaceX API v4

Documentation:
https://docs.spacexdata.com/

Repository:
https://github.com/r-spacex/SpaceX-API

---

# Endpoint Validation

## Launches Endpoint

Endpoint:

/v4/launches

Status:

VALIDATED

Fields Confirmed:

| Project Variable | API Field |
|-----------------|-----------|
| launch_name | name |
| flight_number | flight_number |
| success | success |
| launch_date | date_utc |
| core_id | cores[0].core |
| rocket_id | rocket |

---

## Rockets Endpoint

Endpoint:

/v4/rockets/{id}

Status:

VALIDATED

Fields Confirmed:

| Project Variable | API Field |
|-----------------|-----------|
| rocket_name | name |

---

## Cores Endpoint

Endpoint:

/v4/cores/{id}

Status:

VALIDATED

Fields Confirmed:

| Project Variable | API Field |
|-----------------|-----------|
| reuse_count | reuse_count |

---

# Derived Variables

The following variables are not directly stored in the API and will be generated during data processing.

| Variable | Rule |
|-----------|------|
| launch_year | Extracted from launch_date |
| is_reused | reuse_count > 0 |

---

# Validation Result

All required variables for the project were successfully validated.

The selected endpoints provide all information necessary to answer the research question:

"Does rocket reusability affect mission reliability?"

Validated Endpoints:

- Launches
- Rockets
- Cores

Project Status:

READY FOR DATASET CONSTRUCTION