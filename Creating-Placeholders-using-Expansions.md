## Overview
PlaceholderAPI is using Expansions for it's placeholder, with PlaceholderAPI providing the core. Users can download Expansions from the cloud server using commands in game or [here](https://api.extendedclip.com/all/) if they want to use the Placeholder.

**Note:** It is recommended to create placeholders using expansions for your Plugin instead of creating it inside your Plugin as people can contribute to your placeholders if you upload it onto sites like Github.


To begin, start a new Project using your favorite IDE and create a class that `extends PlaceholderExpansion`.
After implementing the methods, you are ready to work on your Placeholders.

## Example
* Creating Placeholders without any third party plugin.
```java
/**
* This class will automatically register as a placeholder expansion
* when a jar including this class is added to the /plugins/placeholderapi/expansions/ folder
*
*/
public class ExampleExpansion extends PlaceholderExpansion {

    /**
     * This method should always return true unless we
     * have a dependency we need to make sure is on the server
     * for our placeholders to work!
     * This expansion does not require a dependency so we will always return true
     */
    @Override
    public boolean canRegister() {
        return true;
    }

    /**
     * The name of the person who created this expansion should go here
     */
    @Override
    public String getAuthor() {
        return "someaothr";
    }

    /**
     * The placeholder identifier should go here
     * This is what tells PlaceholderAPI to call our onPlaceholderRequest method to obtain
     * a value if a placeholder starts with our identifier.
     * This must be unique and can not contain % or _
     */
    @Override
    public String getIdentifier() {
        return "example";
    }

    /**
     * if an expansion requires another plugin as a dependency, the proper name of the dependency should
     * go here. Set this to null if your placeholders do not require another plugin be installed on the server
     * for them to work
     */
    @Override
    public String getPlugin() {
        return null;
    }

    /**
     * This is the version of this expansion
     */
    @Override
    public String getVersion() {
        return "1.0.0";
    }

    /**
     * This is the method called when a placeholder with our identifier is found and needs a value
     * We specify the value identifier in this method
     */
    @Override
    public String onPlaceholderRequest(Player p, String identifier) {

        // %example_placeholder1%
        if (identifier.equals("placeholder1")) {
            return "placeholder1 works";
        }
        // %example_placeholder2%
        if (identifier.equals("placeholder2")) {
            return "placeholder2 works";
        }

        return null;
    }
}
```
* Creating placeholders with 3rd party plugins
```java
/**
* This class will automatically register as a placeholder expansion
* when a jar including this class is added to the /plugins/placeholderapi/expansions/ folder
*
*/
public class ExampleExpansion extends PlaceholderExpansion {

    private SomePlugin plugin;
    /**
     * Since this expansion requires api access to the plugin "SomePlugin"
     * we must check if "SomePlugin" is on the server in this method
     */
    @Override
    public boolean canRegister() {
        return Bukkit.getPluginManager().getPlugin(getPlugin()) != null;
    }

    /**
     * We can optionally override this method if we need to initialize variables within this class if we need to
     * or even if we have to do other checks to ensure the hook is properly setup.
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
         * "SomePlugin" does not have static methods to access its api so we must
         * create set a variable to obtain access to it
         */
        plugin = (SomePlugin) Bukkit.getPluginManager().getPlugin(getPlugin());
 
        /*
         * if for some reason we can not get our variable, we should return false
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

    /**
     * The name of the person who created this expansion should go here
     */
    @Override
    public String getAuthor() {
        return "someAuthor";
    }

    /**
     * The placeholder identifier should go here
     * This is what tells PlaceholderAPI to call our onPlaceholderRequest method to obtain
     * a value if a placeholder starts with our identifier.
     * This must be unique and can not contain % or _
     */
    @Override
    public String getIdentifier() {
        return "someplugin";
    }

    /**
     * if an expansion requires another plugin as a dependency, the proper name of the dependency should
     * go here. Set this to null if your placeholders do not require another plugin be installed on the server
     * for them to work. This is extremely important to set if you do have a dependency because
     * if your dependency is not loaded when this hook is registered, it will be added to a cache to be
     * registered when plugin: "getPlugin()" is enabled on the server.
     */
    @Override
    public String getPlugin() {
        return "SomePlugin";
    }

    /**
     * This is the version of this expansion
     */
    @Override
    public String getVersion() {
        return "1.0.0";
    }

    /**
     * This is the method called when a placeholder with our identifier is found and needs a value
     * We specify the value identifier in this method
     */
    @Override
    public String onPlaceholderRequest(Player p, String identifier) {

        if (p == null) {
            return "";
        }
        // %example_placeholder1%
        if (identifier.equals("placeholder1")) {
            return plugin.getConfig().getString("placeholder1", "value doesnt exist");
        }
        // %example_placeholder2%
        if (identifier.equals("placeholder2")) {
            return plugin.getConfig().getString("placeholder2", "value doesnt exist");
        }
 
        return null;
    }
}
```