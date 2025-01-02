<a href="./index.md">back</a>

# Generator structure
We will now look at the basic structure of a generator script.

To follow along you can create a new `lua` file in the generators folder `world\generators`. Name it as you like - I will choose `MyGenerator.lua`

Lets paste the following code.

```lua
--world\generators\MyGenerator.lua

local MyGenerator = {}
MyGenerator.id = "my-generator-id"

function MyGenerator.asyncInit(definition, player)
   return {}
end

function MyGenerator.placePlayer(player, options)
    player:setPosition({ x = 2, y = 2 })
end

function MyGenerator.validate(level)
    return true
end

return MyGenerator

```

# Use it in a new Level
> please note: the game will crash if you enter the level making use of the above generator for now as we do not yet generate a level.

1. Open the file `01_roof.lua` file located in: `world\areas`.
2. add a new level as shown in the code example bellow
3. give it a name (in my case just a generic "floor 21") making use of the translation mechanic
4. Tell the level to use the generator `MyGenerator` which we already created by pasting its id
5. add a definition object (empty for now)

```lua
-- world/areas/01_roof.lua

--constants ...

return {
    name = "level_titles.01_roof.area_title",
    levels = {
        -- ADD THIS LEVEL --
        {
            name = { id = "level_titles.generic.floor", variables = { floor = 21 } },
            generator = "my-generator-id",
            definition = {},
        },
        -- OLD First level gets moved down --
        {
            name = "level_titles.01_roof.level.1",
            generator = "room-template",
            definition = {
                ...
            },

        },
    }
}
```
If you run this your game will fail at some point as we did not yet define any generator code yet.

# Lets look at the Generator code

## Main structure
As you can see we create a local object MyGenerator which we return in the end. In this case it does not at all matter what you call this object. For now lets go with MyGenerator. Once you create your own Generator it could make sense to call it with a more describing name, e.g. LavaLevelGenerator

## The Map id
In line 2 we define the id as `my-generator-id`. This is the id which passed into the first level as generator to use the here defined generator. So make sure the id in the generator script and the one in the levels `generator` field are identical. For now I use this id, but as with the filename, later it would make sense to give it a more descriptive id e.g. `lava-level-generator`

## The functions
We need 3 functions in every Generator script. These will get called at different points in time and serve a specific purpose. Lets go trough them one by one starting with the simplest.

### placePlayer
The place player function will place the player in the level. It just needs to update / set the players position. x=1,y=1 is the top left of any map. You will have to ensure that the player is placed on a walkable tile which is not blocked by any mapobjects, enemy and or wall.

Alongside the player object the function also gets passed down a usefull option object which contains information about the generated level. `options.definition` holds the object which gets defined in the `definition` field of the current level.

We will look at a more usefull example in the next section.

### validate
The validate function will be run each time the game is started (after the programm is started via steam) and when a new run starts (after the player and difficulty selection). It checks if all properties for this generator have been defined in each level using it. As an example, lets say your generator needs a size (size: {w: int, h: int}) and if it is missing this property in the levels definition it will crash the game.

If we would not check this in the validation function, the generator script would crash the game as soon as a level is started which does not have the property size defined. In order to give feedback to modders way earlier, I decided to include the possibility of this validate function which will check at game start, or on each run start if all needed properties for all levels are set.

Lets have a look at a validate function which ensures that the property size is set.

```lua
function MyGenerator.validate(level)
    local parsedName = json.stringify({ level.name }, true)
    local definition = level.definition

    if not definition.size then
        return false, "MyGenerator: size is required: " .. parsedName
    end

    return true
end
```

The structure of the validate function is:
1. returns a boolean (true passed, false failed) and an optional string (for the error message only)
2. gets passed down a `level` parameter which contains the definition

Now lets test this in action. Lets enable our mod in the mod manager and then return to the main menu. 

> on windows you should now see an error message in the command line windows, for mac and linux you will have to locate the `Log.log` file after you closed the game.

> if you enable the dev mode (as shown [here](../tipps-and-tricks/devmode.md)) the game will crash if a validation function fails, this is usefull as it will let you know early enough that you messed something up

```
01/02/25 17:25:56: starting version: V.2.0.14-b on Windows
[WARN]: area: 1 level: 1 has no lootTable defined

[...]

```

Ok that is not the error we have expected right? 

There is a reason for this. Even before the validate function of the generator runs, there is a predefined validation function running which will check certain fields which are needed. So lets quickly fix this.

1. open the `world/areas/01_roof.lua` again
2. add a field `lootTable` and pass a value for the common items as shown bellow

```lua
 {
    name = { id = "level_titles.generic.floor", variables = { floor = 21 } },
    generator = "my-generator-id",
    definition = {
        lootTable = {
            common = {}, --no chests in this area
        },
    },
},
```

Now lets test this again. 

```
01/02/25 17:30:44: starting version: V.2.0.14-b on Windows
[WARN]: area: 1 level: 1 MyGenerator: size is required: [{"id":"level_titles.generic.floor","variables":{"floor":21}}]

[...]

```

Now our expected error gets logged, so lets add a size property to the definition.

```lua
{
    name = { id = "level_titles.generic.floor", variables = { floor = 21 } },
    generator = "my-generator-id",
    definition = {
        size = { w = 10, h = 10 },
        lootTable = {
            common = {}, --no chests in this area
        },
    },
},
```

When we run the game now, now warnings should be logged.

We will look at a more refined example in the next section.

### asyncInit, actually generating the level
This function is where the generation magic happens. It has to return an object containing all the information about how the level should look like. This are the collision map, the tile map and information, aswell as which mapobjects (like chests, statues and firebowls) and finally the exit of the level are located.

We will look at this function in detail in the next section.

# Lets now create our own minimalistic generator

<a href="./minimalistic-generator.md" style="margin-left: 48px; font-size: 24px">-> next step</a>