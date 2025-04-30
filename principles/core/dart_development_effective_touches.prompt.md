# Effective Dart Development Principles

## Naming Patterns
- Maintain consistent terminology throughout your codebase.
- Use `UpperCamelCase` for class, enum, and typedef names.
- Apply `lowercase_with_underscores` for files, folders, and import prefixes.
- Use `lowerCamelCase` for variables, parameters, and functions.
- Avoid uncommon abbreviations; prefer clarity over brevity.
- Place the most descriptive word at the end of names.
- For boolean variables, use positive and descriptive names.
- Use short, conventional type parameter names (e.g., `T`, `E`, `K`, `V`).
- Write API names that read naturally in sentences.
- Use PascalCase for widget and class names.

## Types & Functions
- Always specify types for variables and function parameters unless obvious.
- Explicitly declare return types for all functions.
- Use `Future<void>` for async functions that do not return a value.
- Prefer getters for property-like access and setters for property mutation.
- Use class modifiers to restrict inheritance or implementation as needed.
- Prefer initializing variables at declaration.
- Use generic type arguments when inference is insufficient.
- Use inclusive start and exclusive end for range parameters.
- Use `dynamic` only when type inference fails.

## Code Style
- Format code using `dart format` before committing.
- Always use curly braces for control flow statements.
- Use `final` for variables that do not change after assignment.
- Use `const` for compile-time constants.
- Group related code logically within files.
- Limit file length for readability.
- Prefer private declarations unless public access is required.

## Imports & File Organization
- Use relative imports within your package.
- Avoid importing from another package’s `src` directory.
- Do not use `/lib/` or `../` in import paths.
- Add a library-level doc comment at the top of each library file.
- Keep each file focused on a single responsibility.

## Documentation
- Use `///` for documentation comments on public APIs.
- Begin doc comments with a concise summary sentence.
- Separate the summary from details with a blank line.
- Reference in-scope identifiers with square brackets.
- Place doc comments before annotations.
- Explain the purpose and usage of code, not just its behavior.

## Testing Practices
- Write unit tests for business logic.
- Add widget tests for UI components.
- Strive for comprehensive test coverage.
- Use descriptive test names and group related tests.

## Widget Design
- Extract reusable UI into separate widgets.
- Use `StatelessWidget` unless state is required.
- Keep build methods concise and focused.
- Avoid heavy computations in build methods.

## State Management
- Select state management solutions appropriate to app complexity.
- Minimize the use of `StatefulWidget` where possible.
- Keep state as local as feasible.
- Avoid unnecessary rebuilds by optimizing widget trees.
- Use immutable state objects to prevent unintended side effects.
- Prefer using providers or inherited widgets for shared state.
- Separate UI and business logic by using controllers or view models.
- Dispose controllers and listeners to prevent memory leaks.
- Document the state flow and update triggers for maintainability.
- Test state transitions with unit and integration tests.

## Performance Optimization
- Use `const` constructors wherever possible.
- Prevent expensive operations inside build methods.
- Implement pagination for large data sets.
- Profile and optimize slow code paths.
- Cache expensive computations and reuse widgets when possible.
- Use lazy loading for images and heavy resources.
- Minimize widget rebuilds by using keys and const widgets.
- Avoid deeply nested widget trees; refactor into smaller widgets.
- Monitor app performance using Flutter DevTools.
- Optimize list and grid views for smooth scrolling.
- Prefer asynchronous operations for I/O and network requests.
- Reduce widget rebuild scope by splitting widgets logically.

