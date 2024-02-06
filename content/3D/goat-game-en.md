+++
title = 'Goat Heist'
date = 2023-12-14T13:41:00Z
publishDate = 2024-02-01T10:56:00Z
draft = true
tags = ['3D','cpp','PC','University','Physics','AI']
ShowToc = true
+++

 The Goat Heist game was my coursework for the module 'Advanced Game Technologies', where we were tasked with creating a simple game in C++. We were given a codebase which handled the graphics portion of the game, but needed to add a physics system, some physics related game mechanics, state based AI and pathfinding.

The required features for the coursework included:

- Create a game in which the player must attempt to steal a guarded item from a building inside a voxelised world.

- There should be items in the world which can be picked up by the player to aid them.

- Points should be awarded for progressing towards the goal.

- The player should be controlled by the keyboard via forces or impulses

- The game should feature a simple state based menu system

- Collision detection and resolution should be used to create obstacles for the player

- When the player steals the heist item, they must make their way back to the start while all enemies now pathfind towards the player at all times.

- AI characters should patrol the level and pursue the player when a line of slight is made, using raycasting and a pathfinding algorithm.

{{< youtube fiOnfmi2uF0 >}}

## Development

### PNG level loader

As part of this project, I had to change the design of the game level many times to accomodate and show the new features that were added. When the walls of the level are made of cubes, the positions of which must be manually entered line by line in code, this made iterating on the level design very slow.  In order to solve this issue, without spending too much time to do so - as no marks were given for level design - I wrote a couple of functions to load a level using a PNG image, where different coloured pixels represented different objects/entities to be placed on a grid layout.

The [stb_image](http://nothings.org/stb/) image loader was used to load a PNG file, and then, for each pixel in the image, a corresponding world space position was calculated, according to the given scale value, and then one of many objects was placed at the location, such as doors, keys, enemies, or walls.

```cpp
uint8_t* rgb_image = stbi_load("../Assets/map3.png", &width, &height, &bpp, 3);

for (int h = 0; h < height; h++) {
    for (int w = 0; w < width; w++) {
        int locationInt = (w+(h*width)) * 3;
        Vector3 col = Vector3(rgb_image[locationInt], rgb_image[locationInt + 1], rgb_image[locationInt + 2]);

        if (col == Vector3(0, 0, 0)) { continue; }
        Vector3 location = mapOrigin + Vector3(w * 2.0f * cubeHalfDim, 0, h * 2.0f * cubeHalfDim);


        // a switch statement would be nice here but it didnt seem to want to work with vector3s
        if (col == Vector3(255, 255, 255)) {
            AddCubeToWorld(location, cubeDimensions, 0.0f);
            continue;
        }
        if (col == Vector3(255, 0, 0)) {
            AddDoorCube(location,cubeDimensions);
            continue;
        }
        if (col == Vector3(0, 255, 255)) {
            AddStartingPlate(location);
            for (PlayerCharacter* p : players) {
                p->GetTransform().SetPosition(location);
                p->SetInitPosition(location);
            }
            continue;
        }
        if (col == Vector3(255, 255, 0)) {
            AddBonusToWorld(location,GameObject::ObjectType::PickupKey);
            continue;
        }
        if (col == Vector3(255, 0, 255)) {
            AddBonusToWorld(location, GameObject::ObjectType::PickupGoal);
            continue;
        }
        if (col == Vector3(0, 0, 255)) {
            AddBonusToWorld(location, GameObject::ObjectType::PickupInvisibility);
            continue;
        }
        if (col == Vector3(191, 191, 191)) {
            GameObject* cube = AddCubeToWorld(location, cubeDimensions, 2.0f);
            cube->GetRenderObject()->SetColour(Vector4(0.3f, 0.3f, 0.3f,1));
            continue;
        }
        if (col == Vector3(255, 100, 0)) {
            AddEnemyToWorld(location);
            continue;
        }
    }
}

stbi_image_free(rgb_image);
```

An example of the PNG from which the level would be loaded. Upscaled by 400% in order to display properly on this website.

<img title="" src="https://imgur.com/I8HWIpX" alt="">

## Future Work

- Networking and a multiplayer component to the game, with the player able to host a game and connect to other players by entering their IP address

- Complex shaped collision volumes, such as capsules or oriented bounding volumes.

- Behaviour trees could be added to the enemy AI in order to give them more complex behaviours and gameplay interaction.

- The addition of collision layers and trigger volumes would allow for a wider variety of gameplay features, and would improve some present features, such as powerups which the player does not collide with, making them lose all momentum.

### 
