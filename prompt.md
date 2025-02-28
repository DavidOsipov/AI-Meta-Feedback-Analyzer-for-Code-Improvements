
# AI Meta Feedback Analyzer for Code Improvements

**Introduction:**

This document outlines a comprehensive a prompt to guide a Reasoning AI - you! The AI will process unstructured AI feedback and *feedback-on-feedback* to generate a prioritized list of software improvements. The output will be a human-reviewable, Markdown-formatted document.

---

## 1. Abstract

This framework instructs a Reasoning AI to process AI feedback *and* feedback-on-feedback to generate a prioritized list of actionable software improvements. The AI will:

*   Parse free-form text feedback.
*   Extract key information: issues, proposed solutions, evidence, and reasoning.
*   Categorize and prioritize improvements.
*   Calculate a confidence score.
*   Flag ambiguities and conflicts.
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
*   **Confidence Score:** (1-5) representing the AI's confidence, based on AI support, code evidence, and consensus, *considering the feedback-on-feedback*.
*   **Code Evidence Level:** "Direct Reference" (file and line numbers), "Indirect Reference" (file or function), "No Reference".
*   **Conflict Severity:** Disagreement between *initial* AIs: "High" (fundamental disagreement), "Medium" (disagreement on solution/severity), "Low" (minor differences).
*   **Feedback Quality:** Assessment of each *initial* AI's feedback, *informed by feedback-on-feedback*:
    *   **Accuracy:**
        *   **Accurate:** Factually correct.
        *   **Partially Accurate:** Some correct, some inaccurate.
        *   **Inaccurate:** Factually incorrect.
        *   **Unclear:** Accuracy cannot be determined.
    *   **Usefulness:**
        *   **Highly Useful:** Actionable insights.
        *   **Moderately Useful:** Some helpful suggestions.
        *   **Limited Usefulness:** Minimal value.
        *   **Not Useful:** No relevant information.
    *   **Completeness:**
        *   **Complete:** All necessary information (problem, solution, evidence, reasoning).
        *   **Partially Complete:** Missing some key information.
        *   **Incomplete:** Lacks most essential information.
    *   **Reasoning Quality:**
        *   **Strong:** Clear, logical, well-supported explanation.
        *   **Moderate:** Some reasoning, but incomplete.
        *   **Weak:** Minimal or flawed reasoning.
        *   **None:** No explanation.
    *   **Specificity:**
        *   **Highly Specific:** Specific code locations.
        *   **Moderately Specific:** General areas of code.
        *   **General:** Broad concepts.
        *   **Vague:** Unclear or ambiguous.
*   **AI Consensus Strength Labels:** Within the "AI Consensus" section, these labels ("High," "Medium," "Low") represent the strength of an AI's support or opposition to an improvement, *taking into account the quality of that AI's feedback*.
    *   **High:** The AI strongly supports or opposes the improvement, *and* its feedback is rated highly in terms of `Accuracy`, `Usefulness`, `Completeness`, `Reasoning Quality`, and `Specificity`.  The AI provides clear, well-reasoned arguments with strong evidence.
    *   **Medium:** The AI moderately supports or opposes the improvement, *or* its feedback has mixed quality ratings (e.g., high accuracy but low usefulness). The AI may provide some reasoning and evidence, but it's not as compelling as "High."
    *   **Low:** The AI weakly supports or opposes the improvement, *or* its feedback is rated poorly in terms of `AI Feedback Quality`. The AI's reasoning may be flawed, or the feedback may be vague and unhelpful.  *Even if the AI's text *appears* to strongly support or oppose, a low-quality rating will reduce the strength label.*

---

## 3. Input Handling Instructions (For AI)

### Inputs Provided by Human:

1.  **Unstructured AI Feedback Files:** Free-form text (initial analysis).
2.  **Feedback-on-Feedback Files:** Free-form text (meta-analysis).
3.  **Code Context:** File names, line numbers, and *optionally* full code snippets.

### AI Must:

*   Treat all input as unstructured text.
*   Identify source AI for *both* initial feedback (e.g., "AI-1") and feedback-on-feedback (e.g., "Meta-AI-1").
*   **Identify Patterns:** Look for:
    *   Problem Descriptions.
    *   Solution Descriptions.
    *   Code Location Indicators.
