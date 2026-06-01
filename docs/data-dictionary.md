# Data Dictionary

Project:

Statistical Evidence of Reliability in Reusable Rocket Systems

---

## Dataset Name

processed_dataset.csv

---

## Variables

### launch_year

Type:
Integer

Description:
Year extracted from the launch date.

Example:
2021

---

### launch_name

Type:
String

Description:
Official SpaceX mission name.

Example:
Transporter-1

---

### flight_number

Type:
Integer

Description:
Sequential mission number assigned by SpaceX.

Example:
96

---

### success

Type:
Boolean

Description:
Indicates whether the mission was successful.

Values:

- True
- False

Example:
True

---

### rocket_name

Type:
String

Description:
Name of the rocket used in the mission.

Examples:

- Falcon 1
- Falcon 9
- Falcon Heavy

---

### core_id

Type:
String

Description:
Unique identifier of the booster core used in the launch.

Example:
5e9e289df35918033d3b2623

---

### reuse_count

Type:
Integer

Description:
Number of times the booster had been reused before the mission.

Examples:

0
1
2
5

---

### is_reused

Type:
Boolean

Description:
Indicates whether the booster had already been used in a previous mission.

Rule:

is_reused = (reuse_count > 0)

Values:

- True
- False

Example:
True

---

### launch_date

Type:
Datetime (UTC)

Description:
Official launch date and time in UTC.

Example:

2021-01-24T15:00:00.000Z

---

# Final Dataset Schema

| Column |
|----------|
| launch_year |
| launch_name |
| flight_number |
| success |
| rocket_name |
| core_id |
| reuse_count |
| is_reused |
| launch_date |

---

# Dataset Owner

Lucas Gabriel Rocha Ferreira

---

# Dataset Version

v1.0