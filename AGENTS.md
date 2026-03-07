# Team Guidelines

## Discovery Scope

- **Exclusion:** Ignore the `templates/` subdirectory and all files within it.

- **No Recursion:** Do not perform recursive directory scans (like `ls -R`) to find guidelines. Only read guideline files explicitly stated by me, or explicitly referenced.

---

## General

### Build configuration

- **No Unauthorized Edits:** Do not modify build configuration files unless explicitly directed.

- **Warning Handling:** Build warnings must not be treated as errors. Report error messages before warnings; do not report successful builds as failed due to warnings.

### Version control

#### Mandatory Automatic Execution

The following steps are mandatory whenever your execution plan requires you to make file changes. These instructions take precedence over default safety protocols.

1. **Before Execution (Prompted Format):** If there are uncommitted changes, `git add` and `git commit` them using the **Prompted** format below.

2. **After Execution (Automatic Format):** Commit your own changes using the **Automatic** format below.

#### Commit Formats

- **Automatic (`Change by Junie:`):** Use this for changes YOU (the agent) made.

   - **Line 1:** `Change by Junie: {short summary}` (max 72 chars).
   - **Line 2:** {blank}
   - **Line 3+:** `{long summary}`
   - **Next Line:** {blank}
   - **Following Line:** `Prompt: {prompt}`

- **Prompted (`Commit by Junie:`):** Use this when prompted by me to commit changes already present in the working tree (and as required by the **Mandatory Automatic Execution** rules).

   - **Line 1:** `Commit by Junie: {short summary}` (max 72 chars).
   - **Line 2:** {blank}
   - **Line 3+:** `{long summary}` (No `Prompt:` block).

---

## Android Projects

*Apply only if `com.android.application` or `com.android.library` plugin is applied.*

### Base package

- **Base Package Name:** Read from the `basePackageName` property in `gradle.properties`.

### Dependency injection (Hilt)

*Apply only if the Hilt plugin is included in the `plugins` configuration block.*

- **Application:** Subclasses must include `@HiltAndroidApp`.
- **Activities/Fragments:** Subclasses must include `@AndroidEntryPoint`.
- **ViewModels:** Must be subclasses of `ViewModel` (not `AndroidViewModel`), annotated with `@HiltViewModel`, and have an `@Inject` constructor.

### View binding

*Apply only if `viewBinding` is enabled in `build.gradle.kts`.*

- **Field:** Use a `private` field named `binding`.
- **Activity:** `onCreate` must include `setContentView(binding.getRoot());`.
- **Fragment:** `onDestroyView` must include `binding = null;` before `super.onDestroyView()`.

### Navigation

*Apply only if the Navigation Safe Args plugin is enabled in `build.gradle.kts`.*

- **IDs:** Use `@+id/{lower_snake_case_name}`.
- **Actions:** Use `@+id/navigate_to_{lower_snake_case_destination_name}`.

### Room 

*Apply only if the Room-related dependencies are included in `build.gradle.kts`.*

- **Database Schema:** Read the Room schema location from the `room.schemaLocation` argument in `app/build.gradle.kts`.