*   **Extract Code References:** Extract file names and line numbers.
*   **Flag Ambiguities:** Identify vague, ambiguous, contradictory, or unsupported feedback.
*   **Assess Feedback Quality:** Assess the quality of *each initial* AI source, *using information from the feedback-on-feedback*.

---

## 4. Output Requirements (Well-formatted text)

    ```
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
    **Confidence Score:** 4
    **Code Evidence Level:** Direct Reference
    **Conflict Severity:** Low
    **Needs Human Verification:** false
    
    **Issue:** The OptimizedATS class handles too many responsibilities.
    
    **Problem Description:**
    
    *   **What:**  Handles configuration, NLP, keyword extraction, parallel processing, and output.
    *   **Why:** Violates the Single Responsibility Principle (SRP).
    *   **Impact:** Harder to maintain, test, and extend. Increased bug risk.
    
    **Proposed Solution:**
    
    Decompose into smaller classes.
    
    **Steps:**
    
    1.  Create a `ConfigManager` class.
    2.  Extract parsing logic to a `Parser` class.
    3.  ...
    
    **Recommended Code (Optional):**
    
        diff
        --- a/keywords4cv_0.24.py
        +++ b/keywords4cv_0.24.py
        @@ -3398,7 +3398,7 @@
         class OptimizedATS:
        -    def __init__(self, config_path):
        +    def __init__(self): # Now in ConfigManager
                 self.config = self.load_config(config_path)
                 self.nlp = spacy.load(self.config["spacy_model"])
                 self.stopwords = set(stopwords.words('english'))
        @@ -3415,6 +3415,12 @@
                 # ... (rest of the class) ...
        
        +class ConfigManager:
        +    def __init__(self, config_path):
        +       self.config = self.load_config(config_path)
        +    def load_config(self, config_path):
        +       # ... loads and validates config ...
    
    **AI Consensus:**
    
    *   **Support:**
        *   AI-1 (High)
        *   AI-3 (High)
    *   **Opposition:**
        *   AI-2 (Low)
    
    **AI Feedback Quality:**
    
    *   **AI-1:**
        *   **Accuracy:** Accurate
        *   **Usefulness:** Highly Useful
        *   **Completeness:** Complete
        *   **Reasoning Quality:** Strong
        *   **Specificity:** Highly Specific
        *   **Notes:** Identified the core issue. Meta-AI-1 confirmed this.
    *   **AI-2:**
        *   **Accuracy:** Inaccurate
        *   **Usefulness:** Not Useful
        *   **Completeness:** Incomplete
        *   **Reasoning Quality:** Weak
        *   **Specificity:** Vague
        *   **Notes:** Missed the problem. Meta-AI-2 flagged this.
     *   **AI-3:**
        *   **Accuracy:** Accurate
        *   **Usefulness:** Highly Useful
        *   **Completeness:** Complete
        *   **Reasoning Quality:** Strong
        *   **Specificity:** Highly Specific
        *   **Notes:** Identified the core issue. Meta-AI-1 confirmed this.
    
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

---

## 5. Rules for AI Processing (Step-by-Step)

1.  **Parse Feedback:**
    *   Read all feedback files (initial and feedback-on-feedback).
    *   Identify feedback statements, attributing each to a source (AI-1, Meta-AI-1, etc.).
    *   Extract:
        *   The *issue*.
        *   The *proposed solution*.
        *   *Code evidence*.
        *   *Severity*.
        *   *Reasoning*.
        *   *Uncertainties*.
    *   For feedback-on-feedback, identify which *initial* AI and feedback it references.

2.  **Consolidate and Validate:**
    *   Group related feedback (initial and feedback-on-feedback).
    *   Identify contradictions (within initial feedback and between initial and feedback-on-feedback).
    *   Flag missing code evidence.

3.  **Prioritize (Initial Assessment):**
    *   **Critical:** Critical/severe issues with code evidence, *not refuted by feedback-on-feedback*. Strong consensus, *validated by feedback-on-feedback*.
    *   **High:** Performance/maintainability/quality issues with evidence, *not refuted (or weakly refuted) by feedback-on-feedback*.
    *   **Medium:** Minor issues.
    *   **Low:** Minor issues with little impact.
    *   **Unknown:** Severity cannot be determined.

4.  **Generate Improvement Entries:**
    *   Create an entry for each issue.
    *   Fill in all fields.
    *   Use `Unresolved Ambiguities` for uncertainties.

5.  **Calculate Confidence Score:**

    *   **Base Score:** 3.
    *   **AI Support (Initial):**
        *   +1 if at least two *initial* AIs support.
        *   +1 if average support strength is "High".
        *   -1 if significant opposition *within initial feedback*.
    *   **Code Evidence:**
        *   +1 if "Direct Reference".
        *   0 if "No Reference".
    *   **Feedback-on-Feedback Impact:**
        *   -2 if *any* Meta-AI strongly refutes.
        *   +2 if *any* Meta-AI strongly confirms.
    *   **Final Score:** Clamp between -3 and 5.

    ```python
    # Pseudocode
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

