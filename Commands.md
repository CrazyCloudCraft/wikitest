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
* `<player|me>` - The Player to parse values of the placeholder (Use `me` for yourself).
* `<Text with placeholders>` - The text to parse.

**Example**:  
```
/papi bcparse funnycube My name is %player_name%!
```

----
#### `/papi disablecloud`
**Description**:  
Disables the connection to the expansion-cloud.

----
#### `/papi ecloud`
**Description**:  
Shows info about the expansion cloud and performs actions with it.

**Arguments**:
* `clear` - Clears the expansion cloud cache.
* `download` - Download an expansion.
  * `<expansion name>` - Name of the expansion to download.
  * `[version]` - A specific version of the expansion to download (Optional).
* `info` - Get info about an expansion.
  * `<expansion name>` - Name of the expansion to get info from.
* `list`
  * `<all|author <author>|installed>` - List either all expansions, only those of a author or all installed ones.
  * `[page]` - The page to list.
* `placeholders` - Lists placeholders of an expansion.
  * `<expansion name>` - The expansion to show the placeholders from.
* `refresh` - Refreshes the cached data of the expansion cloud.
* `status` - Shows the actual status of the ecloud.
* `versioninfo` - Get info of a specific expansion version.
  * `<expansion name>` - Name of the expansion to get info from.
  * `<version>` - The specific version to get the info from.

**Examples**:  
```
/papi ecloud download Vault
/papi ecloud info Vault
/papi ecloud list author clip
/papi ecloud placeholders Vault
/papi ecloud versioninfo Vault 1.5.0
```

----
#### `/papi info`
**Description**:  
Gives you information about the specified expansion.

**Argument(s)**:  
* `<expansion name>` - The expansion to get info from (Needs to be registered and active).

**Example**:  
```
/papi info Vault
```

----
#### `/papi list`
**Description**:  
Lists all active/registered expansions.  
This is different to [/papi ecloud list installed](#papi-ecloud) in the fact, that it also includes expansions that were installed through a plugin (That aren't a separate jar-file) and it also doesn't show which one have updates available.

----
#### `/papi parse`
**Description**:  
Parses the placeholders in a given text and shows the result.

**Argument(s)**:
* `<player|me>` - The Player to parse values of the placeholder (Use `me` for yourself).
* `<Text with placeholders>` - The text to parse.

**Example**:  
```
/papi parse funnycube My group is %vault_group%
```

----
#### `/papi parserel`
**Description**:  
Parses a relational placeholder.

**Arguments**:  
* `<player1>` - The first player.
* `<player2>` - the second player to compare with.
* `<%placeholder%>` - The actual placeholder to parse.

**Example**:  
```
/papi parserel funnycube extended_clip %placeholder%
```

----
#### `/papi register`
**Description**:  
Registers an expansion from a specified filename.  
This is useful in cases, where you downloaded the expansion manually and don't want to restart the server.  
The file needs to be inside `/plugins/PlaceholderAPI/expansions`.

**Arguments**:
* `<filename>` - The file to register (including the file-extension).

**Example**:  
```
/papi register MyExpansion.jar
```

----
#### `/papi reload`
**Description**:  
Reloads the config settings.

----
#### `/papi unregister`
**Description**:  
Unregisters the specified expansion.

**Arguments**:
* `<filename>` - The expansion to unregister.

**Example**:  
```
/papi unregister MyExpansion.jar
```