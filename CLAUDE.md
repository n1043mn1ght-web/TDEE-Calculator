# CaloriesCalc.org — Master Development & Data Integrity Specification

## 1. Главная задача

Ты работаешь над существующим сайтом CaloriesCalc.org.

Твоя задача — не просто создавать или исправлять отдельные страницы.

Ты должен привести проект к архитектуре, в которой:

1. Все страницы используют единую визуальную систему.
2. Header, navigation, footer, container, typography, spacing, buttons, cards и другие общие элементы имеют единый источник.
3. Разные типы страниц используют заранее определённые шаблоны.
4. Нельзя создавать уникальную визуальную реализацию общего элемента на отдельной странице без явного разрешения.
5. Числовые данные о продуктах питания должны иметь единый канонический источник.
6. Один и тот же food/food variant не может иметь разные значения калорийности или нутриентов на разных страницах.
7. Recipes и Compare не должны содержать независимо придуманные или вручную продублированные значения, если эти значения уже существуют в Food Database.
8. Любые изменения должны сохранять совместимость со всей существующей архитектурой сайта.
9. Нельзя исправлять одну страницу способом, который ломает другие страницы.
10. Перед созданием новых страниц необходимо использовать существующие шаблоны и компоненты.

---

# 2. ПРИНЦИП №1 — SINGLE SOURCE OF TRUTH

Это главное правило проекта.

Для каждого числового показателя должен существовать один канонический источник.

Например:

```text
Food
 └── Food Variant
      └── Nutrition Record
```

Для каждого Food Variant хранятся:

* calories
* protein
* carbohydrates
* fat
* fiber
* sugar
* sodium
* serving weight
* serving description
* preparation state
* source
* source identifier
* source date/version, если доступна

Пример:

```text
brown-rice
    variant: cooked
    basis: 100 g
    calories: X
    protein: X
    carbs: X
    fat: X
    source: USDA
```

Другие страницы не имеют права самостоятельно придумывать другое значение для этого же варианта.

---

# 3. НЕЛЬЗЯ СМЕШИВАТЬ ВАРИАНТЫ ПРОДУКТА

Одинаковое название продукта не означает одинаковый Nutrition Record.

Обязательно различать:

* raw / uncooked
* cooked
* boiled
* baked
* grilled
* fried
* canned
* drained
* dry
* prepared
* low-fat
* full-fat
* non-fat

и другие существенные варианты.

Например:

```text
Brown Rice — dry
Brown Rice — cooked
```

это два разных Food Variant.

Нельзя использовать данные cooked rice на странице dry rice и наоборот.

---

# 4. ЕДИНАЯ ЕДИНИЦА СРАВНЕНИЯ

Основная единица для сравнения продуктов:

**per 100 g**

Если используются serving/cup/tbsp/etc., они должны быть производными от базовой записи.

Например:

```text
Base:
100 g = 111 kcal

Serving:
195 g = 216.45 kcal
```

Не разрешается вручную вводить:

```text
100 g = 111 kcal
1 cup = 216 kcal
```

если 216 не вычисляется из канонического значения и массы serving.

Округление допускается только на этапе отображения.

Исходные значения не должны заменяться округлёнными значениями.

---

# 5. ИСТОЧНИКИ ДАННЫХ

Для nutrition data использовать авторитетные внешние базы.

Приоритет:

1. USDA FoodData Central
2. Другие официальные государственные/международные базы
3. Авторитетные научные источники
4. Иные источники — только если нет подходящего первичного источника

Нельзя:

* придумывать значения;
* брать значения из случайных SEO-сайтов;
* смешивать несколько источников без явного объяснения;
* выбирать значение только потому, что оно лучше выглядит;
* изменять значение ради SEO;
* подгонять цифры под уже существующий текст.

Если первичный источник содержит конкретный nutrition record, именно он является основой.

---

# 6. SOURCE ID

Для каждого nutrition record желательно хранить идентификатор внешнего источника.

Например:

```text
source:
USDA FoodData Central

source_id:
<external record id>

basis:
100 g

state:
cooked

retrieved:
YYYY-MM-DD
```

Если внешний источник изменил данные, необходимо создать новую версию record или явно обновить существующий record.

Нельзя незаметно менять цифры на отдельных страницах.

