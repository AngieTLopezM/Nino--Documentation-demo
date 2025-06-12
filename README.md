# Nino--Documentation-demo
Technical documentation for data warehouse

# NINO DATA WAREHOUSE  
**TECHNICAL DOCUMENTATION**  

## Chapter 1: Introduction to the NINO System  

**Cora Group**  
**2025**  

---

## Document Version Control

| Version | Purpose/Change | Author      | Date        |
|---------|----------------|-------------|-------------|
| 1       |                | Angie Lopez | 20/05/2025  |

---

## Table of Contents

- [NINO DATA WAREHOUSE](#nino-data-warehouse)
  - [Chapter 1: Introduction to the NINO System](#chapter-1-introduction-to-the-nino-system)
  - [Document Version Control](#document-version-control)
  - [Table of Contents](#table-of-contents)
  - [System Overview](#system-overview)
  - [Purpose and Scope](#purpose-and-scope)
  - [Chapter Previews](#chapter-previews)
  - [Specialized Analytical Summary Table](#specialized-analytical-summary-table)
  - [Document Usage \& Audience](#document-usage--audience)

---

## System Overview

The NINO Data Warehouse System is a modern, modular, and cloud-native architecture designed to manage the complete lifecycle of logistics and operational data originating from the Machship Transport Management System (TMS). Built on Microsoft Azure technologies and integrated with Power BI for analytical delivery, NINO ensures secure ingestion, scalable transformation, and high-quality reporting capabilities.

The system ingests external JSON data, processes it through Azure Data Factory pipelines, stores and transforms it into Azure SQL via structured schemas and stored procedures, and delivers insights through semantic models and dashboards orchestrated by Power Automate.

> *Figure 1. NINO Data Warehouse Diagram – General Overview*

---

## Purpose and Scope

This documentation serves as the technical backbone for the NINO Data Warehouse. It provides a detailed breakdown of each system layer, including ingestion mechanisms, data processing pipelines, SQL modelling, and reporting automation.  

This document is designed to support developers, analysts, and system administrators by offering:

- Clear overviews of architectural components  
- Traceable documentation of dataflows, pipelines, and procedures  
- Glossaries and metadata for system transparency  
- Automation mechanisms for orchestration and monitoring  

---

## Chapter Previews

- **Chapter 2: The Staging Area – MachShip to Azure Blob**  
  Describes how JSON data is acquired securely from MachShip into Azure Blob Storage. Explains SAS token governance, file structure, ingestion cadence, and control mechanisms such as ‘.complete’ files for triggering downstream processes.

- **Chapter 3: Automation & Orchestration – Power Automate**  
  Explores the Power Automate flow that orchestrates ingestion, SQL scaling, blob movement, and Power BI refreshes. Introduces the modular flow structure with defined scopes and a control table for execution tracking.

- **Chapter 4: Data Factory – ADF Pipelines and SQL Integration**  
  Documents the Azure Data Factory pipelines (`pl_BlobToSQL_00`, `pl_BlobToSQL`, `pl_BlobToSQL_External`) used to ingest, transform, and upsert JSON data into SQL staging tables. Highlights reusable components, stored procedure chaining, and activity naming conventions.

- **Chapter 5: SQL Layer – Schemas, Stored Procedures, Views & Functions**  
  Covers the multi-schema design in Azure SQL, detailing how data is enriched, validated, and prepared for reporting. Provides procedural documentation and metadata views that support version control and auditability.

- **Chapter 6: Reporting Layer – Power BI Dataflows & Semantic Models**  
  Outlines the reporting strategy using Power BI Dataflows and Semantic Models with scope-based refresh orchestration via Power Automate. Discusses Premium Per User licensing, DAX modelling, role security, and offline integrations.

---

## Specialized Analytical Summary Table

| Chapter | Title                                                                   | Keywords                                  | Lead            | Summary Date  |
|---------|-------------------------------------------------------------------------|-------------------------------------------|-----------------|---------------|
| 2       | Staging Area                                                            | Staging, JSON, SAS Token, ETL             | Manuel Alvarez  | 21 Apr 2025   |
| 3       | Power Automate – Automation & Orchestration                             | Flow, DF Trigger, DTU Scaling             | Manuel Alvarez  | 19 May 2025   |
| 4       | Data Factory                                                            | ADF, Upsert                               | Manuel Alvarez  | 19 May 2025   |
| 5       | SQL                                                                     | SQL, Stored Procedures, Functions         | Manuel Alvarez  | 19 May 2025   |
| 6       | Reporting Layer: Power BI Dataflows, Semantic Models, and Orchestration | Power BI, Dataflows, Semantic Models      | Manuel Alvarez  | 19 May 2025   |

---

## Document Usage & Audience

This documentation is intended for:

- **Data Engineers** responsible for maintaining pipelines and ingestion logic  
- **BI Developers** managing Power BI dataflows and semantic models  
- **System Architects** overseeing cloud integration and orchestration  
- **Stakeholders** requiring governance, audit, and compliance traceability  

It is recommended to update this documentation following any of the following events:

- Schema changes in SQL  
- Pipeline modifications or new ADF versions  
- Power BI model restructuring  
- New automation flows or scope refresh logic additions  

This ensures that the NINO documentation remains a living reference and aligns with operational practices, technical scalability, and compliance standards.

