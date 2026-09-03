# CaloriesCalc.org — Development Rules

This file is the permanent development contract for CaloriesCalc.org.

All future code changes, new pages, refactors, UI changes, SEO changes, calculator changes, and integrations must follow these rules.

## Priorities

1. Correctness
2. Data integrity
3. Visual consistency
4. Maintainability
5. SEO stability
6. Deterministic calculations
7. Reusable architecture
8. Development speed

Never sacrifice correctness or architecture merely to generate pages faster.

## 1. Single Source of Truth

Do not duplicate information that already has a canonical source.

This applies to:
- UI components
- templates
- CSS/design tokens
- calculator formulas
- calculator constants
- food nutrition data
- serving sizes
- structured data

If the same information is needed in multiple places, reference the canonical source instead of creating independent copies.

## 2. Fix Problems at the Correct Architectural Level

If a problem exists across multiple pages, fix the shared component, template, data source, or utility.

Do not patch individual pages when the underlying problem is systemic.

Bad: separate header fixes on individual pages.

Good: fix the global Header component once and verify all affected page types.

## 3. Do Not Invent Authoritative Data

Never invent nutrition values, serving weights, source IDs, scientific claims, medical claims, calculator constants, or formulas.

If authoritative information is unavailable, report that it is unavailable.

Nutrition-specific rules are defined in `DATA_RULES.md`.

## 4. Never Silently Resolve Conflicts

If two existing values conflict, do not arbitrarily choose one. Identify the affected pages, values, food variants, preparation states, nutritional basis, sources, and likely reason for the discrepancy. Resolve it using `DATA_RULES.md`.

## 5. Preserve Existing Functionality

Before changing a shared component or architecture, determine what pages and components depend on it. Never fix one page by breaking another page type.

# Frontend Architecture

## 6. Global UI Components

Global UI elements must have one reusable implementation.

This includes Header, Main Navigation, Mobile Navigation, Footer, Page Container, Breadcrumbs, Buttons, Cards, Tables, Form Controls, common alerts/notes, and common calculator UI.

Do not manually create separate copies for individual pages.

## 7. Design System

Visual rules must be centralized for colors, spacing, typography, line heights, border radius, shadows, container widths, breakpoints, buttons, forms, cards, and tables.

Do not introduce arbitrary page-specific values when a shared token or component should be used.

Avoid CSS hacks whose only purpose is to make one page look correct.

Before adding page-specific CSS ask: Is this truly unique, or is the shared component/template wrong?

## 8. Header and Navigation

Header and navigation are global components. All relevant page types must use the same implementation.

Test desktop, tablet, mobile, dropdowns, mobile menu, closing behavior, outside click, keyboard navigation, focus states, tap targets, long labels, narrow screens, and overflow.

A header/navigation change must be checked across all page types.

## 9. Page Templates

Do not create pages from scratch when a matching template exists.

Maintain reusable templates for at least:

### Food pages
- Global layout
- Header
- Navigation
- Breadcrumbs
- Food hero/summary
- Nutrition section
- Serving information
- Description
- Nutrition notes
- FAQ
- Related content
- Footer

### Calculator pages
- Global layout
- Header
- Navigation
- Breadcrumbs
- Calculator inputs
- Results
- Explanation
- Formula/methodology
- Example
- Limitations/notes
- FAQ
- Related calculators
- Footer

### Recipe pages
- Global layout
- Header
- Navigation
- Breadcrumbs
- Recipe information
- Ingredients
- Instructions
- Nutrition
- Notes
- FAQ
- Related recipes
- Footer

### Comparison pages
- Global layout
- Header
- Navigation
- Breadcrumbs
- Comparison summary
- Comparison table
- Explanation
- Notes
- FAQ
- Related comparisons
- Footer

If the technology changes, preserve the architectural principle.

## 10. New Page Process

Before creating a new page:
1. Identify the page type.
2. Identify the existing template.
3. Identify reusable components.
4. Identify canonical data sources.
5. Identify SEO requirements.
6. Identify internal linking requirements.
7. Identify structured data requirements.
8. Check whether the required data/content already exists.

Do not immediately write a standalone implementation.

# Calculators

## 11. Deterministic Calculator Architecture

Calculator formulas must be deterministic. The same valid inputs must produce the same result.

A formula used by multiple pages must have one implementation. Do not duplicate formulas in individual pages.

Future AI must call deterministic calculation functions instead of independently calculating authoritative results.

Preferred:

    User → UI → Deterministic calculation function → Result → UI / explanation

Future AI:

    User → AI → Calculation tool → Deterministic calculation → Result → AI explanation

# Recipes and Comparisons

## 12. Recipes

Recipe nutrition should use canonical Food Variants.

    Recipe → Ingredients → Canonical Food Variants → Nutrition calculation → Recipe nutrition

Ingredient preparation state must be explicit, e.g. rice dry/cooked and chicken raw/cooked.

## 13. Comparisons

Comparison pages must use canonical Food Variants and normally compare equivalent preparation states, nutritional bases, and units.

Do not compare cooked food A with dry food B unless intentional and explicitly explained.

