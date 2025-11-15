# Flutter App Standards – plan.md

Opinionated standards for how Flutter projects should be structured so they remain consistent, maintainable, and easy to extend (and readable by automation tools).

---

## 1. Goals

- Clear, predictable project structure
- Separation of concerns (presentation / domain / data)
- Feature-first organisation
- Consistent naming and coding style
- Easy to test, refactor, and generate code

---

## 2. Tech & Architecture Assumptions

- **Flutter:** Latest stable channel
- **Language:** Dart with sound null safety
- **Architecture style:** Clean-ish, feature-first
- **Layers:**
  - Presentation (widgets, controllers)
  - Domain (use cases, entities)
  - Data (repositories, data sources, models)

Recommended libraries:
- Routing: `go_router`
- State management: `provider` (or equivalent)
- Networking: `dio`
- Local storage: `shared_preferences` or `hive`

---

## 3. Project Structure

```text
lib/
  core/
    config/
    constants/
    errors/
    routing/
    theme/
    utils/
  features/
    <feature_name>/
      data/
        datasources/
        models/
        repositories/
      domain/
        entities/
        repositories/
        usecases/
      presentation/
        pages/
        widgets/
        controllers/
  shared/
    widgets/
    services/
    models/
  main_development.dart
  main_staging.dart
  main_production.dart
```

---

## 4. Naming Conventions

| Type | Rule | Example |
|------|------|---------|
| Files | snake_case | `login_page.dart` |
| Classes | PascalCase | `LoginController` |
| Variables/methods | camelCase | `isLoading` |
| Page widgets | end in `Page` | `HomePage` |
| DTO models | end in `Model` | `UserModel` |
| Domain entities | end in `Entity` | `UserEntity` |
| Repository interfaces | end in `Repository` | `AuthRepository` |
| Repository impls | end in `RepositoryImpl` | `AuthRepositoryImpl` |

---

## 5. Theming & UI Standards

- All theme assets in `core/theme/`
- Never hard-code colours or text styles in widgets
- Use shared UI components where possible
- Use spacing/constants from core rather than magic numbers
- Widgets should remain focused and lightweight

---

## 6. State Management Guidelines

- Controllers hold business/UI state for each feature
- State objects remain immutable
- Async work returns typed results (e.g. success/error)
- Avoid business logic inside widgets

Folder pattern:

```text
features/<feature>/presentation/controllers/
```

---

## 7. Routing

- Centralised routing under `core/routing/`
- Use named and typed routes
- Authentication guards handled in router config

Example files:
- `app_router.dart`
- `routes.dart`

---

## 8. Data Layer Rules

- Repositories defined in domain layer
- Implemented in data layer
- Local & remote sources kept separate
- Entities are pure domain types
- Models handle JSON and conversion logic

Remote data sources should throw structured exceptions:
- `ServerException`
- `CacheException`

Repositories convert errors into friendly `Failure` types

---

## 9. Entry Points / Environments

```text
main_development.dart
main_staging.dart
main_production.dart
```

Each:
- Loads environment config
- Bootstraps the application

Secrets are not committed to git

---

## 10. Testing

Minimum:

- Use case tests
- Repository tests (mocking datasources)
- Widget tests for key screens

Mirror the source folders in `test/`

```text
test/features/<feature>/
```

---

## 11. Code Quality Requirements

- Run `dart format` and static analysis before commits
- Avoid unused code
- Consistent architecture enforced by PR/code review
- Document feature boundaries when created

---

## 12. Feature Delivery Checklist

Each new feature must include:

☑ Folder structure created under `features/<feature_name>/`  
☑ Entities, repository interface, and use cases defined  
☑ Data source(s) and repository implementation added  
☑ UI pages/widgets/controllers created  
☑ Routes added / updated  
☑ Connected to theme and shared components  
☑ Tests added or updated  
☑ Code formatted and analysis passes  

---

This `flutter_standardsmd` must be kept up-to-date and followed for all new Flutter applications and features.
