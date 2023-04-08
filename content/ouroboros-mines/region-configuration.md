---
title: "Region Configuration"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Region Configuration

## What is a region configuration?
A region-configuration is a configuration that allowes you to customize a single regions cooldowns and materials.
To create a new region-configuration, you just need to stand within the region you want to customize and type `/om customize`. After using the command a new file within the regions directory (which is located within the plugin's directory) will be generated. This directory uses the world structure of the server, which means, that when you create a new configuration, it will be located in 'OuroborosMines/regions/world/region'.

## Setting everything up
**After you set everything up and saved, you don't need to restart your server. Just use `/om reload`**

By default, the configuration from the main config (config.yml) will be copied into the newly created region-configuration. A newly region-configuration should look like this:
```yml
inherit: false
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
    cooldown: 40
  emerald_ore:
    replacements:
    - stone
    cooldown: 50
  redstone_ore:
    replacements:
    - stone
    cooldown: 55
  diamond_ore:
    replacements:
    - stone
    cooldown: 120

```
At the top you can see a setting named `inherit`, which is used to enable/disable the inheritance from the default configuration, which means, that if `inherit` is set to `true`, everything that is not explicitly overwritten or specified in the region-configuration, will be inherited exactly as it's set in `config.yml`. For more detailed information on the material-settings, have a look at the [configuration](/ouroboros-mines/configuration) page.

### Additional configuration
If you have [opening hours](/ouroboros-mines/configuration#opening-hours) configured, you can also override specific messages, that are normally defined in your `config.yml`. Just add this to your region's config file:
```yaml
chat:
  messages:
    announcements:
      opening: "&2The mines are open!"
      closing: "&cThe mines are now closed. Come back tomorrow!"
    minesClosed: "&cThe mines are closed come back later."
```

#### Naming mines
Mines can be named by simply adding the name setting to a region's configuration file. The name of the mine will be used inside the `%ouroborosmines_name_<world>_<region>%` placeholder. If no name is configured, this placeholder falls back to the regions id.