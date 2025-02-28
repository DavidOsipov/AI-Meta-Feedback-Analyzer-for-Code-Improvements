

# Technical Details: AI Prompt Framework

This document explains the algorithms and processes used in the AI Prompt Framework for generating actionable improvement lists.

## Core Processing Pipeline

The framework follows a 5-stage processing pipeline:

```
Input → Parsing → Consolidation → Prioritization → Assessment → Output Generation
```

### 1. Parsing Stage

The parsing stage extracts structured information from unstructured text:

-   **Text Segmentation**: Divides feedback into logical units (sentences, paragraphs).
-   **Source Attribution**: Identifies which AI provided each piece of feedback.
-   **Information Extraction**:
    -   Issues/problems.
    -   Proposed solutions.
    -   Code references (file names, line numbers, and optionally code snippets).
    -   Severity assessments.
    -   Reasoning.
    -   Uncertainties.
    -   Potential false positives.
-   **Meta-Feedback Handling**: Links feedback-on-feedback to the initial feedback it references.

### 2. Consolidation Stage

The consolidation stage groups related feedback:

-   **Similarity Detection**: Identifies feedback discussing the same issues (using techniques like keyword matching, semantic similarity analysis).
-   **Contradiction Detection**: Identifies conflicting opinions or suggestions among the initial AIs and between initial AIs and meta-AIs.
-   **Evidence Validation**: Flags feedback missing code evidence or with weak evidence.
- **False Positive Identification:** Flags potential false positives.

### 3. Prioritization Stage

The prioritization stage assigns initial priority levels based on:

-   **Severity**: Impact on the system (crashes, data loss, security vulnerabilities, performance issues, maintainability problems).
-   **Weighted Consensus**: Level of agreement among AIs, weighted by their feedback quality scores.
-   **Meta-Validation**: Whether feedback-on-feedback confirms or refutes the initial feedback (and the strength of the confirmation/refutation).
-   **Evidence Strength**: Direct code references (file and line number) are given higher weight than indirect references (file or function name) or no references.

### 4. Assessment Stage

The assessment stage evaluates feedback quality for each AI and each improvement:

-   **Accuracy**: Factual correctness (using feedback-on-feedback and, if available, code context).  Rated on a 1-5 scale, with categorical labels (Accurate, Partially Accurate, Inaccurate, Unclear).
-   **Usefulness**: Actionability of the feedback (how easily can the suggestions be implemented). Rated on a 1-5 scale, with categorical labels (Highly Useful, Moderately Useful, Limited Usefulness, Not Useful).
-   **Completeness**: Presence of all necessary information (problem, solution, evidence, reasoning). Rated on a 1-5 scale, with categorical labels (Complete, Partially Complete, Incomplete).
-   **Reasoning Quality**: Logical soundness of the arguments presented in the feedback. Rated on a 1-5 scale, with categorical labels (Strong, Moderate, Weak, None).
-   **Specificity**: Detail level of the feedback (specific code locations vs. general concepts).  Rated on a 1-5 scale, with categorical labels (Highly Specific, Moderately Specific, General, Vague).
-   **False Positive**:  Indicates whether the feedback is likely a false positive (Yes/No/Potential).
-   **Conflicting Suggestion Analysis**:  If conflicting code suggestions are present, this stage involves:
    -   Listing the conflicting suggestions.
    -   Comparing and contrasting the suggestions.
    -   Assessing the potential impact of each suggestion.
    -   Providing a recommendation (if possible) based on the analysis.

### 5. Output Generation Stage

The output stage creates a standardized Markdown document with:

-   **Summary Statistics**: Totals by priority and other relevant metrics.
-   **Detailed Improvement Entries**: Formatted according to the template, including all extracted information, assessments, and analysis.
-   **Cross-References**: Dependencies between improvements.
-   **Human Review Sections**: Placeholders for reviewer input (approval, modification, comments).

## Confidence Score Algorithm

The confidence score (1-5) is calculated using a weighted sum of several factors:

