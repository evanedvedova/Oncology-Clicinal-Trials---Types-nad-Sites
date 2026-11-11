# Oncology-Clicinal-Trials---Types-nad-Sites
What are the most heavily researched cancer types in clinical studies? And where? The report is designed to provide an accessible overview of oncology research activity and highlight geographical and temporal patterns.

## 1. Project Overview

This Power BI project provides an overview of the global oncology clinical trial landscape, focusing on the distribution of studies by cancer type, temporal trends, geographical distribution, and leading clinical trial sites.

The objective is to explore key characteristics of oncology clinical research and answer the following questions:

- Which cancer types are most frequently investigated in clinical trials?
- How has the number of oncology clinical trials evolved over time?
- Which countries account for the largest number of oncology clinical trials?
- Which clinical trial sites are the most frequently represented?

---

## 2. Data Source

- **Source:** AACT (Aggregate Analysis of ClinicalTrials.gov)
- **Provider:** Clinical Trials Transformation Initiative (CTTI)
- **Website:** https://aact.ctti-clinicaltrials.org/
- **Format:** Pipe-delimited (`|`) text files

AACT is a publicly available relational database containing data from ClinicalTrials.gov. It provides structured information about registered clinical studies, including study characteristics, conditions, interventions, locations, and participating facilities.

---

## 3. Data Transformation (Power Query)

All data preparation was performed in Power Query using the Advanced Editor.

The transformations focus on identifying oncology-related clinical trials and simplifying the original AACT data structure for analytical purposes.

### 3.1 Oncology Trial Identification (`oncology_only_nct_id`)

The `browse_conditions.txt` table was used to identify oncology-related studies.

Transformation steps:

1. Import `browse_conditions.txt`.
2. Promote headers and enforce appropriate data types.
3. Create an `Is_Oncology` flag using keyword-based text matching against the `downcase_mesh_term` field.
4. Classify a study as oncology-related if its condition text contains at least one of the following keywords:

   `cancer`, `carcinoma`, `adenocarcinoma`, `sarcoma`, `lymphoma`, `leukemia`, `myeloma`, `melanoma`, `glioma`, `blastoma`, `mesothelioma`, `neoplasm`, `malignan`.

5. Filter records where `Is_Oncology = true`.
6. Remove unnecessary columns.
7. Deduplicate records based on `nct_id`.

**Result:** A distinct list of clinical trial identifiers classified as oncology-related.

**Methodological note:** The conditions table was selected because it offers a relatively efficient way to identify oncology-related studies through structured condition terms. This approach provides a practical and reproducible starting point for exploratory analysis.

However, the keyword-based identification has certain limitations. It depends on the terminology available in the condition records and the predefined keyword list. Consequently, studies with less explicit or differently classified oncology-related conditions may be missed, while some studies may be included based on broad or overlapping terminology. The resulting dataset should therefore be interpreted as an operational approximation of the oncology clinical trial landscape rather than an exhaustive or clinically validated classification.

### 3.2 Studies Table (`studies`)

Transformation steps:

1. Import `studies.txt`.
2. Promote headers and apply appropriate data types.
3. Remove unused metadata columns.
4. Join with `oncology_only_nct_id` using `nct_id`.
5. Retain analytically relevant fields.

**Result:** A cleaned fact table containing oncology-related clinical trials only.

### 3.3 Interventions Table (`interventions`)

Transformation steps:

1. Import `interventions.txt`.
2. Promote headers and enforce appropriate data types.
3. Join with `oncology_only_nct_id`.

**Result:** Structured intervention data associated with oncology-related clinical trials.

### 3.4 Sponsors Table

Transformation steps:

1. Import `sponsors.txt`.
2. Promote headers and enforce appropriate data types.
3. Join with `oncology_only_nct_id`.

**Result:** Structured sponsor data associated with oncology-related clinical trials, including lead institute.

### 3.5 Countries

Transformation steps:

1. Import `countries.txt`.
2. Promote headers and enforce appropriate data types.
3. Join with `oncology_only_nct_id`.

**Result:** Structured countries data associated with oncology-related clinical trials.
---

## 4. Data Model

The Power BI dataset uses a relational data model.

- **Model type:** Relational
- **Primary identifier:** `nct_id`
- **Core table:**
  - `studies`

Single-direction relationships are used to maintain predictable filter propagation and model stability.

The `nct_id` serves as the common study identifier connecting the analytical tables.

---

## 5. Key Findings & Results

The following sections summarize the main insights identified in the Power BI report.

### 5.1 Clinical Trials by Cancer Type

