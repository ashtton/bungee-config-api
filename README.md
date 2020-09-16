# BungeeConfigAPI

This is a Bungee Configuration API made by me, it's extremely useful & easy to use.


**How to use**\
Copy and paste the code at the bottom of the page, into a new class called something like 'Config'. Then in your main bungee class that extends Plugin, create a variable like this above your onEnable();
```java
private Config motdConfig;
```
In your onEnable() initialize the config by doing
```java
motdConfig = new Config("motd.yml", this);
```
After adding those methods into your code, you need to make the actual 'motd.yml' or whatever your confiuration is called. You can then access the configuration file, and get coding using configurations in Bungee.

**Actual code - put in Config.class**
```java
import net.md_5.bungee.api.plugin.*;
import com.qbasic.project.shade.com.google.common.io.*;
import net.md_5.bungee.config.*;
import java.io.*;

public class Config {
    private Configuration config;
    private File file;
    
    public Config(String name, Plugin plugin) {
        this.file = new File(plugin.getDataFolder(), name);
        if (!plugin.getDataFolder().exists()) plugin.getDataFolder().mkdir();
    
        if (!this.file.exists()) {
            try {
                this.file.createNewFile();
                try (final InputStream is = plugin.getResourceAsStream(name);
                     final OutputStream os = new FileOutputStream(this.file)) {
                    ByteStreams.copy(is, os);
                }
            }
            catch (Exception e) { e.printStackTrace(); }
        }
        
        try { this.config = ConfigurationProvider.getProvider((Class)YamlConfiguration.class).load(this.file); }
        catch (Exception e) { e.printStackTrace(); }
    }
    
    public void save() {
        try { ConfigurationProvider.getProvider((Class)YamlConfiguration.class).save(this.config, this.file); }
        catch (Exception e) { e.printStackTrace(); }
    }
    
    public Configuration getConfig() { return this.config; }
}
```