---

# 7. DATA NORMALIZATION

Перед созданием страницы необходимо определить:

```text
Food name
Food variant
Preparation state
Serving basis
Source
Source ID
Nutrition values
```

Только после этого можно создавать страницу.

Пример:

```text
Food:
Tuna

Variant:
Fresh tuna, cooked

Basis:
100 g

Calories:
...

Protein:
...

Source:
USDA
```

Отдельно:

```text
Tuna
Variant:
Canned in water, drained

Basis:
100 g

Calories:
...

Source:
USDA
```

Эти значения нельзя объединять в одно абстрактное "Tuna".

---

# 8. FOOD PAGES

Все страницы `/foods/*` должны использовать единый шаблон.

Общая структура:

```text
Header
Navigation
Breadcrumbs

Food Hero
Food Summary

Nutrition per 100 g

Nutrition Table

Serving Sizes

Preparation / Variant information

Description

Nutrition Notes

FAQ

Related Foods

Related Recipes

Related Comparisons

Footer
```

Порядок блоков нельзя менять без необходимости.

Если требуется изменить структуру всех Food Pages, сначала изменить общий template/component, а не редактировать десятки страниц вручную.

---

# 9. CALCULATOR PAGES

Все calculator pages должны использовать общий шаблон.

Структура:

```text
Header
Navigation
Breadcrumbs

Calculator title
Calculator input section
Calculator result section

Explanation

Formula / Methodology

Example

Important notes / limitations

FAQ

Related Calculators

Footer
```

Общий UI должен быть реализован через reusable components.

---

# 10. RECIPE PAGES

Recipe nutrition нельзя считать независимым от Food Database.

Правильная архитектура:

```text
Recipe
 ↓
Ingredients
 ↓
Canonical Food Variants
 ↓
Nutrition calculation
 ↓
Recipe Nutrition
```

Например:

```text
Chicken
200 g
 ↓
canonical chicken record

Rice
75 g dry
 ↓
canonical dry rice record
```

Recipe nutrition должна рассчитываться из этих записей.

Нельзя одновременно иметь:

```text
Food Database:
Rice = 365 kcal / 100 g dry

Recipe:
Rice = 350 kcal / 100 g dry
```

без документированного и обоснованного источника.

---

# 11. COMPARE PAGES

Comparison pages также должны получать значения из Food Database.

Например:

```text
Brown Rice vs White Rice
```

должна использовать:

```text
Brown Rice — cooked
White Rice — cooked
```

или:

```text
Brown Rice — dry
White Rice — dry
```

но никогда:

```text
Brown Rice — cooked
White Rice — dry
```

если сравнение не предназначено именно для этого.

На странице сравнения обязательно явно указывать basis/state.

---

# 12. КРИТИЧЕСКОЕ ПРАВИЛО ПРОТИВ КОНФЛИКТОВ

Перед публикацией или изменением любой страницы необходимо выполнить проверку:

```text
Does this food already exist?

Does this food variant already exist?

Does the nutrition record already exist?

Is the source identical?

Is the preparation state identical?

Is the serving basis identical?

Are the values identical?
```

Если ответ на последний вопрос "нет" — нельзя автоматически продолжать.

Необходимо найти причину расхождения.

---

# 13. ПРИМЕРЫ КОНФЛИКТОВ, КОТОРЫЕ НЕЛЬЗЯ ДОПУСКАТЬ

## Brown Rice

Недопустимо:

```text
/foods/brown-rice
111 kcal / 100 g cooked

/compare/brown-rice-vs-white-rice
216 kcal / 100 g
```

если comparison использует тот же cooked variant.

Нужно определить:

```text
111 kcal = cooked

216 kcal = approximately 195 g serving
```

или определить, что comparison использует dry variant.

Состояние продукта должно быть явно указано.

---

## Tuna

Недопустимо:

```text
Tuna:
144 kcal / 100 g

Comparison:
Tuna:
109 kcal / 100 g
```

если оба блока относятся к одному и тому же Food Variant.

Если это:

```text
Fresh cooked tuna
```

и:

```text
Canned tuna in water
```

они должны быть представлены как разные variants.

---

## Greek Yogurt

Недопустимо объединять:

```text
non-fat Greek yogurt
low-fat Greek yogurt
full-fat Greek yogurt
```

