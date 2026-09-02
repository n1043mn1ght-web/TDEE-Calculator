# CaloriesCalc.org — Nutrition Data Rules

This document is the authoritative policy for nutrition data used by CaloriesCalc.org.

Its purpose is to prevent contradictory nutrition values across Food pages, Food indexes, Recipes, Comparisons, Calculator examples, Structured Data, the future API, and future AI.

Core principle:

> Every nutrition value must have a clearly defined Food Variant, preparation state, nutritional basis, and source.

# 1. Single Source of Truth

Every Food Variant must have one canonical Nutrition Record.

    Food → Food Variant → Nutrition Record → External Source

All pages using the exact same variant must obtain nutrition values from that canonical record. Do not maintain independent copies of nutrition numbers inside individual pages.

# 2. Food vs Food Variant

A food name alone is insufficient to identify nutrition data.

Potentially separate variants include raw, uncooked, dry, cooked, boiled, baked, grilled, roasted, fried, canned, canned/drained, prepared, non-fat, low-fat, reduced-fat, full-fat, sweetened, and unsweetened.

If preparation, processing, or composition materially changes nutrition, treat it as a separate Food Variant.

# 3. Required Identification

Where applicable, every canonical Nutrition Record should define:
- food name
- food variant
- preparation state
- nutritional basis
- calories
- protein
- carbohydrates
- fat
- fiber
- sugar
- sodium
- serving information
- source
- source identifier
- retrieval/update date

Do not hide important preparation information only in prose.

# 4. Source Policy

For nutrition information, prefer authoritative primary/public databases.

Priority:
1. USDA FoodData Central
2. official government nutrition databases
3. recognized international/public nutrition databases
4. authoritative scientific sources
5. other sources only when an appropriate authoritative source is unavailable

Do not use random SEO websites as authoritative nutrition sources. Do not select a number because it looks better. Do not modify authoritative values for SEO or marketing.

# 5. Source Identification

Where available, store the external source record identifier.

    source: USDA FoodData Central
    source_id: <external record ID>
    basis: 100 g
    state: cooked
    retrieved: YYYY-MM-DD

Never invent source IDs.

If several source records are possible, choose the one that most accurately matches the exact Food Variant and document the decision.

# 6. Nutritional Basis

The default comparison basis should normally be **per 100 g**.

Other bases must be explicit, such as per serving, per cup, per tablespoon, per ounce, or per item.

Never compare incompatible bases without conversion or explanation.

# 7. Serving Size Rules

Serving values should be derived from the canonical base record whenever mathematically appropriate.

Example:

    Canonical: 100 g = 111 kcal
    Serving: 195 g
    Calculation: 111 × 195 / 100 = 216.45 kcal
    Display: 216 kcal

Do not replace the underlying canonical value with the rounded display value. Do not independently type a serving value that conflicts with the canonical calculation.

# 8. Unit Consistency

Use explicit units such as kcal/100 g, g protein/100 g, g carbohydrate/100 g, and g fat/100 g.

Never silently mix dry grams with cooked grams.

# 9. Dry vs Cooked

Dry and cooked foods must be treated as different variants when their per-weight nutrition differs because of water gain/loss.

Example:

    Brown Rice — Dry
    Brown Rice — Cooked

A recipe using 75 g dry rice must use the dry-rice record. A recipe using 195 g cooked rice must use the cooked-rice record. Never silently substitute one state for another.

# 10. Important Variant Examples

## Rice

Distinguish white rice dry/cooked and brown rice dry/cooked.

## Tuna

Distinguish, where applicable, fresh tuna cooked, canned tuna in water, canned tuna drained, and other materially different variants.

## Greek yogurt

Distinguish, where applicable, non-fat, low-fat, full-fat, sweetened, and unsweetened.

## Oats / oatmeal

Distinguish dry rolled oats, cooked oatmeal, and other materially different preparations.

Never merge these into one generic Nutrition Record.

# 11. Conflict Rule

If the same exact Food Variant has different values in different places, this is a data integrity error.

First determine whether the records are actually different variants. If they are different variants, both may be valid. If they represent the same variant, one canonical record must be selected and all dependent pages must be synchronized.

Never resolve the conflict by guessing.

# 12. Conflict Resolution Procedure

When a conflict is found:
1. Identify the exact food.
2. Identify the exact variant.
3. Identify preparation state.
4. Identify nutritional basis.
5. Identify source.
6. Identify source ID.
7. Determine whether records are actually different variants.
8. If they are the same variant, select the authoritative canonical record.
9. Update all dependent pages.
10. Update structured data.
11. Check recipes.
12. Check comparisons.
13. Check food indexes.
14. Run a consistency audit.

Do not silently overwrite one number with another.

# 13. Recipes

Recipes must reference canonical Food Variants.

    Recipe → Ingredient → Food Variant → Nutrition Record

Ingredient quantities must clearly identify whether they are raw, dry, cooked, prepared, or drained.

