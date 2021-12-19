# Datapack Uninstallation (`*`)

## About

This convention ensures that you have a way of uninstalling *most* of the contents of your datapack. The word "most" is used here because Minecraft does not provide the necessary tools for us to actually uninstall or remove the entirety of the datapack.

## Implementation

There are no rules enforcing how you should implement the uninstallation method, as long as you do implement it. However, the following section outlines one of the many ways to implement it.

### Uninstallation Function

You can create an "uninstallation function", which when ran, will remove most of the content added from your datapack. You can tell the user about this function inside your project page or anywhere you like.

---

### **Additional Installation & Uninstallation Guideline (Optional)**

#### **1. scoreboards, storages, or teams, should be removed.**

> In a situation that you cannot remove them, you should includes the reason in the [request review](https://github.com/mc-datapacks/review-tracker/issues) or mcfunction comment.

#### **2. Only use `load.json`**

The `tick.json` file typically runs before `load.json`, sometimes resulting in an incomplete installation. You can avoid that by making the init file to run before the tick function.

```json
{
    "values": [
        "dp:example/setup",
        "dp:example/main"
    ]
}
```

```mcfunction
#> dp:example/main
schedule function dp:example/main 1t
```

> This command makes this file run every single tick just like using `tick.json`. 
> 
> You can make mcfunction run slower tick cycles as you like. For example
> - Every 8 tick `schedule function <function> 8t` 
> - Every 1 second `schedule function <function> 20t or 1s` 
> - *t = tick*, *s = second*, *d = gameday*.

#### **3. Users should install zip files only.**

For a better user experience, just drag it into the datapack folder. But there are things to be careful.

```mcfunction
#> dp:example/uninstall

# Use for datapacking or debugging by the owner.
datapack disable "file/example"

# Use for uninstalling by the user.
datapack disable "file/example.zip"
```

> - Users are strictly prohibited from renaming files. Otherwise, when uninstalling, this command will fail.

#### **4. Uninstallation with /schedule clear \<function> command is not recommended**
Because these problems cause inconvenience and need to be more careful.

> - If you have many schedule functions, you must complete all schedule clear commands.
> - There must be no reload after all schedule clear until the datapack file is removed.

### **Note**

 **Why is it recommended to use the datapack disable command?**
 - If you use `tick.json` even if you remove your scoreboards, storages, or teams, this file will still be working.
 - If you use `load.json` and `schedule function 1t`, it will be issues in section 4.
 - Automatic reload without `/reload`. This command doesn't work with some server launchers.
 - The `datapack disable` command there is a downside to being careful about zip file names. But this command will completely disable both methods above.