# SEO

## 14. SEO Stability

The architecture must preserve crawlability and indexability.

Each indexable page should have appropriate unique title, meta description, canonical, H1, logical heading hierarchy, breadcrumbs, internal links, and structured data where appropriate.

Do not make important SEO content dependent on client-side rendering if that harms crawlability.

## 15. Structured Data

Structured data must match visible content.

Never allow Visible value != Schema value.

Schema should use the same canonical data source as the visible UI whenever practical.

# Responsive Design

## 16. Responsive Rules

Every shared template must work on mobile and desktop. Avoid page-specific responsive hacks. If a responsive issue affects a shared component, fix the shared component.

# Code Quality

## 17. Prefer

- reusable components
- small focused functions
- centralized constants
- clear data models
- predictable naming
- minimal duplication
- explicit variants
- deterministic calculations

## Avoid

- unnecessary duplication
- magic numbers
- hidden dependencies
- page-specific copies of shared logic
- repeated HTML
- CSS hacks
- silently overridden values

# Change Management

## 18. Before a Significant Change

1. Inspect the existing repository.
2. Identify affected components.
3. Identify affected page types.
4. Identify affected data.
5. Identify SEO implications.
6. Make the smallest architectural change that solves the problem.
7. Test affected page types.

Do not rewrite working architecture without a clear reason.

## 19. Before Mass Content Creation

Before generating many pages, verify templates, global UI, canonical data, calculations, responsive layout, and structured data are stable.

Do not multiply technical debt by generating hundreds of pages from unstable templates.

# Future Backend and AI

## 20. Future Architecture

The site may remain largely static while preparing for future backend/API functionality.

Future backend/API/AI must connect to canonical data and deterministic functions.

Preferred:

    Frontend → API → Canonical Data + Deterministic Calculation Engine + AI

AI is not a second source of truth. AI may explain, organize, personalize, and orchestrate tools. AI must not override authoritative nutrition records or deterministic calculator results.

# Uncertainty Protocol

## 21. When Unsure

If unsure about the correct template, component, canonical food variant, source, conflicting numerical data, preparation state, or calculator methodology, do not guess. Report the uncertainty and explain what must be resolved.

# Required Final Check

## 22. After Significant Changes

### UI
Check Header, Navigation, Footer, Container, Typography, Spacing, Buttons, Cards, Tables, Forms, Mobile, Desktop.

### Data
Check nutrition values, units, serving sizes, preparation state, source, source ID, canonical record.

### SEO
Check title, description, canonical, H1, headings, breadcrumbs, internal links, structured data.

### Functionality
Check inputs, calculations, validation, edge cases, links, navigation.

# Core Rules

1. One global component should have one implementation.
2. One calculator formula should have one implementation.
3. One Food Variant should have one canonical nutrition record.
4. Recipes should use canonical Food Variants.
5. Comparisons should use canonical Food Variants.
6. Schema and visible data must agree.
7. Fix systemic problems at the system level.
8. Never invent authoritative numbers.
9. Never silently resolve data conflicts.
10. Stability and correctness are more important than generating pages quickly.

The detailed nutrition-data rules are in `DATA_RULES.md`.

# MANDATORY PROJECT RULES

Before performing ANY task in this repository, Claude MUST read:

1. `CLAUDE.md`
2. `DATA_RULES.md`
3. `DESIGN_SYSTEM.md`

These documents are mandatory project rules.

They are not suggestions.

---

## DESIGN SYSTEM ENFORCEMENT

`DESIGN_SYSTEM.md` is the canonical authority for all visual and layout decisions.

Any task involving:

* HTML;
* CSS;
* layout;
* responsive behavior;
* header;
* navigation;
* footer;
* calculator UI;
* forms;
* cards;
* spacing;
* typography;
* containers;
* page structure

MUST comply with `DESIGN_SYSTEM.md`.

Before modifying UI code, inspect the existing implementation and determine whether the requested change should be made globally or locally.

Do NOT create a local CSS workaround when the problem belongs to a global component.

Do NOT introduce a second container system.

Do NOT introduce a second navigation system.

Do NOT introduce page-specific spacing values when a canonical token exists.

Do NOT add CSS overrides merely to compensate for previous CSS overrides.

---

## REFERENCE WEBSITE

The visual reference for container geometry, spacing rhythm and overall layout consistency is:

https://tdee.co/

The reference is a design benchmark only.

Do NOT copy its source code, CSS, assets or text.

Use measurable layout characteristics as inspiration for the CaloriesCalc design system.

---

## CHANGE DISCIPLINE

Before making UI changes:

1. identify affected global components;
2. inspect all existing implementations;
3. identify duplicate/obsolete CSS;
4. determine the canonical implementation;
5. make the smallest correct change;
6. test representative pages;
7. verify desktop and mobile layouts.

If a change conflicts with `DESIGN_SYSTEM.md`, stop and resolve the conflict instead of silently violating the rule.

---

## IMPORTANT

Never assume that making one page visually correct is sufficient.

A change is successful only if it preserves the unified visual system across the website.

