NINO DATA WAREHOUSENINO DATA WAREHOUSE

TECHNICAL DOCUMENTATION TECHNICAL DOCUMENTATION





Cora GroupCora Group






20252025






Document Version ControlDocument Version Control






# Chapter Chapter 22  The The Staging areaStaging area::

# “Data Import from “Data Import from MachShipMachShip (TMS) to Azure Data Lake” (TMS) to Azure Data Lake”


## OverviewOverview

### Executive OverviewExecutive Overview

This chapter defines the foundational role of the Staging Area in the NINO System, detailing its critical function in acquiring and securely storing data from the Machship Transport Management System (TMS). Positioned as the initial interface in the NINO data ingestion pipeline, this layer ensures that all external logistics data is validated, secured, and structured before entering downstream transformation processes.This chapter defines the foundational role of the Staging Area in the NINO System, detailing its critical function in acquiring and securely storing data from the Machship Transport Management System (TMS). Positioned as the initial interface in the NINO data ingestion pipeline, this layer ensures that all external logistics data is validated, secured, and structured before entering downstream transformation processes.

The ingestion process is based on an incremental model, where JSON-formatted files containing up to 500 records are exported from Machship and uploaded to the live-zip folder in Azure Blob Storage every two hours. Each batch concludes with the appearance of a control file (tx_0.complete) to signal readiness, while a blank.txt file prevents folder deletion if no data is present.The ingestion process is based on an incremental model, where JSON-formatted files containing up to 500 records are exported from Machship and uploaded to the live-zip folder in Azure Blob Storage every two hours. Each batch concludes with the appearance of a control file (tx_0.complete) to signal readiness, while a blank.txt file prevents folder deletion if no data is present.

The system architecture places a strong emphasis on security and scalability. Data uploads are executed via SAS tokens issued by The system architecture places a strong emphasis on security and scalability. Data uploads are executed via SAS tokens issued by SynfoSynfo, which enable time-limited, permission-scoped access without exposing underlying platform credentials. The token lifecycle and access governance are managed under the supervision of Cora Group. Additionally, the ingestion strategy supports growth in dataset volume and can accommodate changes in cadence or table count with minimal reconfiguration., which enable time-limited, permission-scoped access without exposing underlying platform credentials. The token lifecycle and access governance are managed under the supervision of Cora Group. Additionally, the ingestion strategy supports growth in dataset volume and can accommodate changes in cadence or table count with minimal reconfiguration.

The chapter further introduces comprehensive metadata and schema references, enabling internal teams to navigate and interpret incoming data structures. Key entities include carrier accounts, invoice entries, item references, and tracking records.The chapter further introduces comprehensive metadata and schema references, enabling internal teams to navigate and interpret incoming data structures. Key entities include carrier accounts, invoice entries, item references, and tracking records.

In conclusion, this chapter recommends maintaining the SAS token model as a best practice for secure external data exchange, regularly reviewing ingestion frequency to meet operational demand, and keeping schema documentation up to date for consistent downstream integration. These principles uphold NINO’s reliability, auditability, and data integrity standards.In conclusion, this chapter recommends maintaining the SAS token model as a best practice for secure external data exchange, regularly reviewing ingestion frequency to meet operational demand, and keeping schema documentation up to date for consistent downstream integration. These principles uphold NINO’s reliability, auditability, and data integrity standards.


Specialised Analytical Summary for Information SystemsSpecialised Analytical Summary for Information Systems

Chapter: The Staging Area – Data Import from Chapter: The Staging Area – Data Import from MachShipMachShip to Azure Data Lake to Azure Data Lake

System TitleSystem Title::  
NINO Data Warehouse SystemNINO Data Warehouse System

Technical LeadTechnical Lead::  
Manuel AlvarezManuel Alvarez

Document VersionDocument Version::  
v1.0 – Initial Technical Summary (April 2025)v1.0 – Initial Technical Summary (April 2025)

Date of CompletionDate of Completion::  
21 April 202521 April 2025

Technical KeywordsTechnical Keywords::  
Staging, Azure Data Lake, JSON, Staging, Azure Data Lake, JSON, MachShipMachShip, Blob Storage, Incremental Load, ETL, SAS Token, Power BI, Blob Storage, Incremental Load, ETL, SAS Token, Power BI

