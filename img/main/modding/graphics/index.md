<a href="../index.md">back</a>

# Graphic Assests
This section will guide you trough the steps of creating a mod which will overhall graphic assets.

1. We will change how some enemies look
2. We will adapt some tiles in the first level
3. Give Vokra a new hat

## Step 1 create a new Mod called "my-graphic enhancement"
Follow the steps here and come back afterwards.
1. [create a new mod](../mod-creation.md#create-a-new-mod)
2. [rename mod](../mod-creation.md#rename-a-mod)
3. [open mod in working directors](../mod-creation.md#open-a-mods-working-directory)

## Step 2 understand the structure of a mod (graphics)
All the graphics assets which are currently used in the game, are available within a mods working directory. The folder graphics contains these. Descent from Arkov's Tower uses Spritesheets for most of it graphics. These are image files which are sectioned into smaller images (tiles). In order to edit the graphics with your art, you simply have to replace the existing images witch your own.

![alt text](../../img/main/modding/graphics/folder.png)

![alt text](../../img/main/modding/graphics/assets.png)

## Step 3 delete not needed files
In order to keep your mod small and follow the [good practices](../mod-creation.md#good-practice) we will no delete all unescesairy files from the mod, as we will only update the graphics and nothing else.

1. identify the images within the "/graphics" folder which you want to change. In my case this would be: "MinimalisticRogueLikeSpriteSheet.png" for the enemies, "DefaultDungeonTiles.png" for the first level's tiles and the "npc.png" as we want to give vokra a new hat. In addition to that we also keep the image "color-palette" which contains all the games original colors.
2. delete the other images within "/graphics"
3. Navigate one level up to the working directories root. In here delete everything except for the main-config.mod.json (to keep the name of the mod) and the "/graphics" folder.

![alt text](../../img/main/modding/graphics/minimalistic-mod.png)
![alt text](../../img/main/modding/graphics/only-changed-images.png)

## Editing the images
To make sure everything works as expected, you need an image editor which allows the usage of transparency. Also ideally it should support tile grids as this will make your life easier. For the creation of my game assets I used [PixelEdit](https://pyxeledit.com/index.php) which is available for a small few. As you probably just start out, I will use the free online tool [piskelApp](https://www.piskelapp.com) in this tutorial. I would recommend sticking to a free tool for now.

### Open Piskel App and import the MinimalisticRogueLikeSpriteSheet.png
1. navigate to: https://www.piskelapp.com/p/create/sprite
2. find the option "IMPORT" on the right bottom side of the page

<p align="center">
  <img src="../../img/main/modding/graphics/import.png" height="300px">
</p>

3. Select "browse images" to import an image
4. Navigate to your mods working directory and to the "/graphics" folder
5. Select the "MinimalisticRogueLikeSpriteSheet.png"
6. In the Wizzard which opens select "import as spritesheet" and change the FrameSize to 16x16

![alt text](image.png)


