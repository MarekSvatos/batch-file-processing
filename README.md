# Assignment: Modular Data Pipeline Infrastructure

## Overview

Design and implement a modular data pipeline system where files are accepted via a Main-Backend REST API and then submitted to processing pipelines. The Main-Backend integrates an AI Extraction service that leverages OpenAI.

## Context

Users will upload files through the Main-Backend REST API. Each file is processed through one or more pipelines to generate dataset records and artifacts (e.g., previews and thumbnails). The pipelines are defined by flow logic that handles various outcomes (success or failure). Pipelines should be dynamically configurable in e.g. yaml.

## Available Infrastructure

- **Postgres:** For storing file metadata and pipeline states.
- **Amazon S3:** For file storage.
- **Kafka/Redis:** For messaging and coordination.
- **Preview Service, Self-Hosted OCR, Amazon Textract:** For file content extraction.
- **Main-Backend:**
  - Accepts file uploads via a REST API.
  - Integrates the AI Extraction service that calls OpenAI.

## Feature Requirements

1. **Modular Pipeline Processing**
   - **Flow-Driven Design:** Pipelines are defined by their flow logic, outlining the processing steps and transitions.
   - **Example Pipeline Flows:**
     - **AI Record Extraction Pipeline Flow:**
       1. **Initial Extraction (Tika):**  
          Attempt to extract text using Tika. If successful, proceed directly to record extraction.
       2. **Fallback to Self-Hosted OCR:**  
          If Tika fails, use Self-Hosted OCR to extract text.
       3. **Fallback to Amazon Textract:**  
          If OCR fails, use Amazon Textract to extract the text.
       4. **Record Extraction:**  
          With the extracted text, call the AI Extraction service (via OpenAI) to generate the final record.
     - **File View Components Pipeline Flow:**
       1. **Preview Generation:**  
          Generate an in-app preview.
       2. **Thumbnail Generation:**  
          Regardless of the preview result, generate a thumbnail to complete the pipeline.

2. **File Processing Workflow**
   - Files are ingested by the Main-Backend through its REST API and submitted to the appropriate pipelines based on file type and processing needs.
   - The integrated AI Extraction service processes the extracted text to create records.

3. **API Requirements**
   - **File Upload Endpoint:**  
     A REST endpoint that accepts file uploads (single or multiple) and submits them to the processing pipelines.
   - **Pipeline State Endpoint:**  
     A REST endpoint that returns the current state of the processing pipeline for each file, including the active step and any generated artifacts.

4. **Key Challenges**
   - Managing concurrent uploads and processing.
   - Ensuring fair resource usage.
   - Handling fallback mechanisms and service downtime gracefully.
   - Providing real-time visibility into processing statuses.
   - Monitoring and logging key events and metrics.

5. **Database Schema Design**
   - **Files:**  
     Store file metadata, storage references, and the current processing state.
   - **Pipelines:**  
     Store configuration details and the state of each processing pipeline.

6. **Timeline & MVP**
   - The solution should be designed to enable an initial MVP within two weeks, with room for iterative enhancements and the addition of new pipelines.

## Instructions for the Candidate

- Present a high-level architecture showing file ingestion, pipeline dispatch, and status tracking.
- Provide a simple schema for storing file and pipeline information.
- Briefly explain your approach to managing flow transitions and fallback mechanisms.
