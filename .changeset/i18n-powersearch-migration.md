---
'@astryxdesign/core': patch
---

[feat] PowerSearch i18n migration (#3641). All ~50 hardcoded strings in PowerSearch now flow through the i18n system introduced in #3765: UI chrome (`Field`, `Operator`, `+ Add filter`, `Remove filter`, `Delete`, `Cancel`, `Group operator`, `Apply`), value-editor labels + placeholders (`Value`, `Values`, `Time`, `Date`, `Search…`, `Enter number…`, etc.), the 21 built-in operator labels (`contains`, `does not contain`, `is greater than or equal to`, `is any of`, etc.), and the ICU plurals for result counts and overflow summaries (`{count, plural, one {result} other {results}}`, `{count} items`, `{count} entities`, `{count} filters`). Astryx's default English wording is preserved, so consumers who never render an `InternationalizationProvider` see no visible change.

**Public API change (source-breaking, minor in practice)**: `PowerSearchOperator` is now a discriminated union of two variants — `{key, value, label}` (raw text, consumer-owned) OR `{key, value, i18nKey}` (catalog-lookup, resolved through the active locale). Existing consumers passing `label: 'is any of'` compile and behave unchanged. Consumers who want astryx's translated defaults can switch to `i18nKey: '@astryx.powersearch.operator.isAnyOf'` (or any of the 21 shipped keys) and pick up translations for free. The union prevents the "forgot to set a label" footgun at compile time — a bare `{key, value}` is a type error.

New exports: `PowerSearchOperatorBase`, `PowerSearchOperatorWithLabel`, `PowerSearchOperatorWithI18nKey`, `resolveOperatorLabel(op, t)` (used by consumers with custom token renderers via `components.Token`).

@nynexman4464
