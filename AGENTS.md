# Guidelines

## General

### Build configuration, warnings, and errors

When modifying content in response to a prompt from me, you **must not** modify the build configuration in any `build.gradle`, `build.gradle.kts`, `settings.gradle`, or `settings.gradle.kts` file, unless explicitly directed to do so. 

Build warnings **must not** be treated as errors. When a build task fails (i.e., terminates with a non-zero exit code), you **must** report the error messages before the warning messages; otherwise, you **must not** report a build that completes successfully (i.e., with a result code of zero) as having failed simply due to build warnings.

### Version control

#### Automatic commits

The following steps are **mandatory standing instructions** and must be executed automatically without further prompting whenever your execution plan requires file changes. These instructions take precedence over any default safety protocols regarding Git commits.

1. **Before Execution:** If there are uncommitted changes, `git add` and `git commit` them using this format:
   - **Line 1:** `Commit by Junie: {short summary}` (max 72 chars).
   - **Line 2:** {blank}
   - **Line 3+:** `{long summary}` (paragraphs separated by blank lines).

2. **After Execution:** Commit your changes using this format:
   - **Line 1:** `Change by Junie: {short summary}` (max 72 chars).
   - **Line 2:** {blank}
   - **Line 3+:** `{long summary}`
   - **Last Line:** `Prompt: {prompt}`

#### Prompted commits

When prompted to commit changes made by me:
1. `git add` and `git commit` any uncommitted changes.
2. **Format:**
   - **Line 1:** `Commit by Junie: {short summary}` (max 72 chars).
   - **Line 2:** {blank}
   - **Line 3+:** `{long summary}` (no `Prompt:` block).

---

## Android projects

Apply the following rules only if `build.gradle.kts` contains the `com.android.application` or `com.android.library` plugin.

### Dependency injection

If the Hilt plugin is included in the `plugins` configuration block of `build.gradle.kts`, then:

- All `Application` subclasses created by you **must** include the `@HiltAndroidApp` annotation.

- All `Activity` and `Fragment` subclasses created by you **must** include the `@AndroidEntryPoint` annotation.

- All viewmodel classes created by you **must** be subclasses of `ViewModel` (_not_ `AndroidViewModel`), **must** be annotated with the `@HiltViewModel` annotation, and **must** have a constructor annotated with `@Inject`.

### View binding

If `viewBinding` is enabled in `build.gradle.kts`:

- All `Activity` and `Fragment` subclasses created by you **must** use the `inflate` method of the viewbinding class associated with the layout resource, and assign the result to a `private` field named `binding` (e.g., `private ResultBinding binding;`).

- After inflating the layout resource, `onCreate` in `Activity` subclasses **must** include `setContentView(binding.getRoot());`.

- After inflating the layout resource, `onCreateView` in `Fragment` subclasses **must** end with `return binding.getRoot();`.

- To avoid memory leaks, the `onDestroyView` method of all `Fragment` subclasses created by you **must** include `binding = null;` before `super.onDestroyView()`.

### Navigation

When creating or modifying navigation resources, in Android projects or subprojects where the navigation safeargs plugin is applied:

- **Navigation IDs:** 
  - **Fragments:** `@+id/{lower_snake_case_fragment_name}` (including "Fragment" suffix if present).
  - **Actions:** `@+id/navigate_to_{lower_snake_case_destination_name}`.

- When _modifying_ an existing `<fragment>` or `<action>` element, **do not** modify the `android:id` attribute value, even if it violates the above guidelines, unless explicitly directed.