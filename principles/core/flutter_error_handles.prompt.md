# Flutter Error Handling and Debugging Principles

1. When you encounter a "RenderFlex overflowed" message, review your `Row` or `Column` children for missing constraints. Resolve by wrapping widgets in `Flexible`, `Expanded`, or applying explicit constraints.
2. For "Vertical viewport was given unbounded height" errors, ensure scrollable widgets like `ListView` inside a `Column` have a defined height, such as by wrapping with `Expanded` or `SizedBox`.
3. If you see "An InputDecorator...cannot have an unbounded width", wrap widgets like `TextField` in a parent with width constraints, such as `Expanded` or `SizedBox`.
4. Avoid calling `setState` or showing dialogs directly in the build method to prevent "setState called during build" errors. Use user actions or schedule with `addPostFrameCallback` instead.
5. Each `ScrollController` should only be attached to one scrollable widget to prevent "ScrollController is attached to multiple scroll views" errors.
6. For "RenderBox was not laid out" issues, check for missing or infinite constraints, especially with `ListView` or `Column`.
7. Use the Flutter Inspector to analyze widget constraints and debug layout problems. Consult the official documentation for constraint rules.
8. Wrap asynchronous or error-prone code in `try-catch` blocks, using `on` for specific error types and `catch` for general exceptions. Log errors in the `catch` or `on` block for easier debugging.
9. Add debug logs or print statements inside error handlers to capture stack traces and error details during development.
10. For custom error handling, implement a global error widget or override `FlutterError.onError` to catch and log uncaught Flutter errors.