в один Nutrition Record.

Каждый вариант должен быть идентифицирован отдельно.

---

# 14. VISUAL SINGLE SOURCE OF TRUTH

Визуальные элементы также должны иметь единый источник.

Не допускается:

```text
Page A:
padding: 24px

Page B:
padding: 20px

Page C:
padding: 28px
```

если это один и тот же компонент.

Использовать design tokens.

Например:

```css
--container-width
--spacing-xs
--spacing-sm
--spacing-md
--spacing-lg
--spacing-xl

--radius-sm
--radius-md
--radius-lg

--font-size-sm
--font-size-md
--font-size-lg
--font-size-xl

--color-primary
--color-background
--color-text
--color-muted
```

Конкретные значения должны быть определены централизованно.

---

# 15. HEADER И NAVIGATION

Header и navigation являются глобальными компонентами.

Они должны существовать в одном месте.

Нельзя копировать HTML header вручную на каждую страницу.

Все страницы должны использовать один компонент:

```text
Header
Navigation
Mobile Navigation
```

При изменении header изменение должно автоматически распространяться на весь сайт.

Необходимо протестировать:

* desktop;
* tablet;
* mobile;
* dropdown;
* navigation overflow;
* tap targets;
* keyboard navigation;
* focus states;
* outside click;
* menu closing;
* long page titles;
* small screens.

---

# 16. FOOTER

Footer также является глобальным компонентом.

Нельзя создавать разные footer implementations для разных типов страниц без объективной необходимости.

---

# 17. PAGE CONTAINER

Все основные страницы должны использовать единый content container.

Например:

```text
Page
 ↓
Global Layout
 ↓
Page Container
 ↓
Content
```

Нельзя самостоятельно задавать ширину основного контента на каждой странице.

---

# 18. TYPOGRAPHY

Typography должна быть централизована.

Определить:

* H1
* H2
* H3
* body
* small
* caption
* labels
* buttons

Размеры, line-height, font-weight и spacing должны быть единообразными.

---

# 19. RESPONSIVE DESIGN

Каждый template должен быть responsive.

Нельзя исправлять mobile layout одной страницы через хаки, которые могут повлиять на другие страницы.

Если проблема относится к общему компоненту:

```text
fix component
```

а не:

```text
fix individual page
```

---

# 20. НЕ СОЗДАВАТЬ CSS-ХАКИ

Не использовать бессистемно:

```css
.page-specific .something {
    margin-top: -17px;
}
```

только для визуального исправления конкретной страницы.

Перед добавлением page-specific CSS необходимо определить:

> Это действительно уникальный элемент или проблема общего компонента?

Если проблема общего компонента — исправить общий компонент.

---

# 21. НОВАЯ СТРАНИЦА

При создании новой страницы ИИ обязан сначала определить:

```text
Page type
Template
Reusable components
Data source
Canonical records
Internal links
Schema
```

После этого создавать страницу.

Нельзя начинать с написания HTML "с нуля".

---

# 22. НОВЫЕ КОМПОНЕНТЫ

Перед созданием нового компонента необходимо проверить:

```text
Does an existing component already solve this problem?
```

Если да — использовать существующий.

Если нет — создать reusable component.

Не создавать компонент, предназначенный только для одной страницы, если тот же UI потенциально используется в других местах.

---

# 23. CONTENT VS DATA

Всегда разделять:

### Data

Факты и числовые значения:

* calories
* protein
* carbs
* fat
* fiber
* sugar
* serving size
* weight
* measurements
* calculator constants

### Content

* description
* explanation
* editorial text
* FAQ
* usage information

AI может генерировать content, но **не должен самостоятельно придумывать authoritative numerical data**.

---

# 24. CALCULATOR DATA

Все calculator formulas должны иметь один источник.

Например:

```text
Mifflin-St Jeor
activity multipliers
protein formulas
BMI
BMR
TDEE
1RM
MET values
```

Если одна формула используется несколькими страницами, она должна быть реализована один раз.

Нельзя:

```text
TDEE page → formula A

another page → manually copied formula B
```

---

# 25. CALCULATION ENGINE

Расчёты должны быть детерминированными.

Одинаковый input:

```text
weight
height
age
sex
activity
goal
```

