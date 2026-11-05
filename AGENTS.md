# Global iOS Development Instructions

These instructions apply to all of my iOS and Swift projects.

## 1. Autonomy

Work autonomously by default.

Do not ask me questions about routine implementation decisions.

When a reasonable engineering decision can be made from:
- the existing codebase,
- Apple platform conventions,
- modern Swift / SwiftUI practices,
- or common software engineering principles,

make the decision yourself and continue.

Prefer implementation over asking for confirmation.

If multiple valid approaches exist:
1. Prefer the simplest maintainable solution.
2. Prefer native Apple frameworks.
3. Prefer modern APIs.
4. Follow the existing project's conventions when they are reasonable.
5. Continue without asking me.

Do not stop because minor implementation details are unspecified.

If you need to make a reasonable assumption, make it and mention it briefly in the final summary.

---

## 2. When You Should Ask Me

Ask me only when the decision is genuinely important and cannot reasonably be inferred.

Examples:

- A decision significantly changes the intended product behavior.
- Requirements directly contradict each other.
- A destructive or difficult-to-reverse operation is required.
- Existing user data may be deleted or permanently migrated.
- Credentials, certificates, signing identities, secrets, API keys, or production access are required.
- A paid external service would need to be introduced.
- A major architectural decision would substantially affect the entire application and there is not enough project context to choose safely.

Do NOT ask me about routine decisions such as:

- variable or function names,
- file placement,
- minor UI spacing,
- implementation details,
- whether to use an enum or struct,
- standard SwiftUI state management decisions,
- straightforward refactoring choices,
- normal error-handling details,
- standard navigation implementation,
- minor styling decisions.

Make those decisions yourself.

---

## 3. Inspect Before Implementing

Before making significant changes:

1. Inspect the relevant existing files.
2. Understand the current project structure.
3. Identify existing patterns and reusable components.
4. Check whether similar functionality already exists.
5. Reuse existing implementations when appropriate.

Do not introduce a new architectural pattern when the existing project already has a reasonable one.

Do not rewrite working code without a clear reason.

Do not modify unrelated code.

---

## 4. Swift & iOS Defaults

Prefer modern Swift and current Apple platform APIs.

Use:

- Swift concurrency with `async/await`
- structured concurrency
- `Task`
- `@MainActor` where UI-bound state requires it
- modern Swift type safety
- value types when appropriate
- clear access control
- dependency injection where it improves testability
- Apple-native frameworks when practical

Avoid:

- unnecessary singletons
- unnecessary force unwraps
- unnecessary force casts
- callback-based APIs when a clean async/await API is available
- deprecated APIs
- unnecessary Objective-C patterns
- excessive abstraction
- premature optimization

Treat compiler warnings seriously.

Do not silence concurrency or Sendable warnings without understanding the underlying issue.

---

## 5. SwiftUI Defaults

For new user interfaces, prefer SwiftUI unless the existing project or a technical limitation requires UIKit.

Prefer modern SwiftUI APIs and patterns.

Use:

- `NavigationStack` for navigation
- `navigationDestination` for typed navigation where appropriate
- `@State` for local view state
- `@Binding` when a child needs to mutate parent-owned state
- the Observation framework and `@Observable` for shared observable application state when appropriate
- environment-based dependency access when it produces a cleaner design
- reusable SwiftUI components for repeated UI
- `ViewModifier` when a reusable visual behavior makes sense
- `sheet`, `fullScreenCover`, and modern presentation APIs appropriately

Keep views focused.

If a SwiftUI view becomes large or contains clearly independent UI sections, extract meaningful subviews.

Do not split views into tiny components without a readability or reuse benefit.

---

## 6. State Management

Keep state ownership clear.

A view should own state only when that state logically belongs to the view.

Business logic should not be buried inside large SwiftUI view bodies.

Prefer separating:

- UI state
- business logic
- data access
- networking
- persistence

when doing so materially improves maintainability.

Avoid creating unnecessary ViewModels for extremely simple screens.

For screens with meaningful business logic, use a dedicated observable model or feature model.

Do not blindly apply MVVM everywhere.

Choose the smallest architecture that keeps responsibilities clear.

---

## 7. Architecture

