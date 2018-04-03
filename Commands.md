This page shows all commands, including with a detailed description of what every command does.

## Overview
* [/papi info](#papi-info)
* [/papi parse](#papi-parse)
* [/papi parserel](#papi-parserel)
* [/papi reload](#papi-reload)
* [/papi disablecloud](#papi-disablecloud)
* [/papi ecloud](#papi-ecloud)



#### `/papi info`
**Description**:  
Gives you information about the specified placeholder.

**Argument(s)**:  
* `<placeholder name>`

**Example**:  
`/papi info Vault`

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
#### `/papi reload`
**Description**:  
Reloads the config settings.

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