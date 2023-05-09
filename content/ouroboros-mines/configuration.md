---
title: "Configuration"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Configuration
This page describes OMs configuration file. When opening the config file for the first time, you might have recognized the header of the configuration file is starting with something like `# OuroborosMinesv1.0.0-SNAPSHOT`. This is just an information on the version of OM that generated the configuration file.

## Default
Key: `default`

Description: When true every region will be handled as mine unless defined otherwise. This options is not recommended for the most cases and might affect the servers performance, as OM has to process every block change in detail.

## Chat
### Prefix
Key: `chat.prefix`

Description: This option defines the prefix used in player messages.

### Messages
Key: `chat.messages`

Description: This key contains the message templates used by the plugin.

Values:
```yml
consoleOnly: "&cThis command can not be executed from console!"
unrecognizedArgument: "&cUnrecognized argument"
regionCustomize: "&2A new configuration file for &b%region%&2 has been created!"
regionAlreadyCustomized: "&cThere's already a configuration file for this region!"
regionNotFound: "&cNo region found at your position!"
reloadingConfig: "&2Reloading configuration..."
reloadingRegionConfigurations: "&2Reloading region-configurations..."
reloadedRegionConfigurations: "&2Loaded %count% region-specific configurations"
depositDiscovered: "&2You've discovered a %size% &e%material%&2 deposit!"
depositSizes:
      small: "small"
      medium: "medium"
      large: "large"
announcements:
      opening: "&2The mines are open!"
      closing: "&cThe mines are now closed. Come back tomorrow!"
minesClosed: "&cThe mines are closed come back later."
error: "&cAn error occurred: %error%"
```

## Materials
Key: `materials`

