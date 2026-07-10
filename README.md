# insider-threat-behavioral-intelligence-system
PostgreSQL-based insider threat detection system with real-time trigger-based alerting and a risk dashboard view
## Getting Started

This repository includes files with plain SQL that can be used to recreate the database:

- Use [schema.sql](./schema.sql) to create all tables, the auto-flagging trigger, and the risk dashboard view.
- Use [data.sql](./data.sql) to populate tables with a 15-employee sample dataset.
- Check [queries.sql](./queries.sql) for the analysis queries, transaction demos, and live trigger verification. **Important note: this file includes transaction blocks (BEGIN/COMMIT/ROLLBACK) and live INSERTs. Use them responsibly.**

<a name="readme-top"></a>

<!-- TABLE OF CONTENTS -->

# 📗 Table of Contents

- [📖 About the Project](#about-project)
  - [🛠 Built With](#built-with)
    - [Tech Stack](#tech-stack)
    - [Key Features](#key-features)
  - [🚀 Schema Diagram](#schema-diagram)
- [💻 Getting Started](#getting-started)
  - [Setup](#setup)
  - [Prerequisites](#prerequisites)
  - [Install](#install)
  - [Usage](#usage)
- [🧪 Detection Logic](#detection-logic)
- [👥 Author](#author)
- [🔭 Future Features](#future-features)
- [🤝 Contributing](#contributing)
- [⭐️ Show your support](#support)
- [❓ FAQ](#faq)
- [📝 License](#license)

<!-- PROJECT DESCRIPTION -->

# 📖 [Insider Threat Behavioral Intelligence System] <a name="about-project"></a>

> **[insider-threat-behavioral-intelligence-system]** A PostgreSQL-based behavioral intelligence system that tracks employee login and resource-access activity, and automatically flags suspicious behavior — such as high-sensitivity file downloads or deletions outside working hours — using a database trigger. A consolidated risk dashboard view aggregates this activity into a per-employee risk rating.

## 🛠 Built With <a name="built-with"></a>

### Tech Stack <a name="tech-stack"></a>

<details>
  <summary>Database</summary>
  <ul>
    <li><a href="https://www.postgresql.org/">PostgreSQL</a></li>
  </ul>
</details>

<details>
  <summary>Backend (live-connected dashboard)</summary>
  <ul>
    <li>Node.js / Express</li>
  </ul>
</details>

<!-- Features -->

### Key Features <a name="key-features"></a>

- **[Normalized 4-table schema: employees, login_logs, access_logs, flagged_alerts]**
- **[Real-time trigger that auto-flags off-hours, High-sensitivity download/delete events]**
- **[employee_risk_dashboard view classifying employees into Low / Medium / High risk]**
- **[Joins, aggregations, and window functions: RANK, LAG, NTILE]**
- **[CTEs, chained CTEs, and transaction control: COMMIT, ROLLBACK, SAVEPOINT]**
- **[Verified end-to-end with fresh, live inserts — not just the original seed data]**

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- SCHEMA DIAGRAM -->

## 🚀 Schema Diagram <a name="schema-diagram"></a>

![insider_threat_schema_diagram](./schema_diagram.svg)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->

## 💻 Getting Started <a name="getting-started"></a>

To get a local copy up and running, follow these steps.

### Prerequisites

In order to run this project you need:

```sh
 Install PostgreSQL
```

### Setup

Clone this repository to your desired folder:

```sh
  cd my-folder
  git clone https://github.com/<your-username>/insider-threat-behavioral-intelligence-system.git
```

### Install

Create the database and load the schema and sample data:

```sh
  createdb insider_threat_db
  psql -d insider_threat_db -f schema.sql
  psql -d insider_threat_db -f data.sql
```

### Usage

Run the analysis queries:

```sh
  psql -d insider_threat_db -f queries.sql
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- DETECTION LOGIC -->

## 🧪 Detection Logic <a name="detection-logic"></a>

The `trg_flag_suspicious_access` trigger fires after every insert into `access_logs`. It generates a High-severity alert in `flagged_alerts` whenever:

- the resource's `sensitivity_level` is `High`, **and**
- the `action` is `download` or `delete`, **and**
- the access happens before 8 AM or after 8 PM.

No manual review step is required — the moment a risky access is logged, an alert is generated automatically.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- AUTHOR -->

## 👥 Author <a name="author"></a>

👤 **Pragya Mishra Prashasti (Maggie)**

- B.Tech CSE (Data Analytics & Business Intelligence), Dronacharya College of Engineering
- Built as part of Infosys Springboard Virtual Internship 7.0

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- FUTURE FEATURES -->

## 🔭 Future Features <a name="future-features"></a>

- [ ] **[Live-connected HTML risk dashboard reading directly from employee_risk_dashboard]**
- [ ] **[Configurable working-hours window and sensitivity thresholds]**
- [ ] **[Email/Slack alerting on new High-severity flagged_alerts rows]**

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->

## 🤝 Contributing <a name="contributing"></a>

Contributions, issues, and feature requests are welcome!

Feel free to check the [issues page](https://github.com/<your-username>/insider-threat-behavioral-intelligence-system/issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- SUPPORT -->

## ⭐️ Show your support <a name="support"></a>

If you like this project, give it a star.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- FAQ (optional) -->

## ❓ FAQ <a name="faq"></a>

- **Why a trigger instead of a scheduled batch job?**

  - A trigger fires immediately on insert, so suspicious access is flagged in real time rather than on the next scheduled scan.

- **Why classify risk with a view instead of a materialized table?**

  - `employee_risk_dashboard` is a plain view, so it always reflects the current state of `access_logs` and `flagged_alerts` without needing a refresh job.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## 📝 License <a name="license"></a>

This project is [MIT](./LICENSE) licensed.

<p align="right">(<a href="#readme-top">back to top</a>)</p>
