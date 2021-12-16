# Global Ignoring Tag (`***`)

## About

This convention provides a way for datapacks to communicate with each other by specifying entity tags which other datapacks can then look for. This is used heavily to prevent other datapacks from killing/removing custom entities from the world unexpectedly.

There are currently 4 ignoring tags: `global.ignore`, `global.ignore.pos`, `global.ignore.gui` and `global.ignore.kill`.

### 1. `global.ignore.kill`

Any entity with this tag **must not** be killed by other datapacks. This includes, but is not limited to, the `/kill` command.

```mcfunction
execute as @e[type=creeper, tag=!global.ignore, tag=!global.ignore.kill] run kill @s
```

### 2. `global.ignore.gui`

Any entity with this tag **must not** display visual effects around them. This includes, but is not limited to, the `/title` command.

```mcfunction
execute as @a[tag=!global.ignore.gui] at @s run title @s actionbar [{"text": "Hello, World!", "color": "green"}]
```

> This excludes the `/playsound`, `/tellraw` and `/particle` commands.

### 3. `global.ignore.pos`

Any entity with this tag **must not** be moved positionally. This includes, but is not limited to, the `/tp` and`/teleport` commands.

```mcfunction
execute as @e[type=witch, tag=!global.ignore, tag=!global.ignore.pos] at @s run tp @s ~ ~0.1 ~
```

### 4. `global.ignore`

Any entity with this tag **must not** be included in an entity selector at all.

```mcfunction
execute as @e[tag=!global.ignore] at @s run function namespace:internal/logic/function
```

> This tag does not apply to player-only selectors (`@a`, `@e[type=player]`, `@p`, etc.)  

## Note

The convention only applies if your function will affect an unknown entity. If you are trying to target a known entity (e.g. an entity with a special tag attached specific to your datapack), you don't need to follow this convention.

--------------------

### **Additional Implementation Guideline (Optional)**

#### 1. You don't want anyone to kill your entity. But there's no problem if it moves.

```mcfunction
summom zombie ~ ~ ~ {Tags:["dp.example", "global.ignore", "global.ignore.kill"]}
```

> Remove global ignoring tag with `@e[tag=dp.example]` after done using it.

#### 2. This entity not use global ignoring tag since this is a temporary entity that is killed off right away.
```mcfunction
summom area_effect_cloud ~ ~ ~ {Tags: ["dp.example"], Age: 0, Duration: 1}

kill @e[tag=dp.example]
```

#### 3. This is a "chunk marker" entity that shouldn't be move or kill by other datapack, so this entity must have all global ignoring tag.
```mcfunction
summom marker ~ ~ ~ {Tags: [ "dp.example", "global.ignore", "global.ignore.pos", "global.ignore.kill" ]}
```

#### 4. If you don't want the command to execute because the area contains a global ignoring tag. 
```mcfunction
execute if entity @e[tag=!global.ignore,distance=..30]
execute unless entity @e[tag=global.ignore,distance=..30]
```

#### 5. Use score to filter global ignoring tag. ( * )
```mcfunction
scoreboard players set @e[tag=!global.ignore.kill] faq.pass 1
kill @e[scores={faq.pass=1}]
```

> This method may be useful in some situations. ( ** )

#### 6. Use in @s ( * )

```mcfunction
execute as @e[tag=!global.ignore] run function dp:example/kill

dp:example/kill -> kill @s[tag=!global.ignore.kill]
```

> Some cases, you use global ignoring tags at entry selectors, it may cause the command to not work as you want. But then you find that using it in `@s` it works normally. ( ** )


> - **Q:** Can I just only have `!global.ignore` in the `kill`, `tp` command? Because if that entity has `[ "faq.maker", "global.ignore", "global.ignore.pos", "global.ignore.kill" ]` it won't be selected anyway. 
> - **A:** Will be exempt in cases where **# Use in @s** is true but shouldn't. Because if there is any issue in the future, we cannot ignore it. But don't worry, we'll review and clarify for you.

> **Note**
> - ( * ) : **Not recommended if you are unsure.**
> - ( ** ) : Be careful, man in the middle. 

But you need to make sure that your score isn't altered in any other way that you might do accidentally.
