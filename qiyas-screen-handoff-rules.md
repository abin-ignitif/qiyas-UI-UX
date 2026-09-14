# Qiyas Screen Handoff Rules

Reusable delivery contract for every Qiyas screen prototype.

This document is the repeatable screen-format and Flutter handoff guide. The visual source of truth remains qiyas-design-system.html. If this file conflicts with the design system, qiyas-design-system.html wins. If a screen needs something neither file defines, record it in that screen's gap list instead of inventing a one-off.

## 1. Every screen must ship in the same format

Each new screen is delivered as one self-contained, semantic HTML file with:

- a descriptive project filename;
- Arabic as the primary direction and English as the secondary direction;
- light and dark theme states;
- a 390 x 844 mobile preview;
- a 360-wide narrow-mobile check;
- a 1440-wide desktop preview;
- an explicit intermediate-breakpoint behavior;
- loading, empty, error, offline, submitting, disabled, focus, hover, and pressed states where applicable;
- a component mapping table using exact names from qiyas-design-system.html;
- a gap list for unresolved product, platform, or system requirements;
- data-od-id on page regions, headings, CTAs, controls, and repeated cards that engineers or reviewers may need to target;
- a visible mobile handoff sheet with dimensions and implementation notes.

The screen prototype is a handoff artifact, not a production app shell. Prototype controls must be visually separate from the product UI.

## 2. Source of truth and extraction rules

Use qiyas-design-system.html for:

- color primitives, ramps, semantic theme tokens, and contrast decisions;
- typography, weights, line-heights, numerals, and mathematical notation;
- spacing, borders, radii, grid, and elevation;
- component names, variants, states, and motion tokens;
- RTL/LTR rules;
- accessibility requirements;
- copy and tone rules.

Rules for composing a screen:

1. Reuse the exact CSS custom property names from the design system, including names such as --tx-2, --line-strong, --r, and --dur.
2. Reuse an existing class or component variant wherever one exists.
3. Never hardcode a value already represented by a token.
4. Express a screen variation as a modifier on an existing component.
5. Do not restyle a system component locally to solve a layout problem.
6. Keep unresolved requirements in the gap list.
7. Reopen the modified file after editing and verify that the intended rules remain intact.

## 3. Document header controls

Every screen prototype uses the same document-header format. This is the engineering preview control surface, not part of the product screen:

- product and screen title at the logical start;
- theme control with Light and Dark states;
- locale control with English and العربية states;
- page controls when the file contains multiple pages or states;
- Preview label or equivalent context label;
- visible keyboard focus;
- aria-pressed or equivalent selected state;
- controls that wrap cleanly instead of creating horizontal scroll.

The page controls belong in the document header beside the preview controls. They do not belong inside the product screen.

For a multi-page prototype:

- show the actual page numbers, such as 01, 02, 03, 04;
- clicking a page number must show that page's actual content, not only change a step label;
- Continue and Back controls must stay synchronized with the active page;
- each page keeps the same mobile and desktop preview containers;
- product copy must not expose the document-preview controls;
- page selection is a prototype navigation aid and is not automatically a production component.

The current approved header pattern is:

Qiyas · Screen title | Light / Dark | English / العربية | page numbers when needed

## 4. Preview canvases and responsive behavior

### Mobile

Primary mobile canvas:

- viewport: 390 x 844;
- required narrow check: 360px wide;
- portrait is the design target;
- surface is full width;
- content uses controlled logical insets rather than a narrow centered card;
- default mobile inset: 16px on each logical side;
- no horizontal scrolling;
- safe-area padding is included at the top and bottom;
- form content remains in normal document flow;
- keyboard-open states must keep the active field and primary action usable;
- long Arabic strings must wrap without clipping or forced horizontal scroll.

### Preview-shell geometry (reusable rule)

The document wrapper and the product surface are separate layers. At mobile host widths:

