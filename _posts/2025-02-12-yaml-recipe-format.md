---
title: "YAML keeps my recipes looking nice"
excerpt_separator: "<!--more-->"
tags:
  - food
  - software
---

As a dedicated home cook, I've accumulated a ton of recipes over the years. Some from family, some from the internet, some I came up with myself. Each one has a bunch of notes and has evolved over time as I've changed out cooking equipment or as my tastes have changed. I'd been looking for a structured way to store my recipes so that I didn't just have a bunch of word documents or spreadsheets lying around. I also wanted to be able to put my recipes on a website I host on my home network so that I could have them on hand in the kitchen through any device I happened to have with me.

I found this [Recipe schema on Schema.org](https://schema.org/Recipe), which seems to be a pretty standard way of storing recipes across the web because it's parseable by search engines, and used for indexing, but for my home website, it seemed like overkill. I didn't want to migrate plain text to this JSON format because of how many structures there are, and I couldn't easily write a script to automate the job because my recipes were so unstructured.

So, I came up with my own format in YAML. It's easily integrated into Jekyll websites as front matter, it's more easily readable as YAML than as JSON, and  I can write a script to migrate to/from JSON in the future if I really had to.

Here's what I came up with:

```yaml
title: Something Muffins
tags: muffins breakfast
image: something_muffins.jpg
components:
  - name: Muffin base
    ingredients:
      - 2 1/4 cup whole wheat flour
      - 1/4 cup sugar
      - 2 tsp baking powder
      - 1/4 tsp baking soda
      - 1/4 tsp salt
      - 1+ cup pureed whatever
      - 1 egg
      - 1/2 cup oil
      - 1 cup milk
      - 1/2 cup shredded coconut
  - name: Names are optional!
    ingredients:
      - 3 tbsp olive oil
      - 3 tbsp flour
      - Turkey stock
directions:
  - Preheat oven to 375°F. Spray a standard muffin pan (12-cup) with nonstick spray or line them with paper muffin liners.
  - In a large bowl, mix together the flour, sugar, baking powder, baking soda and salt.
  - In another bowl, whisk together the oil, whatever, egg and milk until whatever clumps are mostly dissolved.
  - Fold wet mixture into dry mixture then stir in coconut until just combined. The batter will look a little on the drier side.
  - Fill muffin tins or liners; bake for about 20 to 25 minutes, or until muffins are puffed and turning golden brown on top.
notes:
  - Should work well with any type of fruit puree, like pumpkin, banana, apple, etc.
  - Other reminder of how not to mess this up
yield:
  - 12 muffins
sources:
  - label: NYT Cooking
    url: https://cooking.nytimes.com/recipes/1014060-whole-wheat-muffins
  - label: NYT Cooking
    url: https://anotherwebsite.com/recipe/muffins.html
    
```

I don't have a use case for, say, identifying which fields are quantity measures, like you could do in the JSON recipe format. That would make quantities easy to adjust for different batch sizes or translate between volumetric and weight measures, which is important if you're making a website for a wider audience. For my purposes, I don't own a scale, but I also have few baking recipes so I could easily extend this schema to accommodate or express the amounts as separate objects in the existing schema.

The great benefit to YAML is its simplicity vs the JSON format. Sometimes people ask me for recipes and I can really just copy/paste the YAML doc into a quick email, if I'm not on my wifi, and it doesn't look like a garbled mess to, say, the old people in my life who have enough difficulty with technology.

There's a whole lot this format can't do, but it provides more flexibility than a printed recipe book, which is more than good enough for now.