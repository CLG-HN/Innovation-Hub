# AI Paraphraser Architecture (Three-Candidate Pipeline)

## Overview

The system generates **three high-quality paraphrase candidates**, evaluates them using multiple quality metrics, ranks them, and returns the best result. This architecture is model-agnostic, allowing any LLM API to be integrated.

```
                    Browser Extension
                           │
                           ▼
                  Text Extraction Layer
                           │
                           ▼
                Input Cleaning & Validation
                           │
                           ▼
                 Language Detection
                           │
                           ▼
                Prompt Construction Engine
                           │
                           ▼
                LLM (Gemini / GPT / Claude / Qwen)
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
       Candidate 1    Candidate 2    Candidate 3
             │             │             │
             └─────────────┼─────────────┘
                           ▼
                Quality Evaluation Engine
                           │
         ┌─────────────────┼─────────────────┐
         ▼                 ▼                 ▼
   Semantic Score   Grammar Score   Readability Score
         │                 │                 │
         ├─────────────────┼─────────────────┤
         ▼                 ▼                 ▼
 Style Consistency   Fluency Check   Redundancy Check
         │                 │                 │
         ├─────────────────┼─────────────────┤
         ▼                 ▼                 ▼
    Vocabulary Score   Length Balance   Confidence Score
                           │
                           ▼
                Weighted Ranking Algorithm
                           │
                           ▼
                Best Candidate Selection
                           │
                           ▼
                Final Formatting & Output
                           │
                           ▼
                    Browser Extension
```

---

# Processing Pipeline

### Step 1 — Input Validation

* Remove unnecessary whitespace.
* Normalize punctuation.
* Detect unsupported or empty input.
* Preserve formatting where possible.

---

### Step 2 — Language Detection

Automatically identify the input language.

Benefits:

* Correct prompt generation.
* Future multilingual support.

---

### Step 3 — Prompt Builder

Construct prompts based on the selected paraphrasing mode.

Examples:

* Standard
* Fluent
* Formal
* Academic
* Simple
* Creative
* Concise
* Expand

---

### Step 4 — Candidate Generation

Generate exactly **three distinct paraphrases**.

Each candidate should:

* Preserve meaning.
* Use different sentence structures.
* Avoid repetition.
* Sound natural.

---

# Quality Evaluation Engine

Each candidate is evaluated independently.

## 1. Semantic Similarity

Checks whether the original meaning is preserved.

Weight: **35%**

---

## 2. Grammar & Spelling

Detects grammatical and spelling errors.

Weight: **20%**

---

## 3. Readability

Measures how easy the text is to understand.

Weight: **10%**

---

## 4. Fluency

Evaluates whether the sentence sounds natural.

Weight: **10%**

---

## 5. Style Consistency

Ensures the tone remains consistent with the selected mode.

Weight: **10%**

---

## 6. Vocabulary Enhancement

Rewards appropriate synonym usage without changing meaning.

Weight: **5%**

---

## 7. Redundancy Detection

Penalizes unnecessary repetition.

Weight: **5%**

---

## 8. Length Balance

Prevents outputs that are excessively short or long.

Weight: **5%**

---

# Ranking Formula

```
Final Score =
0.35 × Semantic Similarity +
0.20 × Grammar +
0.10 × Readability +
0.10 × Fluency +
0.10 × Style +
0.05 × Vocabulary +
0.05 × Redundancy +
0.05 × Length Balance
```

The highest-scoring candidate is selected.

---

# Advanced Quality Features

## Intelligent Prompt Routing

Use different prompt templates based on:

* Sentence length
* Paragraph length
* Writing style
* User-selected mode

---

## Named Entity Preservation

Ensure names, organizations, dates, locations, numbers, citations, and technical terms remain unchanged unless explicitly requested.

---

## Technical Terminology Protection

Avoid replacing domain-specific words such as programming terms, medical terminology, legal language, or scientific concepts with incorrect synonyms.

---

## Tone Preservation

Maintain the original tone (professional, academic, casual, persuasive, etc.) unless the user requests a different style.

---

## Acronym Preservation

Keep abbreviations such as AI, HTTP, GPU, CPU, NASA, and API intact.

---

## Citation Protection

Do not modify:

* Citations
* References
* URLs
* DOIs
* Inline citation formats

---

## Formatting Preservation

Retain:

* Bullet lists
* Numbered lists
* Line breaks
* Markdown
* HTML formatting (if applicable)

---

## Duplicate Detection

Reject candidates that are nearly identical to each other and keep only genuinely different rewrites.

---

## Confidence Score

Compute an overall confidence score from the evaluation metrics. If all candidates score below a threshold, automatically request a new set of three paraphrases from the LLM.

---

## Safety Filters

Prevent:

* Hallucinated facts
* Changed numerical values
* Altered dates
* Incorrect units
* Distorted quotations
* Offensive or unsafe rewrites

---

## Caching

Cache paraphrased results for identical inputs to reduce API usage and improve response time.

---

## User Modes

Provide multiple rewrite styles:

* Academic
* Concise
* Expand
* Humanize
