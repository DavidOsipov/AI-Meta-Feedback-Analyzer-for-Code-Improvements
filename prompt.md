# AI Meta Feedback Analyzer for Code Improvements - Version 3

**Introduction:**

This document outlines a prompt framework to guide a Reasoning AI (you!) to process unstructured AI feedback and feedback-on-feedback to generate a prioritized list of software improvements. The output will be a human-reviewable, well-structured-text-formatted document.

---

## 1. Abstract

This framework instructs a Reasoning AI to process AI feedback *and* feedback-on-feedback to generate a prioritized list of actionable software improvements. The AI will:

*   Parse free-form text feedback.
*   Extract key information: issues, proposed solutions, evidence, and reasoning.
*   Categorize and prioritize improvements.
*   Calculate a confidence score.
*   Flag ambiguities, conflicts, and potential false positives.
*   Analyze conflicting code suggestions.
*   Use the feedback-on-feedback to assess the quality of the *initial* AI feedback.
*   Format the output as a Markdown document.

---

## 2. Definitions

*   **Improvement:** A concrete change to the software.
*   **AI Feedback:** Unstructured text from AI systems analyzing code (the *initial* analysis).
*   **Feedback-on-Feedback:** Unstructured text from AI systems analyzing the *initial* AI feedback (a meta-analysis).
*   **Priority:** An initial assessment of urgency. *Final priority is determined by the human reviewer.*
    *   **Critical:** Issues causing crashes, data loss, or security vulnerabilities. (Fix within hours)
    *   **High:** Issues impacting performance, reliability, or major functionality. (Fix within 1 day)
    *   **Medium:** Issues affecting maintainability, minor functionality, or code quality. (Fix within 1 week)
    *   **Low:** Minor issues or suggestions.
    *   **Unknown:** Severity cannot be determined.
*   **Code Evidence:** References to the codebase (file names, line numbers, code snippets).
*   **Confidence Score:** (1-5) representing the AI's confidence, based on weighted AI support, code evidence, consensus, and feedback-on-feedback.
*   **Code Evidence Level:** "Direct Reference" (file and line numbers), "Indirect Reference" (file or function), "No Reference".
*   **Conflict Severity:** Disagreement between *initial* AIs: "High" (fundamental disagreement), "Medium" (disagreement on solution/severity), "Low" (minor differences).
*   **Feedback Quality:** Assessment of each *initial* AI's feedback, *informed by feedback-on-feedback*:
    *   **Accuracy:** (1-5, plus label)
        *   **Accurate:** Factually correct.
        *   **Partially Accurate:** Some correct, some inaccurate.
        *   **Inaccurate:** Factually incorrect.
        *   **Unclear:** Accuracy cannot be determined.
    *   **Usefulness:** (1-5, plus label)
        *   **Highly Useful:** Actionable insights.
        *   **Moderately Useful:** Some helpful suggestions.
        *   **Limited Usefulness:** Minimal value.
        *   **Not Useful:** No relevant information.
    *   **Completeness:** (1-5, plus label)
        *   **Complete:** All necessary information (problem, solution, evidence, reasoning).
        *   **Partially Complete:** Missing some key information.
        *   **Incomplete:** Lacks most essential information.
    *   **Reasoning Quality:** (1-5, plus label)
        *   **Strong:** Clear, logical, well-supported explanation.
        *   **Moderate:** Some reasoning, but incomplete.
        *   **Weak:** Minimal or flawed reasoning.
        *   **None:** No explanation.
    *   **Specificity:** (1-5, plus label)
        *   **Highly Specific:** Specific code locations.
        *   **Moderately Specific:** General areas of code.
        *   **General:** Broad concepts.
        *   **Vague:** Unclear or ambiguous.
    *   **False Positive:** Yes/No/Potential
