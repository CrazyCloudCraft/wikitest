[version]: https://img.shields.io/nexus/r/http/repo.extendedclip.com/me.clip/placeholderapi.svg?label=API-Version
[Spigot page]: https://spigotmc.org/resources/6245
[GitHub release]: /PlaceholderAPI/PlaceholderAPI/releases/latest

> ![version]  
> **[Spigot page]** | **[GitHub Release]***
>
> *The GitHub release may be different from the spigot release

This page is about using PlaceholderAPI in your own plugin, to either let other plugins use your plugin, or just use placeholders from other plugins in your own.

Please note, that the examples in this page are only available for **PlaceholderAPI 2.0.6 or higher**!

## First steps
Before you can actually make use of PlaceholderAPI, you first have to import it into your project.

### Import with Maven
To import PlaceholderAPI, simply add the following code to your **pom.xml**  
Replace `{VERSION}` with the version listed at the top of this page.  
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
          <version>{VERSION}</version>
         <scope>provided</scope>
        </dependency>
    </dependencies>
```

### Import with Gradle
Here is how you can import PlaceholderAPI through gradle.  
Put this into your **Gradle.build**.  
Replace `{VERSION}` with the version listed at the top of this page.  
```gradle
repositories {
    maven {
        name = 'placeholderapi'
        url = 'http://repo.extendedclip.com/content/repositories/placeholderapi/'
    }
}

dependencies {
    compile group: 'me.clip', project: 'placeholderapi', version: '{VERSION}'
}
```

### Import manually
Do the following steps, if you can't import PlaceholderAPI through Maven or Gradle.

#### Eclipse
Rightclick your project and choose "Properties -> Java Build Path".  
Click on "Add external jar" and add the PlaceholderAPI.jar ([Pluginpage](https://www.spigotmc.org/resources/placeholderapi.6245/)) to your project.  
![](https://img.extendedclip.com/02-23-16-11:50:21.png)

#### IntelliJ  
1. Click on **File** -> **Project Structure** (Ctrl + Alt + Shift + S on windows)
2. Go to the tab **Modules**
3. Click the **+** on the right side and choose **Library...** -> **Java**
4. Select the PlaceholderAPI.jar

### Set PlaceholderAPI as (soft)depend
Next step is to go to your plugin.yml and add PlaceholderAPI as a depend or softdepend, depending (no pun intended) on if it is optional or not.

**Example Softdepend**:
```yaml
name: ExamplePlugin
version: 1.0
author: author
main: your.main.path.here

softdepend: [PlaceholderAPI] # This is used, if your plugin works without PlaceholderAPI.
```

**Example Depend**:
```yaml
name: ExamplePlugin
version: 1.0
author: author
main: your.main.path.here

depend: [PlaceholderAPI] # If your plugin requires PlaceholderAPI, to work, use this.
```

## Adding placeholders to PlaceholderAPI

### NOTES
- If you are not including placeholders from within the dependency, you probably want to create a placeholder expansion. A guide on how to create placeholder expansions can be found [[here|PlaceholderExpansion]]!
- The later mentioned `EZPlaceholderHook` is deprecated since the implementation of `PlaceholderExpansion`. Just use the method explained in the above-linked page.

## Using placeholders from PlaceholderAPI in your plugin
To use placeholders from other plugins in our own plugin, we simply have to use the `setPlaceholders` method.

Let's assume we want to send an own join message that shows the group a player has.  
To achieve that, we can do the following:  
```java
package at.helpch.placeholderapi;

import me.clip.placeholderapi.PlaceholderAPI;

import org.bukkit.Bukkit;
import org.bukkit.event.EventHandler;
import org.bukkit.event.EventPriority;
import org.bukkit.event.Listener;
import org.bukkit.event.player.PlayerJoinEvent;
import org.bukkit.plugin.java.JavaPlugin;
import me.clip.placeholderapi.PlaceholderAPI;

public class JoinExample extends JavaPlugin implements Listener {

    @Override
    public void onEnable() {
 
        if (Bukkit.getPluginManager().getPlugin("PlaceholderAPI") != null) {
            /*
             * We register the EventListeneres here, when PlaceholderAPI is installed.
             * Since all events are in the main class (this class), we simply use "this"
             */
            Bukkit.getPluginManager().registerEvents(this, this);
        } else {
            throw new RuntimeException("Could not find PlaceholderAPI!! Plugin can not work without it!");
        }
    }

    @EventHandler(priority = EventPriority.HIGHEST)
    public void onJoin(PlayerJoinEvent event) {
        String joinText = "%player_name% &ajoined the server! He/she is rank &f%vault_rank%";

        // We parse the placeholders using "setPlaceholders"
        joinText = PlaceholderAPI.setPlaceholders(event.getPlayer(), joinText);

        event.setJoinMessage(withPlaceholdersSet);
    }
}
```