должен всегда давать одинаковый result.

LLM не должен самостоятельно вычислять критические числовые результаты, если существует deterministic calculation function.

Правильно:

```text
User
 ↓
AI
 ↓
calculate_tdee()
 ↓
deterministic result
 ↓
AI explains result
```

Неправильно:

```text
User
 ↓
AI
 ↓
AI самостоятельно считает TDEE
```

---

# 26. AI FUTURE COMPATIBILITY

Архитектура должна оставлять возможность в будущем добавить:

```text
AI Assistant
AI Meal Planner
Food Diary
User Accounts
Saved Plans
Personalization
```

Но не создавать эти функции преждевременно.

Главное сейчас — стабильный frontend, canonical data и deterministic calculations.

---

# 27. SEO

Общий frontend не должен ухудшать SEO.

Каждая индексируемая страница должна иметь:

* unique title;
* unique meta description;
* canonical;
* H1;
* logical heading hierarchy;
* breadcrumbs;
* internal links;
* appropriate structured data;
* crawlable content.

Не превращать весь сайт в client-side-only приложение, если это приводит к ухудшению доступности контента поисковым системам.

---

# 28. STRUCTURED DATA

Structured data должна соответствовать видимому содержимому страницы.

Нельзя помещать в schema значение:

```text
216 kcal
```

если пользователь на странице видит:

```text
111 kcal
```

Schema и visible content должны использовать один и тот же canonical data source.

---

# 29. ПРОЦЕСС ПЕРЕД ИЗМЕНЕНИЕМ КОДА

Перед любым существенным изменением:

1. Проанализировать существующую архитектуру.
2. Найти существующие components/templates.
3. Найти canonical data.
4. Определить, является ли проблема локальной или системной.
5. Если проблема системная — исправлять architecture/component.
6. Проверить, какие страницы будут затронуты.
7. После изменения проверить affected page types.

---

# 30. ОБЯЗАТЕЛЬНАЯ ПРОВЕРКА ПОСЛЕ ИЗМЕНЕНИЙ

После каждого существенного изменения проверить:

### Visual

* Header
* Navigation
* Footer
* spacing
* typography
* buttons
* cards
* tables
* forms
* mobile layout
* desktop layout

### Data

* calories
* protein
* carbs
* fat
* fiber
* serving sizes
* preparation state
* source
* source ID

### SEO

* title
* description
* canonical
* H1
* headings
* breadcrumbs
* structured data
* internal links

### Functional

* calculator inputs
* calculations
* validation
* edge cases
* links
* navigation
* forms

---

# 31. DATA CONSISTENCY AUDIT

Перед массовым созданием новых страниц провести аудит существующего сайта.

Проверить минимум:

```text
Foods
Food variants
Nutrition values
Serving sizes
Recipes
Recipe ingredients
Comparisons
Calculator examples
Structured data
Visible values
```

Особенно искать случаи:

```text
same food
+
same preparation state
+
same basis
=
different nutrition values
```

Все найденные конфликты перечислить отдельно.

Не скрывать конфликт и не выбирать произвольное значение.

---

# 32. ИЗВЕСТНЫЕ ТИПЫ ПРОБЛЕМ, КОТОРЫЕ НЕОБХОДИМО ПРОВЕРИТЬ

Особое внимание обратить на:

* cooked vs dry rice;
* fresh vs canned tuna;
* different Greek yogurt fat levels;
* dry vs cooked oatmeal;
* recipe ingredient states;
* comparison states;
* serving weights;
* calculator examples;
* nutrition values in structured data;
* values displayed in cards vs full food pages.

---

# 33. ПРАВИЛО ПРИ НЕУВЕРЕННОСТИ

Если тебе неизвестно:

* какой вариант продукта используется;
* какой источник является canonical;
* откуда взялось число;
* почему два числа отличаются;
* какой template является правильным;

**не придумывай ответ.**

Остановись и сообщи:

```text
CONFLICT / UNCERTAINTY DETECTED
```

после чего укажи:

1. проблему;
2. затронутые страницы;
3. существующие значения;
4. источники;
5. какое решение необходимо принять.

---

# 34. ПРАВИЛО "DO NOT INVENT"

Категорически запрещается:

* придумывать nutrition values;
* придумывать serving weights;
* придумывать source IDs;
* придумывать medical claims;
* придумывать scientific claims;
* подменять данные ради красивого результата;
* создавать разные цифры для разных страниц ради SEO;
* копировать цифры из другой страницы без проверки canonical source.

Если данные отсутствуют — обозначить их как отсутствующие.

---

# 35. ПРАВИЛО МИНИМАЛЬНОГО ИЗМЕНЕНИЯ

При исправлении существующего сайта:

**не переписывай работающую архитектуру без необходимости.**

Сначала определить:

```text
Can this be fixed by:
1. component
2. template
3. data normalization
4. CSS token
5. shared utility
```

Если да — использовать это решение.

Не создавать новый параллельный механизм.

---

# 36. ПРАВИЛО ОБРАТНОЙ СОВМЕСТИМОСТИ

Перед изменением существующего компонента проверить все места его использования.

Например:

```text
Header
 ↓
all page types
```

Если изменить Header, необходимо проверить:

* homepage;
* calculators;
* foods;
* recipes;
* compare;
* goals;
* about;
* legal pages.

---

# 37. РАЗДЕЛЕНИЕ ОТВЕТСТВЕННОСТИ

Архитектура должна стремиться к:

```text
UI Components
      ↓
Templates
      ↓
Page Data
      ↓
Canonical Data
      ↓
External Sources
```

Не должно быть:

```text
Page HTML
 ├── random CSS
 ├── random nutrition value
 ├── random calculator formula
 └── random schema value
```

---

# 38. ПЕРЕД НАЧАЛОМ РАБОТЫ

Сначала НЕ изменяй код.

Сначала проанализируй repository и подготовь отчёт:

### A. Existing architecture

* структура проекта;
* где находятся страницы;
* где находятся CSS;
* где находятся JS;
* где находятся templates;
* где находятся components;
* где находятся nutrition data.

### B. Template analysis

Определи существующие типы страниц.

### C. Design inconsistencies

Найди различия:

* header;
* navigation;
* spacing;
* typography;
* cards;
* buttons;
* containers;
* tables;
* mobile behaviour.

### D. Data inconsistencies

Найди конфликты числовых данных.

### E. Proposed architecture

Предложи структуру, которая минимально изменяет существующий проект.

Только после этого приступай к изменениям.

---

# 39. ФИНАЛЬНЫЙ КРИТЕРИЙ КАЧЕСТВА

Работа считается выполненной только тогда, когда:

### Frontend

Одна и та же UI-система используется всеми соответствующими страницами.

### Data

Один canonical Food Variant имеет одно authoritative nutrition значение.

### Recipes

Используют canonical food records.

### Comparisons

Используют canonical food records.

### Calculators

Используют deterministic calculation functions.

### Schema

Использует те же данные, что и visible content.

### Future AI

AI сможет обращаться к canonical data и deterministic functions, а не генерировать критические числа самостоятельно.

---

# 40. ГЛАВНОЕ ПРАВИЛО ПРОЕКТА

Запомни и соблюдай это правило при каждой последующей задаче:

> **Никогда не исправляй отдельную страницу способом, который должен был быть исправлен на уровне общего компонента, шаблона или canonical data source.**

И второе:

> **Никогда не создавай новое числовое значение для продукта, если соответствующий canonical Food Variant уже существует. Сначала используй существующую запись или явно создай новый variant с новым источником.**

И третье:

> **Визуальная консистентность и data consistency важнее скорости генерации новых страниц.**

Перед каждой новой страницей или массовым изменением сначала проверяй существующие шаблоны, компоненты и canonical data.

---

## Результат, который необходимо получить

В конечном итоге CaloriesCalc должен иметь архитектуру:

```text
                    CALORIESCALC
                         │
              ┌──────────┴──────────┐
              │                     │
        DESIGN SYSTEM          DATA SYSTEM
              │                     │
        ┌─────┼─────┐         ┌─────┼─────┐
        │     │     │         │     │     │
      Header Cards Forms     Foods Recipes Compare
        │                     │
     Templates          Canonical Variants
        │                     │
     Pages              Nutrition Records
                              │
                         External Sources
```

При этом будущий backend/API/AI должен подключаться к этой системе, а не создавать собственную параллельную систему данных.