Prefer simple, maintainable architecture over elaborate patterns.

For new projects, favor feature-oriented organization when appropriate.

Example:

App/
Features/
    Home/
    Settings/
    Profile/
Core/
    Networking/
    Persistence/
    Services/
    Extensions/
    Utilities/
Resources/

Do not create layers merely for architectural purity.

Introduce protocols when they provide real benefits such as:

- dependency inversion,
- testing,
- multiple implementations,
- clear service boundaries.

Do not create a protocol for every class automatically.

---

## 8. Networking

Prefer:

- `URLSession`
- `async/await`
- `Codable`
- strongly typed request and response models

Create a networking abstraction when the project has enough networking complexity to justify it.

Handle:

- HTTP errors
- decoding errors
- connectivity failures
- server errors
- invalid responses

appropriately.

Never hard-code secrets or API keys into source code.

---

## 9. Persistence

Choose persistence according to actual requirements.

For modern local model persistence, prefer SwiftData when it fits the deployment target and requirements.

Use simpler storage such as `UserDefaults` / `@AppStorage` only for genuinely small preference values.

Do not store sensitive information in `UserDefaults`.

Use Keychain for secrets, credentials, authentication tokens, or other sensitive values when appropriate.

Think about migrations before making breaking persistence-model changes.

---

## 10. Error Handling

Never silently ignore meaningful errors.

Avoid empty `catch` blocks.

Provide useful error propagation and user feedback where appropriate.

Separate:

- developer/debug information
- user-facing error messages

Do not expose internal technical details to end users unnecessarily.

---

## 11. UI & UX Decisions

When UI details are not specified, follow Apple platform conventions and established patterns from the existing application.

Prefer:

- clear visual hierarchy
- standard system controls
- predictable navigation
- readable typography
- appropriate spacing
- native interaction patterns

Do not ask me to decide every small visual detail.

Choose a sensible implementation and continue.

Do not create overly decorative interfaces unless the product design specifically calls for it.

---

## 12. Accessibility

Treat accessibility as part of normal implementation, not an optional afterthought.

When appropriate:

- provide meaningful accessibility labels
- provide accessibility hints only when they add value
- use appropriate accessibility traits
- support Dynamic Type
- avoid fixed layouts that break with larger text
- maintain sufficient tappable areas
- ensure VoiceOver interactions make sense
- avoid relying only on color to communicate meaning

Prefer native SwiftUI components because they often provide better platform behavior automatically.

---

## 13. Localization

Do not unnecessarily hard-code user-facing strings deep inside business logic.

Structure user-facing text so localization can be introduced or maintained cleanly.

If the project already uses localization infrastructure, follow it.

Do not translate product copy or invent additional languages unless requested.

---

## 14. Dependencies

Prefer Apple-native solutions before adding third-party packages.

Do not add a dependency merely to solve a small problem that can be implemented cleanly with native APIs.

Before adding a new package, consider:

- maintenance status
- necessity
- complexity
- impact on the project
- whether the same result can be achieved cleanly using Apple frameworks

A major third-party dependency or paid service may require asking me first.

---

## 15. Code Quality

Write code that is:

- readable
- maintainable
- idiomatic Swift
- appropriately modular
- easy to understand

Prefer clear code over clever code.

Use meaningful names.

Avoid:

- giant functions
- deeply nested logic
- duplicated business logic
- unnecessary comments
- unnecessary abstractions

Comments should explain why something exists when the reason is not obvious.

Do not comment code that is already self-explanatory.

---

## 16. Testing

When meaningful business logic is introduced or changed, consider whether tests should be added or updated.

Prioritize testing:

- business rules
- transformations
- state transitions
- networking behavior
- persistence behavior
- edge cases

Do not create meaningless tests merely to increase test count.

Follow the project's existing testing framework and conventions.

---

## 17. Build Verification

After meaningful implementation changes:

1. Check for obvious compiler issues.
2. Build the relevant target when possible.
3. Run relevant tests when available.
4. Fix issues caused by your changes.

Do not claim something builds or tests successfully unless it was actually verified.

If verification cannot be performed, state that clearly in the final summary.

---

## 18. Scope Control

Stay focused on the requested task.

Do not:

