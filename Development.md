This page is for developers who wanted to hook Placeholder API in their plugin.


## Adding PlaceholderAPI as a dependency

To use PlaceholderAPI as a dependency, add `[PlaceholderAPI]` to your depends section of `plugin.yml`

```yaml
depends: [PlaceholderAPI]
```

After that you can either Maven or putting PlaceholderAPI in your Build Path

## Maven Repository
Repository is avaliable [here](http://repo.extendedclip.com/content/repositories/placeholderapi/), check the repository frequently for updates
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
            <version>2.8.3</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
```

## Sections
* Creating Placeholders using Expansions
* Creating Placeholders inside your Plugin
* Using PlaceholderAPI for your plugin's placeholders


 