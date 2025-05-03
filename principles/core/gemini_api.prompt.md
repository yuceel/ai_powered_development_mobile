# Gemini API Prompt Usage Guide (Pro Edition)

## Introduction

`flutter_gemini` is a robust SDK that enables seamless integration of Google's advanced large language models (LLMs), Gemini AI, into your Flutter applications. This package supports text generation, multi-modal data interaction, and multi-turn conversations, making it ideal for AI-powered development.

This guide is tailored for professional developers aiming to leverage Gemini API efficiently within modern development environments such as VSCode.

---

## Getting Started

### Setting Up Your API Key

To use the Gemini API, you need an API key. If you don't have one, generate it via [Google AI Studio](https://aistudio.google.com/).

**Best Practice:** Store your API key securely using environment variables or secure storage solutions. Avoid hardcoding sensitive information.

### Initializing Gemini

Initialize Gemini in your app's main function:

```dart
const apiKey = '--- Your Gemini Api Key ---';

void main() {
  Gemini.init(apiKey: apiKey);
  runApp(const MyApp());
}
```

**Tip:** For scalable projects, consider using a configuration management approach to handle different environments (development, staging, production).

---

## Content-Based APIs

### promptStream

`promptStream` allows flexible and efficient interaction with various data types using different `Part` types. This method is ideal for streaming responses and handling large or multi-part prompts.

**Example Usage:**

```dart
Gemini.instance.promptStream(parts: [
  Part.text('Write a story about a magical backpack'),
]).listen((value) {
  print(value?.output);
});
```

#### Available Part Types:
1. `Part.text` | `TextPart`: For sending text data.
2. `Part.inline` | `InlinePart`: For sending raw byte data.
3. `Part.file` | `FilePart`: For sending file data (coming soon).

**Pro Tip:** The modular design of `Part` types allows for future extensibility. You can customize the behavior of `promptStream` to fit your application's unique requirements.

---

### prompt

The `prompt` method sends a question or request and receives an immediate response. It supports various `Part` types, including text, video, or audio.

**Example Usage:**

```dart
Gemini.instance.prompt(parts: [
  Part.text('Write a story about a magical backpack'),
]).then((value) {
  print(value?.output);
}).catchError((e) {
  print('Error: $e');
});
```

**Best Practice:** Use `prompt` for simple, one-off tasks where streaming is unnecessary.

---

## Multi-Turn Conversations (Chat)

Leverage Gemini for multi-turn, free-form conversations to deliver a more natural and interactive user experience.

**Example Usage:**

```dart
final gemini = Gemini.instance;
// Manage your chat history and new messages here
```

**Pro Principle:** Maintain a structured chat history and implement context management for advanced conversational AI features.

---

## Advanced Usage

### Security Settings

Configure various safety settings to control content generation behavior and filter unwanted content.

**Example:**

```dart
Gemini.instance.prompt(
  parts: [Part.text('Your prompt')],
  safetySettings: [
    SafetySetting(
      category: SafetyCategory.hateSpeech,
      threshold: SafetyThreshold.medium,
    ),
    // Add more settings as needed
  ],
);
```

---

### Generation Configuration

Control content generation with advanced parameters:

- **stop sequences:** Array of sequences that will halt generation.
- **temperature:** Controls randomness (0 to 1). Higher values yield more random outputs.
- **topK:** Limits the number of possible outputs considered at each step.
- **topP:** Controls output diversity (0 to 1).

**Example:**

```dart
Gemini.instance.prompt(
  parts: [Part.text('Your prompt')],
  generationConfig: GenerationConfig(
    stopSequences: ['\n'],
    temperature: 0.7,
    topK: 40,
    topP: 0.95,
  ),
);
```

---

## AI Development Principles for VSCode Workflows

- **Code Modularity:** Structure your codebase for easy integration and testing of AI features.
- **Prompt Engineering:** Design prompts carefully for optimal model performance. Maintain a prompt library for reuse and consistency.
- **Version Control:** Use Git for tracking changes in prompts, configurations, and AI logic.
- **Testing:** Implement unit and integration tests for AI-driven features. Mock Gemini responses for reliable CI/CD pipelines.
- **Documentation:** Maintain up-to-date documentation for all AI-related modules and prompt templates.
- **Security:** Never expose API keys or sensitive data in your codebase. Use `.env` files and VSCode extensions for secret management.
- **Monitoring:** Log and monitor AI interactions for quality assurance and debugging.

---

## Example: Pro-Level Integration in VSCode

1. **Environment Setup:**  
   - Store your API key in a `.env` file.
   - Use the `flutter_dotenv` package to load environment variables.

2. **Prompt Library:**  
   - Create a `/prompts` directory to manage and version your prompt templates.

3. **AI Service Layer:**  
   - Abstract Gemini API calls into a dedicated service class for maintainability.

4. **Testing:**  
   - Use mock data and dependency injection for robust testing.

---

## Resources

- [flutter_gemini Documentation](https://pub.dev/packages/flutter_gemini)
- [Google AI Studio](https://aistudio.google.com/)
- [VSCode Extensions for Flutter & Dart](https://marketplace.visualstudio.com/)

⸻
This guide serves as a comprehensive reference for integrating the Gemini API into your Flutter applications. By following these best practices and principles, you can ensure a robust and efficient development process, especially when working within the VSCode environment.