# Production-Grade AI Paraphraser Architecture (Token Optimized)

## Design Goals

* Minimize LLM token usage
* Maximize paraphrase quality
* Preserve original meaning
* Keep response time low
* Support any LLM (Gemini, GPT, Claude, Qwen, Llama, etc.)
* Modular and scalable architecture

---

# Overall Architecture

```text
                    Browser Extension
                           │
                           ▼
──────────────────────────────────────────────────────────────
                 PREPROCESSING LAYER (0 LLM Tokens)
──────────────────────────────────────────────────────────────

Text Extraction
        │
        ▼
Input Cleaning & Validation
        │
        ▼
Language Detection
        │
        ▼
Formatting Parser
        │
        ▼
Named Entity Recognition (NER)
        │
        ▼
Protected Token Masking
(Persons, Dates, URLs, Emails,
Numbers, Citations, Technical Terms)
        │
        ▼
Prompt Builder
        │
        ▼

──────────────────────────────────────────────────────────────
                   LLM GENERATION LAYER
──────────────────────────────────────────────────────────────

Single LLM Request
Generate exactly 3 diverse paraphrases

        │
        ▼

Candidate 1
Candidate 2
Candidate 3

        │
        ▼

──────────────────────────────────────────────────────────────
                 POST PROCESSING LAYER
──────────────────────────────────────────────────────────────

Restore Protected Tokens
        │
        ▼
Semantic Similarity
        │
        ▼
Grammar Checker
        │
        ▼
Readability Analyzer
        │
        ▼
Duplicate Detection
        │
        ▼
Style Verification
        │
        ▼
Length Balance
        │
        ▼
Ranking Engine
        │
        ▼
Best Candidate
        │
        ▼
Cache
        │
        ▼
Browser Extension
```

---

# Layer 1 — Text Extraction

### Purpose

Extract selected text from:

* Web pages
* Google Docs
* PDF viewers (where supported)
* Textareas
* Input fields

Output:

```
Raw User Text
```

---

# Layer 2 — Input Cleaning

### Responsibilities

* Remove extra whitespace
* Normalize quotation marks
* Normalize punctuation
* Fix line breaks
* Remove invisible Unicode characters
* Detect empty input

No LLM required.

---

# Layer 3 — Language Detection

Instead of asking the LLM:

> "What language is this?"

Use:

* franc
* cld3
* lingua

Output

```
English
Spanish
French
Hindi
...
```

Used for selecting prompts.

Cost:

**0 Tokens**

---

# Layer 4 — Formatting Parser

Parse

* Markdown
* HTML
* Lists
* Tables
* Headers
* Bold
* Italics

Example

Input

```
- Apple
- Banana
```

Only

```
Apple
Banana
```

is paraphrased.

Formatting is restored later.

---

# Layer 5 — Named Entity Recognition (NER)

Detect

* Person names
* Companies
* Locations
* Organizations
* Products
* Dates
* Times

Example

```
John Smith works at OpenAI.
```

becomes

```
<PERSON_1> works at <ORG_1>.
```

Benefits

* Prevents hallucinations
* Keeps names unchanged
* Reduces prompt size

---

# Layer 6 — Protected Token Masking

Mask everything that should never change.

Protect

* URLs
* Emails
* Phone numbers
* Dates
* Currency
* Measurements
* Citations
* References
* DOIs
* Technical terms
* Programming keywords
* API names
* Model names

Example

```
https://github.com
```

↓

```
<URL_1>
```

Later restored automatically.

---

# Layer 7 — Prompt Builder

Construct a compact prompt.

Instead of

```
Don't modify URLs.
Don't modify citations.
Don't modify names.
Don't modify dates.
...
```

Use

```
Protected placeholders must remain unchanged.
Generate exactly 3 diverse paraphrases.
Preserve meaning.
```

Much fewer tokens.

---

# Layer 8 — Single LLM Request

Instead of

```
API Call 1
API Call 2
API Call 3
```

Make

ONE request.

Example

```
Generate exactly three diverse paraphrases.
Return valid JSON.

{
 "candidate1":"",
 "candidate2":"",
 "candidate3":""
}
```

Benefits

* Lower latency
* Lower API cost
* Better consistency

---

# Layer 9 — Restore Protected Tokens

Convert

```
<PERSON_1>
```

back into

```
John Smith
```

Same for

* URLs
* Emails
* Citations
* Dates
* Numbers

---

# Layer 10 — Quality Evaluation Engine

No LLM required.

Everything runs locally.

---

## Feature 1 — Semantic Similarity

Purpose

Ensure meaning is preserved.

Use

Embedding models

Examples

* all-MiniLM-L6-v2
* BGE Small
* E5 Small

Method

Cosine Similarity

---

## Feature 2 — Grammar Checker

Use

* LanguageTool
* GECToR
* Gramformer

Checks

* Grammar
* Spelling
* Agreement
* Punctuation

---

## Feature 3 — Readability

Use formulas

* Flesch Reading Ease
* Flesch Kincaid
* Gunning Fog

No AI required.

---

## Feature 4 — Duplicate Detection

Reject nearly identical candidates.

Methods

* Cosine similarity
* Levenshtein Distance
* Jaccard Similarity

---

## Feature 5 — Style Verification

Verify

Academic

Formal

Creative

Simple

Professional

Business

Matches selected mode.

---

## Feature 6 — Length Balance

Reject outputs

Too short

Too long

Too verbose

Too compressed

---

# Layer 11 — Ranking Engine

Score every candidate.

Example weights

Semantic Similarity

35%

Grammar

20%

Readability

10%

Style

10%

Duplicate Penalty

10%

Vocabulary Diversity

10%

Length Balance

5%

Final formula

```
Final Score =
0.35 Semantic +
0.20 Grammar +
0.10 Readability +
0.10 Style +
0.10 Diversity +
0.10 Duplicate Penalty +
0.05 Length
```

Highest score wins.

---

# Layer 12 — Cache

Hash

```
SHA256(Input Text)
```

If already paraphrased

↓

Return cached output.

No LLM call.

Huge savings.

---

# Twelve Token Optimization Features

### 1.

Local Language Detection

No LLM tokens.

---

### 2.

Local Grammar Checking

No LLM tokens.

---

### 3.

Embedding-Based Semantic Similarity

No LLM tokens.

---

### 4.

Formula-Based Readability

No LLM tokens.

---

### 5.

Embedding-Based Duplicate Detection

No LLM tokens.

---

### 6.

Named Entity Masking

Smaller prompts.

Better accuracy.

---

### 7.

Citation Protection

Avoid unnecessary instructions.

---

### 8.

Technical Term Masking

Prevent incorrect synonym replacement.

---

### 9.

Formatting Preservation

Only paraphrase actual text.

---

### 10.

Compact Prompt Engineering

Short prompts.

Lower token usage.

---

### 11.

Single API Call

Generate all three candidates in one request.

---

### 12.

Caching

Avoid repeated API calls.

---

#
