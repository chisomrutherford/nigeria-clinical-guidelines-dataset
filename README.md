# NSTG 2022 Structured Clinical Dataset

A machine-readable JSON dataset of 270 clinical conditions extracted and structured from the **Nigeria Standard Treatment Guidelines (NSTG) 2022**, a public clinical reference document published by the Federal Ministry of Health, Nigeria.

---

## Overview

This dataset converts the NSTG 2022 into a consistent, schema-driven JSON format designed for use in clinical applications, AI/ML pipelines, and medical education tools.

Each condition is represented as a single JSON file with a unified schema covering clinical features, investigations, treatment protocols, differential diagnoses, complications, and prevention.

---

## Dataset at a Glance

The dataset is derived from the Nigeria Standard Treatment Guidelines (NSTG) 2022 and contains 270 clinical conditions. It is structured in JSON format, with each condition represented as a separate file, and follows schema version 1.0.

The data was created using a clinician-supervised pipeline that involved optical character recognition (OCR), followed by manual curation, and subsequent structuring using a large language model. Validation is currently ongoing through spot-checking, with additional details outlined in the limitations section.

The dataset is released under the Creative Commons Attribution 4.0 (CC BY 4.0) license.

## Schema

Each JSON file follows this structure:

```json
{
  "condition_name": "string",
  "condition_slug": "string",
  "source": "NSTG 2022",
  "introduction": "string",
  "clinical_features": [
    {
      "type": "string (e.g. General, Severe, Paediatric)",
      "features": ["string"]
    }
  ],
  "investigations": ["string"],
  "treatment": {
    "goals": ["string"],
    "non_drug": ["string"],
    "drug": ["string"],
    "adverse_reactions_and_cautions": ["string"],
    "supportive_measures": ["string"]
  },
  "differential_diagnoses": ["string"],
  "complications": ["string"],
  "prevention": ["string"],
  "other_investigations": ["string"],
  "definitive_treatment": ["string"],
  "prognosis": ["string"]
}
```

### Data Fields

| Field | Type | Description |
|---|---|---|
| `condition_name` | string | Full name of the clinical condition |
| `condition_slug` | string | URL-safe identifier |
| `source` | string | Always "NSTG 2022" |
| `introduction` | string | Definition and epidemiological context |
| `clinical_features` | array of objects | Typed lists of signs and symptoms |
| `investigations` | array of strings | Recommended diagnostic workup |
| `treatment.goals` | array of strings | Stated therapeutic objectives |
| `treatment.non_drug` | array of strings | Non-pharmacological interventions |
| `treatment.drug` | array of strings | Pharmacological treatments with dosing |
| `treatment.adverse_reactions_and_cautions` | array of strings | Drug safety notes |
| `treatment.supportive_measures` | array of strings | Supportive care measures |
| `differential_diagnoses` | array of strings | Conditions to distinguish from |
| `complications` | array of strings | Known disease complications |
| `prevention` | array of strings | Preventive measures |
| `other_investigations` | array of strings | Additional workup (sparse) |
| `definitive_treatment` | array of strings | Definitive interventions (sparse) |
| `prognosis` | array of strings | Outcome information (sparse) |



## Intended Use Cases

- **Clinical AI/ML**: Structured ground truth for evaluating models on clinical reasoning tasks, differential diagnosis, or treatment planning in West African clinical contexts.
- **Clinical applications**: Backend data source for decision support tools, offline reference apps, or community health worker tools.
- **Medical education**: Flashcard systems, quiz generators, and study tools for medical students training under Nigerian or similar guidelines.
- **Research**: Comparative analysis of treatment guideline content, NLP benchmarking, or health informatics research.

---

## Extraction Methodology

This dataset was produced through a clinician-supervised, multi-stage pipeline designed to preserve fidelity to the source document at each step.

**Stage 1 — OCR Extraction**
The NSTG 2022 PDF was processed using GPT-4o to extract raw text, producing a full-text representation of the source document.

**Stage 2 — Manual Clinician Curation**
The author clinically reviewed the extracted text for each of the 270 conditions and manually organised content into labelled sections (clinical features, treatment, investigations, etc.). This step was performed before any automated structuring, ensuring that the intermediate text representation was clinically coherent and section boundaries were accurate.

**Stage 3 — Automated Splitting**
A script split the curated text into individual per-condition files, one per clinical condition.

**Stage 4 — Schema Design**
The JSON schema was designed by the author based on clinical judgment about how treatment guideline content is structured.

**Stage 5 — LLM Structuring**
Each condition file was passed to GPT-4o via an asynchronous Python pipeline, with a structured prompt mapping curated text content to the defined Pydantic schema. Outputs were validated for schema compliance.

**Stage 6 — Validation** *(in progress)*
A sample of 40 condition `.txt` files were verified against the NSTG 2022 source document to assess OCR and curation accuracy. A further 40 JSON files were  checked against their corresponding `.txt` files to assess structuring accuracy. 

---

## Geographic and Clinical Scope

This dataset reflects treatment recommendations from a Nigerian national guideline. Drug availability, dosing conventions, and clinical protocols may differ from international guidelines. It is best suited for applications targeting Nigerian or similar West African clinical contexts.
---

## Citation

### BibTeX

```bibtex
@dataset{rutherford2025nigeria,
  author = {Rutherford, Chisom},
  title = {Nigeria Clinical Guidelines Dataset},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/chisomrutherford/nigeria-clinical-guidelines-dataset}
}
```


## License

This dataset is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.

You are free to use, share, and adapt this dataset for any purpose, including commercial use, provided appropriate credit is given.

---

## Contact

**Chisom Rutherford**
Email: chisomrutherford@gmail.com
LinkedIn: [Chisom Rutherford](https://www.linkedin.com/in/chisomrutherford/)

Feedback, corrections, and collaboration enquiries are welcome.