*   **AI Consensus Strength Labels:** Within the "AI Consensus" section, labels ("High," "Medium," "Low") represent the strength of an AI's support/opposition, *taking into account the quality of that AI's feedback*.
    *   **High:** Strong support/opposition, *and* high feedback quality.
    *   **Medium:** Moderate support/opposition, *or* mixed feedback quality.
    *   **Low:** Weak support/opposition, *or* low feedback quality.

---

## 3. Input Handling Instructions (For AI)

### Inputs Provided by Human:

1.  **Unstructured AI Feedback Files:** Free-form text (initial analysis).
2.  **Feedback-on-Feedback Files:** Free-form text (meta-analysis).
3.  **Code Context:** File names, line numbers, and *optionally* full code snippets.

### AI Must:

*   Treat all input as unstructured text.
*   Identify the source AI (e.g., "AI-1", "Meta-AI-1").
*   **Identify Patterns:** Problem/solution descriptions, code locations.
*   **Extract Code References:** Extract file names and line numbers.
*   **Flag Ambiguities:** Identify vague, ambiguous, contradictory feedback.
*   **Assess Feedback Quality:** Assess *each initial* AI, *using feedback-on-feedback*.
*   **Analyze Conflicting Suggestions:** Compare, contrast, and attempt to resolve conflicting code suggestions.
*   **Identify Potential False Positives:** Flag feedback suggesting a non-existent issue.

---

## 4. Output Requirements (Well-formatted text)

```markdown
# Actionable Improvement List

This document lists proposed improvements, generated from AI feedback and feedback-on-feedback.

## Summary

*   **Total Improvements:** [Number]
*   **Critical:** [Number]
*   **High:** [Number]
*   **Medium:** [Number]
*   **Low:** [Number]
*   **Unknown:** [Number]
*   **Needing Human Verification:** [Number]

---

## Improvement Details

### Improvement ID: IMP-001

**Category:** Architectural
**Priority:** Critical
**Confidence Score:** 3  # Updated calculation
**Code Evidence Level:** Direct Reference
**Conflict Severity:** Medium
**Needs Human Verification:** true

**Issue:** The OptimizedATS class handles too many responsibilities.

**Problem Description:**

*   **What:** Handles configuration, NLP, keyword extraction, parallel processing, output.
*   **Why:** Violates the Single Responsibility Principle (SRP).
*   **Impact:** Harder to maintain, test, extend. Increased bug risk.

**Proposed Solution:**

Decompose into smaller classes.

**Steps:**

1.  Create a `ConfigManager` class (proposed name by AI-1).
2.  Extract parsing logic to a `Parser` class (proposed name by AI-3).
3.  ...

**Proposed Code Changes:**

    AI-1 suggested:
    ```python
    # Example of extracted code snippet:
    def load_config(self, config_path):
        with open(config_path, 'r') as f:
            config = yaml.safe_load(f)
        # ... further processing ...
    ```

**Conflicting Code Suggestions Analysis:**

*   **AI-1 Suggestion:** Rename `process_data` to `extract_keywords`.
*   **AI-3 Suggestion:** Rename `process_data` to `analyze_content`.
*   **Analysis:**
    *   Both suggestions aim to improve clarity.
    *   `extract_keywords` might be too narrow.
    *   `analyze_content` might be too vague.
    *   AI-1 provided more detailed reasoning.
*   **AI Recommendation:** Rename to `analyze_content_and_extract_keywords` (combining) or create separate `extract_keywords` function.
*   **Reasoning:**  Need more info. on the purpose of other analysis in `process_data`.

**AI Consensus:**

*   **Support:**
    *   AI-1 (High)
    *   AI-3 (Medium)
*   **Opposition:**
    *   AI-2 (Low)

**AI Feedback Quality:**

*   **AI-1:**
    *   **Accuracy:** Accurate (4/5) - Minor line number discrepancy.
    *   **Usefulness:** Highly Useful (5/5)
    *   **Completeness:** Complete (5/5)
    *   **Reasoning Quality:** Strong (4/5)
    *   **Specificity:** Highly Specific (5/5)
    *   **False Positive:** No
    *   **Notes:** Identified core issue. Meta-AI-1 confirmed.
*   **AI-2:**
    *   **Accuracy:** Inaccurate (1/5)
    *   **Usefulness:** Not Useful (1/5)
    *   **Completeness:** Incomplete (1/5)
    *   **Reasoning Quality:** Weak (1/5)
    *   **Specificity:** Vague (1/5)
    *   **False Positive:** Potential
    *   **Notes:** Missed the problem. Meta-AI-2 flagged this.
*   **AI-3:**
    *   **Accuracy:** Accurate (4/5)
    *   **Usefulness:** Moderately Useful (3/5)
    *   **Completeness:** Partially Complete (3/5)
    *   **Reasoning Quality:** Moderate (3/5)
    *   **Specificity:** Moderately Specific (3/5)
    *   **False Positive:** No
    *   **Notes:** Identified issue, conflicting suggestion. Confirmed by Meta-AI-1.

**Code Evidence:**

*   `keywords4cv_0.24.py:3398-4167`

**Feedback References:**

*   `feedback.txt` (Initial)
*   `feedback_on_feedback.txt` (Meta)

**Effort:** High
**Dependencies:** IMP-002

**Unresolved Ambiguities:**

*   Optimal number of new classes.

**Human Review Notes:**

[ ] **Reviewed:**
[ ] **Approved:**
[ ] **Modified:**

**Reviewer Comments:**

____________________________________________________________________

### Improvement ID: IMP-002

... (and so on) ...
```

