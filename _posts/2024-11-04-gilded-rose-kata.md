---
title: "The Senior Engineer's Guide: The Gilded Rose Code Kata"
excerpt_separator: "<!--more-->"
classes: wide
tags:
  - software
  - code-kata
  - career
  - senior-engineers-guide
---

### Introduction

Code katas are a great way to brush up on some of the fundamentals of software engineering - the skills you'll actually use on the job. I'm particularly thankful for [Emily Bache's GitHub repos](https://github.com/emilybache?tab=repositories&q=kata&type=&language=&sort=) with all the code and test fixtures that makes it easy to work on one when the inspiration strikes.

I want to talk about one particular kata today: [the Gilded Rose refactoring kata](https://github.com/emilybache/GildedRose-Refactoring-Kata).

<!--more-->

Take a few minutes to familiarize yourself with the instructions [here](https://github.com/emilybache/GildedRose-Refactoring-Kata/blob/main/GildedRoseRequirements.md).

View my [whole solution on GitHub](https://github.com/jgerace/GildedRose-Refactoring-Kata).

We'll be working in Python today. Here's the code that updates the quality of our items:

```python
class GildedRose(object):

    def __init__(self, items):
        self.items = items

    def update_quality(self):
        for item in self.items:
            if item.name != "Aged Brie" and item.name != "Backstage passes to a TAFKAL80ETC concert":
                if item.quality > 0:
                    if item.name != "Sulfuras, Hand of Ragnaros":
                        item.quality = item.quality - 1
            else:
                if item.quality < 50:
                    item.quality = item.quality + 1
                    if item.name == "Backstage passes to a TAFKAL80ETC concert":
                        if item.sell_in < 11:
                            if item.quality < 50:
                                item.quality = item.quality + 1
                        if item.sell_in < 6:
                            if item.quality < 50:
                                item.quality = item.quality + 1
            if item.name != "Sulfuras, Hand of Ragnaros":
                item.sell_in = item.sell_in - 1
            if item.sell_in < 0:
                if item.name != "Aged Brie":
                    if item.name != "Backstage passes to a TAFKAL80ETC concert":
                        if item.quality > 0:
                            if item.name != "Sulfuras, Hand of Ragnaros":
                                item.quality = item.quality - 1
                    else:
                        item.quality = item.quality - item.quality
                else:
                    if item.quality < 50:
                        item.quality = item.quality + 1


class Item:
    def __init__(self, name, sell_in, quality):
        self.name = name
        self.sell_in = sell_in
        self.quality = quality

    def __repr__(self):
        return "%s, %s, %s" % (self.name, self.sell_in, self.quality)
```

To summarize the task:

- Refactor the code
- Implement a new feature: Conjured Item

Let's keep in mind that we have a couple constraints:

- No updating the `Item` class
- No updating `items` property

### Taking stock of the code

We have no unit tests!

```python
# -*- coding: utf-8 -*-
import unittest

from gilded_rose import Item, GildedRose


class GildedRoseTest(unittest.TestCase):
    def test_foo(self):
        items = [Item("foo", 0, 0)]
        gilded_rose = GildedRose(items)
        gilded_rose.update_quality()
        self.assertEqual("fixme", items[0].name)

        
if __name__ == '__main__':
    unittest.main()
```

There's one failing test that illustrates how to use the `GildedRose` class, but that's it.

We also notice that the code in GildedRose relies on checking the `name` property of an `Item` to determine how to update quality.

Sifting through all the if statements, we can also see that the code updates `quality` before it decrements the `sell_in` field, then it update `quality` again for any items that adjust further once the `sell_in` date has passed.

### Tests, please!

We definitely need to write unit tests first so that we can run them as we update the code. Let's outline a few cases before we fill in the code.

```python
    def test_quality_upper_limit(self):
        # The Quality of an item is never more than 50
        pass

    def test_quality_lower_limit(self):
        pass

    def test_quality_degrades_regular_item_before_sell_date(self):
        # degrades by 1x
        pass

    def test_item_past_sell_by_date_degrades_twice_as_fast(self):
        # Quality degrades twice as fast
        pass

    def test_update_quality_decrements_sell_in(self):
        pass

    def test_no_degredation_on_sulfuras(self):
        # no decrementing quality or sell by
        pass

    def test_aged_brie_quality_increases_before_sell_date(self):
        # Quality increases 1x
        pass

    def test_aged_brie_quality_increases_after_sell_date(self):
        # Quality increases 2x
        pass

    def test_backstage_passes_quality_increases_more_than_10_days(self):
        # Quality increases 1x
        pass

    def test_backstage_passes_quality_increases_less_than_10_days(self):
        # Quality increases 1x
        pass

    def test_backstage_passes_quality_increases_less_than_5_days(self):
        # Quality increases 1x
        pass

    def test_backstage_passes_quality_0_after_sell_date(self):
        pass

    def test_conjured_degrades_2x_normal(self):
        # new functionality
        pass
```

Once we fill in the body of all the test cases, minus the last one for Conjured items, we can think about refactoring the current logic. We'll work on adding new functionality later.

### A close read of the requirements specification

In reading the requirements specification, a few things stand out to me:

- **"Aged Brie"** actually increases in `Quality` the older it gets
- **"Sulfuras"**, being a legendary item...
- Backstage passes have much more complicated logic than other items
- **"Conjured"** items degrade in `Quality` twice as fast as normal items

The common theme between all these statements is that they pertain to the *type* of item we're dealing with. However, this concept isn't reflected in the code. There's no mention of "normal" or "legendary" or "aging" but we can see that the person who wrote the spec is thinking in these terms.

This reminds me of the concept of a [ubiquitous language](https://martinfowler.com/bliki/UbiquitousLanguage.html). Can we change the architecture of the code to reflect the language we use to describe it?

The code as we have received it relies on the names of items, but what if we want more than one type of cheese? Or tickets for different concerts? Or more than one kind of legendary item? What kind of Conjured items should we even expect to deal with? For this latter concern, the spec doesn't even go into detail, so we should build flexibility into our refactored solution.

The approach here is to figure out what the stakeholder actually needs. Think beyond what the code currently does. What *should* it do? How do we expect it to be used?

In short: What's the user story?

There's enough ambiguity here to suggest that we have the potential to deal with many types of items, and that if we don't build this in at the outset, we'll run into trouble down the line. Relying on name comparison isn't usually a great architectural pattern!

### Separation of concerns

Let's consider the architecture of this inn from a high level. We have two concepts here:

- The inn itself
- The items it sells

As it stands, the inn is in charge of knowing the logic for how each item degrades. For any future addition to the inventory (Conjured items), we would have to update the inn to consider the name and work the quality update logic into this mess of if statements. This doesn't sound ideal!

I propose that we alter our code such that the inn only decides *when* to update quality. The logic of *how* to update quality should belong to our respective items.

We know we can't update the `Item` class because we aren't allowed to touch it. Fair enough!

This conundrum screams INHERITANCE anyway.

We can create a series of sub classes that inherit from `Item`:

- NormalItem
- AgedItem
- TicketItem
- LegendaryItem
- ConjuredItem - we'll implement this one later

The result:

```python
class NormalItem(Item):
    def __init__(self, name, sell_in, quality):
        super().__init__(name, sell_in, quality)

    def update_quality(self):
        pass


class LegendaryItem(Item):
    def __init__(self, name, sell_in, quality):
        super().__init__(name, sell_in, quality)

    def update_quality(self):
        pass


class AgedItem(Item):
    def __init__(self, name, sell_in, quality):
        super().__init__(name, sell_in, quality)

    def update_quality(self):
        pass


class TicketItem(Item):
    def __init__(self, name, sell_in, quality):
        super().__init__(name, sell_in, quality)

    def update_quality(self):
        pass
```

### Modifying the legacy code

Let's extract our logic for a normal item into our new `NormalItem` class.

```python
class NormalItem(Item):
    def __init__(self, name, sell_in, quality):
        super().__init__(name, sell_in, quality)

    def update_quality(self):
        if self.quality > 0:
            self.quality = self.quality - 1
        self.sell_in = self.sell_in - 1
        if self.sell_in < 0:
            if self.quality > 0:
                self.quality = item.quality - 1
```

The code isn't particularly elegant because we've basically copied it straight from the code as is. Let's clean that up a bit.

```python
class NormalItem(Item):
    def __init__(self, name, sell_in, quality):
        super().__init__(name, sell_in, quality)

    def update_quality(self):
        self.quality -= 1
        self.sell_in -= 1
        if self.sell_in < 0:
            self.quality -= 1
        self.quality = max(self.quality, 0)
```

Now let's work this into the `update_quality()` function so we can run our unit tests against it. Note that the texttest fixture allows for normal items to have different names, so this allows us to ignore the special items for now. We still want our tests to pass for those, so we're setting this up to run new code only for the normal items.

```python
def update_quality(self):
    for item in self.items:
        if item.name not in ["Aged Brie", "Backstage passes to a TAFKAL80ETC concert", "Sulfuras, Hand of Ragnaros"]:
            item.update_quality()
            continue
        # <rest of code follows...>
```

We also need to write our unit tests for normal items to use this new `NormalItem` class. One example:

```python
def test_quality_lower_limit(self):
    items = [NormalItem("foo", 10, 0)]
    gilded_rose = GildedRose(items)
    gilded_rose.update_quality()
    self.assertEqual(items[0].quality, 0)
```

If we've done our job, all tests should still pass!

We can now update the rest of the items.

```python
class LegendaryItem(Item):
    def __init__(self, name, sell_in, quality):
        super().__init__(name, sell_in, quality)

    def update_quality(self):
        pass


class AgedItem(Item):
    def __init__(self, name, sell_in, quality):
        super().__init__(name, sell_in, quality)

    def update_quality(self):
        self.quality += 1
        self.sell_in -= 1
        if self.sell_in < 0:
            self.quality += 1
        self.quality = min(self.quality, 50)


class TicketItem(Item):
    def __init__(self, name, sell_in, quality):
        super().__init__(name, sell_in, quality)

    def update_quality(self):
        self.quality = self.quality + 1
        if self.sell_in < 11:
            self.quality = self.quality + 1
        if self.sell_in < 6:
            self.quality = self.quality + 1
        self.quality = min(self.quality, 50)

        self.sell_in = self.sell_in - 1
        if self.sell_in < 0:
            self.quality = 0
```

Now we should be able to delete the entire old body of the update_quality() function in favor of this much more svelte one:

```python
def update_quality(self):
    for item in self.items:
        item.update_quality()
```

It should be trivial now to add a new `ConjuredItem` class (left as an exercise to the user). No tangle of if statements or anything.

### TextTests and an ideal state

We should note that TextTests won't pass unless we modify the fixture to use our new item classes. This highlights one of the drawbacks to this solution: we have to update all callers of the `Item` class to use our new classes.

This, to me, is a small price to pay for what I see as cleaner organization, increased flexibility, and a proper separation of concerns. We've broken our dependence on the item's `name` property. That alone is huge for maintainability, readability, and extensibility!

We don't have more context than this, but I could see a world in which we created an item factory so that whatever classes are creating these items can do so with a single object instead of having to juggle multiple. The factory would abstract even the type of item, which we only need upon creation. Gilded Rose just cares about receiving a list of items. It really doesn't care what they are, and rightly so.

### Conclusion

I'm sure there's more we could do here, and we could even talk about different approaches to the refactor.

We didn't try to refactor the legacy code before creating our new item classes. In a different kind of problem, one in which the requirements spec didn't suggest the concept of item types, we might have wanted to do that to see if patterns emerged that way. Our particular solution just happened to be viable without that step, but it's an important strategy to keep in mind for other refactoring work.

I cannot stress enough how important it is to get the user stories straight at the outset. Really grok what you're building.

If we hadn't parsed out the idea that we need flexibility in our items, we might have crafted a solution that continued to rely on name comparison and it would have been much more difficult to handle Conjured items without even knowing what they could be named. We had one example in the texttest fixture, but for a normal item we had two different examples. Which way to go?

I hope this helps someone hone their craft. Keep up the great work, you glorious coders.

And once more for the road: Get the user stories!!!