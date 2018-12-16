# MemoryCache
MemoryCache is a plugin api that allows other plugins to store data in-memory and retrieve it later.
Plugins have optional features to expire items after a specific amount of time.

## CacheItemOptions
Options used to modify the behavior of the cached object
```csharp
// Gets or sets an absolute expiration date for the cache entry.
public System.DateTimeOffset? AbsoluteExpiration { get; set; }

// Gets or sets an absolute expiration time, relative to now.
public System.TimeSpan? AbsoluteExpirationRelativeToNow { get; set; }

// A callback that is fired when the cached entry is being expired
public System.Action<System.Object> ExpirationCallback { get; set; }

// Gets or sets how long a cache entry can be inactive (e.g. not accessed) before it will be removed.
// This will not extend the entry lifetime beyond the absolute expiration (if set).
public System.Timespan? SlidingExpiration { get; set; }

// The plugin responsible for this cached item. Used to determine if the entry needs to be removed when a plugin is unloaded (BROKEN)
public Oxide.Core.Plugins.Plugin Plugin { get; set; }
```


## Requiring Plugin
Not required but allows you to use the CacheItemOptions class helper

```csharp
// Require: MemoryCache
using System;
using Oxide.Core.Plugins;

namespace Oxide.Plugins
{
  [Info("My Plugin", "austinv90", "1.0.0")]
  internal class MyPlugin : CovalencePlugin
  {
    [PluginReference]MemoryCache MemoryCache;
    
    private void Init()
    {
      var options = new MemoryCache.CacheItemOptions()
      {
        Plugin = this,
        SlidingExpiration = TimeSpan.FromMinutes(30)
      }
      
      MemoryCache.Add("MyPlugin_Key0", "I'm a cached string", options);
    }
    
    private void HelloTest()
    {
      // Output: "I'm a cached string"
      Puts(MemoryCache.Get<string>("MyPlugin_Key0"));
    }
  }
}
```

## API

```csharp
private bool Add(string key, object item, Action<object> expireCallback, DateTimeOffset? absoluteExpire, TimeSpan? slidingExpire, Plugin plugin);

private bool Add(string key, object item, Action<object> expireCallback, DateTimeOffset? absoluteExpire, TimeSpan? slidingExpire);

private bool Add(string key, object item, Action<object> expireCallback, TimeSpan? slidingExpire, Plugin plugin);

private bool Add(string key, object item, Action<object> expireCallback, TimeSpan? slidingExpire);

private bool Add(string key, object item, Action<object> expireCallback, DateTimeOffset? absoluteExpire, Plugin plugin);

private bool Add(string key, object item, Action<object> expireCallback, DateTimeOffset? absoluteExpire);

private bool Add(string key, object item, TimeSpan? slidingExpire, Plugin plugin);

private bool Add(string key, object item, TimeSpan? slidingExpire);

private bool Add(string key, object item, DateTimeOffset? absoluteExpire, Plugin plugin);

private bool Add(string key, object item, DateTimeOffset? absoluteExpire);

private bool Add(string key, object item, Plugin plugin);

private bool Add(string key, object item);

private object Get(string key);

private bool Remove(string key);

private void Remove(Plugin plugin);

```
