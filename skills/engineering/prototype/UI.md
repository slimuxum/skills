# UI/HMI Prototype

Generate **several radically different UI/HMI variations** in the runtime that will actually render them. Give the user a repeatable, environment-appropriate way to switch or compare the variants, choose one, or combine parts of several.

Use this branch only when the unresolved question is genuinely visual or interactive. If the question is about logic, state, interfaces, timing, or target behaviour rather than presentation and interaction, use [LOGIC.md](LOGIC.md).

## Identify the rendering environment first

Before choosing an artifact or selector, establish:

- UI runtime and framework;
- display resolution, pixel density, colour depth, orientation, and refresh constraints;
- input model: touch, buttons, rotary controller, keyboard, steering-wheel controls, or another device;
- existing screen/navigation host, design system, fonts, assets, and localization constraints;
- data source and states to render, including faults, unavailable data, startup, and degraded modes;
- cheapest environment that uses the real renderer: host preview, simulator, emulator, target, or HIL.

Do not substitute a browser mockup for a Qt/QML, LVGL, native, or instrument-cluster question unless the user explicitly accepts the fidelity loss.

Typical shapes:

- **Qt/QML:** mount variants in the existing view or a small host using the project's Qt version, imports, theme, fonts, and renderer. Switch with a development-only property, `Loader`, `StackLayout`, debug control, or separate launch argument.
- **LVGL:** use the actual display geometry, theme, font assets, and input driver contract. Switch with a development-only menu, button/encoder gesture, compile-time option, or separate simulator build.
- **Native or instrument HMI:** preserve the real screen state, navigation model, signal inputs, and renderer. Use a diagnostic-only selector, external harness, replay input, or separate development image appropriate to the platform.
- **Web:** an existing route with a query parameter and a development-only floating switcher is valid when the product surface is genuinely Web. It is one implementation, not the default for every UI.

If the runtime, renderer, or input assumptions are unknown and materially affect the answer, stop and ask rather than selecting Web by convenience.

## Prefer the real host

Variants are easier to judge against real density, navigation, fonts, inputs, and surrounding chrome.

1. **Existing surface preferred.** Mount the alternatives at the current screen or view boundary. Keep upstream state and data preparation unchanged; swap only the presentation subtree under evaluation.
2. **Standalone host when necessary.** If no suitable surface exists, create the smallest non-production host that uses the intended renderer, geometry, inputs, and representative states. Do not invent a Web route for a non-Web runtime.

Use recorded, replayed, or synthetic data by default. Keep vehicle, device, backend, and shared-environment mutations disabled unless the user explicitly authorizes them.

## Process

### 1. State the question and comparison conditions

Write one sentence naming the UI question, runtime, host, selector, rendering environment, and states to compare. For example:

> Three telltale-layout variants in the existing QML cluster view, selected by a development launch argument and rendered in the project simulator at the target resolution.

Confirm the writable repository and subtree. For target or HIL use, also confirm the exact equipment, exclusive-use rules, operation authorization, safe state, stop conditions, and recovery.

### 2. Generate radically different variants

Default to **3 variants** and cap at 5. Make them disagree about layout, information hierarchy, grouping, prioritization, navigation, or primary affordance—not merely colour or copy.

Hold every variant to the same:

- real display and input constraints;
- component/theme/font conventions;
- representative normal, boundary, fault, startup, and degraded states relevant to the question;
- data inputs and reset conditions, so the comparison is fair;
- production constraints that materially affect the visual answer, such as update rate, memory, or safety-related visibility.

Give each variant a clear development-only identity such as `A`, `B`, and `C`. Do not add a production abstraction merely to share prototype code.

### 3. Add an environment-appropriate selector

Make switching repeatable without turning one platform's mechanism into a universal rule:

- Qt/QML may use a launch property, debug control, `Loader`, or `StackLayout`;
- LVGL may use a debug menu, physical input gesture, compile-time setting, or distinct simulator targets;
- native/instrument HMI may use an external harness, diagnostic-only control, replay configuration, or separate development images;
- Web may use a route/query parameter and floating switcher.

The selector must:

- identify the current variant clearly;
- preserve or reset inputs consistently between variants;
- remain outside production output through the project's own build or feature mechanism;
- avoid real vehicle, account, device, or backend mutations;
- be removable without changing the chosen design.

When live switching would distort timing, memory, startup, or target behaviour, prefer separately built variants with an identical replay or test procedure.

### 4. Render and compare in a faithful environment

Build and run the cheapest environment that uses the intended renderer. Static code inspection alone does not validate what a UI looks like.

Exercise the relevant display sizes, inputs, and states. Capture screenshots, recordings, simulator output, or a bounded human observation as appropriate. Record the runtime, source revision, build or launch command, inputs, expected observation, actual observation, and fidelity limits.

Host or simulator rendering does not prove target-only timing, GPU/display integration, memory, startup, physical controls, luminance, or electrical behaviour. Use target or HIL only when those properties are load-bearing and the operation is explicitly authorized with a recovery path.

### 5. Capture the answer and clean up

Record which variant or combination answered the question and why. Fold the chosen direction into the real production surface and run its normal verification; do not carry losing variants or the development selector into production. Commit the prototype and comparison evidence to a throwaway branch outside main, leave a context pointer to that branch on the implementation issue, and never push.

Keep losing variants and selectors only on the throwaway prototype branch as comparison evidence; remove them from production output and record how to reproduce the comparison.

## Anti-patterns

- **Browser by convenience.** A Web mockup cannot answer a native renderer, target geometry, physical-input, or target-performance question.
- **One selector everywhere.** URL parameters, QML properties, LVGL debug menus, and separate target images solve different constraints.
- **Cosmetic variants.** Three colour palettes are not three design alternatives.
- **Unfair comparison.** Variants use different inputs, states, resolutions, or reset conditions.
- **Unapproved target operation.** A visual question does not authorize flashing, vehicle interaction, target writes, or HIL use.
- **Prototype leakage.** Development selectors, shortcuts, and losing variants remain in production output.
