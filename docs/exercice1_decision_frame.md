# Exercice 1 : Diagnostic d'Albert's Marketplace & Decision Frame

## 1. Diagnostic de l'architecture actuelle (PostgreSQL)
- **Structure actuelle** : Albert's Marketplace is built on a PostgreSQL database. 
- **Problèmes identifiés** : 
  - Customers run heavy analytical queries directly on the production database while customers are making purchases
  - The database only keeps the instantaneous state, data historization is not managed
  - The ML team needs clean, daily data structured by a precise grain (1 row per product x country, day), which the current architecture does not allow to provide.

## 2. Grille d'évaluation (Decision Frame)

| Critère | Data Warehouse | Data Lake | Lake + Warehouse | Lakehouse |
| :--- | :---: | :---: | :---: | :---: |
| **Workload** (BI / ML / Both) | BI | ML | BI / ML | BI / ML |
| **Data Types** (Tables / Text / Images) | Tables | Text / Images | Tables / Text / Images | Tables / Text / Images |
| **Freshness** (Days / Hours / Seconds) | Days | Days | Days / Hours | Days / Hours / Seconds |
| **Team & Skills** (SQL / Python / Ops) | SQL | Python | SQL / Python | SQL / Python / Ops |
| **Cost** (Storage / Compute / People) | Low | High | High | Medium |
| **Governance** (GDPR / Access / Audit) | High | Low | Medium | High |

## 3. Justifications détaillées par critère
Data Warehouse (−): Excellent for BI and analytical SQL, but poorly suited for directly training ML models that require fast access to raw files.
Data Lake (−): Ideal for Python/ML notebooks, but mediocre for fast BI, which requires ACID transactions and SQL indexes.
Lake + Warehouse (+): Meets both needs by providing a Lake for ML and a Warehouse for BI, at the cost of increased complexity.
Lakehouse (+): Unifies BI and ML on a single platform through open storage and an ACID management layer.