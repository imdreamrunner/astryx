---
'@astryxdesign/core': patch
---

[feat] i18n migration for remaining ~27 components (#3641). Completes the migration started in #3765 (scaffold + Pagination) and #3922 (PowerSearch): all remaining hardcoded English strings in core components now flow through the `useTranslator` scaffold. 58 new catalog entries under `@astryx.<component>.*`.

Migrated components: AlertDialog, AppShell, AvatarGroup, Banner, Calendar, Carousel, Chat (message metadata + pasted-text token), CheckboxList, CommandPalette, DateRangeInput, DateTimeInput, Dialog, DropdownMenu, Lightbox, Markdown (task list + tables), MobileNav, MultiSelector, Popover, Selector, SideNav, TabList, Table (base + filter/selection/sort plugins), Toast (viewport + dismiss), Tokenizer, TopNav, TreeList, Typeahead.

For components with translated prop defaults (e.g. `Selector.searchPlaceholder`), the convention is: alias the prop in the destructure (`searchPlaceholder: searchPlaceholderFromProps`), then resolve at component top (`const searchPlaceholder = searchPlaceholderFromProps ?? t('@astryx.selector.searchPlaceholder');`). Downstream JSX reads the resolved value with the original prop name. Documenting this pattern for future contributions is tracked in #3931.

`ChatMessageMetadata` uses a module-level config object with `i18nKey` fields resolved at render — pattern established for future enum-like status maps.

`TreeList` switches from `querySelector('[aria-label="Toggle children"]')` to `querySelector('[data-tree-toggle]')` for locale-independent DOM identity — the aria-label is now locale-dependent.

`Markdown.renderBlock` accepts `t: TranslatorFn` as a new required parameter, threaded from the top-level `Markdown` component. Callsites and the function signature updated together.

@nynexman4464
