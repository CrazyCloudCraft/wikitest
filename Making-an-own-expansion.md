![](https://i.imgur.com/Elxkfb8.png)

## About
PlaceholderAPI utilizes a .jar based module system which allows placeholders to be packaged in a .jar file and loaded/enabled dynamicly when the .jar for a specific set of placeholders is located within the `/PlaceholderAPI/expansions/` folder.  
This system offers many benefits, including, but not limited to:
- The core plugin of PlaceholderAPI is not dependent on any external resources aside from the Spigot API
- The end user can choose which expansions are loaded / enabled, reducing memory usage and/or bloat​
- If any placeholders break, the entire plugin does not break or require an update​
- It is easier for maintainability.  
Most placeholders depend on an external 3rd-party plugin to obtain data from. Sometimes those plugins update and cause the expansion or a placeholder to break.  
The developer of the expansion can update the single expansion to support the new plugin version.​
- It is easier for a developer who wants to add new placeholders to contribute​
- Updates be deployed and accessed in real time without restarting the server.​

Do you have your own expansion, that you want to share with all the PlaceholderAPI-users?  
[[Click here|Expansion-cloud]] to get info on how to do that.

## Creating an expansion WITHOUT extra dependencies
Creating a expansion, that doesn't require any 3rd-party dependencies is as easy, as making a new project... Which is actually the case.  
Simply create a new project, add the Spigot-API and PlaceholderAPI as a dependency ([[how-to|Hook-into-PlaceholderAPI#first-steps]]) and make a single class, that extends the PlaceholderExpansion.  
When you compile this class as a jar and add it to the `/PlaceholderAPI/expansions/` folder, it will be loaded when PlaceholderAPI handles registering all of the expansions if finds within that folder on startup.

Here's an example-expansion, that doesn't require any dependencies:  
```java
package at.helpchat.placeholderapi.example;

import me.clip.placeholderapi.expansion.PlaceholderExpansion;

import org.bukkit.entity.Player;

/*
 * This class will automatically register as a placeholder expansion
 * when a jar including this class is added to the
 * /plugins/PlaceholderAPI/expansions/ folder
 */
public class ExampleExpansion extends PlaceholderExpansion {

  /*
   * This method should always return true unless we
   * have a dependency we need to make sure is on the server
   * for our placeholders to work!
   * This expansion does not require a dependency so we will always
   * return true
   */
  @Override
  public boolean canRegister() {
    return true;
  }

  /*
   * The name of the person who created this expansion should go here
   */
  @Override
  public String getAuthor() {
    return "HelpChat";
  }

  /*
   * The placeholder identifier should go here
   * This is what tells PlaceholderAPI to call our onRequest
   * method to obtain a value if a placeholder starts with
   * our identifier.
   * This must be unique and can not contain % or _
   */
  @Override
  public String getIdentifier() {
    return "example";
  }

  /*
   * if an expansion requires another plugin as a dependency, the proper
   * name of the dependency should go here.
   * Set this to null if your placeholders do not require another plugin
   * to be installed on the server for them to work.
   */
  @Override
  public String getPlugin() {
    return null;
  }

  /*
   * This is the version of this expansion
   * It doesn't need to be a number tho
   */
  @Override
  public String getVersion() {
    return "1.0.0";
  }

  /*
   * This is the method called when a placeholder with our
   * identifier is found and needs a value.
   * We specify the value identifier in this method
   */
  public String onRequest(Player p, String identifier) {

    // %example_placeholder1%
    if(identifier.equals("placeholder1")) {
      return "placeholder1 works";
    }

    // %example_placeholder2%
    if(identifier.equals("placeholder2")) {
      return "placeholder2 works";
    }

    // If an invalid placeholder was called, we return null.
    return null;
  }
}
```

## Creating an expansion WITH extra dependencies
This is kinda the same like above, with the difference, that we also add our 3rd-party dependency to the dependencies-list.  
This expansion will load, when the class is setup, to require a dependency and this dependency is found on the server.

Example of such an expansion-class (Note how similar it is to the previous one):
```java
package at.helpchat.placeholderapi.example;

import me.clip.placeholderapi.PlaceholderAPI;
import me.clip.placeholderapi.expansion.PlaceholderExpansion;

import at.helpch.someplugin.SomePlugin;

import org.bukkit.Bukkit;
import org.bukkit.entity.Player;

/*
 * This class will automatically register as a placeholder expansion
 * when a jar including this class is added to the
 * /plugins/PlaceholderAPI/expansions/ folder
 */
public class ExampleExpansion extends PlaceholderExpansion{

  private SomePlugin plugin;
  /*
   * Since this expansion requires access to the API of "SomePlugin"
   * we must check if "SomePlugin" is on server in this method
   */
  @Override
  public boolean canRegister() {
      return Bukkit.getPluginManager().getPlugin(getPlugin()) != null;
  }
  /*
   * We can optionally override this method if we need to initialize
   * variables within this class if we need to or even if we have to
   * do other checks to ensure the hook is properly setup.
   */
  @Override
  public boolean register() {
    /*
     * Make sure "SomePlugin" is on the server
     */
    if (!canRegister()) {
      return false;
    }
 
    /*
     * "SomePlugin" does not have static methods to access
     * its api so we must create set a variable to obtain access to it
     */
    plugin = (SomePlugin) Bukkit.getPluginManager().getPlugin(getPlugin());
 
    /*
     * If for some reason the plugin isn't active (null), we return false
     */
    if (plugin == null) {
      return false;
    }
    /*
     * Since we override the register method, we need to manually
     * register this hook
     */
    return PlaceholderAPI.registerPlaceholderHook(getIdentifier(), this);
  }

  /*
   * The name of the person who created this expansion should go here
   */
  @Override
  public String getAuthor() {
    return "HelpChat";
  }

  /*
   * The placeholder identifier should go here
   * This is what tells PlaceholderAPI to call our onRequest
   * method to obtain a value if a placeholder starts with
   * our identifier.
   * This must be unique and can not contain % or _
   */
  @Override
  public String getIdentifier() {
    return "example";
  }

  /*
   * If an expansion requires another plugin as a dependency, the proper
   * name of the dependency should go here.
   * Set this to null if your placeholders do not require another plugin
   * to be installed on the server for them to work.
   *
   * This is extremely important to set if you do have a dependency because
   * if your dependency is not loaded when this hook is registered, it will
   * be added to a cache to be registered when plugin: "getPlugin()" is
   * enabled on the server and that will cause issues.
   */
  @Override
  public String getPlugin() {
    return "SomePlugin";
  }

  /*
   * This is the version of this expansion
   * It doesn't need to be a number tho
   */
  @Override
  public String getVersion() {
    return "1.0.0";
  }

  /*
   * This is the method called when a placeholder with our
   * identifier is found and needs a value
   * We specify the value identifier in this method
   */
  public String onRequest(Player p, String identifier) {

    if(p == null) {
      return "";
    }

    // %example_placeholder1%
    if(identifier.equals("placeholder1")) {
      return plugin.getConfig().getString("placeholder1", "value doesnt exist");
    }
    // %example_placeholder2%
    if(identifier.equals("placeholder2")) {
      return plugin.getConfig().getString("placeholder2", "value doesnt exist");
    }

    // If an invalid placeholder was called, we return null.
    return null;
  }
}
```

Those are just the key things that you need.  
You can do a lot more of stuff. Go to the [PlaceholderAPI-GitHub](https://github.com/PlaceholderAPI) to see some expansions, that are used.