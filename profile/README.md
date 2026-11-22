<p align="center">
  <a href="https://culpritsql.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/culpritsql/.github/main/profile/banner-dark.png">
      <img alt="Culprit SQL Studio: Find the root cause. Prove the fix." src="https://raw.githubusercontent.com/culpritsql/.github/main/profile/banner-light.png" width="100%">
    </picture>
  </a>
</p>

<p align="center">
  Open-source performance diagnostics and root-cause analysis for relational databases.
</p>

<p align="center">
  <img alt="SQL Server" src="https://img.shields.io/badge/SQL%20Server-CC2927?style=flat-square&logo=microsoftsqlserver&logoColor=white">
  <img alt="Azure SQL" src="https://img.shields.io/badge/Azure%20SQL-0078D4?style=flat-square&logo=microsoftazure&logoColor=white">
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-336791?style=flat-square&logo=postgresql&logoColor=white">
  <img alt="TimescaleDB" src="https://img.shields.io/badge/TimescaleDB-F4CE41?style=flat-square&logo=timescale&logoColor=black">
  <img alt="YugabyteDB" src="https://img.shields.io/badge/YugabyteDB-C94B25?style=flat-square">
  <img alt="MySQL" src="https://img.shields.io/badge/MySQL-00758F?style=flat-square&logo=mysql&logoColor=white">
  <img alt="MariaDB" src="https://img.shields.io/badge/MariaDB-003545?style=flat-square&logo=mariadb&logoColor=white">
  <img alt="SQLite" src="https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white">
</p>

<p align="center">
  <a href="https://culpritsql.com">culpritsql.com</a> ·
  <a href="https://github.com/culpritsql/culprit-sql-studio/blob/main/docs/guide.md">Guide</a> ·
  <a href="https://github.com/culpritsql/culprit-sql-studio/blob/main/docs/ARCHITECTURE.md">Architecture</a> ·
  <a href="https://github.com/culpritsql/culprit-sql-studio/blob/main/docs/agents.md">Coding Agents (MCP)</a> ·
  <a href="https://github.com/culpritsql/culprit-sql-studio/blob/main/docs/collector.md">Collector &amp; ADX</a> ·
  <a href="https://github.com/culpritsql/culprit-sql-studio/releases/latest">Releases</a>
</p>

#### Two ways to run it

| Mode | How it works |
| :--- | :--- |
| **Direct**<br><sub>Desktop · CLI · MCP</sub> | Connect directly to a database (or open a saved snapshot offline) to inspect top queries, ranked findings, execution plans, live blocking, virtual indexes, and SQL migrations in CI or coding agents. |
| **Fleet Collector**<br><sub>ADX · ClickHouse</sub> | Deploy a bounded container in your database network and push verified telemetry and synthesized root-cause findings to **Azure Data Explorer (ADX)** or **ClickHouse** (with Blob or S3 recovery). |

#### What it diagnoses

- **Top queries by database time, wait attribution & ranked findings**
- **Execution plan regressions, parameter sensitivity & side-by-side plan diffs**
- **`@TVP` cardinality skew, memory-grant starvation & disk spills**
- **Page-latch (`PAGELATCH`) hot pages & `tempdb` allocation contention**
- **Blocking chains, DDL metadata-lock sandwiches & deadlocks**
- **Worker-thread (`THREADPOOL`) & compiler-gateway starvation**
- **PostgreSQL subtransaction (`SubtransSLRU`) & `XID` wraparound cliffs**
- **Transaction-log stalls, slow client consumers & replica redo blocks**
- **Version-store (`PVS` / vacuum) retention & cloud tier throttling**
- **Missing/unused indexes & unsafe SQL migrations in pull requests**

#### Alert on root causes directly from ADX

Point an Azure Monitor Scheduled Query Rule or Grafana alert at ADX to page on-call with the culprit and fix already attached:

```kql
// Runs every 1m in Azure Monitor / Grafana -> PagerDuty, Slack, or Teams
CulpritIncidentsV1
| where UpdatedAt > ago(5m) and Status == "active" and Severity == "high"
| project StartedAt, DatabaseName, CulpritKind, Headline, CulpritSql, DeveloperAction
```
