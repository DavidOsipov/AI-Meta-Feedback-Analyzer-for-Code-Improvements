# Technical Details: AI Prompt Framework

This document explains the algorithms and processes used in the AI Prompt Framework for generating actionable improvement lists.

## Core Processing Pipeline

The framework follows a 5-stage processing pipeline:

```
Input → Parsing → Consolidation → Prioritization → Assessment → Output Generation
```

### 1. Parsing Stage

The parsing stage extracts structured information from unstructured text:

- **Text Segmentation**: Divides feedback into logical units (sentences, paragraphs)
- **Source Attribution**: Identifies which AI provided each piece of feedback
- **Information Extraction**: 
  - Issues/problems
  - Proposed solutions
  - Code references
  - Severity assessments
  - Reasoning
  - Uncertainties
- **Meta-Feedback Handling**: Links feedback-on-feedback to the initial feedback it references

### 2. Consolidation Stage

The consolidation stage groups related feedback:

- **Similarity Detection**: Identifies feedback discussing the same issues
- **Contradiction Detection**: Identifies conflicting opinions
- **Evidence Validation**: Flags feedback missing code evidence

### 3. Prioritization Stage

The prioritization assigns initial priority levels based on:

- **Severity**: Impact on the system (crashes, data loss, security, etc.)
- **Consensus**: Level of agreement among AIs
- **Meta-Validation**: Whether feedback-on-feedback confirms or refutes
- **Evidence Strength**: Direct vs. indirect code references

### 4. Assessment Stage

The assessment stage evaluates feedback quality:

- **Accuracy**: Factual correctness (using feedback-on-feedback)
- **Usefulness**: Actionability of the feedback
- **Completeness**: Presence of all necessary information
- **Reasoning Quality**: Logical soundness of arguments
- **Specificity**: Detail level of the feedback

### 5. Output Generation Stage

The output stage creates a standardized Markdown document with:

- **Summary Statistics**: Totals by priority
- **Detailed Improvement Entries**: Formatted according to the template
- **Cross-References**: Dependencies between improvements
- **Human Review Sections**: Placeholders for reviewer input

## Confidence Score Algorithm

The confidence score (1-5) is calculated using the following algorithm:

```python
def calculate_confidence(support, opposition, code_evidence, meta_feedback):
    base_score = 3
    if len(support) >= 2: base_score += 1
    avg_strength = sum(s["strength"] for s in support) / len(support) if support else 0
    if avg_strength >= 2.5: base_score += 1 # Assuming "High"=3, "Medium"=2, "Low"=1
    if any(o["strength"] == "High" for o in opposition): base_score -= 1
    if code_evidence == "Direct Reference": base_score += 1
    elif code_evidence == "No Reference": base_score -= 1
    for meta in meta_feedback:
        if meta["strongly_refutes"]:
            base_score -= 1
            break  # Strong refutation takes precedence
        elif meta["strongly_confirms"]:
            base_score += 1
    return max(1, min(5, base_score))
```

## AI Consensus Strength Label Determination

The strength labels ("High," "Medium," "Low") in the AI Consensus section are determined using:

```python
def determine_strength_label(initial_ai_feedback, ai_feedback_quality):
    # Initial assessment based on text (keywords, tone, etc.)
    initial_strength = assess_strength_from_text(initial_ai_feedback)  # Returns "High", "Medium", or "Low"

    # Adjust based on AI Feedback Quality
    quality = ai_feedback_quality
    if (quality["Accuracy"] == "Accurate" and
        quality["Usefulness"] == "Highly Useful" and
        quality["Completeness"] == "Complete" and
        quality["Reasoning Quality"] == "Strong" and
        quality["Specificity"] == "Highly Specific"):
        # High Quality
        if initial_strength == "Low":
            return "Medium" # Consider upgrade
        return initial_strength # Keep "High" or "Medium"
    elif (quality["Accuracy"] == "Inaccurate" or
          quality["Usefulness"] == "Not Useful" or
          quality["Completeness"] == "Incomplete" or
          quality["Reasoning Quality"] == "Weak" or
          quality["Specificity"] == "Vague"):
        # Low Quality
        return "Low"
    else:
        # Mixed Quality
        if initial_strength == "High":
            return "Medium"  # Downgrade
        return initial_strength  # Keep "Medium" or "Low"
```

## Human Verification Trigger Logic

An improvement is flagged for human verification if any of these conditions are met:

- Code evidence is "No Reference" or "Indirect Reference"
- Conflict severity is "High"
- There are unresolved ambiguities
- Any meta-AI raises significant concerns

## Performance Considerations

For large feedback files, the framework employs:

- **Chunking**: Processes feedback in 500-word chunks
- **Caching**: Caches code locations and issues
- **Fallback Logic**: Includes raw, unparsed feedback if processing fails
