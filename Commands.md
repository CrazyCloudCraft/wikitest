This page shows all commands, including with a detailed description of what every command does.

## Overview
* [/papi bcparse](#papi-bcparse)
* [/papi disablecloud](#papi-disablecloud)
* [/papi ecloud](#papi-ecloud)
* [/papi info](#papi-info)
* [/papi list](#papi-list)
* [/papi parse](#papi-parse)
* [/papi parserel](#papi-parserel)
* [/papi register](#papi-register)
* [/papi reload](#papi-reload)
* [/papi unregister](#papi-unregister)

#### `/papi bcparse`
**Description**:  
Parses an expansion and broadcasts the result to all players.

**Arguments**:
* `<expansion name>`

**Example**:
* `/papi bcparse %player_name%`

----
#### `/papi disablecloud`
**Description**:  
Disables the connection to the expansion-cloud.

----
#### `/papi ecloud`
**Description**:  
Shows information about the expansion cloud.

**Arguments**:  
* `status` - Shows the actual status of the ecloud.  
* `list`
  * `<all/author>` - Lists all or author-specific placeholders.
  * `<page>` - used to scroll through multiple pages.
* `info`
  * `<expansion name>` - Gives info about a expansion.
* `download`
  * `<expansion name>` - downloads a expansion.

----
#### `/papi info`
**Description**:  
Gives you information about the specified expansion.

**Argument(s)**:  
* `<expansion name>`

**Example**:  
`/papi info Vault`

----
#### `/papi list`
**Description**:  
Lists all active/registered expansions.  
This is different to [/papi ecloud list installed](#papi-ecloud) in the fact, that it also includes expansions that where installed through a plugin (That aren't a seperate jar-file).

----
#### `/papi parse`
**Description**:  
Shows you the output of the given placeholder.

**Argument(s)**:  
* `<%placeholder%>`

**Example**:  
`/papi parse %vault_group%`

----
#### `/papi parserel`
**Description**:  
Parses a relational placeholder.

**Arguments**:  
* `<player1>`  
* `<player2>`  
* `<args>`

**Example**:  
`/papi parserel funnycube extended_clip %placeholder%`

----
#### `/papi register`
**Description**:  
Registers a expansion from a specified filename.  
This is usefull in cases, where you downloaded the expansion manually and don't want to restart the server.

**Arguments**:
* `<filename>`

**Example**:
* `/papi register MyExpansion.jar`

----
#### `/papi reload`
**Description**:  
Reloads the config settings.

----
#### `/papi unregister`
**Description**:  
Unregisters the specified expansion.

**Arguments**:
* `<filename>`

**Example**:
* `/papi unregister MyExpansion.jar`