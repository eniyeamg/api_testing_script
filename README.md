# api_testing_script
Identity Verification Automation Suite: A Python-based integration for a vendors workflow, automating secure document authentication, client-side image optimisation, and asynchronous verification polling with Vendor APIs.

This repository contains a production-grade Python integration suite designed to automate and optimise the identity verification (IDV) workflow between the Eni platform and Vendor authentication services.

The Business Challenge
In a high-stakes KYC (Know Your Customer) environment, manual document verification is a bottleneck that leads to high drop-off rates. However, automated APIs often face friction due to:

Payload Failures: Large, unoptimised images causing 413 Request Entity Too Large errors.

Latency: Asynchronous processing times leading to timed-out client sessions.

Compliance Risks: Improper handling of PII (Personally Identifiable Information) and consent logs.

The Solution

This suite acts as a technical bridge that ensures a 99% successful submission rate by handling client-side pre-processing and intelligent API orchestration.

Key Features
Dynamic Image Optimisation: Automatically resizes and compresses document images (Passport/DL) to meet Vendor's strict 10MB limits while maintaining OCR-readable quality.

Asynchronous Polling Engine: A robust polling mechanism with configurable intervals to manage the transition from PROCESSING to VERIFIED without blocking the main application thread.

Compliance-First Architecture: Integrates a dedicated Consent (Portrait) step to align with global data privacy regulations (GDPR/CCPA).

Automated Audit Logging: Generates session-specific JSON and CSV logs for Post-Sales analysis and PoC (Proof of Concept) reporting.


Technical Challenges Overcome

Handling Large Payloads & Latency: Vendor APIs often have strict timeout thresholds and size limits (e.g., 10MB) for image uploads. I implemented a recursive client-side resizing algorithm using PIL to ensure documents are automatically optimised for high-speed transmission without losing OCR-critical resolution.

Managing Asynchronous Processing: Document verification is rarely instantaneous. To prevent blocking the user experience in the Eni application, I developed a robust polling mechanism with exponential backoff. This ensures the system efficiently waits for the Vendor's "VERIFIED" status while proactively managing connection retries.

Data Integrity in Multi-Part Verification: Standard identity workflows require a synchronised "Front + Back + Portrait" submission. I architected the script to handle diverse document types (e.g., single-page Passports vs. two-sided IDs), ensuring that metadata and image headers are correctly mapped to prevent 400-level API validation errors.

Security & Credential Management: To align with Eni's security posture and GitHub best practices, I refactored the legacy hardcoded authentication into a secure environment variable framework, preventing the exposure of sensitive API keys during public demonstrations.


Integration Steps (How it Works):

Identity Creation: Initialises a unique session in the Vendor ecosystem.

Consent Logging: Programmatically records the user's PORTRAIT consent for legal compliance.

Multipart Document Capture: Uploads optimised Front and Back images using multipart/form-data.

Status Polling: Monitors the Vendor engine until a final decision (Success/Fail) is reached.

Proof Archiving: Downloads and archives the final verification proof into a .tar.gz for Eni's internal records.

Prerequisites:
Python 3.8+
Pillow (Image Processing)
requests (API Orchestration )