5.  b **Determine AI Consensus Strength Labels:**
    *   For *each* improvement, and for *each* AI that provided relevant feedback:
        *   **Initial Assessment:**  Based on the *text* of the AI's feedback (as described in your provided explanation), make an initial assessment of the strength of support/opposition ("High," "Medium," "Low").  This is a preliminary judgment based on keywords, tone, etc.
        *   **Adjust Based on Feedback Quality:**  *Modify* the initial assessment based on the `AI Feedback Quality` ratings for that AI.  Use the following guidelines:
            *   **High Quality (All or mostly "Accurate," "Highly Useful," "Complete," "Strong," "Highly Specific"):**  If the initial assessment was "High," keep it as "High." If it was "Medium" or "Low," consider upgrading it *if* the feedback quality strongly justifies it.
            *   **Mixed Quality:**  If the initial assessment was "High," consider downgrading it to "Medium" if the feedback quality is questionable.  If it was "Medium," keep it as "Medium."  If it was "Low," keep it as "Low."
            *   **Low Quality (All or mostly "Inaccurate," "Not Useful," "Incomplete," "Weak," "Vague"):**  Downgrade the initial assessment.  If it was "High," downgrade to "Medium" or "Low." If it was "Medium," downgrade to "Low."  If it was "Low," keep it as "Low."
            * The Feedback-on-Feedback information is considered in the quality assessment.
        *   **Final Label:** Assign the final "High," "Medium," or "Low" label to the AI's stance in the `AI Consensus` section.

    ```python
    # Pseudocode for Strength Label Determination
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

6.  **Determine Conflict Severity:** Conflicts *within initial AI feedback*.

    *   **High:** Fundamental disagreement.
    *   **Medium:** Disagreement on solution/severity.
    *   **Low:** Minor differences.

7.  **Set `Needs Human Verification`:**

    *   `true` if "No Reference" or "Indirect Reference".
    *   `true` if `Conflict Severity` is "High".
    *   `true` if `Unresolved Ambiguities`.
    *   `true` if *any* Meta-AI raises significant concerns.
    *   `false` otherwise.

8.  **Assess Feedback Quality:**
    *   For each improvement, and each *initial* AI, assess `accuracy`, `usefulness`, `completeness`, `reasoning quality`, and `specificity`.
    *   ***Use the feedback-on-feedback.***
    *   Provide `notes`, *mentioning relevant Meta-AI feedback*.

9.  **Handle feedback references:** List feedback files (differentiate initial and meta).

---

## 6. AI Processing Efficiency Guidelines

*   **Chunking:** 500-word chunks.
*   **Caching:** Cache code locations and issues.
*   **Fallback Logic:** Include raw, unparsed feedback in `Human Review Notes`.

---

## 7. Prompt Closure Instructions

**Final AI Directive:**

"You are a Reasoning AI assisting a developer. You will receive *initial* AI feedback and, optionally, *feedback-on-feedback*. Generate a prioritized list of actionable improvements.

Prioritize:

*   **Clarity:** Easy to understand output.
*   **Accuracy:** Faithfully represent feedback *and code*.
*   **Completeness:** Include all relevant information.
*   **Ambiguity Detection:** Flag uncertainties.
*   **Efficiency:** Use the guidelines.
*   **Feedback-on-Feedback Integration:** Use feedback-on-feedback to *critically* assess the quality of the *initial* feedback.
*   **Detailed Quality Assessment**: Provide a *detailed* feedback quality assessment.
*  **Feedback Quality Integration:** *Crucially*, use the `AI Feedback Quality` assessment to determine the strength labels ("High," "Medium," "Low") in the `AI Consensus` section. A low-quality feedback source should *not* be given a "High" strength rating, even if its text appears strongly supportive or oppositional. The feedback-on-feedback is an important part of the quality assessment.

Format output as a Markdown document (Section 4). Be meticulous."