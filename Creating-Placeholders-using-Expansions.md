## Overview
PlaceholderAPI is using Expansions for it's placeholders, with PlaceholderAPI providing the core. Users can download Expansions from the cloud server using commands in game or [here](https://api.extendedclip.com/all/) if they want to use the Placeholder.

**Note:** It is recommended to create placeholders using expansions for your Plugin instead of creating it inside your Plugin as people can contribute to your placeholders if you upload it onto sites like Github.


To begin, start a new Project using your favorite IDE and create a class that `extends PlaceholderExpansion`.
After implementing the methods, you are ready to work on your Placeholders.

## Examples
### Creating Placeholders without any third party plugin.
This is on how you would do it, when your expansion doesn't depend on any seperate plugin.

Examples of such expansions are:
- [Player expansion](/PlaceholderAPI/Player-Expansion)
- [Math expansion](/PlaceholderAPI/Math-Expansion)
- [Statistics expansion](/PlaceholderAPI/Statistics-Expansion)

```java
package at.helpch.placeholderapi.expansions;
import org.bukkit.OfflinePlayer;

import me.clip.placeholderapi.expansion.PlaceholderExpansion;

/**
* This class will automatically register as a placeholder expansion
* when a jar including this class is added to the directory
* {@code /plugins/PlaceholderAPI/expansions} on your server.
*/
public class ExampleExpansion extends PlaceholderExpansion {

  /**
   * This method should always return true unless we
   * have a dependency we need to make sure is on the server
   * for our placeholders to work!
   *
   * @return always true since we do not have any dependencies.
   */
  @Override
  public boolean canRegister(){
    return true;
  }

  /**
   * The name of the person who created this expansion should go here.
   * 
   * @return The name of the author as a String.
   */
  @Override
  public String getAuthor(){
    return "someauthor";
  }

  /**
   * The placeholder identifier should go here.
   * This is what tells PlaceholderAPI to call our onRequest method to obtain
   * a value if a placeholder starts with our identifier.
   * This must be unique and can not contain % or _
   *
   * @return The identifier in {@code %<identifier>_<value>%} as String.
   */
  @Override
  public String getIdentifier(){
    return "example";
  }

  /**
   * if an expansion requires another plugin as a dependency, the proper name of the 
   * dependency should go here.
   * <br>Set this to {@code null} if your placeholders do not require another plugin 
   * be installed on the server for them to work.
   *
   * @return Always {@code null} since we do not have a dependency.
   */
  @Override
  public String getPlugin(){
    return null;
  }

  /**
   * This is the version of this expansion.
   * <br>You don't have to use numbers, since it is set as a String.
   *
   * @return The version as a String.
   */
  @Override
  public String getVersion(){
    return "1.0.0";
  }

  /**
   * This is the method called when a placeholder with our identifier is found and 
   * needs a value.
   * <br>We specify the value identifier in this method.
   * <br>Since version 2.9.1 can you use OfflinePlayers in your requests.
   *
   * @param  player
   *         A {@link org.bukkit.OfflinePlayer OfflinePlayer}.
   * @param  identifier
   *         A String containing the identifier/value.
   *
   * @return possibly-null String of the requested identifier.
   */
  @Override
  public String onRequest(OfflinePlayer player, String identifier){

    // %example_placeholder1%
    if(identifier.equals("placeholder1")){
      return "placeholder1 works";
    }

    // %example_placeholder2%
    if(identifier.equals("placeholder2")){
      return "placeholder2 works";
    }

    // We return null if an invalid placeholder (f.e. %example_placeholder3%) 
    // was provided
    return null;
  }
}
```
----

### Creating placeholders with 3rd party plugins
This example here applies to people who want to provide information from their own plugin through placeholders from PlaceholderAPI.  
In our example do we have the plugin `SomePlugin` and want to show certain placeholders with it.

