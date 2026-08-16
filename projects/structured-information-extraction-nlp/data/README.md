# Structured Information Extraction with NLP

An NLP project focused on transforming unstructured operational text into structured, machine-readable information using spaCy Named Entity Recognition, domain-specific rules, linguistic patterns and JSON output.

The project goes beyond basic entity detection by evaluating how reliably an extraction pipeline transfers from controlled development examples to previously unseen operational language.

## Project Objective

The objective is to convert short operational communications into structured records containing:

- person
- organisation
- location
- date
- time
- event type
- follow-up action
- relevant quantity

The project follows an iterative extraction and evaluation process:

1. Establish a baseline using pretrained spaCy Named Entity Recognition.
2. Identify entity recognition errors.
3. Add domain-specific entity rules.
4. Build schema-level extraction logic.
5. Evaluate the pipeline on a controlled development corpus.
6. Test the initial pipeline on unseen operational text.
7. Analyse generalisation failures.
8. Improve the extractor using broader linguistic patterns.
9. Freeze the improved pipeline.
10. Evaluate it on an independent final holdout set.
11. Convert the extracted information into structured JSON.

## Data

The project uses a **synthetic operational text corpus** created specifically for the information extraction task.

The texts simulate realistic business communications involving:

- service incidents
- customer escalations
- system outages
- staffing shortages
- delivery disruptions
- operational meetings
- maintenance events
- follow-up actions

No confidential, personal or proprietary company data is used.

### Development Corpus

The initial controlled corpus contains **12 operational texts**.

Reference annotations were manually defined before evaluation, creating a gold-standard structured record for each text.

### Robustness Set

A separate set of **6 previously unseen texts** was created to expose weaknesses in the initial extraction logic.

This set was subsequently used for error analysis and pipeline improvement and therefore was **not treated as the final independent test set**.

### Final Holdout Set

A final set of **6 additional unseen texts** was created after the improved extraction logic had been defined.

The V2 pipeline was frozen before this final evaluation, and no rules were modified after seeing the holdout results.

## Target Schema

Each text is transformed into the following structure:

```json
{
  "id": "HT001",
  "person": "Owen Brooks",
  "organisation": "RedStone Logistics",
  "location": "Prague",
  "date": "28 August 2026",
  "time": "07:50",
  "event_type": null,
  "action": "restart the platform and confirm recovery by 09:00",
  "quantity": null
}
```

Missing information is represented explicitly as `null` rather than inferred.

## Baseline Named Entity Recognition

The first stage used the pretrained spaCy `en_core_web_sm` model without custom rules.

The generic model successfully detected many people, locations, dates and organisations, but also produced several important classification errors.

Examples included:

- BluePeak Retail classified as a person
- Meridian Support classified as a person
- Apex Mobility classified as a geographical location
- Helena Costa classified as an organisation
- Porto classified as a person
- Valencia classified as a person
- Felix Bauer classified as an organisation
- Atlas Digital classified as a person
- 10:45 classified as a cardinal number instead of a time
- Orion Systems not detected
- SilverLine Distribution not detected

The baseline also extracted linguistically valid entities that were not necessarily the primary information required by the operational schema.

These results showed that generic Named Entity Recognition alone was insufficient for reliable structured operational extraction.

## Domain-Specific Entity Rules

A spaCy `EntityRuler` was added before the pretrained NER component.

The project introduced **34 explicit entity patterns** covering known:

- people
- organisations
- locations

These rules corrected the major entity-type errors observed in the development corpus while allowing pretrained spaCy NER to remain part of the pipeline.

The custom pipeline successfully corrected examples such as:

- BluePeak Retail → ORG
- Meridian Support → ORG
- Apex Mobility → ORG
- Helena Costa → PERSON
- Porto → GPE
- Valencia → GPE
- Felix Bauer → PERSON
- Atlas Digital → ORG

It also recovered previously missed organisations including Orion Systems and SilverLine Distribution.

## Schema-Level Extraction

Entity recognition was combined with additional extraction logic for information that could not be obtained reliably from NER alone.

The pipeline includes:

- regular-expression based date extraction
- independent clock-time detection
- filtering of deadline times such as `by 11:00`
- selection of the new date in rescheduled events
- organisation recovery from sentence structure
- location recovery from operational prepositions
- correction of location/person conflicts
- event categorisation using semantic triggers
- follow-up action extraction
- operational quantity extraction

This hybrid approach separates **entity detection** from **field selection and operational interpretation**.

## Controlled Development Evaluation

The complete initial pipeline achieved exact agreement with the manually defined annotations on the 12-text development corpus.

Results:

- **96/96 individual fields correctly extracted**
- **100.00% field-level accuracy**
- **12/12 records completely correct**

However, this corpus had also been used during pipeline development.

The 100% result therefore demonstrates successful implementation of the target schema, but it does **not** demonstrate generalisation to unseen language.

## Initial Robustness Evaluation

The original extraction pipeline was then applied to six previously unseen operational texts without modifying its rules.

