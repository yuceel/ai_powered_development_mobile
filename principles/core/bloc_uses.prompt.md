# Bloc and Cubit Usage Principles
# This document outlines best practices for using Bloc and Cubit in Flutter applications.
# Cubit-Centric State Management Principles

## Event and State Naming
- Name Cubit methods as imperative verbs reflecting the action (e.g., `loadUser`, `submitForm`).
- For Bloc events, use past tense to indicate completed actions (e.g., `LoginRequested`).
- State classes should be nouns, representing a snapshot (e.g., `LoginSuccess`, `ProfileLoaded`).
- Use `CubitSubjectState` as the base state class for Cubits.
- For Cubit states, suffix with status indicators like `Initial`, `Loading`, `Success`, `Failure`.
- Prefer enums for status within single-state Cubits (e.g., `LoginStatus.loading`).
- Avoid abbreviations in state and event names for clarity.
- Use PascalCase for all Cubit and state class names.

## Modeling State in Cubit
- Extend `Equatable` for all Cubit state classes to ensure value equality.
- Annotate Cubit state classes with `@immutable` for immutability.
- Implement a `copyWith` method for easy state updates.
- Use `const` constructors for Cubit states when possible.
- Store shared properties in the base state class; keep unique fields in subclasses if needed.
- Use nullable fields for optional data and error handling.
- Always emit a new state instance to trigger UI updates.
- Prefer a single state class with a status enum for simple flows; use subclasses for exclusive states.
- Handle all possible state statuses explicitly in the UI.

## Cubit Usage Patterns
- Use Cubit for straightforward state management without events.
- Define the initial state in the Cubit constructor.
- Only call `emit` within Cubit methods; never externally.
- Expose public methods on Cubit for triggering state changes; return `void` or `Future<void>`.
- UI should rebuild in response to Cubit state changes, not internal variables.
- Duplicate state emissions are ignored; only new state instances trigger updates.
- Override `onChange` in Cubit to observe state transitions.
- Use a custom `BlocObserver` to monitor all Cubit and Bloc changes globally.
- Override `onError` in Cubit and BlocObserver for error logging.
- Dispose resources in Cubit’s `close` method to prevent leaks.
- Use event transformers for debouncing or throttling Cubit method calls if needed.
- Prefer Cubit for less boilerplate and easier testing; migrate to Bloc if event-driven logic is required.
- Initialize `BlocObserver` in `main.dart` for global debugging.
- Keep business logic inside Cubit, not in UI widgets.
- Document Cubit methods and state transitions for maintainability.

## Architecture and Layering
- Organize code into Presentation, Logic (Cubit), and Data layers.
- The Data Layer manages data sources and repositories.
- Inject repositories into Cubits via constructors; Cubits should not access data providers directly.
- Avoid Cubit-to-Cubit direct communication; coordinate via the UI layer.
- Use dependency injection for sharing repositories across Cubits.
- Maintain loose coupling between layers for scalability.
- Structure your project consistently; adapt patterns as needed.

## Flutter Cubit Integration
- Use `BlocProvider` to inject Cubits into widget trees.
- Use `BlocBuilder` to rebuild widgets when Cubit state changes.
- Use `BlocListener` for side effects like navigation or showing dialogs.
- Use `BlocConsumer` when both building and listening are required.
- Use `MultiBlocProvider` to provide multiple Cubits efficiently.
- Use `context.read<CubitType>()` for Cubit access in callbacks.
- Use `context.watch<CubitType>()` in build methods to listen for state changes.
- Use `context.select<CubitType, T>()` to listen to specific state fields.
- Avoid unnecessary rebuilds by scoping Cubit listeners.
- Handle all Cubit state statuses in the UI (e.g., loading, error, success).
- Use `Builder` widgets to scope rebuilds when using multiple Cubits.

## Testing Cubits
- Add `test` and `bloc_test` to dev dependencies for Cubit testing.
- Group related tests for shared setup and teardown.
- Create a dedicated test file for each Cubit.
- Use `setUp` and `tearDown` for Cubit lifecycle management in tests.
- Test initial Cubit state before transitions.
- Use `blocTest` to verify state changes in response to Cubit method calls.
- Assert the sequence of emitted states for each Cubit action.
- Keep tests focused and maintainable.
- Mock Cubits in widget tests to verify UI for all state scenarios.