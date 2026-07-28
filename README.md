# CaloriesCalc.org

Free nutrition calculators and food calorie database.

🌐 **[caloriescalc.org](https://caloriescalc.org)**

## Calculators

| Calculator | URL |
|---|---|
| TDEE Calculator | `/` |
| BMR Calculator | `/bmr-calculator.html` |
| Macro Calculator | `/macro-calculator.html` |
| Protein Calculator | `/protein-calculator.html` |
| BMI Calculator | `/bmi-calculator.html` |
| Calorie Deficit | `/calorie-deficit-calculator.html` |

## Foods Database

136 foods across 11 categories — `/foods/`

**Categories (planned):**
- ✅ Fruits (20) — banana, apple, orange, strawberry, blueberries, grapes, watermelon, pineapple, mango, avocado, kiwi, peach, pear, lemon, lime, cherry, raspberry, blackberry, pomegranate, coconut
- 🔜 Vegetables (20)
- 🔜 Meat (10)
- 🔜 Fish & Seafood (10)
- 🔜 Eggs & Dairy (10)
- 🔜 Grains (10)
- 🔜 Nuts & Seeds (10)
- 🔜 Legumes (6)
- 🔜 Drinks (10)
- 🔜 Fast Food (10)
- 🔜 Desserts (10)
- 🔜 Popular (10)

## Structure

```
caloriescalc.org/
├── index.html                    # TDEE Calculator
├── bmr-calculator.html
├── macro-calculator.html
├── protein-calculator.html
├── bmi-calculator.html
├── calorie-deficit-calculator.html
├── about.html
├── sitemap.xml
├── robots.txt
├── _headers                      # Cloudflare cache rules
├── _redirects                    # Cloudflare redirects
└── foods/
    ├── index.html                # Food catalog (136 items)
    ├── banana.html + banana.png
    ├── apple.html + apple.png
    └── ...                       # 20 fruits + growing
```

## Tech

- Pure HTML/CSS/JS — no frameworks, no build step
- Hosted on Cloudflare Pages
- Data source: USDA FoodData Central

## License

MIT
