# Objectives
This DSL is created to illustrate the steps to create a DSL with Langium and its associated tooling. The DSL is chosen to be reasonably realistic and simple rather than to exercise many parts of the Langium ecosystem.

# Language goals
The language aims at supporting individuals that are interested in following a diet. The DSL supports:
1. defining aliments and their nutrition profile
2. registering daily food intake
3. defining health objectives (weight loss, waist size improvement, etc.)
4. reporting physical exercise
5. recording health assessment for a given day (weight, waist size, etc.)
6. Allows some user configuration/customization of the language semantics (default values, types and categories, etc.)
 
In the first draft of this DSL, we will only pursue the first two goals and the last one.

# Language definition
## Aliments and their nutrition profile
Aliments may have a type and category (legumes, liquid). Aliments provide a given number of calories and have a nutritional profile.
The nutritional profile should logically have mandatory components (proteins, carbs, fats) but also optional user-defined components (fiber, etc.).
If the nutrient information is not available for a category, a ? can be used to indicate that unavailability. The list of available measuring units should be fixed (g, mg, ml, cl, dl, l). Unless otherwise indicated, 1l is converted to 1kg.

An example is as follows:

```haskell
-- Aliments
100g of alpro almond vanilla (liquid, milk product) provides 29 kcal, with the following nutrients:
- proteines: 0.40 g
- glucides: 3.80g
- lipides: 1.1g
- fiber: 1.3  g
- sodium:35mg
- cholesterol: ?
- calcium: 120 mg
- fer: ?
- magnesium: -

100g of Dr Oetker Spinaci Ristorante Pizza (solid, ?) provides 220 kcal, with the following nutrients:
- proteines: 7.1 g
- glucides: 21g
- lipides: 11g
- fiber: 1.9  g
- sodium:0.27g
- cholesterol: 0
- calcium: ? mg
- fer: ?
- magnesium: ?

-- Portions
alpro almond vanilla:
- small: 250ml
- large: 500ml

```

## Daily food intake
The daily food intake section of the DSL lets the user declare his food intake across the meals of a given day. Meals are a list of aliments whose portion/quantity can be specified. To make it more user-friendly and closer to natural language, we define two distinct formats to enter that list:

An example is as follows:

```haskell
On the DATE_FORMAT:
- at breakfast:
  - alpro almond vanilla (small)
- at lunch:
  - ?
- at dinner:
  - x1 Dr Oetker Spinaci Ristorante Pizza
- at USER_DEFINED_MEAL:
  - ...

On the DATE_FORMAT, at breakfast:
- alpro almond vanilla (small)
- ...

On the DATE_FORMAT, at dinner:
- Dr Oetker Spinaci Ristorante Pizza
- ...

```

Note that the existence of two formats is creating edge cases that will have to be handle. Such is the case when on the same day, for the same meal a user defines distinct food intakes in distinct formats. The food intakes should be summed appropriately nonetheless.

## User configuration
There should be defaults so that this is rather infrequent.

An example is as follows:

```yaml
Meals:
- breakfast
- lunch
- dinner

Categories:
- Fruit
- Vegetable
- Legume
- Sweet
- Fish 
- Meat
- Milk product

Type:
- Solid
- Liquid (cream-like)
- Liquid (sauce-like)
- Liquid (oil-like)
- Liquid (water-like)

```
