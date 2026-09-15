# CMS Quality Reporting: Hospital Quality Reporting (HQR) End-to-End Submission Track Documentation
September 2026 Connectathon

## Welcome
This repository contains the documentation, resources, and reference configurations for the CMS Hospital Quality Reporting (HQR) End-to-End Submission Track at the September 2026 HL7 Connectathon. Participants can use this as a guide to follow along the technical demo and/or deep-dive on specific artifacts or resources. 

## Overview
In this demo, you can:
1. Configure the Postman collection and authenticate with the HQR Receiver. 
2. Select one of the provided sample clinical datasets and measures. 
3. Submit the clinical data and `MeasureReport` using the DEQM `$submit-data` operation. 
4. Review the response and validate whether the submission was accepted or returned a FHIR OperationOutcome. 
5. Optionally run `$evaluate-measure` to generate a `MeasureReport` and compare the result with the submitted test data. 

## Important Links
<ul>
  <li>
    <a href="https://confluence.hl7.org/spaces/FHIR/pages/477660436/2026+-+09+CMS+Quality+Reporting+Hospital+Quality+Reporting+HQR+End-to-End+Submission+Track">Connectathon Track Page</a>
  </li>
  <li>
    <a href="https://bellese.github.io/hqr-ig/data-element-testing-guide.html">Draft Implementation Guide</a>
  </li>
  <li>
    <a href="https://fhir-connectathon.test.cms.gov/start?code=99Z6-R66H">Temporary public test environment</a>
    <i> - Note: This environment will only be accessible during the Connectathon.</i>
  </li>
  <li>
    <a href="#">Postman Collection</a>
  </li>
</ul>


## How to Run the Demo Using Postman

### Getting Started
This is a companion Postman collection for the September 2026 HL7 Connectathon [CMS Quality Reporting: Hospital Quality Reporting (HQR) End-to-End Submission Tack](https://confluence.hl7.org/spaces/FHIR/pages/477660436/2026+-+09+CMS+Quality+Reporting+Hospital+Quality+Reporting+HQR+End-to-End+Submission+Track). This demo is meant to illustrate some of the initial concepts and data exchange in the [Draft FHIR Implementation Guide](https://bellese.github.io/hqr-ig/index.html).

### Initial Environment Setup
1. Download the Postman Collection: `Download the HQR_Connectathon_Demo.postman_collection.json` file from the `/postman` directory in this repository to your local machine.
2. Import Into Postman: Open Postman, click the Import button in the top-left header bar, and drag-and-drop the downloaded .json file into the upload window (or click Choose Files) to load the requests into your active workspace.
3. [Navigate to the demo environment](https://fhir-connectathon.test.cms.gov/start?code=99Z6-R66H) and get credentials by following the instructions on the page.
4. Navigate to the "Variables" section of this collection in the top bar. Populate the variable url field with `https://fhir-connectathon.test.cms.gov`.
5. Click the "`POST token`" request in Postman and populate the "Authorization" tab with "Basic Auth" and set the username and password to your `clientid` and `clientSecret` that you collected from the demo environment interface.
6. Run the `POST` token now. This should set your `access_token` variable so you can freely call the FHIR sandbox server. Note: In this sample scenario, the FHIR sandbox is both the "submitter system" and the "HQR receiver" that is mentioned in the Implementation Guide.

Postman variables needed to accomplish the task:

| Variable | Purpose |
| -------- | -------- |
| `url` | Base URL endpoint for the HQR Receiver service. | 
| `clientId` | OAuth 2.0 client ID used to identify the requesting application. |
| `clientSecret` | Confidential secret key paired with clientId to request access tokens. | 
| `accessToken` | OAuth 2.0 Bearer token included in request headers for authorization. |
| `measureID` | Unique identifier for the CMS measure being submitted or evaluated. |

### Preparing the Sample Data
We have prepared sample, synthetic clinical data for three distinct measures for the purposes of this demo. You can find and download the clinical datasets on the [Downloads page of the Implementation Guide](https://bellese.github.io/hqr-ig/downloads.html).

- CMS71 - (STK-3): Anticoagulation Therapy for Atrial Fibrillation Flutter - It assess ischemic stroke patients who are prescribed or continuing anticoagulation therapy at hospital discharge.
- CMS506: Safe Use of Opioids - Tracks the proportion of hospital patients aged 18 and older who are prescribed two or more opioids or an opioid and a benzodiazepine concurrently at discharge.
- EXMConnectathonSetp2026Simple: This is a sample measure that is designed with as much simplicity as possible to allow for the easiest demonstration of the end-to-end data flow. 

Once the data is downloaded, copy the contents of the file, then open the `POST $submit-data` section in the left navigation of Postman and paste the data into , in the "Body" tab to prepare for submission.

### Data Submission
In the `POST $submit-data` section, update the variable `measureId` to match the canonical ID or resource ID of the measure you are submitting.

Set the `measureId` variable to the identifier for the measure being submitted. For example, when using `EXMConnectathonSetp2026Simple` simple measure: 
`measureId` = `EXMConnectathonSetp2026Simple`

Once everything has been correctly entered, click "Send" to submit the request.

### Response
The response validates conformance and gives a detailed message if the data is invalid.

**HTTP 200 OK / 201 Created (Success):**
- The payload passed all profile validation, terminology, and structural checks.
- The response body contains the persisted `MeasureReport` resource complete with server-assigned IDs and indexing metadata.

**HTTP 400 Bad Request / 422 Unprocessable Entity (Validation Error):** 
- The payload failed schema validation, profile alignment checks, or slicing rules.
- The response body returns a FHIR OperationOutcome resource. Inspect `OperationOutcome.issue[].diagnostics` and `OperationOutcome.issue[].expression` to locate the exact JSON path and rule violation (e.g., missing mandatory `period.start` or mismatched code system). Note: A potential alternative is "Upload this data to the sandbox using the "POST resource batch" call in the collection". This is an open discussion topic.

### Additional Steps
If you want to compare the test data `MeasureReport` to a live generated `MeasureReport` on the population, use the `GET $evaluate-measure` request and update the `measureId` variable.

The `helpers` folder has additional that may be helpful to explore/navigate the FHIR server.

## Open Questions + How to Get Involved
[Content TBD]