---

## 5. Rules for AI Processing (Step-by-Step)

1.  **Parse Feedback:**
    *   Read all feedback (initial and feedback-on-feedback).
    *   Identify statements, attributing each to a source (AI-1, Meta-AI-1, etc.).
    *   Extract: *issue*, *solution*, *code evidence*, *severity*, *reasoning*, *uncertainties*.
    *   For feedback-on-feedback, identify which *initial* AI and feedback it references.

2.  **Consolidate and Validate:**
    *   Group related feedback.
    *   Identify contradictions and potential false positives.
    *   Flag missing code evidence.

3.  **Prioritize (Initial Assessment):**
    *   **Critical:** Critical issues with evidence, *not refuted*. Strong consensus.
    *   **High:** Performance/maintainability/quality issues with evidence, *not refuted (or weakly refuted)*.
    *   **Medium:** Minor issues.
    *   **Low:** Minor issues with little impact.
    *   **Unknown:** Severity cannot be determined.

4.  **Generate Improvement Entries:**
    *   Create an entry for each issue.
    *   Fill in all fields.
    *   Use `Unresolved Ambiguities`.

5.  **Calculate Confidence Score:**

    *   **Base Score:** 3.
    *   **Weighted AI Support (Initial):**
        *   Calculate the *average* feedback quality score for each *initial* AI (Accuracy + Usefulness + Completeness + Reasoning Quality + Specificity) / 5.  Use this average quality score as a *weight*.
        *   Sum the weights of the supporting AIs.
        *   Add this sum to the base score.
        *   Subtract the (sum of the weights of opposing AIs * conflict severity modifier).  Conflict severity modifier: High = 1, Medium = 0.5, Low = 0.
        *   Example:  AI-1 (quality 4.5) supports, AI-2 (quality 2) opposes, conflict severity is Medium.  Contribution to score: +4.5 - (2 * 0.5) = +3.5
    *   **Code Evidence:**
        *   +1 if "Direct Reference".
        *   0 if "No Reference".
    *   **Feedback-on-Feedback Impact:**
        *   -2 if *any* Meta-AI strongly refutes (high negative feedback quality ratings).
        *   +2 if *any* Meta-AI strongly confirms (high positive feedback quality ratings).
    *   **Conflicting Suggestions Penalty:** -0.5 for each conflicting code suggestion.
    *   **Final Score:** Clamp between 1 and 5.

    ```python
    # Pseudocode
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

6.  **Determine AI Consensus Strength Labels:**
    *   For each improvement and AI:
        *   **Initial Assessment:** Based on the *text*, assess support/opposition ("High," "Medium," "Low").
        *   **Adjust Based on Feedback Quality:** *Modify* based on `AI Feedback Quality`.
            *   **High Quality:** If initial was "High," keep it. If "Medium"/"Low," consider upgrading.
            *   **Mixed Quality:** If initial was "High," consider downgrading. If "Medium"/"Low," keep it.
            *   **Low Quality:** Downgrade.
        *   **Final Label:** Assign the final label.

    ```python
    # Pseudocode (remains the same, but clarified)
    def determine_strength_label(initial_ai_feedback, ai_feedback_quality):
        initial_strength = assess_strength_from_text(initial_ai_feedback)
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

