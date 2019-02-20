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
You have to import PlaceholderAPI manually, if you don't use maven.  
The following example shows the manual import in Eclipse:

Rightclick your project and choose "Properties -> Java Build Path".  
Click on "Add external jar" and add the PlaceholderAPI.jar ([Pluginpage](https://www.spigotmc.org/resources/placeholderapi.6245/)) to your project.  
![](https://img.extendedclip.com/02-23-16-11:50:21.png)

If you use IntelliJ, do the following steps:  
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
**Note**: If you are not including placeholders from within the dependency, you probably want to create a placeholder expansion. A guide on how to create placeholder expansions can be found [[here|Creating-Placeholders-using-Expansions]]!  
**Note 2**: The later mentioned `EZPlaceholderHook` is deprecated since the implementation of PlaceholderExpansion. If you don't want to make a seperate jar for registering placeholders, [[click here|Hook-into-PlaceholderAPI#using-placeholder-expansion-to-register-placeholders-in-your-local-jar]] for how to use another method.

This method lets you register your plugins placeholders in PlaceholderAPI, which then can be used by and other plugin that supports and use PlaceholderAPI-placeholders.

In our example, we set PlaceholderAPI as a softdepend and won't register our placeholders, if the plugin isn't installed and enabled:
```java
package at.helpch.placeholderapiexample;

import org.bukkit.Bukkit;
import org.bukkit.plugin.java.JavaPlugin;

public class ExamplePlugin extends JavaPlugin {
    
    @Override
    public void onEnable(){
        if(Bukkit.getPluginManager().getPlugin("PlaceholderAPI") != null) {
            // Your register-code here
        }
    }
}
```

There are multiple ways to register your own placeholders. The best (and also cleanest) way would be to make a seperate class that extend the abstract `PlaceholderHook` or `EZPlaceholderHook` to handle it.

**Note**: These two abstract methods are basicly the same, but `EZPlaceholderHook` makes it easier for you to register your placeholders, because it has a few methods built in to do the work in terms of registering.

Lets assume I made the class `MyPlaceholderExtension` wich extends `EZPlaceholderHook`.  
As soon as I did that, does it make a constructor for me and tells me, that I need to implement some methods that are missing.  
This method will do the work for us, when a value is needed.

In our example will we keep track of who's staff and how many staff are online.  
In the main class did we made some eventlisteners, to determine if a player is staff, when he joins. If they are, add them to a set of player names of the staff online. On quit we do the oposite and remove them from the set.  
```java
package at.helpch.placeholderapiexample;

import org.bukkit.Bukkit;
import org.bukkit.plugin.java.JavaPlugin;

import java.util.HashSet;
import java.util.Set;

public class ExamplePlugin extends JavaPlugin implements Listener {
    
    // We make a Set, that stores the player names as String
    private final Set<String> staff = new HashSet<>();
    
    @Override
    public void onEnable() {
        /*
         * We register the EventListeneres here.
         * Since all events are in the main class (this class), we simply use "this"
         */
        Bukkit.getPluginManager().registerEvents(this, this);

        if(Bukkit.getPluginManager().getPlugin("PlaceholderAPI") != null) {
            // Your register-code here
        }
    }

    @EventListener
    public void onJoin(PlayerJoinEvent event) {
        if(event.getPlayer().hasPermission("exampleplugin.staff");
            addStaff(event.getPlayer().getName());
        }
    }
    
    @EventHandler
    public void onQuit(PlayerQuitEvent event) {
        if(isStaff(event.getPlayer().getName()) {
            removeStaff(event.getPlayer().getName());
        }
    }
    
    public boolean isStaff(String name) {
        return staff.contains(name);
    }
    
    public boolean addStaff(String name) {
        return staff.add(name);
    }

    public boolean removeStaff(String name) {
        return staff.remove(name);
    }

    // We use this one here in the later example
    public int getStaffCount() {
        return staff.size();
    }
}
```

Now we can setup the stuff in our placeholder-class to make it work!  
```java
package at.helpch.placeholderapiexample;

import org.bukkit.entity.Player;
import me.clip.placeholderapi.external.EZPlaceholderHook;

public class MyPlaceholderExtension extends EZPlaceholderHook {
    
    // Getting our main class with the stuff here
    private ExamplePlugin myPlugin;
    
    public MyPlaceholderExtension(ExamplePlugin myPlugin) {
        /*
         * We register and associate our plugin with an identifier.
         * The format for placeholders is this:
         * %<identifier>_<anything you define below>%
         * The placeholder identifier can be anything you want, as long as it isn't already registered by
         * another plugin.
         * It also can't contain % or _
         */
        super(myPlugin, "exampleplugin");
        
        // This is to access our main class below
        this.myPlugin = myPlugin;
    }
    
    @Override
    public String onPlaceholderRequest(Player player, String identifier) {
        // Placeholder: %exampleplugin_staff_count%
        if(identifier.equals("staff_count")) {
            // Since we return a String, we have to convert it from the integer
            return String.value(myPlugin.getStaffCount());
        }
        
        // We always check, if player is null for player-related placeholders.
        if(player == null) {
            return "";
        }
        
        // Placeholder: %exampleplugin_is_staff%
        if(identifier.equals("is_staff")) {
            // Syntax: return <check-value> ? <true> : <false>;
            return myPlugin.isStaff(player.getName()) ? "yes" : "no";
        }
        
        // We return null if any other identifier was provided
        return null;
    }
}
```

And that's it! Now we only have to create an instance of the class we made and call the `hook` method it provides.  
```java
package at.helpch.placeholderapiexample;

import org.bukkit.Bukkit;
import org.bukkit.plugin.java.JavaPlugin;

import java.util.HashSet;
import java.util.Set;

public class ExamplePlugin extends JavaPlugin {
    
    // We make a Set, that stores the player names as String
    private final Set<String> staff = new HashSet<>();
    
    @Override
    public void onEnable() {
        /*
         * We register the EventListeneres here.
         * Since all events are in the main class (this class), we simply use "this"
         */
        Bukkit.getPluginManager().registerEvents(this, this);

        if(Bukkit.getPluginManager().getPlugin("PlaceholderAPI") != null) {
            new MyPlaceholderExtension(this).hook();
        }
    }
    
    // All the stuff we did previously.
```

## Using placeholders from PlaceholderAPI in your plugin
To use placeholders from other plugins in our own plugin, we simply have to use the `setPlaceholders` method.  
Lets assume we want to send a own join message that shows the group a player has.  
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
        String withoutPlaceholdersSet = "%player_name% &ajoined the server! He/she is rank &f%vault_rank%";

        // We parse the placeholders using "setPlaceholders"
        String withPlaceholdersSet = PlaceholderAPI.setPlaceholders(event.getPlayer(), withoutPlaceholdersSet);

        event.setJoinMessage(withPlaceholdersSet);
    }
}
```

## Using Placeholder Expansion to register placeholders in your local jar
We first create the main class like normal:  
```java
package at.helpch.placeholderapiexample;

import org.bukkit.Bukkit;
import org.bukkit.plugin.java.JavaPlugin;

public class ExamplePlugin extends JavaPlugin {
    
    @Override
    public void onEnable(){
        if(Bukkit.getPluginManager().getPlugin("PlaceholderAPI") != null) {
            // Your register-code here
        }
    }
}
```

Now to create the expansion you simply have to create a class, that extends the `PlaceholderExpansion`.  
For more info, [[click here|Creating-Placeholders-using-Expansions]].  
```java
package at.helpch;

import me.clip.placeholderapi.expansion.PlaceholderExpansion;
import org.bukkit.Bukkit;
import org.bukkit.entity.Player;

public class ExampleExpansion extends PlaceholderExpansion {

    /*
     * The identifier, shouldn't contain any _ or %
     */
    public String getIdentifier() {
        return "exampleplugin";
    }

    public String getPlugin() {
        return null;
    }

    /*
     * The author of the Placeholder
     * This cannot be null
     */
    public String getAuthor() {
        return "HelpChat";
    }

    /*
     * Same with #getAuthor() but for version
     * This cannot be null
     */
    public String getVersion() {
        return "OurOwnVersion";
    }

    /*
     * Use this method to setup placeholders
     * This is somewhat similar to EZPlaceholderhook
     */
    public String onRequest(Player player, String identifier) {
        // Placeholder: %exampleplugin_online_players%
        if(identifier.equalsIgnoreCase("online_players")){
            return String.valueOf(Bukkit.getOnlinePlayers().size());
        }
  
        // We check if the player is null (not online) before any player-related placeholder
        if(player == null){
            return "";
        }
  
        // Placeholder: %exampleplugin_player_name%
        if(identifier.equalsIgnoreCase("player_name")){
            return player.getName();
        }
        
        // We return null, if an invalid placeholder was called.
        return null;
    }
}
```

After that is done, we simply register our PlaceholderExpansion-class:  
```java
package at.helpch.placeholderapiexample;

import org.bukkit.Bukkit;
import org.bukkit.plugin.java.JavaPlugin;

public class ExamplePlugin extends JavaPlugin {
    
    @Override
    public void onEnable(){
        if(Bukkit.getPluginManager().getPlugin("PlaceholderAPI") != null) {
            new ExampleExpansion().register();
        }
    }
}
```