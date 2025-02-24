# Assignment: Batch File-Processing Feature

## Context

The application displays a dataset in a tabular format. Users can upload a **batch of files**; each file must be processed to produce a new record in the dataset.

### Available Infrastructure

- **Postgres**  
- **Amazon S3**  
- **Kafka**  
- **Redis**  
- **Dolphin Scheduler**  
- **Preview Service**  
- **Self-hosted OCR**  
- **Amazon Textract**

## Feature Requirements

When users upload files in bulk, each file goes through **three pipelines**:

1. **Main Pipeline**  
   - The file is persisted and linked to the dataset.  
   - The text content of the file is extracted. If needed self-hosted ocr is used.  
   - If self-hosted ocr fails or is unavailable, the system should use Amazon Textract instead.  
   - The extracted text is sent to an AI extraction service, which generates a dataset record linked to that file.

2. **Preview Generation**  
   - Certain file types (for example, documents) should have an in-app preview generated.

3. **Thumbnail Generation**  
   - Whenever possible, generate a thumbnail for the file.

## API Requirements

1. **Batch Upload**  
   An endpoint to upload multiple files at once, initiating processing.

2. **Status Listing**  
   An endpoint to list files in a dataset, showing their current processing stage and any artifacts (e.g., record, preview, thumbnail).

3. **Removal**  
   An endpoint to remove files (and their generated records) once processing is completed or no longer needed.

## Key Challenges

1. **Concurrent Uploads**  
   Large numbers of files can be uploaded simultaneously.

2. **Fair Resource Usage**  
   No single user or upload should block others.

3. **Fallback**  
   If the primary text-extraction mechanism fails, the system should fall back to Amazon Textract.

4. **Service Downtime**  
   If any service is temporarily unavailable, the process should handle it gracefully.

5. **Visibility**  
   Users must see each file’s status at any point in the pipelines.

## Monitoring & Logging

- A minimal approach should be in place to track system performance (e.g., error rates, processing times).  
- Basic logs for critical events (e.g., failures, retries) should be collected.

## Database Schema Design

Candidates should propose a **basic database schema**. This includes details on how you plan to store:
- File references
- Processing status
- Extracted text
- Any generated artifacts (e.g., preview, thumbnail)

## Timeline & MVP

- The entire solution must be implementable in **no more than two weeks** for an initial MVP.  
- It should be **iterable** for future enhancements.

## Instructions for the Candidate

Design a **high-level architecture** that satisfies these requirements. Demonstrate how you will:

- Handle the **three pipelines** (main processing, previews, and thumbnails)  
- Deal with **fallback** for text extraction  
- Manage **concurrency** and **service downtime**  
- Propose a **basic database schema** for storing files, extracted data, and artifacts  
- Incorporate **basic monitoring and logging**