Document DescriptionDocument Description::  
This document is a technical chapter from the NINO System Documentation. It describes the architecture, ingestion design, and control mechanisms of the staging layer responsible for initial data acquisition from the external This document is a technical chapter from the NINO System Documentation. It describes the architecture, ingestion design, and control mechanisms of the staging layer responsible for initial data acquisition from the external MachShipMachShip Transport Management System (TMS) into Azure Data Lake Storage. Transport Management System (TMS) into Azure Data Lake Storage.

Technical ReferencesTechnical References::

Azure Documentation (Blob Storage, SAS Tokens)Azure Documentation (Blob Storage, SAS Tokens)

MachShipMachShip File Integration Guidelines File Integration Guidelines

Cora Group Internal IT Security Policies, enforced by Cora Group Internal IT Security Policies, enforced by SynfoSynfo

Technologies UsedTechnologies Used::  
Azure Blob Storage, JSON format, Shared Access Signature (SAS), Azure Data Lake, Azure Blob Storage, JSON format, Shared Access Signature (SAS), Azure Data Lake, SynfoSynfo Access Management, Microsoft Azure Infrastructure Access Management, Microsoft Azure Infrastructure

Technical SummaryTechnical Summary::

The staging layer of the NINO Data Warehouse serves as the primary landing zone for transport and logistics data exported by The staging layer of the NINO Data Warehouse serves as the primary landing zone for transport and logistics data exported by MachShipMachShip (TMS). Using an incremental ingestion pattern, the system collects and stores up to 76 JSON datasets in a structured, scheduled manner within Azure Blob Storage (machship container, live-zip folder). Each JSON file contains no more than 500 records and follows a controlled naming convention (e.g., CarrierAccounts_01.json). (TMS). Using an incremental ingestion pattern, the system collects and stores up to 76 JSON datasets in a structured, scheduled manner within Azure Blob Storage (machship container, live-zip folder). Each JSON file contains no more than 500 records and follows a controlled naming convention (e.g., CarrierAccounts_01.json).

A control file (tx_0.complete) signals the readiness of a batch for downstream processing. A placeholder file (blank.txt) prevents the folder from being deleted when A control file (tx_0.complete) signals the readiness of a batch for downstream processing. A placeholder file (blank.txt) prevents the folder from being deleted when empty. Data is ingested every two hours, although frequency is configurable based on operational needs. The system uses a reliable and secure upload mechanism via SAS tokens, which limits direct user access and enforces strict upload authentication.empty. Data is ingested every two hours, although frequency is configurable based on operational needs. The system uses a reliable and secure upload mechanism via SAS tokens, which limits direct user access and enforces strict upload authentication.

Data files are uploaded by Data files are uploaded by MachShipMachShip, but access to the Azure environment is managed externally by , but access to the Azure environment is managed externally by SynfoSynfo (Cora’s IT support provider), ensuring data isolation and compliance with organisational security standards. The staging layer guarantees scalable ingestion and consistent upstream data availability for ETL processes and Power BI dashboards. (Cora’s IT support provider), ensuring data isolation and compliance with organisational security standards. The staging layer guarantees scalable ingestion and consistent upstream data availability for ETL processes and Power BI dashboards.

Technical MethodologyTechnical Methodology::  
The system follows a modular and scalable ELT architecture. JSON files are processed through scheduled ingestion using Azure-native tools, applying a flexible, file-based control flow with metadata triggers for staging pipeline activation.The system follows a modular and scalable ELT architecture. JSON files are processed through scheduled ingestion using Azure-native tools, applying a flexible, file-based control flow with metadata triggers for staging pipeline activation.

Evaluation CriteriaEvaluation Criteria::

Successful ingestion of all JSON tables in under 5 minutes per batchSuccessful ingestion of all JSON tables in under 5 minutes per batch

100% compliance with naming and control file conventions100% compliance with naming and control file conventions

Zero unauthorised access incidents to blob storage containersZero unauthorised access incidents to blob storage containers

