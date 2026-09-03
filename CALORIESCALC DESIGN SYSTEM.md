# CALORIESCALC DESIGN SYSTEM
## Mandatory UI, Layout and Spacing Rules

Version: 1.0

This document defines the canonical visual and layout system for CaloriesCalc.

These rules are mandatory for every page, calculator, hub, food page, recipe page, article and future UI component.

---

# 1. CORE PRINCIPLE

CaloriesCalc must use ONE unified visual system.

Every page must feel like part of the same website.

A new page must NOT create its own:

- container width;
- horizontal page padding;
- spacing scale;
- typography scale;
- card spacing;
- heading spacing;
- button dimensions;
- header height;
- navigation spacing;
- footer spacing.

Existing components and design tokens must be reused.

Do not invent local values when a canonical design token already exists.

---

# 2. REFERENCE DESIGN PHILOSOPHY

The visual target is a clean, compact, content-first calculator website.

The layout should have the same general characteristics as high-quality modern calculator sites such as tdee.co:

- consistent centered content container;
- stable distance from viewport edges;
- consistent vertical rhythm;
- predictable spacing between sections;
- consistent calculator card width;
- consistent heading hierarchy;
- consistent navigation;
- consistent footer;
- responsive behavior without layout jumps.

IMPORTANT:

Do NOT copy proprietary code, CSS, HTML or assets from another website.

Use the reference site only as a visual/layout benchmark.

CaloriesCalc must use its own implementation.

---

# 3. CANONICAL CONTAINER

The website MUST use one canonical content container.

Define it centrally using CSS variables or a shared class.

Example:

.container {
    width: 100%;
    max-width: [CANONICAL_VALUE];
    margin-inline: auto;
    padding-inline: [CANONICAL_VALUE];
}

Do NOT create page-specific container widths unless explicitly approved.

The following must NOT independently redefine container width:

- calculator pages;
- food pages;
- recipe pages;
- hub pages;
- articles;
- homepage.

---

# 4. HORIZONTAL PAGE PADDING

Horizontal spacing between viewport edge and main content must be consistent across the entire site.

Desktop:

Use the canonical container system.

Tablet:

Use the canonical responsive padding.

Mobile:

Use the canonical mobile page padding.

Do not create arbitrary values such as:

padding-left: 17px;

margin-left: 23px;

padding-inline: 31px;

unless there is a documented component-specific reason.

All global horizontal spacing must use design tokens.

---

# 5. VERTICAL SPACING SYSTEM

Use a predefined spacing scale.

Recommended base scale:

--space-1: 4px;
--space-2: 8px;
--space-3: 12px;
--space-4: 16px;
--space-5: 20px;
--space-6: 24px;
--space-7: 32px;
--space-8: 40px;
--space-9: 48px;
--space-10: 64px;
--space-11: 80px;

Use these tokens consistently.

Do not randomly introduce:

13px
19px
27px
37px
43px
53px

unless required by an actual component geometry.

---

# 6. SECTION SPACING

All major content sections must follow a consistent vertical rhythm.

Recommended:

small internal separation:
16–24px

normal component separation:
24–32px

major section separation:
40–64px

large page section separation:
64–80px

Do not allow every page to establish its own spacing rhythm.

---

# 7. CALCULATOR LAYOUT

Every calculator must use the same structural pattern whenever applicable:

Page header
↓
Calculator card
↓
Result card
↓
Supporting explanation
↓
Related content
↓
FAQ
↓
Footer

The calculator itself must have predictable internal spacing.

Input groups must use the same:

- label spacing;
- input height;
- border radius;
- field gap;
- section gap;
- button spacing.

---

# 8. CALCULATOR CARD

Calculator cards must use the canonical card component.

Do not create a new card style for each calculator.

All calculators should share:

- max width;
- border;
- radius;
- padding;
- shadow;
- background;
- heading spacing.

If a calculator requires a special layout, extend the canonical component rather than replacing it.

---

# 9. INPUTS

All calculator inputs must use the canonical input component.

Required consistency:

- same height;
- same border radius;
- same border thickness;
- same font size;
- same label position;
- same label-to-input spacing;
- same focus behavior;
- same error behavior.

Do not create:

calculator-specific input styling