Performance fell substantially:

| Field | Accuracy |
|---|---:|
| Person | 83.33% |
| Organisation | 66.67% |
| Location | 66.67% |
| Date | 100.00% |
| Time | 100.00% |
| Event type | 0.00% |
| Action | 16.67% |
| Quantity | 50.00% |

Overall results:

- **60.42% field-level accuracy**
- **0/6 completely correct records**

This exposed a clear distinction between generalisable logic and overly specific rules.

Date and time extraction transferred successfully, while event and action extraction depended too heavily on exact wording from the development corpus.

## Error-Driven Pipeline Improvement

The robustness failures were analysed by **error type**, rather than corrected through record-specific memorisation.

The improved V2 extractor introduced broader patterns for:

- organisation recovery
- location recovery
- alternative event formulations
- action extraction
- quantity detection
- rescheduling language
- system and payment failures
- staffing problems
- vehicle breakdowns
- customer escalations

### V2 Performance on the Error-Analysis Set

After improvement:

- field-level accuracy increased from **60.42% to 95.83%**
- completely correct records increased from **0/6 to 4/6**

Seven of the eight fields achieved 100% accuracy on this set.

Action extraction reached **66.67%**, with the remaining errors primarily involving extraction boundaries rather than failure to recognise the underlying action.

Because this set informed the V2 design, it was not used as the final independent performance estimate.

## Final Holdout Evaluation

The frozen V2 pipeline was evaluated on a separate final holdout set of six previously unseen texts.

No extraction rules were modified after reviewing these results.

### Final Performance

| Field | Accuracy |
|---|---:|
| Person | 100.00% |
| Organisation | 83.33% |
| Location | 100.00% |
| Date | 100.00% |
| Time | 100.00% |
| Event type | 83.33% |
| Action | 83.33% |
| Quantity | 100.00% |

Overall:

- **93.75% field-level accuracy**
- **45/48 fields correctly extracted**
- **3/6 records completely correct**

Five of the eight target fields achieved perfect accuracy:

- Person
- Location
- Date
- Time
- Quantity

### Remaining Holdout Errors

Three errors remained:

- a previously unseen formulation of a warehouse system outage was not mapped to the existing event category
- one unseen organisation was not recognised
- one rescheduling action was not extracted even though the event itself was correctly identified

These errors were retained rather than corrected after evaluation in order to preserve the independence of the final holdout set.

## Structured JSON Output

The final extraction is converted into JSON records suitable for downstream processing.

Potential applications include:

- workflow automation
- incident management systems
- operational dashboards
- database ingestion
- APIs
- quality monitoring
- human review workflows

The JSON output preserves missing information explicitly as `null`.

## Key Findings

- Generic pretrained NER provides a useful starting point but is insufficient for a domain-specific structured extraction task.
- Domain-specific entity rules can correct systematic entity classification errors.
- Dates and times generalised particularly well when extracted independently of NER labels.
- Exact lexical rules can produce misleadingly strong development results while generalising poorly.
- Initial unseen-text performance fell from 100% development accuracy to **60.42%**.
- Error-driven redesign increased robustness-set accuracy to **95.83%**.
- The frozen pipeline achieved **93.75% field-level accuracy on the independent final holdout set**.
- Independent evaluation is essential for distinguishing implementation success from genuine generalisation.
- Missing information should remain explicit rather than being inferred without evidence.

## Technologies

- Python
- spaCy
- `en_core_web_sm`
- EntityRuler
- Regular Expressions
- Pandas
- JSON
- Google Colab

## Repository Structure

```text
structured-information-extraction-nlp/
│
├── README.md
├── structured_information_extraction_nlp.ipynb
└── data/
    └── README.md
```

## Limitations

The project uses a relatively small synthetic corpus and a rule-based hybrid extraction architecture.

Although the final holdout evaluation demonstrates transfer to unseen wording, the results should not be interpreted as evidence of production-level performance across unrestricted operational language.

Rule-based systems remain sensitive to:

- previously unseen linguistic formulations
- unfamiliar entity names
- sentence structure variation
- extraction boundary ambiguity
- expanding event taxonomies

Future work could explore larger annotated datasets, statistical or transformer-based entity models, relation extraction, confidence scoring and human-in-the-loop review.

## Conclusion

This project demonstrates an end-to-end NLP workflow for converting unstructured operational communications into structured information.

The pipeline combines pretrained Named Entity Recognition with domain knowledge, linguistic rules, schema-level reasoning and systematic error analysis.

The most important result is not the perfect score obtained on the controlled development corpus, but the contrast between development and independent evaluation.

An initial **100% development result fell to 60.42% on unseen text**, revealing substantial overfitting in the first rule design.

After error-driven redesign, the frozen extraction pipeline achieved **93.75% field-level accuracy on a completely separate final holdout set**, correctly extracting **45 of 48 fields**.

The project therefore illustrates both the practical value of hybrid NLP extraction and the importance of independent validation when evaluating whether a system genuinely generalises beyond the examples used to build it.