Seamless batch triggering across Power Automate and ADFSeamless batch triggering across Power Automate and ADF

Technical Challenges & SolutionsTechnical Challenges & Solutions::


Technical DiagramsTechnical Diagrams::  
Included in the broader documentation:Included in the broader documentation:

Data Flow Overview: Data Flow Overview: MachShipMachShip → Azure Blob → Azure Blob

Technical ConclusionsTechnical Conclusions::  
The staging component provides a highly secure, automated and modular framework for controlled ingestion of external data. The use of time-triggered pipelines and control files ensures reliability. SAS token management enhances data governance, and incremental loads allow scalability. The staging architecture is foundational to the broader NINO Data Warehouse pipeline.The staging component provides a highly secure, automated and modular framework for controlled ingestion of external data. The use of time-triggered pipelines and control files ensures reliability. SAS token management enhances data governance, and incremental loads allow scalability. The staging architecture is foundational to the broader NINO Data Warehouse pipeline.

AuthorAuthor::  
Angie LópezAngie López


## IntroductionIntroduction

The NINO System’s ingestion lifecycle begins with external data acquisition from Machship, the organisation’s Transport Management System (TMS). This first stage, referred to as the staging area, involves the controlled and secure transfer of structured JSON datasets into Azure Blob Storage. The outcome is a reliable and scalable repository of raw transport data, foundational to all downstream transformations and reporting processesThe NINO System’s ingestion lifecycle begins with external data acquisition from Machship, the organisation’s Transport Management System (TMS). This first stage, referred to as the staging area, involves the controlled and secure transfer of structured JSON datasets into Azure Blob Storage. The outcome is a reliable and scalable repository of raw transport data, foundational to all downstream transformations and reporting processes..

## ObjectiveObjective

To describe the functionality, tools, and safeguards involved in securely receiving JSON-based logistics data from Machship and storing it in Azure Blob Storage, forming the initial staging layer for the NINO Data Warehouse.To describe the functionality, tools, and safeguards involved in securely receiving JSON-based logistics data from Machship and storing it in Azure Blob Storage, forming the initial staging layer for the NINO Data Warehouse.

## ResourcesResources

The following table summarises key The following table summarises key resources and resources and parameters involved in the ingestion process:parameters involved in the ingestion process:

Table Table  SEQ Table \* ARABIC 11. Key Parameters for External Data Ingestion from Machship. Key Parameters for External Data Ingestion from Machship

## System ConsiderationsSystem Considerations

To ensure reliability and adaptability, the import process is designed with the following considerations:To ensure reliability and adaptability, the import process is designed with the following considerations:

To maintain robustness across evolving business needs, the ingestion mechanism includes several safeguards:To maintain robustness across evolving business needs, the ingestion mechanism includes several safeguards:

ScalabilityScalability: The number of tables and datasets is expected to grow as new data domains are integrated.: The number of tables and datasets is expected to grow as new data domains are integrated.

FlexibilityFlexibility: Ingestion intervals are adaptable, allowing for performance tuning based on load or operational constraints.: Ingestion intervals are adaptable, allowing for performance tuning based on load or operational constraints.

Security ControlsSecurity Controls::

Access to Azure Blob Storage is secured via a Access to Azure Blob Storage is secured via a SAS (Shared Access Signature) tokenSAS (Shared Access Signature) token..

Direct access to Azure is not granted to Machship.Direct access to Azure is not granted to Machship.

The SAS token is issued and managed by The SAS token is issued and managed by SynfoSynfo under the direction of Cora. under the direction of Cora.

All access requests must be submitted to All access requests must be submitted to support@synfo.com.ausupport@synfo.com.au  under the supervision of Cora. under the supervision of Cora. SynfoSynfo will create and manage a support ticket for each request. will create and manage a support ticket for each request.

Token PermissionsToken Permissions: Read, Add, Create, Write (as of January 2025; valid until January 2027).: Read, Add, Create, Write (as of January 2025; valid until January 2027).

Like a gate that opens only for authorised couriers, the SAS token enables secure deliveries to the Blob container without revealing the keys to the kingdom.Like a gate that opens only for authorised couriers, the SAS token enables secure deliveries to the Blob container without revealing the keys to the kingdom.


