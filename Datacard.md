# Dataset Card: NSTG 2022 Structured Clinical Dataset

## Dataset Summary

A machine-readable, schema-unified JSON dataset of 270 clinical conditions derived from the Nigeria Standard Treatment Guidelines (NSTG) 2022. Each condition is represented as a structured JSON object covering clinical presentation, investigations, treatment protocols, differential diagnoses, complications, and prevention measures.

Designed for use in clinical AI/ML pipelines, decision support applications, and medical education tools — particularly in Nigerian and West African clinical contexts.

---

## Dataset Details

### Dataset Description

- **Curated by:** Chisom Rutherford
- **Language(s):** English
- **License:** Creative Commons Attribution 4.0 International (CC BY 4.0)
- **Source document:** Nigeria Standard Treatment Guidelines (NSTG), Federal Ministry of Health, Nigeria, 2022 edition

### Dataset Sources

- **Repository:** https://github.com/[your-username]/nstg-2022-structured
- **Contact:** chisomrutherford@gmail.com

---

## Uses

### Direct Use

- Retrieval-augmented generation (RAG) knowledge bases for clinical assistants
- Structured evaluation sets for clinical NLP models — differential diagnosis, treatment recommendation, complication prediction
- Backend data for offline clinical reference applications
- Flashcard and quiz generation for medical education

### Out-of-Scope Use

- **Direct clinical decision-making.** This dataset has not been validated for clinical use and should not be used as a substitute for verified clinical references or professional judgment.
- **Non-Nigerian clinical contexts without adaptation.** Drug availability, dosing, and protocols reflect Nigerian national guidelines and may not generalize to other settings.
- **Fine-tuning large language models alone.** At 270 records, this dataset is too small for meaningful fine-tuning. It is better suited as an evaluation or retrieval corpus.

---

## Dataset Structure

### Data Instances

Each instance is a JSON file representing one clinical condition. Example:

```json
{
  "condition_name": "Bronchial Asthma",
  "condition_slug": "bronchial-asthma",
  "source": "NSTG 2022",
  "introduction": "...",
  "clinical_features": [
    { "type": "General", "features": ["Episodic dyspnoea", "Wheezing", "..."] }
  ],
  "investigations": ["..."],
  "treatment": {
    "goals": ["..."],
    "non_drug": ["..."],
    "drug": ["..."],
    "adverse_reactions_and_cautions": ["..."],
    "supportive_measures": ["..."]
  },
  "differential_diagnoses": ["..."],
  "complications": ["..."],
  "prevention": ["..."],
  "other_investigations": [],
  "definitive_treatment": [],
  "prognosis": []
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

### Data Splits

No train/validation/test splits. This is a reference dataset, not a supervised learning dataset.

---

## Dataset Creation

### Curation Rationale

Clinical treatment guidelines are authoritative references but are published in unstructured prose, making them inaccessible to programmatic use. This dataset addresses that gap for the NSTG 2022 — a nationally authoritative document covering clinical practice in Nigeria — by imposing a consistent, clinician-designed schema across all 270 conditions.

Prior automated parsing attempts resulted in data loss, particularly for clinically important sections such as resuscitation protocols and subtyped clinical features. The schema and extraction approach were designed specifically to preserve this information.

### Source Data

#### Data Collection and Processing

The dataset was produced through a six-stage clinician-supervised pipeline:

1. **OCR Extraction** — The NSTG 2022 PDF was processed using GPT-4o to extract full text from the source document.
2. **Manual Clinician Curation** — The author, a practicing clinician, reviewed the extracted text for all 270 conditions and manually organised content into labelled clinical sections (clinical features, treatment, investigations, etc.) before any automated structuring was applied.
3. **Automated Splitting** — A script segmented the curated text into individual per-condition files.
4. **Schema Design** — The JSON schema was designed by the author based on clinical judgment, with explicit attention to preserving sections lost in earlier parsing attempts, including resuscitation protocols and subtyped clinical features.
5. **LLM Structuring** — Each condition file was passed to GPT-4o-mini via an asynchronous Python pipeline with a structured prompt mapping content to the Pydantic schema. All 270 conditions were successfully mapped and validated for schema compliance.
6. **Validation** *(in progress)* — A sample of 30 `.txt` files is being verified against the source document, and 30 JSON files are being checked against their corresponding `.txt` files. Results will be reported in v1.1.

The manual curation step in Stage 2 is the primary quality control layer. It distinguishes this pipeline from fully automated extraction approaches and reduces the risk of section misclassification propagating into the final JSON output.

#### Who are the source data producers?

The Federal Ministry of Health, Nigeria. The NSTG 2022 is a public government document.

### Annotations

This dataset contains one significant human contribution beyond schema design: the manual clinician curation in Stage 2, where the author reviewed and organised extracted text for all 270 conditions prior to automated structuring. This is not annotation in the traditional labelling sense, but it represents substantive human judgment applied to the intermediate data.

All final JSON structuring was performed by a large language model.

#### Annotation process

Manual review and section organisation was performed by the author across all 270 conditions. No annotation guidelines or inter-rater reliability measures were applied.

#### Who are the annotators?

Chisom Rutherford (author) — clinician.

### Personal and Sensitive Information

This dataset contains no personal data. All content is derived from a public clinical guideline document.

---

## Evaluation

### Testing Data, Factors & Metrics

#### Testing Data

A two-layer spot-check validation is in progress:

- **Layer 1:** 30 `.txt` condition files verified against the NSTG 2022 source document to assess OCR and curation accuracy.
- **Layer 2:** 30 JSON files verified against their corresponding `.txt` files to assess LLM structuring accuracy.

This two-layer design isolates extraction errors from structuring errors. Results will be reported in v1.1.

#### Factors

Extraction quality may vary by:
- Condition complexity (multi-section conditions with subtypes are harder to extract cleanly)
- Source text formatting (poorly formatted source sections may produce incomplete extractions)
- Field ambiguity (particularly `non_drug` vs `supportive_measures`)

#### Metrics

No formal accuracy metrics have been computed for v1.0. See Limitations.

### Results

Not available for v1.0.

---

## Limitations and Biases

### Known Limitations

**Validation:** This dataset has not been formally validated against the source document. LLM extraction introduces risk of subtle errors including misclassified features, truncated dosing information, and occasional hallucinations in sparse sections. Users should verify critical clinical content against the NSTG 2022 directly.

**Schema ambiguity:** The `non_drug` and `supportive_measures` fields under `treatment` overlap in a subset of conditions. This is a known issue being addressed in v1.1.

**Sparse fields:** `other_investigations`, `definitive_treatment`, and `prognosis` are empty in many entries. This reflects sparse coverage in the source document, not extraction failure.

**Dataset size:** 270 conditions is insufficient for fine-tuning. Suitable for evaluation, retrieval, and application use cases.

### Geographic and Clinical Bias

All content reflects Nigerian national treatment guidelines. Drug formulary, dosing conventions, and clinical protocols are calibrated to the Nigerian healthcare context and may not reflect WHO, NICE, or other international guidelines. This is a feature for Nigerian clinical applications but a limitation for general use.

---

## Citation

### BibTeX

```bibtex
@dataset{rutherford2025nstg,
  author    = {Rutherford, Chisom},
  title     = {NSTG 2022 Structured Clinical Dataset},
  year      = {2025},
  publisher = {GitHub},
  url       = {https://github.com/[your-username]/nstg-2022-structured},
  version   = {1.0}
}
```

---

## Dataset Card Authors

Chisom Rutherford — chisomrutherford@gmail.com

## Dataset Card Contact

chisomrutherford@gmail.com