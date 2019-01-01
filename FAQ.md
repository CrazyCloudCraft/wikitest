Here are frequently asked questions about stuff related to PlaceholderAPI.

## It only shows %placeholder% and not the variable
Make sure, that you've tried the following steps:
- PlaceholderAPI is installed and running
- You downloaded the expansion with `/papi ecloud download [expansion]`  
A list of available expansions and their placeholders can be found [[here|Placeholders]]!
- You reloaded PlaceholderAPI with `/papi reload` after downloading an expansion.
- Any possible dependency for the expansion (Plugin) is installed and running.

## I can't download the expansion
Make sure, that the connection to the ecloud (https://api.extendedclip.com/all) isn't blocked by a firewall or similar.  
Next step would be to check, if the expansion actually exists on the ecloud. Not all plugins provide their placeholders through a seperate jar on the ecloud. Some have them build in.  
If after the both checks it still doesn't work, go to the ecloud-page and download the jar manually. Put it then in the `expansions` folder of PlaceholderAPI (`/plugins/PlaceholderAPI/expansions`)

## How can other plugins use my placeholders with PlaceholderAPI?
A tutorial can be found [[here|Hook into PlaceholderAPI]]!