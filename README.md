## Update to UE5

I really liked the functionality of this plugin, so I figured I'd give a shot at updating it. I moved the logic into an EditorUtilityActor, replaced some deprecated nodes, and rebuilt the plugin for UE 5.4. Should work for any version of UE5, but I won't be testing as 5.4 is what I am using for my project at the moment.

# Greebler
## UE4 & Unity plugin to automatically add detail in the nooks and crannies of your scene

![header](images/header.png)

In her [2017 GDC presentation](https://www.youtube.com/watch?v=WumyfLEa6bU), Tracery developer Kate Compton talks of *greebling* and *footing* as two vital elements of believable environments. Greebling and footing is fine detailing added to the surface and cracks of a larger object that makes it appear more complex, and therefore more visually interesting. In games, this is often a vital part of the level art pass and most game developers have various takes on the practice (my personal favourite is Hidetaka Miyazaki's addiction to [planting headstones all over the place](http://cdn.cheatcc.com/guide_screens/dark_souls_3/ds3_cemetery_bonfire.jpg).

While nowadays realtime mesh painting tools allow for impressive customization and complex detailing, the final greebling pass is often done manually and (to the pained cries of level artists) often requires touch-ups when (not if, *when*) the level design changes. With this engine-agnostic plugin, you can now simply set a bounding volume, throw in the meshes you'd like to use and you will then be able to interactively instance them all over the map (while also being able to control values like noise magnitude, mesh orientation and density).

### Usage - Unity

![unityspawn](images/unityspawn.png)

The package installed, simply navigate to the new *Tools/Spawn Greebler* menu item to spawn the greebler manager object in the scene. If you have gizmos enable you should now see a bunch of black dots in the bounding volume - these are the preview locations for your greeble.

![unitycomponent](images/unitycomponent.png)

You now have a new asset type, the *Greeble Component*. It's pretty self explanatory (message me if you still have issues) - this is essentially the asset type you use to populate your scene with the greeble manager.

![unityspawn](images/unityusage.png)

You can tweak a few settings (if you're feeling anxious they all have in-engine tooltips):

- Voxel Size: How dense should the generated previews and greeble be. Smaller numbers = higher detail
- Noise Magnitude: Noise applied to the final spawn positions.
- Footing Threshold: Higher values here will make greeble only appear in cracks. 3 is a good value, use 2 for stuff like grass.
- Gravity Direction: Vector direction of where greeble will settle. Set to 0,1,0 for stalactites?
- Drop Threshold: Voxel threshold to spawn greeble. Higher values = Higher density though you probably should just decrease voxel size.
- Spawn Mask: Spawn mask to decide on which layers to spawn greeble.

When you're happy with the preview, press the *Greeble* button and your scene should get populated with meshes. If you're unhappy with it, press *Reseed* and keep tweaking!

![unityresults](images/unityresults.png)

Per usual for detail meshes, remember to have them non-static and with a GPU instanced shader to save on performance. Don't feel guilty about dynamic meshes, that's what the Made with Unity did for the *Book of the Dead* foliage!

### Usage - Unreal

![unrealsettings](images/unrealsettings.png)

After setting up which static meshes you'd like to use in the static meshes array, you can tweak a few settings:

- Voxel Size: How dense should the generated previews and greeble be. Smaller numbers = higher detail
- Noise Magnitude: Noise applied to the final spawn positions.
- Footing Threshold: Higher values here will make greeble only appear in cracks. 3 is a good value, use 2 for stuff like grass.
- Gravity Direction: Vector direction of where greeble will settle. Set to 0,1,0 for stalactites?
- Drop Threshold: Voxel threshold to spawn greeble. Higher values = Higher density though you probably should just decrease voxel size.

When you're happy with the preview, press the *Greeble* button and the plugin should pop out an instancedStaticMesh actor into your scene.

![unrealresults](images/unrealresults.png)

### Install process
The repository is split for the two engines: the *Plugin* folder's for Unreal and the *Packages* folder for Unity.

#### Unity 2019.2

![packman](images/packman.png)

This is a plugin that makes use of Unity's *Package Manager* feature. Just drop the *com.alexismorin.greebler* folder into your *packages* folder (found at the same level as the Assets folder) and it should work out-of-the-box. If you're using a pre-packman version of Unity (maybe supported, can be made to work if you're courageous), take the stuff inside the *com.alexismorin.greebler* folder and then drag it anywhere into your regular project hierarchy.

#### Unreal 4.22

![plugins](images/plugins.png)

Just drag the *Greebler* folder into your project's *Plugins* folder (create it at the same level as your content folder if you don't have one already) and open your project - things should work out by themselves.

Unreal will probably bicker if you don't, but you should also probably:
- Turn on Blueprint Editor Utilities in your Editor preferences.
- Raise your maximum loop iteration count to 5000000 in the project settings - the plugin needs to do a lot of number crunching when creating the point cloud and will trip out Unreal's infinite loop alarm system otherwise.

#### Bugs

Per usual, this was whipped up in a couple of hours on my couch (and a few on my bed here and there, give me a break) - bugs beware, tweak or fork as you wish 👨🏻‍🎨

- Unity: Some stuff still spawns under large objects - not that much of a problem but could be a performance issue for larger projects