unless explicitly required.

---

# 10. BUTTONS

All primary Calculate buttons must use the canonical primary button.

Every Calculate button must have:

- identical visual treatment;
- consistent height;
- consistent radius;
- consistent typography;
- consistent hover state;
- consistent disabled state;
- consistent focus state.

Do not create a unique Calculate button for each calculator.

---

# 11. TYPOGRAPHY

Use one canonical typography hierarchy.

Page title:
H1

Major section:
H2

Subsection:
H3

Body:
body

Supporting text:
muted text

Do not change heading sizes on individual pages without documented reason.

The same semantic heading must have the same visual hierarchy throughout the website.

---

# 12. HEADER

There must be ONE canonical header implementation.

Do NOT create:

NAV v2
NAV v3
NAV v4
NAV v5

on individual pages.

If the header changes, update the canonical header implementation.

All pages must consume the same header.

Do not append additional navigation CSS to individual pages to override previous navigation systems.

---

# 13. FOOTER

There must be ONE canonical footer implementation.

The footer must maintain:

- same width;
- same container;
- same columns;
- same typography;
- same spacing;
- same responsive behavior.

Do not create page-specific footer layouts.

---

# 14. RESPONSIVE DESIGN

The site must be designed from one responsive system.

Required breakpoints must be defined centrally.

Do not invent page-specific breakpoints.

Every page must be tested at minimum at:

- 320px;
- 375px;
- 768px;
- 1024px;
- 1280px;
- 1440px.

The page must not:

- overflow horizontally;
- produce unexpected horizontal scrolling;
- shift the main container;
- create inconsistent side margins;
- break navigation;
- produce overlapping cards;
- produce clipped text.

---

# 15. MOBILE RULE

Mobile is NOT a separate design.

It is the same design system adapted to a smaller viewport.

The same:

- spacing logic;
- typography hierarchy;
- card hierarchy;
- component hierarchy

must remain recognizable.

Do not redesign individual pages independently for mobile.

---

# 16. PAGE WIDTH CONSISTENCY

The following pages must use the same canonical content grid:

- homepage;
- calculator hub;
- calculator pages;
- food hub;
- food pages;
- recipe hub;
- recipe pages;
- articles;
- informational pages.

A page must not appear noticeably wider or narrower than another page unless its content type explicitly requires it.

---

# 17. FULL-BLEED SECTIONS

If a section intentionally uses full viewport width, keep the section full-width but place its content inside the canonical container.

Correct:

.full-width-section
    .container
        content

Incorrect:

.full-width-section
    content with custom width

---

# 18. NO INLINE LAYOUT PATCHES

Avoid repeated inline styles such as:

style="margin-left: 18px"

style="padding-top: 27px"

style="max-width: 923px"

when the same result can be achieved using the design system.

Inline styles must not be used to patch layout inconsistencies.

---

# 19. NO CASCADING PATCH STACK

Never solve a layout problem by repeatedly adding overrides.

Bad:

.nav { ... }

.nav { ... }

.nav { ... }

.nav { ... }

.nav-v2 { ... }

.nav-v3 { ... }

.nav-v4 { ... }

Instead:

1. identify the canonical component;
2. remove obsolete implementation;
3. fix the canonical implementation;
4. verify every page.

---

# 20. DUPLICATE CSS AUDIT

When modifying a page, check whether the page contains duplicate or obsolete CSS.

Pay special attention to:

- navigation;
- header;
- footer;
- container;
- buttons;
- cards;
- mobile navigation;
- responsive media queries.

Do not add another CSS block on top of an existing conflicting block.

---

# 21. DESIGN TOKENS

Global values should be centralized.

At minimum define tokens for:

- container width;
- mobile horizontal padding;
- desktop horizontal padding;
- spacing;
- typography;
- border radius;
- input height;
- button height;
- card padding;
- section spacing.

Example:

:root {
    --container-max: ...;
    --page-padding-mobile: ...;
    --page-padding-desktop: ...;

    --space-1: 4px;
    --space-2: 8px;
    --space-3: 12px;
    --space-4: 16px;
    --space-5: 20px;
    --space-6: 24px;
    --space-7: 32px;
    --space-8: 40px;
    --space-9: 48px;
    --space-10: 64px;

    --radius-sm: ...;
    --radius-md: ...;
    --radius-lg: ...;
}