- Let the phone preview frame use the full available inline size of the device viewport. If the document container has 16px gutters, make the phone frame full-bleed within that wrapper; do not let the wrapper turn a 390px canvas into 358px.
- Keep exactly one 16px logical start/end inset inside the product surface. Do not stack document gutter + preview-frame padding + form padding.
- At 390px, expected usable content is 358px (`390 - 16 - 16`). At 360px, expected usable content is 328px (`360 - 16 - 16`).
- The phone frame itself must not scroll horizontally. Verify at both 390px and 360px, including long Arabic labels and keyboard-open states.
- This full-bleed rule applies to the preview shell only; desktop document sections retain their controlled reading gutters.

Do not cap the entire mobile surface at 358px. A 390px device should use the available width while preserving internal 16px insets and component-level maximum widths where required.

### Desktop

Desktop intent:

- viewport: 1440px wide;
- show the desktop preview as a wide, clear row below or beside the mobile preview;
- do not squeeze the desktop preview into a narrow column;
- preserve the intended two-column layout for authenticated or unauthenticated shells;
- use a practical content cap so very wide screens do not make text measures unreadable;
- form columns normally cap at 480px;
- brand or context rails may expand within their defined shell, but must not force the form into a cramped vertical strip;
- wide empty margins are acceptable outside the product frame, but not inside a form or product content column without a layout reason.

Recommended screen-level caps:

- auth and student product content: 1180px;
- admin or data-dense content: 1280px;
- form column: 480px;
- mobile content: full width minus logical insets.

The desktop preview must be proportionally visible in the document. If the available host viewport is smaller than 1440px, the preview should scale or fit inside its own preview container without cropping the right side.

### Intermediate breakpoints

| Host width | Layout rule |
|---|---|
| 360–389px | Single-column mobile layout. Keep 16px logical insets. Never create horizontal scroll. |
| 390–767px | Single-column mobile layout. Standard controls remain at least 44px high. |
| 768–919px | Use two columns only when both columns remain readable and touchable; otherwise keep one column. |
| 920–1023px | Collapse auth or onboarding rails if they make the form cramped. Keep the form centered with a controlled maximum width. |
| 1024–1439px | Desktop shell may use a brand rail and form column. Preserve the designed proportions. |
| 1440px and above | Apply content caps and keep the desktop preview wide enough to show the intended composition clearly. |

At short desktop heights, allow the product surface to scroll inside the preview. Do not compress type, controls, or touch targets to make every element fit into a short viewport.

## 5. Mobile dimensions for Flutter handoff

These are the standard measurements to put in every screen's mobile handoff sheet. Use design-system tokens where a token exists; the values below describe the intended physical result.

### Global frame

| Element | Mobile specification |
|---|---|
| Primary frame | 390 x 844 |
| Narrow check | 360px wide |
| Horizontal content inset | 16px logical start and end |
| Minimum top safe-area treatment | SafeArea plus at least 32px visual breathing room where the screen begins with a form |
| Bottom safe-area treatment | SafeArea plus at least 16px after the final action |
| Standard control target | Minimum 44 x 44px |
| Exam answer target | Minimum 48 x 48px |
| Compact control | Minimum 40 x 40px only for low-risk secondary controls |
| Default full-width button height | 48px |
| Large primary button height | 56px |
| OTP cell target | At least 48 x 48px, with equal-width cells and a 8px gap unless the source component specifies otherwise |
| Field-to-field vertical gap | 16px default |
| Label-to-control gap | 8px |
| Section gap | 24px or 32px depending on hierarchy |
| Card and control radius | Use the design-system radius token, normally 4px |
| Modular accents | 0px radius |

The Flutter handoff sheet must state:

- SafeArea usage;
- Padding values on the logical start and end;
- ConstrainedBox or max-width behavior;
- SizedBox values between sections;
- TextField or equivalent field heights;
- button minSize and tap target behavior;
- IconButton constraints and logical-end placement;
- OTP cell size and gap;
- keyboard-open behavior;
- scroll behavior when the keyboard or text scaling increases content height.

### Recommended Flutter translation

