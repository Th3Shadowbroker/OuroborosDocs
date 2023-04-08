---
title: "Placeholders"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Placeholders

## PlaceholderAPI support
Since 1.7.1-SNAPSHOT OuroborosMines supports PlaceholderAPI.

### Available placeholders
Currently available are the following placeholders:
- `%ouroborosmines_opening_<world>_<region>%`:
  - **Alias**: `%ouroborosmines_oh_<world>_<region>%`
  - **Description**: This placeholder displays a countdown until the mine opens.
  - **Customizable**: Yes. The `opening` placeholder can be customized in your `config.yml`. `format` defines the format of the countdown and `open` contains the message that is shown as long as the the mine is opened. `format` supports the following placeholders: `%h%` for the hours, `%m` for the minutes and `%s` for the seconds.
- `%ouroborosmines_name_<world>_<region>%`:
  - **Alias**: `%ouroborosmines_n_<world>_<region>%`
  - **Description**: This placeholder displays the name of the specified mine. The name can be configured in the region's configuration file.
  - **Customizable**: No. The name of a mines is configured in its configuration file.

### Configuring placeholders
You can customize the placeholders by editing your `config.yml`