Exact values must be determined from the current canonical CaloriesCalc design and validated against the reference layout.

---

# 22. REFERENCE MEASUREMENT RULE

When the task is to match the layout of the reference website:

Do NOT estimate visually if the dimensions can be measured.

Compare:

1. viewport width;
2. content container width;
3. left/right container margins;
4. header height;
5. H1 position;
6. calculator card width;
7. calculator card top position;
8. distance between calculator and result;
9. distance between major sections;
10. footer position.

Record measurements before changing CSS.

---

# 23. REFERENCE SITE RULE

Reference:

https://tdee.co/

Use it only as a visual/layout benchmark.

The objective is NOT:

"make CaloriesCalc identical by copying source code."

The objective is:

"make CaloriesCalc follow the same level of layout consistency and comparable container/spacing geometry."

Do not copy:

- source code;
- CSS;
- images;
- text;
- proprietary assets;
- hidden implementation details.

---

# 24. BEFORE MODIFYING ANY PAGE

Before making UI changes:

1. inspect the existing global CSS;
2. inspect existing header;
3. inspect existing footer;
4. inspect container classes;
5. inspect media queries;
6. inspect duplicate styles;
7. determine the canonical implementation;
8. check whether the issue is global or page-specific.

If the problem is global, fix the global component.

Do NOT patch every page individually.

---

# 25. BEFORE ADDING CSS

Ask:

"Does this value already exist as a design token?"

If yes:
use the token.

If no:
determine whether a new global token is actually necessary.

Do not create arbitrary one-off values.

---

# 26. PAGE IMPLEMENTATION RULE

A new page must consume existing:

- header;
- navigation;
- footer;
- container;
- typography;
- cards;
- buttons;
- forms;
- spacing tokens.

A new page must NOT establish a parallel design system.

---

# 27. CHANGE SAFETY

Before modifying a global component:

identify all pages that use it.

After modifying it:

verify at least:

- homepage;
- calculator hub;
- one calculator;
- one food page;
- one other content page.

Global changes must not be approved based on one page only.

---

# 28. VISUAL REGRESSION

After significant UI changes compare:

Before
vs
After

at:

320px
375px
768px
1024px
1280px
1440px

Check:

- container;
- margins;
- vertical rhythm;
- header;
- navigation;
- cards;
- forms;
- buttons;
- footer.

---

# 29. DEFINITION OF DONE

A UI task is NOT complete if:

- the target page looks correct but other pages break;
- the new component has different spacing without justification;
- the page has duplicate navigation CSS;
- the page introduces another container width;
- mobile layout is broken;
- desktop layout is broken;
- the same component looks different elsewhere;
- arbitrary CSS overrides were added to hide an underlying problem.

A UI task is complete only when the change follows the canonical design system.

---

# 30. HIGHEST PRIORITY RULE

CONSISTENCY > LOCAL PERFECTION.

If a page looks slightly imperfect but follows the global design system, do not create a local exception.

Fix the design system itself if the global value is wrong.

---

# 31. MANDATORY PRE-COMMIT CHECK

Before considering a UI task complete, verify:

[ ] canonical container used

[ ] no page-specific container width

[ ] canonical horizontal padding used

[ ] spacing tokens used

[ ] canonical typography used

[ ] canonical header used

[ ] canonical footer used

[ ] canonical buttons used

[ ] canonical input styles used

[ ] no duplicate navigation system

[ ] no obsolete CSS overrides added

[ ] mobile checked

[ ] desktop checked

[ ] no horizontal overflow

[ ] no unrelated visual changes

[ ] other pages using the modified component checked

---

# FINAL RULE

When working on ANY CaloriesCalc page:

DO NOT ask:

"How can I make this individual page look good?"

Ask:

"How does this page fit into the existing CaloriesCalc design system?"

Every page must belong to ONE system.

ONE CONTAINER.
ONE SPACING SYSTEM.
ONE TYPOGRAPHY SYSTEM.
ONE HEADER.
ONE NAVIGATION.
ONE FOOTER.
ONE COMPONENT LANGUAGE.

No exceptions without explicit approval.