- Page frame: SafeArea with logical EdgeInsets.
- Mobile content: Padding with 16px horizontal inset.
- Form measure: ConstrainedBox with a max width only when the screen is wider than mobile.
- Full-width action: SizedBox with width infinity and a 48px or 56px height from the system variant.
- Secondary control: keep the visible target at least 44 x 44px even when the icon itself is smaller.
- Text inputs: preserve the system field height, focus ring, error treatment, autocomplete, and text direction.
- Scroll view: use when content can exceed the viewport; never solve overflow by shrinking the type scale.

## 6. RTL and LTR rules

Arabic is the primary locale. The layout is direction-aware, not duplicated by hand.

### Mirrors

- page layout and column order;
- navigation order;
- text alignment;
- list markers and logical bullets;
- progress direction;
- arrows and chevrons that communicate direction;
- the logo lockup asset selection;
- leading and trailing icon placement when the icon expresses direction.

### Does not mirror

- Western Arabic digits 0–9;
- email addresses and passwords;
- mathematical expressions and equations;
- timers and clock digits;
- duration strings that contain numeric values;
- charts with numeric axes;
- media playback controls;
- code, URLs, or other explicitly LTR content.

Use logical properties throughout: margin-inline-start, margin-inline-end, padding-inline-start, padding-inline-end, inset-inline-start, inset-inline-end, block-size, and inline-size. Do not use left and right for layout.

Credentials, equations, timers, and other LTR content must use direction isolation or an explicit LTR wrapper so punctuation and numbers do not reorder inside Arabic paragraphs.

Never mirror the English logo with CSS. Use the supplied Arabic lockup for Arabic and the supplied English lockup for English.

Do not redraw, approximate, or generate the Qiyas logo with CSS primitives. Use the approved lockup or mark geometry from qiyas-design-system.html or the supplied logo assets.

## 7. Locale and theme controls

Every screen includes a quiet, dedicated language control in the prototype header and in the product screen when the product screen itself requires an in-app language switch.

The control follows this rule:

- Arabic view shows EN as the action;
- English view shows عر as the action;
- the control sits at the logical top end;
- the tap target is at least 44 x 44px;
- the label is short and never competes with the primary action;
- switching happens without restart, re-login, or layout jump;
- the selected language is exposed through aria-pressed or an equivalent state;
- long Arabic labels are checked at 360px wide.

Use the established preview hooks:

- qiyas-preview-locale for the document preview state;
- root data-preview-locale for the selected preview locale;
- product canvas data-locale, dir, and lang for the product state;
- qiyas-preview-theme and data-theme for light and dark.

Theme controls must use semantic tokens. Do not add a new palette in a screen file.

## 8. Qiyas visual language

The design system's visual character is precise, quiet, flat, and slightly austere.

- Base spacing comes from the 4px unit and the established scale: 4, 8, 12, 16, 24, 32, 48, 64, 96.
- Use borders and tonal surface changes before shadows.
- Use at most the two elevation levels defined by the system.
- Reserve hard-edged low-blur shadow for overlays and modals.
- Keep radius at or below the system maximum. No pill shapes.
- Use the square frame, 45-degree diagonal, dot pair, and triangle only as defined motifs.
- Maximum one decorative diagonal per screen.
- No decorative gradients.
- No mascots, confetti, sparkles, neon, stock filler, XP bars, streak flames, coin counters, or exclamation-mark tone.
- Primary action buttons use Ink according to the system decision.
- Green is reserved for the mark and correctness feedback.
- Gold is reserved for achievement and must not become warning text.
- Terracotta carries warning, destructive, and critical states when the system assigns it.

## 9. Typography and content fit

Use the system typography:

- IBM Plex Sans for Latin;
- IBM Plex Sans Arabic for Arabic;
- IBM Plex Mono or the system numeric style for changing numbers and timers;
- tabular lining figures for timers, scores, counters, cooldown days, and other values that change in place.

Arabic requires more leading than Latin. Use the per-script line-height values from qiyas-design-system.html rather than applying one shared line-height.

Use Western Arabic digits in both locales. Mathematical notation remains LTR inside Arabic text and must not mirror.

At 200% text scaling:

- controls may become taller;
- content may scroll vertically;
- columns may collapse;
- text may not clip;
- text may not be hidden to preserve the original height;
- Arabic diacritics and ascenders must remain clear;
- avoid orphaned one- or two-word final lines where a container can be adjusted.

## 10. Existing component vocabulary

Use the exact component names and variants already defined in qiyas-design-system.html. Common screen-level mappings include:

- button-primary, button-secondary, button-ghost, button-destructive;
- card and the established surface variants;
- field, field-control, field-label, field-error;
- password-wrap, inline-control, password-checklist;
- otp-group and otp-cells;
- tabs and tab;
- feedback-success, feedback-warning, feedback-error, feedback-info;
- offline, state-error, empty, skeleton, and stale states;
- unauthenticated shell and its scaffold rails;
- badge ladder, badge tile (including the shared capped variant), eligibility chip, progress, and cooldown components;
- dialog and sheet for overlays;
- the existing eye icon and inline-control pattern for password reveal.

For the Learning Library and future learning-browser surfaces, use the confirmed composition rule from Screen 08:

- concept is the stable content identity and browsing spine;
- level and badge requirement are lenses over the same concept and learning-card content, not duplicate cards or separate libraries;
- practice is contextual under Learn, while the eligibility test is visually identified as timed and consequential before its action;
- untouched material remains visually neutral. Show light progress state only for content that is cleared or in progress, so open browsing does not become a backlog.

Do not create a second reveal button when the system's eye icon already supplies the password visibility control. Place it at the logical end of the existing password field.

For functional icons not already specified, use the system's Lucide rule: 1.5px stroke, square linecaps, square linejoins or mitered geometry as specified, and logical direction handling.

## 11. State coverage

Every screen must show or document the relevant states:

- default and empty;
- filled and valid;
- focused;
- hover and pressed where the platform supports them;
- disabled;
- submitting or loading;
- inline field error;
- form-level error;
- offline;
- server unreachable or 5xx;
- timeout;
- stale or resume-after-abandonment;
- successful completion;
- any domain-specific state such as rate limited, expired, unverified, locked, cooldown, or invite pending.

All failure copy follows:

what happened -> why it happened -> what to do next

Errors must preserve typed values where safe, move focus to the first errored field, and use icon plus label or text. Never rely on color alone.

## 12. Multi-page screen previews

When one user flow has multiple pages, use one HTML file with a persistent document-header page selector and actual page panels.

Required behavior:

- page selector shows each page number and selected state;
- selecting a page changes the visible page content;
- Continue advances one page;
- Back returns one page;
- page names and visible step counts remain synchronized;
- mobile and desktop previews show the same active page;
- Arabic and English content remain synchronized;
- theme switching does not reset the active page;
- page state can be persisted in localStorage when the screen is a multi-step flow;
- use no scrollIntoView call;
- every page receives its own data-od-id and state-specific notes.

This pattern is especially useful for sign-up and onboarding screens: the engineer can inspect every page without needing a separate prototype URL for each one.

## 13. Component mapping table

Every screen file includes a table with this exact structure:

| Element on screen | System component | Tokens / variant | Notes for devs |
|---|---|---|---|
| Page frame | Contextual shell | unauthenticated shell / student shell / parent shell / admin shell | Choose the exact established shell for the route. Full-bleed product surface; document controls remain outside it. |
| Locale control | Locale switch | locale-switch, logical top end, 44px target | Arabic action is EN; English action is عر. |
| Theme control | Theme switch | light / dark semantic tokens | Preserve active screen and direction. |
| Primary action | Button | button-primary | Use the established height and pending state. |
| Secondary action | Button | button-secondary or button-ghost | Must not compete with the primary action. |
| Text field | Field | field, field-control | Include direction, autocomplete, focus, error, and keyboard behavior. |
| Password field | Password field | password-wrap, inline-control | Use the existing eye icon at logical end. |
| Verification field | OTP input | otp-group, otp-cells | Equal cells, numeric keyboard, paste behavior documented. |
| Inline failure | Feedback | feedback-error, field-error | Icon and text; preserve safe input. |
| Offline state | Offline banner | offline | Keep retry and typed values. |
| Progress | Progress | progress or established step component | State the current page and total. |
| Page selector | Prototype navigation | document-header page controls | Preview-only; not automatically a production component. |