<img width="480" height="349" alt="image" src="https://github.com/user-attachments/assets/bd3d073f-8095-4641-8027-9a9dfa79a9c3" />

*Figure 1. Distribution of oncology clinical trials by cancer type.*

Breast cancer represents the most frequently investigated cancer type, with approximately **8,900 clinical trials**.

The next most frequently represented cancer types are:

- **Prostate cancer:** 4,440 trials
- **Colorectal cancer:** 3,505 trials

These results indicate a substantial concentration of oncology research activity in these three cancer areas.

### 5.2 Clinical Trials Over Time

<img width="474" height="358" alt="image" src="https://github.com/user-attachments/assets/d2c07440-8ae6-4822-b967-0598be63d2d1" />

*Figure 2. Temporal development of oncology clinical trials based on study start date.*

The temporal analysis reveals several notable patterns.

- A decline in the number of newly started clinical trials is visible after 2008.
- The decline coincides with the period of the global financial crisis; however, the available data alone do not establish a causal relationship.
- Another decrease is visible in recent years, particularly from 2024 onwards.

The recorded figures for recent years are:

| Start year | Number of studies |
|---|---:|
| 2023 | 1,324 |
| 2024 | 1,307 |
| 2025 | 1,023 |
| 2026 | 1,049 |

The figures suggest a recent decrease in the number of oncology trials. However, the 2026 figure should be interpreted with particular caution, as the year is still in progress and registrations or study start dates may be updated retrospectively.

Similarly, the 2025 figure may be affected by reporting delays and incomplete data coverage.

### 5.3 Geographical Distribution of Clinical Trials

<img width="505" height="352" alt="obrazek" src="https://github.com/user-attachments/assets/581dcbbc-eac0-4c21-a615-11e12056c1f5" />

*Figure 3. Geographical distribution of oncology clinical trials by country.*

The United States dominates the geographical distribution of oncology clinical trials, with **45,451 studies**.

China ranks second, with **16,990 studies**.

This substantial difference highlights the leading position of the United States in the geographical distribution of registered oncology research, followed by China.

### 5.4 Leading Clinical Trial Sites

<img width="474" height="376" alt="image" src="https://github.com/user-attachments/assets/d4c786d7-0bda-432b-a7fb-3bc7a2fcdd9e" />

*Figure 4. Six most frequently represented clinical trial sites in the dataset.*

The analysis of the six leading clinical trial sites reveals a strong representation of institutions from both the United States and China.

Key observations:

- US institutions occupy three of the top six positions, including ranks 1, 2, 3 and 6.
- Chinese universities occupy the third and fourth positions.
- This distribution demonstrates the prominent role of major academic and research institutions in oncology clinical research.

---


## 7. Validation

In order to validate at least some outcomes of this exploratory study, the comparison with the published data was executed. Phesi’s annual “world’s most studied diseases” report, published 8 January 2026 and covering 2025, analyzed all recruiting clinical trials for all diseases, and breast cancer was the most studied disease for the fifth year running. This fiinding consistent with result of this study.

https://www.manufacturingchemist.com/breast-cancer-world-s-most-studied-disease-phesi

---

## 6. Limitations & Considerations

Several methodological considerations should be taken into account when interpreting the results:

- **Keyword-based identification:** Oncology studies are identified using a predefined list of keywords in the `downcase_mesh_term` field. This may result in both false positives and false negatives.
- **Condition classification:** The analysis relies on the condition table terminology available in AACT and does not include independent clinical validation of each study.
- **Country attribution:** Clinical trials may involve multiple countries and locations. Country-level counts therefore depend on how study locations are represented and aggregated.
- **Site attribution:** A single clinical trial may involve multiple sites. Site-level counts reflect the representation of facilities in the source data and should not necessarily be interpreted as the number of unique trials conducted exclusively at a given institution.
- **Recent-year data:** The most recent years may be affected by incomplete reporting, registration delays, and ongoing updates to ClinicalTrials.gov records.
- **Study start date:** Temporal trends are based on the recorded study start date rather than the registration or publication date.

The results should be interpreted as an **exploratory overview** of registered oncology clinical research rather than a comprehensive measure of all oncology research activity worldwide.

---



## 8. Conclusion

This Power BI project provides an exploratory overview of the global oncology clinical trial landscape.

The analysis identifies breast cancer as the most frequently investigated cancer type, highlights the United States and China as the leading countries by trial representation, and reveals changes in the number of registered oncology studies over time, with main peaks in 2008 and 2024.

Overall, the report offers a structured view of oncology clinical research activity and provides a foundation for further analysis of therapeutic areas, geographical patterns, and research trends.
