---
title: "Drop Groups"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Drop Groups
Drop groups are a new feature (officially implemented in 1.8.0), that provides you with new customization options!
Drop groups are item collections, that can be assigned to [material definitions](/ouroboros-mines/configuration#materials) and define a blocks dropping behaviour, as well as the drops themselves. Drop groups are defined in `drops.yml`.

## Defining drop groups
Defining drop groups is easy! You could write down each item stack by hand, but there's a much easier way to do it. You can generate a droup-group, by creating a chest, which contains all items that should be added to the group. Once the chest is setup, you only need to type `/om dropgroup <group name>`, followed by a right-click on the chest (**You can cancel the process at any time, by typing `/om dropgroup`**). This process will create a new drop group within your `drops.yml`, which looks like this:
```yaml
mygroup:
  multidrop: true
  override: true
  drops:
    '0':
      chance: 1.0
      amount: 1
      item:
        ==: org.bukkit.inventory.ItemStack
        v: 2578
        type: COAL
```
By default, the [drop mode](#modes) will be set to `multidrop` and the amount and drop-`chance` are set to 1. The **amount value supports ranges** and defines the possible `amount` of items, while the `chance` defines the drop-`chance` in decimal percentage (1 = 100%, 0.5 = 50% etc.).
The `override` option allows you to control if the default drops should be overridden or "expanded". The default value is `true`.

### Execute commands
If you want to execute commands instead of giving items, you can easily do so by using the `commands` property **instead** of the `item` property as show in this example:

_Execute a single command:_
```yaml
somedrop:
      chance: 0.5
      commands: 'say You shall not pass!'
```

_Execute multiple commands:_
```yaml
somedrop:
      chance: 0.5
      commands: 
        - 'say Hello there!'
        - 'say General Kenobi!'
```
Both examples are executed with a chance of 50% with the first example executing only 1 and the second executing 2 commands.

## Assigning drop groups
Drop groups are assigned on a material level. Go to your [material definition](/ouroboros-mines/configuration#materials) and add the option `dropGroup`, with the name of the drop group as its value. In the end it should look like this:
```yaml
jungle_leaves:
    replacements:
    - air
    cooldown: 10
    dropGroup: mygroup
```
This example shows a material definition for jungle leaves, with the drop group `mygroup` assigned to it, which means, that in the context of the [example above](/ouroboros-mines/drop-groups#defining-drop-groups), these jungle leaves drop coal, with a chance of 100% and a maximal amount of 1, per drop.

## Modes
Drop groups can be set up with two different modes. The mode can be switched by changing the value of `mutlidrop` to `true` or `false`.
- `Singledrop`: Singledrop draws a random number between 0 and 100, which is than compared to the randomly drawn number. If the random number is within the range of 0 and the defined `chance`, the item will be dropped with the given amount, which supports ranges and therefore can also be randomly generated. Each time the drawn number isn't within the said range, the chance of the item will be added to the drop chance of its successor, which optionally leads to a final drop chance of 100 percent and the last item dropping.
- `Multidrop`: Multidrop works like `Singledrop` with a tiny difference. When multidrop is enabled, the drop chance of each item will be drawn individually which leads to the possibility of multiple dropping items.