- redesign unrelated screens,
- rename unrelated types,
- reorganize the whole repository,
- change formatting across unrelated files,
- introduce unrelated features,
- perform large refactors without a clear benefit to the requested task.

Small supporting changes are acceptable when necessary to implement the requested feature correctly.

---

## 19. Application Requirements

These requirements apply to the current SwiftUI application project.

### Language

The application must be in Turkish.

All user-facing text should be written in Turkish unless there is a clear technical or product reason not to.

Avoid unnecessary English product copy inside the app.

System-like messages, empty states, alerts, button titles, screen titles, and form labels should be Turkish.

### Notification System

The application will include a notification system with both:

- Push Notification
- Push Inbox

Push notifications should also appear inside the application's in-app notification inbox.

The notification flow should support the following behavior:

1. A push notification is received.
2. The notification is saved or represented in the in-app notification inbox.
3. The notification can be shown as unread inside the app.
4. When the user taps the notification, the app navigates to the related device detail screen.
5. The notification should contain or resolve enough information to identify the related device.

Use a clean routing approach for notification taps.

Do not hard-code navigation directly inside low-level notification handling code if a cleaner routing abstraction is practical.

Notification-related code should be structured so that real push notification integration can later replace mock behavior without rewriting the entire UI.

### Push Inbox

Create an in-app notification inbox experience.

The inbox should show notification items in a clean list.

Each notification item should support, when appropriate:

- title
- message/body
- date
- read/unread state
- related device identifier
- notification type

Tapping a notification should navigate to the related device detail screen.

If the notification is unread, tapping it should mark it as read.

If there are no notifications, show a Turkish empty state.

### Device Detail Navigation

The app should support navigating to the related device detail screen from:

- device lists
- notification inbox items
- push notification tap events

Use a consistent navigation model.

Avoid duplicating navigation logic in multiple unrelated places.

### Database

Use SwiftData as the local database.

Do not introduce Core Data, Realm, SQLite wrappers, or other persistence frameworks unless there is a strong reason and I explicitly approve it.

Model the application's local data in SwiftData where appropriate.

Expected SwiftData-backed concepts may include:

- devices
- notification inbox items
- read/unread notification state
- relevant app data needed for the demo

Keep SwiftData models clean and simple.

Think about future migrations before making breaking model decisions.

### Mock Data

Use mock data during development and demo.

The app should be cleanly fed by mock data until a real backend is introduced.

Create a mock data provider or mock service layer instead of scattering hard-coded sample data randomly inside views.

The mock structure should make it easy to later replace mock services with real API services.

Avoid messy placeholder code.

Avoid leaving unused sample code, random test strings, or dead demo branches.

### Working UI

All visible buttons and interactive elements inside the application should work.

Do not leave buttons with empty actions.

If a feature is not fully implemented yet, use a reasonable demo behavior such as:

- navigating to a placeholder detail screen,
- showing a Turkish alert,
- updating local mock state,
- opening a related screen,
- toggling a meaningful local state.

Avoid fake UI that does nothing when tapped.

Prioritize:

- clean folder structure
- understandable naming
- visually clean SwiftUI screens
- working interactions
- realistic mock data
- minimal unnecessary complexity
- no broken buttons
- no unfinished-looking placeholder clutter

The app does not need a real backend unless explicitly requested.

The app should be structured so that a real backend can be added later.

---

## 20. Communication Style

Keep communication concise and implementation-focused.

Do not repeatedly explain what you are about to do.

Do not ask for confirmation before routine steps.

Do the work first.

During implementation, only interrupt me when an important decision genuinely requires my input.

At the end, provide a concise summary containing:

### Changed
What you implemented or modified.

### Decisions
Important assumptions or engineering decisions you made autonomously.

### Verification
Builds, tests, or checks you actually performed.

### Remaining
Only real remaining issues, limitations, or decisions that require my input.

Do not invent remaining work just to fill this section.

---

## 21. Core Rule

When uncertain about a minor implementation detail:

**Inspect → Infer → Decide → Implement → Verify**

Do not:

**Inspect → Ask user about every detail → Wait**

My default expectation is that you behave like an experienced iOS engineer who can make reasonable technical decisions independently.
