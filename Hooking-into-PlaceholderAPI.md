So, you want to show stats or information from your plugin in other plugins that obtain placeholder information from PlaceholderAPI? Here is a guide on how to do it!

**This guide uses API methods and classes only available in PlaceholderAPI 2.0.6 or higher!**

## Adding the plugin depedency
### Maven
If you are using Maven, it's as simple as adding the dependency information for PlaceholderAPI to your pom.xml, like this:
```xml
<repositories>
    <repository>
        <id>placeholderapi</id>
        <url>http://repo.extendedclip.com/content/repositories/placeholderapi/</url>
    </repository>
</repositories>
<dependencies>
    <dependency>
        <groupId>me.clip</groupId>
        <artifactId>placeholderapi</artifactId>
        <version>LATEST</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```
### Other IDEs not using Maven/Gradle
If you are not using Maven (or Gradle), you'll have to manually add the plugin's .jar file to your buildpath. This will vary depending on your IDE, so we can't show examples here. There plenty of documentation for all IDEs though, and it should be pretty easy to find information for your IDE of choice

## Registering placeholders in your plugin

**If you are not including placeholders from within the dependency, you probably want to create a placeholder expansion. A guide on how to create placeholder expansions can be found [here (to be added)](#adding-placeholders-from-your-plugin-to-placeholderapi)**

We'll show you how to obtain register your own placeholders from your plugin which can be used in any plugin that obtains placeholders from PlaceholderAPI.

***

* Our plugin is going to soft-depend on PlaceholderAPI. If PlaceholderAPI is not found, we will simply ignore adding our custom placeholders.

```java
public class ExamplePlugin extends JavaPlugin {

    @Override
    public void onEnable() {

        if (Bukkit.getPluginManager().isPluginEnabled("PlaceholderAPI"))
            // code to register our placeholders will go here
        }
    }

}
```

* Now we're going to create the expansion in a seperate class which extends PlaceholderExpansion

```java
import me.clip.placeholderapi.expansion.PlaceholderExpansion;
import org.bukkit.Bukkit;
import org.bukkit.entity.Player;

public class TutorialPlaceholder extends PlaceholderExpansion {

    // This is the most important method when registering an expansion from your plugin
    public String persist() {
        // This tells PlaceholderAPI to not unregister this hook when /papi reload is executed
        return true;
    }

    // The identifier, shouldn't contain any _ or %
    public String getIdentifier() {
        return "tutorial";
    }

    public String getPlugin() {
        return null;
    }

    // The author of the placeholder. This cannot be null
    public String getAuthor() {
        return "extendedclip";
    }

    // Same with #getAuthor() but for versioon
    public String getVersion() {
        return "SomeMagicalVersion";
    }

    // Use this method to setup placeholders. This is somewhat similar to EZPlaceholderHook
    public String onPlaceholderRequest (Player player, String identifier) {

        // %tutorial_onlines% - Returns the number of online players
        if (identifier.equalsIgnoreCase("onlines")) {
            return String.valueOf(Bukkit.getOnlinePlayers().size());
        }
   
        // Check if the player is online. You should do this before doing anything regarding players
        if (player == null) {
            return "";
        }
   
        // %tutorial_name% - Returns the player name
        if (identifier.equalsIgnoreCase("name")) {
            return player.getName();
        }

        return null;
    }
}
```

* Finally, we are going to register the expansion!

```java
if (Bukkit.getPluginManager().isPluginEnabled("PlaceholderAPI")) {
    //Registering placeholder will be use here
    new TutorialPlaceholder().register();
}
```