```python
def calculate_confidence(support, opposition, code_evidence, meta_feedback, conflicting_suggestions):
    base_score = 3

    # Weighted AI Support
    supporting_weight = sum(ai["quality_score"] for ai in support)
    opposing_weight = sum(ai["quality_score"] * ai["conflict_modifier"] for ai in opposition)
    base_score += supporting_weight - opposing_weight

    # Code Evidence
    if code_evidence == "Direct Reference": base_score += 1
    elif code_evidence == "No Reference": base_score -= 1

    # Feedback-on-Feedback Impact
    for meta in meta_feedback:
        if meta["strongly_refutes"]:  # High negative feedback quality
            base_score -= 2
            break  # Strong refutation takes precedence
        elif meta["strongly_confirms"]: # High positive feedback quality
            base_score += 2

    # Conflicting Suggestions Penalty
    base_score -= 0.5 * len(conflicting_suggestions)

    return max(1, min(5, base_score))

# Helper functions (example)
def calculate_quality_score(feedback_quality):
  return (feedback_quality["Accuracy"] +
          feedback_quality["Usefulness"] +
          feedback_quality["Completeness"] +
          feedback_quality["Reasoning Quality"] +
          feedback_quality["Specificity"]) / 5

def determine_conflict_modifier(conflict_severity):
  if conflict_severity == "High": return 1
  elif conflict_severity == "Medium": return 0.5
  else: return 0
```

*   **Weighted AI Support:**  The average feedback quality score (1-5) for each AI is calculated and used as a weight.  AIs with higher average quality scores have a greater influence on the confidence score.
*   **Conflict Severity Modifier:**  Opposition from AIs is weighted by the conflict severity (High=1, Medium=0.5, Low=0).
*   **Code Evidence:**  Direct references increase the score; no references decrease it.
*   **Feedback-on-Feedback:** Strong refutations significantly decrease the score; strong confirmations significantly increase it.
*   **Conflicting Suggestions:**  Each conflicting code suggestion slightly decreases the score.

## AI Consensus Strength Label Determination

The strength labels ("High," "Medium," "Low") in the `AI Consensus` section are determined by combining an initial assessment based on the text of the feedback with the AI's feedback quality ratings:

```python
def determine_strength_label(initial_ai_feedback, ai_feedback_quality):
    initial_strength = assess_strength_from_text(initial_ai_feedback)  # Returns "High", "Medium", or "Low"
    quality = ai_feedback_quality
    if (quality["Accuracy"] >= 4 and
        quality["Usefulness"] >= 4 and
        quality["Completeness"] >= 4 and
        quality["Reasoning Quality"] >= 4 and
        quality["Specificity"] >= 4):
        if initial_strength == "Low": return "Medium"
        return initial_strength
    elif (quality["Accuracy"] <= 2 or
          quality["Usefulness"] <= 2 or
          quality["Completeness"] <= 2 or
          quality["Reasoning Quality"] <= 2 or
          quality["Specificity"] <= 2):
        return "Low"
    else:
        if initial_strength == "High": return "Medium"
        return initial_strength
```

*   **Initial Assessment:**  The AI analyzes the text of the feedback to make an initial judgment of support/opposition strength ("High," "Medium," "Low").
*   **Quality Adjustment:**  The initial assessment is adjusted based on the AI's feedback quality ratings.  High-quality feedback strengthens the label; low-quality feedback weakens it.

## Human Verification Trigger Logic

An improvement is flagged for human verification if any of these conditions are met:

-   Code evidence is "No Reference" or "Indirect Reference."
-   Conflict severity is "High."
-   There are unresolved ambiguities.
-   Any meta-AI raises significant concerns (e.g., strong refutation, low feedback quality ratings).
-   There are conflicting code suggestions.
- There is a potential false positive.

## Performance Considerations

For large feedback files, the framework employs:

-   **Chunking**: Processes feedback in 500-word chunks to manage memory and processing time.
-   **Caching**: Caches code locations and issue summaries to avoid redundant processing.
-   **Fallback Logic**: Includes raw, unparsed feedback in the `Human Review Notes` section if processing fails for any reason, ensuring no information is lost.
