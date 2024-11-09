<a href="./edit-config.md">back</a>

>If you stumble upon anything outdated, please reach out via [discord](https://discord.gg/uJjuuAH5uX)

# Implement a unique play style for the paladin
In this chapture we will make the first part of the changes. We will edit the json `player-selection.mod.json` file, using already existing properties to change the gameplay of the paladin and make it different from the knight.

## Changing the description of the paladin
This can be made in an easy way, so that the description will not get translated, or in a slightly more complicated way, which will then translate to the users selected language. Lets look at the easier solution first.

### static non translatable derscription
1. open the `player-selection.mod.json` you already know
2. find our newly added section for the paladin (we added it in the last chapture)
3. lets adapt the field `description` as shown bellow:

> as usual I added some `...`'s to make it easier to read!

```json
{
    "player_types": [
        {
            "description": "player_selection.rogue_description",
            "name": "rogue",
            ...
        },
        {
            "description": "my description|my description line 2",
            "name": "paladin",
            ...
        },
        {
            "description": "player_selection.knight_description",
            "name": "knight",
            ...
        },
        {
            "description": "player_selection.vampire_description",
            "name": "vampire",
            ...
        }
    ]
}
```

4. lets test this as usual
    - save your changes in `player-selection.mod.json`
    - start the game
    - open the mod menu
    - make sure that the `paladin? mod is (still) enabled
    - hit keyboard [esc] or [start] on gamepad
    - start a new game
    - you should see the following description

<p align="center">
  <img src="paladin-simple-description.png" alt="description" style="max-width: 500px;">
</p>

> as you can see the `|` character will seperate two lines of text.

Lets now adjust the description to one which makes sense:

`A holy warrior, clad in shining armor,|capable of healing themselves and|feared by undead foes. They are|blessed by the gods`

replace our placeholder text with this description. When you repeat the usual steps to test this, you should now see the correct description.

If you only plan to distribute to english users, you can skip to [the next part](./paladin-basic-changes.md#actual-gameplay-changes)

### translated solution

# Actual gameplay changes

<p align="center">
  <img src="../../../wip.png" alt="description" style="max-width: 500px;">
</p>