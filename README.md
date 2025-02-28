# AI Meta Feedback Analyzer for Code Improvements

This framework (complex prompt) guides Reasoning AI systems to process unstructured AI feedback and feedback-on-feedback to generate prioritized lists of software improvements. The output is a human-reviewable, Markdown-formatted document that helps developers implement changes efficiently.

## 🌟 Features

- Parses free-form text feedback from multiple AI sources
- Extracts key information: issues, solutions, evidence, and reasoning
- Categorizes and prioritizes improvements based on severity
- Calculates confidence scores using a multi-factor algorithm
- Flags ambiguities and conflicts that require human attention
- Evaluates feedback quality with detailed assessment metrics
- Uses feedback-on-feedback to critically evaluate initial AI analyses
- Formats output as a structured Markdown document

## 📋 Use Cases

- Code review and refactoring projects
- Technical debt management
- Quality assurance processes
- AI-augmented development workflows
- Meta-analysis of AI code suggestions

## 🚀 Getting Started

### Prerequisites

- A Reasoning AI system capable of following complex instructions (Claude 3 Opus, GPT-4, etc.)
- Unstructured AI feedback files from code analysis
- Optional: Feedback-on-feedback files (meta-analysis)

### Basic Usage

A human developer will use this framework as follows:

1.  **Gather Unstructured Feedback:** Collect unstructured text feedback on your code from several different AI systems.  This feedback should analyze your code and suggest improvements.

2.  **"Feedback on Feedback" (Optional but Recommended):** Gather additional unstructured text feedback where AIs analyze and critique the *initial* feedback.  This helps identify conflicting opinions and improve the overall quality of the analysis.

3.  **Prepare Input Files:**
    *   Create separate text files for each AI's initial feedback (e.g., `feedback_ai1.txt`, `feedback_ai2.txt`).
    *   Create separate text files for each AI's feedback-on-feedback (e.g., `meta_feedback_ai1.txt`, `meta_feedback_ai2.txt`).  These files should clearly indicate which initial feedback they are referencing.
    *   Ensure your code files are accessible to the Reasoning AI.

4.  **Framework as Prompt:** Copy the entire framework (the prompt) from [prompt.md](prompt.md).

5.  **Send to Reasoning AI:**  Input the following to your Reasoning AI, in this order:
    *   The complete framework (prompt) from [prompt.md](prompt.md).
    *   The initial AI feedback files, one after another.
    *   The feedback-on-feedback files, one after another.
    *   Instructions to begin processing, referencing your code files as context, something like: "Begin processing the feedback, using the provided code files as context."

6.  **AI-Generated List:** The Reasoning AI, guided by the framework, will process the input and produce a draft Actionable Improvement List (in Markdown format).

7.  **Human Review and Refinement:** *Critically review* and *manually adjust* the AI-generated list.  Apply your domain knowledge and judgment.  You may add, modify, or remove entries. The AI-generated list is a *starting point*, not a final product.

8.  **Iterative Improvement:**  This process can be repeated with updated code and feedback.

**Goal:** The framework aims to produce the most useful and accurate *draft* Actionable Improvement List possible, minimizing the manual effort required for post-processing and validation.

### Example Input

```
<Provide the AI with the prompt>

AI Feedback File 1:
The OptimizedATS class is handling too many responsibilities, which violates the Single Responsibility Principle. This class currently manages configuration, NLP processing, keyword extraction, parallel processing, and output formatting. This makes the code harder to maintain and test.

Feedback-on-Feedback File 1:
I've reviewed the initial feedback about the OptimizedATS class. The analysis is accurate - the class does handle multiple responsibilities. The feedback provides specific file references and code locations, which makes it highly actionable.
```

### Example Output

See [example_output.md](examples/example_output.md) for a complete example of the generated improvement list.

## 📊 Implementation Details

The framework employs a structured approach to processing feedback:

1. **Parsing Stage**: Extracts key information from unstructured text
2. **Consolidation Stage**: Groups related feedback and identifies contradictions
3. **Prioritization Stage**: Assigns initial priority based on severity and consensus
4. **Assessment Stage**: Evaluates the quality of each feedback source
5. **Output Generation Stage**: Creates a standardized document with all findings

For a detailed explanation of the algorithms and logic, see [technical_details.md](docs/technical_details.md).

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📞 Contact

If you have questions or feedback, please open an issue on this repository.

## 🙏 Acknowledgments

- The open source AI community for inspiration and feedback
- All contributors who help improve this framework
