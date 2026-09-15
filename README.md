# CMS Quality Reporting: Hospital Quality Reporting (HQR) End-to-End Submission Track Documentation
September 2026 Connectathon

## Welcome
This repository contains the documentation, resources, and reference configurations for the CMS Hospital Quality Reporting (HQR) End-to-End Submission Track at the September 2026 HL7 Connectathon. Participants can use this as a guide to follow along the technical demo and/or deep-dive on specific artifacts or resources. 

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
1. [Navigate to the demo environment](https://fhir-connectathon.test.cms.gov/start?code=99Z6-R66H) and get credentials by following the instructions on the page.
2. Navigate to the "Variables" section of this collection in the top bar. Populate the variable url field with `https://fhir-connectathon.test.cms.gov/`.
3. Click the "`POST token`" request in Postman and populate the "Authorization" tab with "Basic Auth" and set the username and password to your `clientid` and `clientSecret` from Step 1.
4. Run the `POST` token now. This should set your `access_token` variable so you can freely call the FHIR sandbox server. Note: In this sample scenario, the FHIR sandbox is both the "submitter system" and the "HQR receiver" that is mentioned in the Implementation Guide.

### Preparing the Sample Data
We have prepared sample, synthetic clinical data for three distinct measures for the purposes of this demo. You can find and download the clinical datasets on the [Downloads page of the Implementation Guide](https://bellese.github.io/hqr-ig/downloads.html).

- CMS71 - (STK-3): Anticoagulation Therapy for Atrial Fibrillation Flutter - It assess ischemic stroke patients who are prescribed or continuing anticoagulation therapy at hospital discharge.
- CMS506: Safe Use of Opioids - Tracks the proportion of hospital patients aged 18 and older who are prescribed two or more opiods or an opiod and a benzodiazepine concurrently at discharge.
- EXMConnectathonSetp2026Simple: This is a sample measure that is designed with as much simplicity as possible to allow for the easiest demonstration of the end-to-end data flow.

Once the data is downloaded, copy the contents of the file, then open the `POST $submit-data` section in the left navigation of Postman and paste the data into , in the "Body" tab to prepare for submission.

### Data Submission
To submit your generated clinical data and `MeasureReport` payload to the HQR Receiver, this demo implements the DEQM `$submit-data` push operation via HTTP `POST`.

<img width="946" height="91" alt="Screenshot 2026-09-15 at 9 23 06 AM" src="https://github.com/user-attachments/assets/0bdc2761-edd1-496e-b991-d4c65e14f796" />

In the `POST $submit-data` section, update the variable `measureId` to match the Measure resource tied to the `MeasureReport`.

Once everthing has been correctly entered, click "Send" to submit the request.

### Response
The response validates conformance and gives a detailed message if the data is invalid.

Note: A potential alternative is "Upload this data to the sandbox using the "POST resource batch" call in the collection". This is an open discussion topic. - Ask Karrie (maybe link to this open question)

### Additional Steps
If you want to compare the test data `MeasureReport` to a live generated `MeasureReport` on the population, use the `GET $evaluate-measure` request and update the `measureId` variable.

The `helpers` folder has additional that may be helpful to explore/navigate the FHIR server.