```java
package at.helpch.placeholderapi.expansions;
import org.bukkit.OfflinePlayer;

import me.clip.placeholderapi.expansion.PlaceholderExpansion;
import at.helpch.somePlugin.SomePlugin;

/**
* This class will automatically register as a placeholder expansion
* when a jar including this class is added to the directory
* {@code /plugins/PlaceholderAPI/expansions} on your server.
*/
public class ExampleExpansion extends PlaceholderExpansion {

  // We get an instance of the plugin later.
  private SomePlugin plugin;

  /**
   * Since this expansion requires api access to the plugin "SomePlugin" 
   * we must check if said plugin is on the server or not.
   *
   * @return true or false depending on if the required plugin is installed.
   */
  @Override
  public boolean canRegister(){
    return Bukkit.getPluginManager().getPlugin(getPlugin()) != null;
  }

  /**
   * We can optionally override this method if we need to initialize variables 
   * within this class if we need to or even if we have to do other checks to 
   * ensure the hook is properly setup.
   *
   * @return true or false depending on if it can register.
   */
  @Override
  public boolean register(){

    // Make sure "SomePlugin" is on the server
    if(!canRegister()){
      return false;
    }
 
    /*
     * "SomePlugin" does not have static methods to access its api so we must 
     * create a variable to obtain access to it.
     */
    plugin = (SomePlugin) Bukkit.getPluginManager().getPlugin(getPlugin());

    // if for some reason we can not get our variable, we should return false.
    if(plugin == null){
      return false;
    }

    /*
     * Since we override the register method, we need to manually
     * register this hook
     */
    return PlaceholderAPI.registerPlaceholderHook(getIdentifier(), this);
  }

  /**
   * The name of the person who created this expansion should go here.
   * 
   * @return The name of the author as a String.
   */
  @Override
  public String getAuthor(){
    return "someauthor";
  }

  /**
   * The placeholder identifier should go here.
   * This is what tells PlaceholderAPI to call our onRequest method to obtain
   * a value if a placeholder starts with our identifier.
   * This must be unique and can not contain % or _
   *
   * @return The identifier in {@code %<identifier>_<value>%} as String.
   */
  @Override
  public String getIdentifier(){
    return "someplugin";
  }

  /**
   * If an expansion requires another plugin as a dependency, the proper name of the 
   * dependency should go here.
   * <br>Set this to {@code null} if your placeholders do not require another plugin 
   * be installed on the server for them to work.
   * <br>
   * <br>This is extremely important to set your plugin here, since if you don't do 
   * it, your expansion will throw errors.
   *
   * @return The name of our dependency.
   */
  @Override
  public String getPlugin(){
    return "SomePlugin";
  }

  /**
   * This is the version of this expansion.
   * <br>You don't have to use numbers, since it is set as a String.
   *
   * @return The version as a String.
   */
  @Override
  public String getVersion(){
    return "1.0.0";
  }

  /**
   * This is the method called when a placeholder with our identifier is found and 
   * needs a value.
   * <br>We specify the value identifier in this method.
   * <br>Since version 2.9.1 can you use OfflinePlayers in your requests.
   * <br>This is not the case for us, since we need an online player.
   *
   * @param  player
   *         A {@link org.bukkit.Player Player}.
   * @param  identifier
   *         A String containing the identifier/value.
   *
   * @return possibly-null String of the requested identifier.
   */
  @Override
  public String onPlaceholderRequest(Player player, String identifier){

    if(p == null){
      return "";
    }

    // %someplugin_placeholder1%
    if(identifier.equals("placeholder1")){
      return plugin.getConfig().getString("placeholder1", "value doesnt exist");
    }

    // %someplugin_placeholder2%
    if(identifier.equals("placeholder2")){
      return plugin.getConfig().getString("placeholder2", "value doesnt exist");
    }
 
    // We return null if an invalid placeholder (f.e. %someplugin_placeholder3%) 
    // was provided
    return null;
  }
}
```