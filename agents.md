# Cocobean - Android App Guidelines & Architecture

Welcome to **Cocobean**, an offline-first, privacy-focused Android application designed to support expectant and new mothers through every stage of pregnancy and postpartum milestones.

---

## 1. Core Architectural Pillars

### 1.1. 100% On-Device & Privacy-First
- **No Cloud Dependencies**: All user health data, logs, dietary notes, and milestones must remain strictly on the user's device.
- **Local Persistence**: Use **Room Database** for structured relational data (milestones, food logs, reminders, history) and **Jetpack DataStore** for preferences and user settings.
- **Data Backup & Export**: Because data is strictly local, implement robust JSON/SQLite local export and import capabilities so users can securely back up and migrate their records across devices without cloud accounts.

### 1.2. Target Audience & Domain Scope
- **Expectant & New Mothers**: UX/UI must be compassionate, calming, accessible, and reassuring. Avoid cluttered interfaces or alarming red warning colors unless flagging genuine food safety or medical urgency.
- **Pregnancy & Postpartum Journey**: Track gestational weeks/trimesters, fetal development milestones, maternal physical/mental changes, and postpartum recovery milestones (newborn tracking, vaccinations, feeding/sleep logs).

### 1.3. Food Safety & Risk Tracking
- **Dietary Guidance Engine**: Quick lookup and categorization of foods (Safe, Consume in Moderation, Avoid/High Risk) addressing common pregnancy concerns (e.g., unpasteurized cheeses, raw fish/seafood mercury limits, caffeine thresholds, herbal teas).
- **Nutritional & Supplement Guidance**: Contextual warnings and daily tips tailored to specific pregnancy trimesters or postpartum recovery needs.

### 1.4. Reminders & Local Notifications
- **Smart Reminders**: Reliable local notifications via **WorkManager** and **AlarmManager** for:
  - Hydration goals
  - Gentle physical activity (prenatal yoga, walking, pelvic floor exercises)
  - Daily supplements (prenatal vitamins, iron, calcium, folic acid)
  - Self-care & mental health check-ins
- **Calendar Integration**: Seamless integration with the system Calendar provider or in-app scheduling for prenatal appointments, ultrasounds, pediatric check-ups, and immunization schedules.

---

## 2. Additional Recommended Features & Considerations

To make Cocobean an exceptional, comprehensive companion, consider implementing:
1. **Contraction Timer & Kick Counter**: Essential utility tools during the third trimester.
2. **Symptom & Mood Tracker**: Daily logging for morning sickness, back pain, sleep quality, and emotional well-being with trend visualization.
3. **Emergency Quick-Access & Emergency Contacts**: Quick access to doctor info, hospital bag checklist, and emergency guidelines.
4. **Dark Mode & Accessibility**: High-contrast text options, large tap targets, and full TalkBack support for ease of use during late pregnancy or sleep-deprived newborn nights.

---

## 3. Android Development Best Practices

### 3.1. Tech Stack & Libraries
- **Language**: Kotlin (strict null safety, idiomatic functional constructs).
- **UI Toolkit**: Jetpack Compose with Material 3 design system, supporting edge-to-edge layout (`enableEdgeToEdge()`).
- **Architecture**: MVVM (Model-View-ViewModel) with Clean Architecture principles (Presentation, Domain/UseCases, Data/Repositories).
- **Asynchronous Programming**: Kotlin Coroutines & Flows (`StateFlow`, `SharedFlow`).
- **Dependency Injection**: Hilt (or manual constructor injection for lightweight modules).

### 3.2. Coding Standards
- **Unidirectional Data Flow (UDF)**: ViewModels expose immutable `StateFlow<UiState>` to Composables and receive user actions via clean event functions.
- **Separation of Concerns**: Keep Composables lightweight and focused on rendering state; business logic lives in ViewModels and UseCases.
- **Resource Management**: Never block the main thread. All database and disk operations must run on `Dispatchers.IO`.
- **Testing**: Write unit tests for ViewModels and UseCases using JUnit and Turbine for Flows. Include UI tests with Compose JUnit4.
