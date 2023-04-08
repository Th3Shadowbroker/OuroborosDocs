---
title: "Permissions"
weight: 1
# bookFlatSection: false
bookToc: false
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Permissions
| Command | Permission | Default |
| ------- | ---------- | ------- |
| /om | ouroboros.mines.command.info | true |
| /om reload | ouroboros.mines.command.reload | op |
| /om customize | ouroboros.mines.command.customize | op |
| /om dropgroup | ouroboros.mines.command.dropgroup | op |
| - | ouroboros.mines.autopickup | false |
| - | ouroboros.mines.mine | true |

### Note on ouroboros.mines.autopickup
Allows a player to auto-pickup drops from blocks. This permission **overrides** the `autoPickup` setting.

### Note on ouroboros.mines.mine
Allows players to mine in mining regions. This permission is given by default and has to be explicitly revoked.