Add one row for every distinct visible element. Name the exact component and variant from qiyas-design-system.html, not a descriptive substitute.

## 14. Gap list convention

Every screen ends with a short gap list. A gap is a requirement that cannot be implemented faithfully with the current system or that needs product/backend/platform decisions.

Typical gaps include:

- biometric prompt and OS-specific enrollment behavior;
- share sheet or secure credential handoff;
- temporary credential expiration and recovery;
- parent-mediated student password recovery;
- server retry and timeout thresholds;
- email delivery, resend, and anti-abuse behavior;
- consent, minor privacy, data retention, and guardian confirmation;
- platform keyboard, autofill, password-manager, and browser behavior;
- deep-link routing and preserved destinations;
- maintenance and forced-update payloads;
- native iOS LaunchScreen and Android windowBackground asset generation;
- screen-reader announcement wording when the system has not defined it.

Do not hide a gap in a custom component. Name the required component or token, explain why it is needed, and leave the screen's implementation conservative.

## 15. Accessibility and interaction QA

Before handoff, verify:

- normal body text is at least 4.5:1;
- large text and UI boundaries are at least 3:1;
- every focusable element has a visible focus ring with the system offset;
- standard interactive targets are at least 44 x 44px;
- exam answer options are at least 48 x 48px;
- correctness and errors are not conveyed by color alone;
- the screen has no accidental overlap, clipping, or horizontal scroll;
- Arabic and English both fit at 360px;
- email, password, equations, numbers, and timers preserve their intended LTR behavior;
- password reveal uses the existing eye icon;
- keyboard-open layout leaves the active field and primary action usable;
- 200% text scaling does not collapse the layout;
- reduced-motion mode removes sweep, bounce, and unnecessary motion;
- screen readers announce the screen name once and do not announce decorative animation;
- offline and error recovery preserve safe input and offer a next action;
- locale and theme changes do not lose typed values or active page state.

## 16. Pre-delivery verification checklist

### Structure

- semantic HTML file exists at the expected project-relative path;
- no placeholder copy, blank sections, or unfinished panels remain;
- all tags and scripts close correctly;
- every important region has data-od-id;
- the document header controls are present and functional;
- page controls, when present, show real page content.

### Responsive

- inspect 390 x 844;
- inspect 360px wide;
- inspect the intermediate breakpoint;
- inspect 1440px wide;
- inspect a short desktop viewport;
- confirm the desktop preview is wide enough to see the intended composition;
- confirm the mobile surface uses full available width with controlled internal insets;
- confirm no right-side cropping or hidden desktop content.

### Bidi and themes

- inspect Arabic and English;
- inspect light and dark;
- verify the correct Qiyas logo asset is used for each locale;
- verify language and theme toggles remain at the logical top end;
- verify LTR content does not reorder inside RTL copy;
- verify the active page does not reset when toggling theme or locale.

### Handoff

- component mapping table names exact system components and variants;
- Flutter dimensions list SafeArea, insets, targets, fields, buttons, gaps, OTP cells, and scroll behavior;
- state coverage includes the relevant failure and recovery paths;
- gap list names unresolved system or product requirements;
- changed files are reopened and checked after writing.

## 17. Current approved delivery pattern

For future Qiyas screen work, use this sequence:

1. Read qiyas-design-system.html.
2. Read this handoff rules file.
3. Inspect the existing screen files for the established document-header and preview structure.
4. Decide the screen states and page count before writing the layout.
5. Build the mobile surface first at 390 x 844, then check 360px.
6. Build the desktop preview as a clear wide composition at 1440px.
7. Add both directions and both themes.
8. Add the component mapping table, Flutter handoff sheet, and gap list.
9. Verify the full checklist.

This is the stable format for the Qiyas screen library and the default handoff contract for the Flutter team.
