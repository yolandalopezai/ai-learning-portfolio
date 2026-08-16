# Data

## Source

This project uses a small **synthetic corpus of operational text** created specifically for the information extraction task.

The texts simulate realistic business and operational communications such as:

- service incidents,
- customer escalations,
- delivery disruptions,
- system outages,
- staffing issues,
- meeting updates,
- and operational follow-up actions.

No confidential, personal or proprietary company data is used.

## Why a Synthetic Dataset

A controlled corpus makes it possible to define the expected structured output in advance and evaluate whether the NLP pipeline extracts the correct information.

This is particularly useful for analysing:

- Named Entity Recognition,
- event extraction,
- dates and times,
- organisations,
- locations,
- people,
- operational actions,
- and extraction errors.

## Target Output

Each text will be transformed into a structured representation containing relevant information such as:

- people,
- organisations,
- locations,
- dates,
- events,
- actions,
- and other operational details.

The final structured output will be represented in **JSON format**.

## Evaluation

Because the expected information is known for each example, extracted results can be compared with manually defined reference annotations.

This allows the project to evaluate not only whether the pipeline produces output, but also **how accurately it converts unstructured text into structured information**.
