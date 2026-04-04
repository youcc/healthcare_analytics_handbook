# Glossary

Healthcare analytics is dense with acronyms. This glossary covers the terms you will encounter most frequently. When a term links to a chapter, that chapter provides deeper context.

---

## Clinical Terms

**ADT** — Admission, Discharge, Transfer. HL7 message type that captures patient movement events in a hospital. A key data source for occupancy and throughput analytics.

**ADE** — Adverse Drug Event. A harm caused by medication use, including errors and side effects. A major target for medication safety analytics.

**CDSS** — Clinical Decision Support System. Software that provides clinicians with patient-specific recommendations at the point of care, often powered by analytics models.

**Chief Complaint** — The patient's stated reason for a visit, recorded at intake. Used to classify encounter types and route care.

**Comorbidity** — The presence of two or more chronic conditions in the same patient. Relevant to risk stratification and quality measurement.

**CPOE** — Computerized Physician Order Entry. The system clinicians use to submit medication and diagnostic orders electronically, replacing handwritten orders.

**DRG** — Diagnosis-Related Group. A classification system that groups hospital inpatient stays by diagnosis and procedure for the purpose of standardizing Medicare payment.

**eMAR** — Electronic Medication Administration Record. Documents when medications are actually given to a patient, as opposed to when they were ordered.

**LOS** — Length of Stay. The number of days a patient spends in a hospital from admission to discharge. A key operational and quality metric.

**SDOH** — Social Determinants of Health. Non-clinical factors (housing, income, education, transportation) that affect health outcomes. Increasingly included in risk models.

---

## Data Standards

**CPT** — Current Procedural Terminology. A code set maintained by the AMA that describes medical procedures and services performed by providers. Used for billing.

**FHIR** — Fast Healthcare Interoperability Resources (pronounced "fire"). An HL7 standard for representing and exchanging healthcare information using modern web APIs (REST/JSON). The current standard for healthcare interoperability.

**HL7** — Health Level Seven. The standards organization that develops healthcare data exchange protocols, including both the legacy v2.x messaging standard and the modern FHIR standard.

**ICD-10** — International Classification of Diseases, 10th Revision. The standard diagnostic code set used in the US for documenting diagnoses on claims. ICD-10-CM is the clinical modification used in the US.

**LOINC** — Logical Observation Identifiers Names and Codes. A universal standard for identifying laboratory and clinical observations (e.g., a specific lab test type).

**NPI** — National Provider Identifier. A unique 10-digit identifier assigned to every healthcare provider in the US. The key join key when working with provider data across datasets.

**RxNorm** — A standardized nomenclature for clinical drugs maintained by the NLM. Used to normalize medication data across EHR systems and pharmacies.

**SNOMED CT** — Systematized Nomenclature of Medicine — Clinical Terms. A comprehensive clinical terminology used to encode clinical concepts in EHRs. More granular than ICD codes.

---

## Payer and Revenue Cycle Terms

**ACO** — Accountable Care Organization. A group of providers who agree to be jointly responsible for the quality and cost of care for a defined patient population under value-based contracts.

**Claim** — A bill submitted by a provider to a payer for services rendered. Claims data is one of the richest sources for population-level analytics.

**Clearinghouse** — An intermediary that translates and routes claims from providers to payers, checking for errors before submission.

**CMS** — Centers for Medicare & Medicaid Services. The federal agency that administers Medicare, Medicaid, and the Children's Health Insurance Program (CHIP). Also sets many quality reporting standards.

**Denial** — A payer's refusal to reimburse a submitted claim. Denial rate and denial reason are key revenue cycle analytics metrics.

**EOB** — Explanation of Benefits. A document sent by a payer to a patient explaining how a claim was processed and what the patient owes.

**FFS** — Fee-for-Service. The traditional payment model where providers are paid per service rendered, regardless of outcome.

**PA** — Prior Authorization. A payer requirement that certain services or medications be approved before they are provided.

**RCM** — Revenue Cycle Management. The end-to-end process of managing claims, payments, and revenue — from patient registration through final payment collection.

**VBC** — Value-Based Care. Payment models that tie reimbursement to quality outcomes and cost efficiency rather than volume of services.

---

## Data and Systems Terms

**ePHI** — Electronic Protected Health Information. Any PHI stored, transmitted, or processed electronically. Subject to HIPAA Security Rule requirements.

**EHR / EMR** — Electronic Health Record / Electronic Medical Record. Often used interchangeably. Technically, an EHR is designed to share data across organizations; an EMR is a single-organization record.

**HIPAA** — Health Insurance Portability and Accountability Act (1996). The primary US federal law governing the privacy and security of health information. See [Chapter 2](ch2/hipaa-data-privacy.md).

**MPI / EMPI** — Master Patient Index / Enterprise Master Patient Index. A database that uniquely identifies patients across multiple systems, resolving duplicate and linked records.

**PHI** — Protected Health Information. Any individually identifiable health information maintained by a covered entity or business associate under HIPAA.

**Payer Mix** — The distribution of a provider's patients by insurance type (Medicare, Medicaid, commercial, self-pay). Affects revenue, reporting requirements, and analytics priorities.

---

## Analytics Terms

**Attribution** — The process of assigning patients to a provider or care team for accountability purposes, particularly in value-based contracts.

**Care Gap** — A recommended preventive service or follow-up that a patient has not received within the expected timeframe. Closing care gaps is a core population health analytics use case.

**Hedis** — Healthcare Effectiveness Data and Information Set. A widely used set of standardized performance measures for health plans, maintained by NCQA. Many quality analytics workflows are built around HEDIS measures.

**Risk Stratification** — Categorizing a patient population by predicted healthcare cost or clinical risk, used to prioritize interventions and allocate care management resources.

**SCD** — Slowly Changing Dimension. A dimensional modeling pattern for handling attributes (like patient address or provider affiliation) that change over time. See [Chapter 5](ch5/dimensional-modeling-for-healthcare-data.md).

**Tuva Project** — An open-source healthcare data transformation framework that provides standardized data models and dbt packages for common healthcare analytics use cases. Referenced throughout this handbook.
