




NINO DATA WAREHOUSE
TECHNICAL DOCUMENTATION



Cora Group




2025




Document Version Control




# Chapter 2 The Staging area:
# “Data Import from MachShip (TMS) to Azure Data Lake”

## Overview
### Executive Overview
This chapter defines the foundational role of the Staging Area in the NINO System, detailing its critical function in acquiring and securely storing data from the Machship Transport Management System (TMS). Positioned as the initial interface in the NINO data ingestion pipeline, this layer ensures that all external logistics data is validated, secured, and structured before entering downstream transformation processes.
The ingestion process is based on an incremental model, where JSON-formatted files containing up to 500 records are exported from Machship and uploaded to the live-zip folder in Azure Blob Storage every two hours. Each batch concludes with the appearance of a control file (tx_0.complete) to signal readiness, while a blank.txt file prevents folder deletion if no data is present.
The system architecture places a strong emphasis on security and scalability. Data uploads are executed via SAS tokens issued by Synfo, which enable time-limited, permission-scoped access without exposing underlying platform credentials. The token lifecycle and access governance are managed under the supervision of Cora Group. Additionally, the ingestion strategy supports growth in dataset volume and can accommodate changes in cadence or table count with minimal reconfiguration.
The chapter further introduces comprehensive metadata and schema references, enabling internal teams to navigate and interpret incoming data structures. Key entities include carrier accounts, invoice entries, item references, and tracking records.
In conclusion, this chapter recommends maintaining the SAS token model as a best practice for secure external data exchange, regularly reviewing ingestion frequency to meet operational demand, and keeping schema documentation up to date for consistent downstream integration. These principles uphold NINO’s reliability, auditability, and data integrity standards.

Specialised Analytical Summary for Information Systems
Chapter: The Staging Area – Data Import from MachShip to Azure Data Lake
System Title: 
NINO Data Warehouse System
Technical Lead: 
Manuel Alvarez
Document Version: 
v1.0 – Initial Technical Summary (April 2025)
Date of Completion: 
21 April 2025
Technical Keywords: 
Staging, Azure Data Lake, JSON, MachShip, Blob Storage, Incremental Load, ETL, SAS Token, Power BI
Document Description: 
This document is a technical chapter from the NINO System Documentation. It describes the architecture, ingestion design, and control mechanisms of the staging layer responsible for initial data acquisition from the external MachShip Transport Management System (TMS) into Azure Data Lake Storage.
Technical References:
Azure Documentation (Blob Storage, SAS Tokens)
MachShip File Integration Guidelines
Cora Group Internal IT Security Policies, enforced by Synfo
Technologies Used: 
Azure Blob Storage, JSON format, Shared Access Signature (SAS), Azure Data Lake, Synfo Access Management, Microsoft Azure Infrastructure
Technical Summary:
The staging layer of the NINO Data Warehouse serves as the primary landing zone for transport and logistics data exported by MachShip (TMS). Using an incremental ingestion pattern, the system collects and stores up to 76 JSON datasets in a structured, scheduled manner within Azure Blob Storage (machship container, live-zip folder). Each JSON file contains no more than 500 records and follows a controlled naming convention (e.g., CarrierAccounts_01.json).
A control file (tx_0.complete) signals the readiness of a batch for downstream processing. A placeholder file (blank.txt) prevents the folder from being deleted when empty. Data is ingested every two hours, although frequency is configurable based on operational needs. The system uses a reliable and secure upload mechanism via SAS tokens, which limits direct user access and enforces strict upload authentication.
Data files are uploaded by MachShip, but access to the Azure environment is managed externally by Synfo (Cora’s IT support provider), ensuring data isolation and compliance with organisational security standards. The staging layer guarantees scalable ingestion and consistent upstream data availability for ETL processes and Power BI dashboards.
Technical Methodology: 
The system follows a modular and scalable ELT architecture. JSON files are processed through scheduled ingestion using Azure-native tools, applying a flexible, file-based control flow with metadata triggers for staging pipeline activation.
Evaluation Criteria:
Successful ingestion of all JSON tables in under 5 minutes per batch
100% compliance with naming and control file conventions
Zero unauthorised access incidents to blob storage containers
Seamless batch triggering across Power Automate and ADF
Technical Challenges & Solutions:

Technical Diagrams: 
Included in the broader documentation:
Data Flow Overview: MachShip → Azure Blob
Technical Conclusions: 
The staging component provides a highly secure, automated and modular framework for controlled ingestion of external data. The use of time-triggered pipelines and control files ensures reliability. SAS token management enhances data governance, and incremental loads allow scalability. The staging architecture is foundational to the broader NINO Data Warehouse pipeline.
Author: 
Angie López

## Introduction
The NINO System’s ingestion lifecycle begins with external data acquisition from Machship, the organisation’s Transport Management System (TMS). This first stage, referred to as the staging area, involves the controlled and secure transfer of structured JSON datasets into Azure Blob Storage. The outcome is a reliable and scalable repository of raw transport data, foundational to all downstream transformations and reporting processes.
## Objective
To describe the functionality, tools, and safeguards involved in securely receiving JSON-based logistics data from Machship and storing it in Azure Blob Storage, forming the initial staging layer for the NINO Data Warehouse.
## Resources
The following table summarises key resources and parameters involved in the ingestion process:
Table 1. Key Parameters for External Data Ingestion from Machship
## System Considerations
To ensure reliability and adaptability, the import process is designed with the following considerations:
To maintain robustness across evolving business needs, the ingestion mechanism includes several safeguards:
Scalability: The number of tables and datasets is expected to grow as new data domains are integrated.
Flexibility: Ingestion intervals are adaptable, allowing for performance tuning based on load or operational constraints.
Security Controls:
Access to Azure Blob Storage is secured via a SAS (Shared Access Signature) token.
Direct access to Azure is not granted to Machship.
The SAS token is issued and managed by Synfo under the direction of Cora.
All access requests must be submitted to  under the supervision of Cora. Synfo will create and manage a support ticket for each request.
Token Permissions: Read, Add, Create, Write (as of January 2025; valid until January 2027).
Like a gate that opens only for authorised couriers, the SAS token enables secure deliveries to the Blob container without revealing the keys to the kingdom.

## Data Processing Flow


The NINO System uses a structured and reliable approach for data ingestion:
Token Provisioning: A SAS (Shared Access Signature) token is created by Synfo, Cora Group’s IT support partner. This token is then securely provided to Machship.
Token Usage: Machship does not have direct access to Azure. Instead, data is uploaded via a tokenised URL, ensuring restricted and role-scoped access.
Token Validation: Upon receiving files, Azure’s token service validates the SAS token before accepting the upload into the storage account.
Data Generation: Structured JSON datasets are exported by Machship according to a predefined cadence. Each source table may produce multiple JSON files, with each file containing up to 500 records.
File Upload: These files are delivered to Azure Blob Storage. Each one is placed under the appropriate folder, grouped by entity type.
Naming Convention: Files follow a strict naming pattern (e.g., CarrierAccounts_01.json, CarrierAccounts_02.json).
Batch Completion: Once a batch is fully transferred, the presence of tx_0.complete indicates readiness for downstream processing.

## Outcomes
Once the ingestion process completes successfully, the uploaded JSON data becomes accessible for downstream processing by Azure Data Factory pipelines. Each complete batch is validated via the presence of the tx_0.complete file, triggering automated transformation into structured SQL format. This setup ensures traceability, repeatability, and the ability to scale as new entities are added over time.

## Appendices
Appendix 1: Glossary of Terms

Appendix 2: Metadata Inventory & Schema Reference
The following table lists the JSON table names along with their corresponding descriptions. These descriptions have been drafted to support internal understanding of each dataset’s function but have not been reviewed or approved by Machship and therefore should not be considered official documentation.
Each table listed below has a JSON sample and schema associated with it. You can view this detailed information through the accompanying Excel documentation provided as part of the data dictionary. (See: )
This resource provides:
Field-level descriptions
Sample payload structures
Schema mappings for each imported entity

