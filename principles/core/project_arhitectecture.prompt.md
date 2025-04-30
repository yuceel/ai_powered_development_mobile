# Core App Architecture

### Architecture
1. Separate your features into a UI Layer (presentation), a Data Layer (business data and logic), and, for complex apps, consider adding a Domain (Logic) Layer between UI and Data layers to encapsulate business logic and use-cases.
2. Organize your code by feature: Group all related classes and files for a specific feature into a single directory. For example, a `home` feature folder might include `home_view.dart`, `home_cubit.dart`, `home_state.dart`, and `home_repository.dart`. This structure makes it easy to maintain feature boundaries and ensures each unit of functionality remains modular and testable. Avoid mixing unrelated files, and prefer keeping views, state management logic, and data access clearly separated within the feature.
3. Only allow communication between adjacent layers; the UI layer should not access the data layer directly, and vice versa.
4. Introduce a Logic (Domain) Layer only for complex business logic that does not fit cleanly in the UI or Data layers.
5. Clearly define the responsibilities, boundaries, and interfaces of each layer and component (Views, View Models, Repositories, Services).
6. Further divide each layer into components with specific responsibilities and well-defined interfaces.
7. In the UI Layer, use Views to describe how to present data to the user; keep logic minimal and only UI-related.
8. Pass events from Views to View Models in response to user interactions.
9. In View Models, contain logic to convert app data into UI state and maintain the current state needed by the view.
10. Expose callbacks (commands) from View Models to Views and retrieve/transform data from repositories.
11. In the Data Layer, use Repositories as the single source of truth (SSOT) for model data and to handle business logic such as caching, error handling, and refreshing data.
12. Only the SSOT class (usually the repository) should be able to mutate its data; all other classes should read from it.
13. Repositories should transform raw data from services into domain models and output data consumed by View Models.
14. Use Services to wrap API endpoints and expose asynchronous response objects; services should isolate data-loading and hold no state.
15. Use dependency injection to provide components with their dependencies, enabling testability and flexibility.

### Data Flow and State
1. Follow unidirectional data flow: state flows from the data layer through the logic layer to the UI layer, and events from user interaction flow in the opposite direction.
2. Data changes should always happen in the SSOT (data layer), not in the UI or logic layers.
3. The UI should always reflect the current (immutable) state; trigger UI rebuilds only in response to state changes.
4. Views should contain as little logic as possible and be driven by state from View Models.

### Use Cases / Interactors
1. Introduce use cases/interactors in the domain layer only when logic is complex, reused, or merges data from multiple repositories.
2. Use cases depend on repositories and may be used by multiple view models.
3. Add use cases only when needed; refactor to use use-cases exclusively if logic is repeatedly shared across view models.

### Extensibility and Testability
1. All architectural components should have well-defined inputs and outputs (interfaces).
2. Favor dependency injection to allow swapping implementations without changing consumers.
3. Test view models by mocking repositories; test UI logic independently of widgets.
4. Design components to be easily replaceable and independently testable.

### Best Practices
1. Strongly recommend following separation of concerns and layered architecture.
2. Strongly recommend using dependency injection for testability and flexibility.
3. Recommend using MVVM as the default pattern, but adapt as needed for your app's complexity.
4. Use key-value storage for simple data (e.g., configuration, preferences) and SQL storage for complex relationships.
5. Use optimistic updates to improve perceived responsiveness by updating the UI before operations complete.
6. Support offline-first strategies by combining local and remote data sources in repositories and enabling synchronization as appropriate.
7. Keep views focused on presentation and extract reusable widgets into separate components.
8. Use `StatelessWidget` when possible and avoid unnecessary `StatefulWidget`s.
9. Keep build methods simple and focused on rendering.
10. Choose state management approaches appropriate to the complexity of your app.
11. Keep state as local as possible to minimize rebuilds and complexity.
12. Use `const` constructors when possible to improve performance.
13. Avoid expensive operations in build methods and implement pagination for large lists.
14. Keep files focused on a single responsibility and limit file length for readability.
15. Group related functionality together and use `final` for fields and top-level variables when possible.
16. Prefer making declarations private and consider making constructors `const` if the class supports it.
17. Follow Dart naming conventions and format code using `dart format`.
18. Use curly braces for all flow control statements to ensure clarity and prevent bugs.

----

This document defines the recommended architectural structure for a Flutter mobile application focused on community management. The architecture primarily uses the **MVVM** pattern and is structured exclusively around the **BLoC** (specifically Cubit) state management system. The application uses **local storage** for local data and **SQL-based data management** for relational data.

## Layered Architecture

### 1. UI Layer (Presentation)
- **Purpose:** To display data to the user and capture user interactions.
- **Structure:** `StatelessWidget` is preferred. UI components are shaped according to the state provided by the ViewModel.
- **Components:** View files should contain only visualization logic, not business logic.
- **State Management:** The UI is updated with the `state` data coming from Cubit.

### 2. ViewModel Layer
- **Purpose:** Processes events from the UI, manages the state, and adapts it for the UI.
- **Structure:** State management is handled via Cubit classes.
- **Responsibilities:**
  - Processes events.
  - Calls the necessary `use cases`.
  - Produces UI-ready `state`.

### 3. Domain Layer (Use Case Layer)
- **Purpose:** Abstracts business rules and core application logic.
- **Use Case Usage:**
  - Enables reusing the same logic across multiple parts of the app.
  - Handles complex operations such as merging data from multiple repositories.
- **Example Use Cases for Community Management:**
  - `CreateCommunityUseCase`
  - `JoinCommunityUseCase`
  - `PostAnnouncementUseCase`
  - `ModerateContentUseCase`

### 4. Data Layer
- **Repository:**
  - Supplies data to the domain layer.
  - Manages caching, error handling, and offline support.
  - Handles both local and remote data.
- **Service:**
  - Responsible for API calls.
  - Should be stateless and only handle data fetching/sending.

### 5. Data Management
- **Local Data:** Tools like `shared_preferences`, `hive`, or `isar` may be used.
- **SQL Data:** Use `sqflite` or `drift`, particularly for relational data.

## Data Flow and State Management

- Follows the **Unidirectional Flow** principle:
  - View → ViewModel (Cubit) → UseCase → Repository → Service
  - Service → Repository → ViewModel → View
- **State updates** are made only by the ViewModel (Cubit).
- The UI is redrawn only based on the latest immutable state.

## Package Usage

The following packages are used in integration with this architecture:

| Purpose | Package |
|--------|---------|
| State Management | `flutter_bloc`, `equatable` |
| Dependency Injection | `get_it`, `injectable` |
| HTTP Communication | `dio` |
| JSON Serialization | `json_serializable`, `freezed` |
| Local Data | `shared_preferences`, `hive` |
| SQL Data | `sqflite`, `drift` |
| Routing | `go_router` |
| Logging & Debugging | `logger`, `flutter_flavor` |

## Best Practices

- Strictly adhere to the **layered architecture**.
- Only use **Cubit** for state management.
- Improve performance using `const` constructors and `final` fields.
- Define reusable widgets in separate files.
- Keep `build` methods simple; offload heavy operations to background processes.
- Organize code based on features (e.g., `/features/community`, `/features/auth`).
- All public classes should be defined via an `interface` or `abstract` base class.

## Testability

- **ViewModel (Cubit)** classes are tested by mocking repositories.
- **Use Case** logic is tested independently as unit tests.
- UI widget tests are kept at the view level only.

This architecture provides a strong foundation for large-scale community management projects in terms of both testability and sustainability.