The materials section contains the definitions of mineable materials. These definitions are structured as follows:
```yml
stone: <-- The key represents the mined material.
  replacements: <-- The replacements-list contains every material that will could replace the mined material.
    -cobblestone_block <-- In this case stone can be replaced with cobblestone-blocks.
  cooldown: 10 <-- The cooldown (in seconds) defines the time for the mined block to respawn.
```
by default the configuration looks like this:
```yml
materials:
  stone:
    replacements:
      - cobblestone
    cooldown: 10

  coal_ore:
    replacements:
      - stone
    cooldown: 20

  gold_ore:
    replacements:
      - stone
    cooldown: 30

  iron_ore:
    replacements:
      - stone
    cooldown:  40

  emerald_ore:
    replacements:
      - stone
    cooldown: 50

  diamond_ore:
    replacements:
      - stone
    cooldown: 120
```
### Adding custom blocks using ItemsAdder
OuroborosMines supports [ItemsAdder](https://www.spigotmc.org/resources/%E2%9C%A8itemsadder%E2%AD%90custom-items-armors-hud-gui-mobs-emoji-blocks-wings-hats-liquids.73355/). You can setup custom blocks with ease as shown in the example below:
```yaml
itemsadder:aqua_aura_ore: <-- Material name prefixed by the material's namespace
    replacements:
      - air
    cooldown: 5
```
As you can see, a namespace has been added which allows OuroborosMines to identify the material as custom. Now you can start mining custom blocks!

### Check additional properties
Some blocks like `WHEAT` or other plants have additional properties. OuroborosMines supports additional checks and modifications on these. To add such check simply add a `properties` section to the material definition like so:

```yaml
wheat:
  replacements:
  - air
  cooldown: 10
  properties:
    age: 7 <-- Only fully grown wheat
    resetAge: true <-- Reset the age of the wheat to 0 when regenerating
```
#### Supported properties
| Property | Type    | Description                                                   |
|----------|---------|---------------------------------------------------------------|
| age      | int     | The required age of a material that ages (for example wheat). |
| resetAge | boolean | Reset the age of a material when regenerating.                |

### Additional customizations

#### Auto pickup
Auto pickup is a global feature that was added with the clan-update (1.7.0).
When set to true, all items a player gathers will be directly transferred to the inventory.
However, if the players' inventory is full, the mined blocks will drop as usual.
For obvious reasons this feature is set globally in the `config.yml` file and **applies to all mines**.


> **The following feature is only available in the 1.9.3 dev build and has not been implemented into any releases yet:**
>
> If you want to apply `autoPickup` to specific players only, you can give them a [specific permission](/ouroboros-mines/permissions#note-on-ouroborosminesautopickup).

_Auto pickup in config.yml_
```yaml
autoPickup: false
```

#### Randomizing cooldowns:
Cooldowns can be randomized. Just use a range as cooldown. A cooldown with the value of "10-60" will generate
a random cooldown between ten and 60 seconds.
#### Rich deposits:
To make mining even more interesting it is possible to give regular ores a small chance to replace themselves withtheir own material. You can do so by adding the `rich-chance` and `rich-amount` property to a material defined in the configuration.
The property `rich-chance` accepts doubles as type, which means you're allowed to use frictional digits.
The property `rich-amount` accepts ranges, as well as single numbers as value with the first one being the minimum and the second one as maximum. If only one number is given, it will be used as fixed amount.
This is an example on how to use both of these customizations:
```yml
  gold_ore:
    replacements:
      - stone
    cooldown: 30-45
    rich-chance: 5
    rich-amount: 2-4
```
This exmaple shows a gold-ore configuration with a cooldown between 30 and 45 seconds and a chance of 5% of getting replaced by its original material with an amount of 2-4 meaning it can be mined betwenn two and four times, before it's respawning.
#### Play effects
If you want to add effects to improve your server's user experience, you can use the effects section in the configuration. This section is not required an therefore will not be patched into the confiuration file if you used an older version than 1.3.0 before.

The effects section is structured as follows:
```yml
effects:
  deposit_discovered:
    sound: entity_player_levelup
    sound-volume: 1
    sound-pitch: 0
```
The example above shows a configuration for the `deposit_discovered` event that plays the `entity_player_levelup` sound with the `sound-volume` of 1, which is equal to 100% (0.25 = 25%, 0.1 = 10% and so on...) and the pitch of 0.

**Key** | **Description** | **Type** | **Default**
--- | ----------- | ---- | -------
`sound`  | The name of the sound. A list of sounds can be found [here](https://www.spigotmc.org/wiki/cc-sounds-list/) | String/Enum | None
`sound-volume` | The volume this sound is played with. | Float | 1
`sound-pitch` | The pitch this sound is played with. | Float | 0

##### Add particle effects
You can also add particle effects to an event. The following example shows a configuration that plays a totem effect from the blocks center with an amount of 10 particles. **You can combine particle and sound effects in one single definition!**
```yml
effects:
  deposit_discovered:
    particle: totem
    particle-amount: 10
```
If you are using the redstone particle, you can also define the color of the spawned particles using the `particle-color` property. Here's an example using the redstone particle.
```yml
effects:
  deposit_discovered:
    particle: redstone
    particle-amount: 10
    particle-color:
      r: 204
      g: 0
      b: 0
    particle-size: 1
    particle-pattern: center
```

**Key** | **Description** | **Type** | **Default**
--- | ----------- | ---- | -------
`particle` | The particle that should be displayed. A list of particles can be found [here](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Particle.html). | String | None
`particle-amount` | The amount of particles that should be spawned. (Multiplied when `particle-pattern` is set to borders). | Integer | 5
`particle-color` | The color of the spawned particles defined by 3 sub-properties (r,g and b) (**only supported by redstone!**) | Section with integer properties (r, g and b) | 0
`particle-size` | The size of the spawned particles (**only supported by redstone!**) | Float | 1
`particle-pattern` | The pattern in which the particles are spawned. | Pattern (center or borders) | center

##### Particle-patterns
The plugin supports 2 `particle-pattern`s:

**center**: *The particles are spawned in the exact center of the block.*

**borders**: *The particles are spawned alongside the blocks borders.*

##### Supported events
This list contains all supported events. The names used in the configuration are **not** case sensitive.

Name | Description
---- | -----------
`DEPOSIT_DISCOVERED` | This event is triggered when a player discovered a [rich-deposit](#rich-deposits).

#### Give experience for mined blocks
You can add two different types of experience to a material. One being `experience` and the other one being `rich-experience` as you might have already guessed, the first one sets the amont of `experience` a player can earn by mining a certain material while the `rich-experience` sets the value of the experience that is earned from materials that were spawned by a deposit. Below you can find a basic example. Keep in mind, that the first time a mineral is mined is also the point in time in which the rich-amount get's drawn. Both settings support ranges between integers.
```yml
coal_ore:
    replacements:
    - stone
    cooldown: 5-10
    experience: 1-3
    rich-chance: 75
    rich-amount: 1-4
    rich-experience: 1-2
```
This definition describes a coal ore that gives between 1 and 3 experience-points when mined from a common material and 1-2 experience-points when mined from a deposit. By default orbs with the given amounts of experience are spawned at the mined blocks. If you want to give the experience to the player directly, you can use the setting `experience.spawnOrbs` in the plugins `config.yml` to do so.

#### Opening hours
Opening hours are a feature, that was first introduced with the clan-update (1.7.0).
This feature offers a way to allow your players to mine at specific times and is **only available on a regional level!**
This means, that this feature is only a available if you've created a [region configuration](/ouroboros-mines/region-configuration) before. To keep regional configuration files as clean as possible, the `openingHours` sections is not copied by default, as it also is part of the more advanced functions of OuroborosMines. However, opening-hours can be inherited from the preset contained in `config.yml`.
The opening hours can be configured as follows:
```yaml
openingHours:
  enabled: false
  time: 0-12000
  realtime: 11:00-16:00
  announcements:
    opening: true
    closing: true
    worlds:
      - world
```
Let me explain how it works. The `time` setting defines the opening-hours of the mine in ticks. Ticks are a Minecraft specific time unit. You can learn more about that [here](https://minecraft.gamepedia.com/Day-night_cycle). However, if the setting `realtime` is set, `time` will be ignored and can be removed from the configuration without any problems. If you want to use **realtime timespans**, make sure to **use the 24h time-format**. One difference here, is that `realtime` also supports multiple timespans. If you want to open your mines twice a day, you can do something like this:
```yaml
realtime: 
  - 8:00-12:00
  - 14:00-18:00
```
This example would make the mine open from 8:00 to 12 and from 2PM to 6PM. As you now know how you can configure the opening hours let's get to the `announcements` section, which looks as follows:
```yaml
announcements:
  opening: true
  closing: true
  worlds:
    - world
```
Depending if `opening`/`closing` is enabled the `opening`/`closing` will be announced in all worlds listed in the `worlds` section. The `worlds` section is fully optional and can be removed, if you want to broadcast `opening`/`closing` globally. The messages, that are send when a mine opens or closes are defined in your `config.yml` **or** the regions configuration file:
```yaml
chat:
  ...
  messages:
    ...
    announcements:
      opening: "&2The mines are open!"
      closing: "&cThe mines are now closed. Come back tomorrow!"
    minesClosed: "&cThe mines are closed come back later."
    ...
```

#### Placeholders
You can customize OuroborosMine's placeholders. For example: The `opening` placeholder can be customized as shown below. `format` defines the format of the countdown and `open` contains the message that is shown as long as the the mine is opened. You can learn more about the placeholders on [their wiki page](/ouroboros-mines/placeholders).
```yaml
placeholders:
  openingHours:
    format: '%h%:%m%:%s%'
    open: Now!
```