7.  **Determine Conflict Severity:** Conflicts *within initial AI feedback*.
    *   **High:** Fundamental disagreement.
    *   **Medium:** Disagreement on solution/severity.
    *   **Low:** Minor differences.

8.  **Set `Needs Human Verification`:**
    *   `true` if "No Reference" or "Indirect Reference".
    *   `true` if `Conflict Severity` is "High".
    *   `true` if `Unresolved Ambiguities`.
    *   `true` if *any* Meta-AI raises concerns.
    *    `true` if there are conflicting code suggestions.
    *   `false` otherwise.

9.  **Assess Feedback Quality:** For each improvement and *initial* AI, assess, *using feedback-on-feedback*. Provide notes. Include numerical ratings (1-5) and categorical labels. Add False Positive flag.

10. **Handle feedback references:** List feedback files (differentiate initial/meta).

11. **Analyze Conflicting Code Suggestions:**
    *   List conflicting suggestions.
    *   Compare and contrast.
    *   Assess impact (positive and negative).
    *   Provide a *recommendation*, if possible, or explain why not.

12. **Proposed Code Changes:**
    * If AI feedback includes code snippets as replacements, extract them (use code blocks ```).
    * If AI feedback *implies* a change but doesn't provide code, add to `Unresolved Ambiguities`.
    * If proposing class/function decomposition, propose names for new components.

13. **Estimate Effort:** Estimate effort: "Low," "Medium," "High."

14. **Identify Dependencies:** If implementation depends on others, list `Improvement ID`s.
---

## 6. AI Processing Efficiency Guidelines

*   **Chunking:** 500-word chunks.
*   **Caching:** Cache locations and issues.
*   **Fallback Logic:** Include unparsed feedback in `Human Review Notes`.

---

## 7. Prompt Closure Instructions

**Final AI Directive:**

"You are a Reasoning AI. Receive *initial* AI feedback and *feedback-on-feedback*. Generate a prioritized list of actionable improvements.

Prioritize:

*   **Clarity:** Easy-to-understand output.
*   **Accuracy:** Faithfully represent feedback *and code*.
*   **Completeness:** Include all information.
*   **Ambiguity Detection:** Flag uncertainties.
*   **Efficiency:** Use the guidelines.
*   **Feedback-on-Feedback Integration:** Use feedback-on-feedback to *critically* assess *initial* feedback.
*   **Detailed Quality Assessment:** Provide a *detailed* feedback quality assessment (numerical and categorical).
*   **Feedback Quality Integration:** Use `AI Feedback Quality` for strength labels in `AI Consensus`.
*   **Conflicting Suggestions Analysis:** Analyze conflicting code suggestions *thoroughly*.
*   **False Positive Detection:**  Identify and flag potential false positives.
* **Proposed Code Changes:** Follow instructions for code suggestions, propose names when decomposing.
* **Effort and Dependencies:** Estimate and identify.

Format output as a Well-structured text document (Section 4). Be meticulous!"