Example: 75 g dry white rice must use White Rice — Dry unless the recipe explicitly measures rice after cooking.

# 14. Recipe Nutrition

Recipe nutrition should be calculated from ingredient records whenever practical. If an ingredient amount changes, recipe nutrition should be recalculated.

Do not maintain an unrelated hardcoded nutrition total that can contradict the ingredients.

# 15. Comparison Pages

Comparisons must use canonical Food Variants and normally compare equivalent preparation states, nutritional bases, and units.

A cooked-vs-dry comparison requires an explicit reason and clear labeling.

# 16. Food Index Pages

Food index cards must reference the same canonical record as the corresponding Food detail page.

Do not maintain different nutrition values for the same exact Food Variant. If values differ intentionally because the variants differ, label the variants explicitly.

# 17. Structured Data

Structured data must use the same canonical data as visible content.

Never allow a visible value and schema value to disagree for the same represented quantity/variant.

Generate schema from canonical records whenever practical.

# 18. Rounding

Keep source/underlying values at appropriate precision. Round for presentation only. Do not repeatedly round intermediate values.

Preferred:

    source value → conversion/calculation → final value → display rounding

# 19. Missing Data

If an authoritative source does not provide a value:
- do not invent it;
- do not present an unrelated food's value as the answer;
- do not silently estimate it as authoritative.

If an estimate is ever necessary for a clearly non-authoritative purpose, label it explicitly as an estimate. Do not allow estimates to overwrite canonical authoritative data.

# 20. Scientific and Health Claims

Nutrition numbers and medical/scientific claims are separate categories.

Do not infer medical conclusions directly from a nutrition value.

Avoid unsupported claims such as "the #1 food for cardiovascular health", "clinically lowers LDL by X%", "best food for weight loss", or "safe for diabetics".

Claims must be properly supported, precisely scoped, and not exaggerated.

# 21. Calculator Constants and Formulas

Calculator formulas and constants are also data. Examples include activity multipliers, MET values, energy conversion constants, formula coefficients, and methodology assumptions.

A formula used in multiple places must have one canonical implementation. Do not copy and modify constants independently across pages.

# 22. AI Rules

Future AI must never become a second nutrition database.

Correct:

    User → AI → Canonical Food Lookup → Authoritative Record → AI Explanation

Incorrect:

    User → AI → AI invents nutrition value

For calculations:

    User → AI → Deterministic calculation tool → Result → AI explanation

# 23. Pre-Publication Data Check

Before publishing or updating a food-related page, verify:
- the food does not already exist under another name unexpectedly;
- the exact variant is identified;
- preparation state is correct;
- nutritional basis is correct;
- source is authoritative;
- source ID is recorded where available;
- calories match the canonical record;
- macronutrients match;
- serving size derives correctly;
- visible content matches schema;
- dependent recipes remain consistent;
- dependent comparisons remain consistent;
- the food index remains consistent.

# 24. Data Audit

Periodically search for:

    same food + same variant + same preparation state + same nutritional basis = different values

This is a critical integrity error.

Also audit for dry/cooked mismatches, raw/cooked mismatches, serving/basis mismatches, stale source values, inconsistent recipe ingredients, inconsistent comparison values, and inconsistent schema values.

# 25. High-Risk Food Categories

Pay special attention to:
- rice
- pasta
- oats/oatmeal
- meat
- fish
- canned foods
- drained foods
- yogurt
- dairy
- oils
- foods whose weight changes significantly during cooking

# 26. No SEO Manipulation

Nutrition data must never be changed to improve rankings, make a food look healthier, make a comparison more attractive, increase clicks, match a competitor, or produce a preferred marketing claim.

Accuracy takes priority over marketing.

# 27. Canonical Data Changes

If a canonical record changes:
1. Record the reason.
2. Record the source.
3. Identify affected pages.
4. Update dependent pages.
5. Update structured data.
6. Check recipes.
7. Check comparisons.
8. Check food index cards.
9. Run a consistency audit.

Do not update only the page where the discrepancy was discovered.

# 28. Uncertainty Protocol

If the correct record cannot be established confidently, stop and report:

    DATA CONFLICT / DATA UNCERTAINTY

Include food, variant, preparation state, basis, existing values, sources, source IDs, and recommended resolution.

Do not guess.

# Final Principle

The nutrition system must be treated as structured data, not as a collection of independent webpages.

    Authoritative External Source
              ↓
       Canonical Data Record
              ↓
          Food Variant
              ↓
       ┌──────┼──────────┐
       ↓      ↓          ↓
      Food   Recipe    Compare
      Page    Page       Page
       │       │          │
       └───────┼──────────┘
               ↓
        Structured Data
               ↓
        Future API / AI

There must be no unofficial parallel nutrition values hidden inside individual pages.

## Core Rule

> If a number describes a food, the system must be able to answer exactly which Food Variant, preparation state, nutritional basis, and authoritative source produced that number.

If that cannot be answered, the number is not ready to be treated as canonical data.