## Data Processing FlowData Processing Flow


right101351Figure Figure  SEQ Figure \* ARABIC 11. Data ingestion process from Machship to Blob (ADL). Data ingestion process from Machship to Blob (ADL)Figure Figure  SEQ Figure \* ARABIC 11. Data ingestion process from Machship to Blob (ADL). Data ingestion process from Machship to Blob (ADL)left34065000

The NINO System uses a structured and reliable approach for data ingestion:The NINO System uses a structured and reliable approach for data ingestion:

Token ProvisioningToken Provisioning: A SAS (Shared Access Signature) token is created by : A SAS (Shared Access Signature) token is created by SynfoSynfo, Cora Group’s IT support partner. This token is then securely provided to Machship., Cora Group’s IT support partner. This token is then securely provided to Machship.

Token UsageToken Usage: Machship does not have direct access to Azure. Instead, data is uploaded via a tokenised URL, ensuring restricted and role-scoped access.: Machship does not have direct access to Azure. Instead, data is uploaded via a tokenised URL, ensuring restricted and role-scoped access.

Token ValidationToken Validation: Upon receiving files, Azure’s token service validates the SAS token before accepting the upload into the storage account.: Upon receiving files, Azure’s token service validates the SAS token before accepting the upload into the storage account.

Data GenerationData Generation: Structured JSON datasets are exported by Machship according to a predefined cadence. Each source table may produce multiple JSON files, with each file containing up to 500 records.: Structured JSON datasets are exported by Machship according to a predefined cadence. Each source table may produce multiple JSON files, with each file containing up to 500 records.

File UploadFile Upload: These files are delivered to Azure Blob Storage. Each one is placed under the appropriate folder, grouped by entity type.: These files are delivered to Azure Blob Storage. Each one is placed under the appropriate folder, grouped by entity type.

Naming ConventionNaming Convention: Files follow a strict naming pattern (e.g., CarrierAccounts_01.json, CarrierAccounts_02.json).: Files follow a strict naming pattern (e.g., CarrierAccounts_01.json, CarrierAccounts_02.json).

Batch CompletionBatch Completion: Once a batch is fully transferred, the presence of tx_0.complete indicates readiness for downstream processing.: Once a batch is fully transferred, the presence of tx_0.complete indicates readiness for downstream processing.


## OutcomeOutcomess

Once the ingestion process completes successfully, the uploaded JSON data becomes accessible for downstream processing by Azure Data Factory pipelines. Each complete batch is validated via the presence of the tx_0.complete file, triggering automated transformation into structured SQL format. This setup ensures traceability, repeatability, and the ability to scale as new entities are added over time.Once the ingestion process completes successfully, the uploaded JSON data becomes accessible for downstream processing by Azure Data Factory pipelines. Each complete batch is validated via the presence of the tx_0.complete file, triggering automated transformation into structured SQL format. This setup ensures traceability, repeatability, and the ability to scale as new entities are added over time.


## AppendicesAppendices

Appendix 1: Appendix 1: Glossary of TermsGlossary of Terms


Appendix 2: Appendix 2: Metadata Inventory & Schema ReferenceMetadata Inventory & Schema Reference

The following table lists the JSON table names along with their corresponding descriptions. These descriptions have been drafted to support internal understanding of each dataset’s function but have not been reviewed or approved by Machship and therefore should not be considered official documentation.The following table lists the JSON table names along with their corresponding descriptions. These descriptions have been drafted to support internal understanding of each dataset’s function but have not been reviewed or approved by Machship and therefore should not be considered official documentation.

Each table listed below has a JSON sample and schema associated with it. You can view this detailed information through the accompanying Excel documentation provided as part of the data dictionary. (See: Each table listed below has a JSON sample and schema associated with it. You can view this detailed information through the accompanying Excel documentation provided as part of the data dictionary. (See: JSON'sTableInventory&SchemaDescriptionJSON'sTableInventory&SchemaDescription))

This resource provides:This resource provides:

Field-level descriptionsField-level descriptions

Sample payload structuresSample payload structures

Schema mappings for each imported entitySchema mappings for each imported entity