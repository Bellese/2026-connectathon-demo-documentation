# CMS Quality Reporting: Hospital Quality Reporting (HQR) End-to-End Submission Track Documentation
September 2026 Connectathon

## Welcome
This repository contains the documentation, resources, and reference configurations for the CMS Hospital Quality Reporting (HQR) End-to-End Submission Track at the September 2026 HL7 Connectathon. Participants can use this as a guide to follow along the technical demo and/or deep-dive on specific artifacts or resources. 

## Important Links
<ul>
  <li>
    <a href="https://confluence.hl7.org/spaces/FHIR/pages/477660436/2026+-+09+CMS+Quality+Reporting+Hospital+Quality+Reporting+HQR+End-to-End+Submission+Track" target="_blank">Connectathon Track Page</a>
  </li>
  <li>
    <a href="https://bellese.github.io/hqr-ig/data-element-testing-guide.html" target="_blank">Draft Implementation Guide</a>
  </li>
  <li>
    <a href="https://fhir-connectathon.test.cms.gov/start?code=99Z6-R66H" target="_blank">Temporary public test environment</a>
    <i> - Note: This environment will only be accessible during the Connectathon.</i>
  </li>
  <li>
    <a href="#" target="_blank">Download the Postman Collection</a>
  </li>
</ul>


## Overview & Architecture
The demo environment is a lightweight, synchronous end-to-end data exchange loop to test core technical assumptions around authentication, payload packaging, profile validation, and report delivery for CMS dQM submissions.

### 1. Authentication
Uses SMART Backend Services (OAuth 2.0 client credentials grant) exchanging a signed JWT assertion (private_key_jwt) for a short-lived Bearer access token.

### 2. Submission
POST request using the DEQM $submit-data operation containing a FHIR Parameters resource wrapping a MeasureReport and supporting resources.

### 3. Validation & Persistence
HAPI FHIR JPA Server (v8.14),Validates profiles against loaded DEQM/QI-Core NPM packages and persists valid resources to PostgreSQL.

### Response Delivery / Reporting
(v8.14) backed by a PostgreSQL database validates incoming resources against DEQM/QI-Core NPM conformance packages and persists valid records.
