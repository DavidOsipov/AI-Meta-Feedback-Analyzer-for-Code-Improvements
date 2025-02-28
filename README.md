# AI Meta Feedback Analyzer for Code Improvements

This framework guides Reasoning AI systems to process unstructured AI feedback and feedback-on-feedback to generate prioritized lists of software improvements. The output is a human-reviewable, Markdown-formatted document that helps developers implement changes efficiently.

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

1. Prepare your AI feedback files and feedback-on-feedback files
2. Copy the prompt from [prompt.md](prompt.md)
3. Send the prompt to your AI system, followed by your feedback files
4. Review the generated improvement list

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
