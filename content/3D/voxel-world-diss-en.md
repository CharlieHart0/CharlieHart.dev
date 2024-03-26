+++
title = 'Voxel Rendering and World Generation System'
date = 2023-05-07T11:19:00Z
publishDate = 2024-01-17T11:19:00Z
draft = false
tags = ['3D','Unity','PC','Dissertation','University','C Sharp', 'Featured']
ShowToc = true
+++

My undergraduate dissertation project was to create and optimise a voxel based rendering and world generation system. Inspiration for this project was clearly drawn from Minecraft, one of the most popular games of all time, but also from games such as Astroneer - which uses a voxel system at the core of its player destructible world.

I used Unity for this project, and created a voxel rendering system which was able to generate a set of triangular meshes, with the correct textures applied, to represent an inputted three dimensional array of blocks, which had been generated using smooth noise functions.

Optimisations were then added to the project, and their impacts were evaluated. It was found that using object pooling for face meshes increased terrain load speeds by an average of 30.7%, and that 'internal face culling' - that is, generating meshes only for block faces deemed to be exposed to the 'air' in the game world - increased frame rates substantially, by over 1,700%, and allowed significantly larger voxel worlds to be rendered.

<img title="" src="https://i.imgur.com/ZED5342.png" alt="">

Pictured: A screenshot of a 112x48x112 voxel world, using the world generation system to create both hills and caves.

## Dissertation

[Dissertation PDF document](https://github.com/CharlieHart0/VoxelDissertation/blob/main/Charlie_Hart_Dissertation.pdf)

The dissertation document goes into significantly more detail about this project, many details are omitted in this post to keep it an appropriate length.

## Try It!

[Dissertation Github Repository](https://github.com/CharlieHart0/VoxelDissertation) - Contains the excecutable project, recorded test data, and the Unity project files, including all written code.

Required Device: Windows PC 

Recommended: At least 8GB of RAM, and a dedicated GPU with at least 2GB of VRAM.

## Development

### Voxel Rendering

Each voxel - wether empty or a solid block - has a corresponding Unity gameobject in the respective voxel position within the world, and when a chunk's `RenderChunk()` function is called, it calls functions within each voxel object to generate the face meshes required to render the scene. 

Then, the correct block material is applied, with each block's material containing 6 textures to be applied, one for each direction on the world's 3D axis, both positive and negative. These textures are stored on a single image, and loaded into memory together, in the same way a sprite atlas may be used for 2D graphics.

<img title="" src="https://i.imgur.com/djnyScP.png" alt="">

### World Generation

Terrain is generated in a fixed radius around the camera, in a square shape. As the camera moves on either the X or Z axis, more chunks of terrain are generated, and the chunks which are now outside the specified render distance are removed.

The world generation algorithm uses a seed to generate worlds, so that the same world could be used when evaluating the effectiveness of optimisations. For each chunk, a set of nested `List`s are used to store block information, one for each axis. 

I realise that this is far from optimal, as reading and writing to lists is much slower than using an array, and there will be a significant amount of repeated data here, for example the approximately 50% of the game world consisting of stone blocks. It may be worth investigating ways to optimise this, as terrain generation was significantly slower than I'd like it to be.

#### Algorithm

Each voxel's position is used as an input to the `evaluateBlock()` function, which returns the ID of the block which has been generated at that position. This function completes the following steps:

- If the Y position is 0, a block of lava is generated, so that any generated caves are guaranteed to have solid blocks at the bottom, and are not open to the void.

- The height of the hills at this position on the X and Z axis is calculated. Perlin noise is sampled here, and is used to interpolate between the given minimum and maximum hill heights in the world config. A Unity `AnimationCurve` is used to modify the gradient of hills visually inside the Unity editor.

- Caves are generated. Three dimensional Perlin noise is processed by comparing the square of a sampled value to a threshold value, in order to turn the float samples into boolean values. The samples which evaluate to true here are then set to air blocks, carving holes into the game world.

<img title="" src="https://i.imgur.com/GTy34BH.png" alt="">

## Future Work

Areas of future work include:

- Variations of block shapes, such as half blocks, ramps, window panes etc.

- Variations of block shading, such as semi transparent blocks like glass, leaves, windows, or water blocks which could add post processing effects to the screen

- A voxel lighting system could be added, such that some blocks could emit light, with the light attenuating over a distance.

- A system to save and import world configurations, so that custom worlds could be displayed, for example a voxel model instead of a naturally generated world.

- Asynchronous terrain generation could be implemented, or the terrain generation could be further optimised, so that the generation of terrain does not cause the game to freeze.

### 
