So, you want to show stats or information from your plugin in other plugins that obtain placeholder information from PlaceholderAPI? Here is a guide on how to do it!

**This guide uses API methods and classes only available in PlaceholderAPI 2.0.6 or higher!**

## Adding the plugin depedency

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

If you are not using Maven (or Gradle), you'll have to manually add the plugin's .jar file to your buildpath. This will vary depending on your IDE, so we can't show examples here. There plenty of documentation for all IDEs though, and it should be pretty easy to find information for your IDE of choice

## Adding placeholders from your plugin to PlaceholderAPI

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