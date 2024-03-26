+++
title = 'Goat Heist'
date = 2023-12-14T13:41:00Z
publishDate = 2024-02-01T10:56:00Z
draft = false
tags = ['3D','cpp','PC','University','Physics','AI','Featured']
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

### Physics

The base game I was given to build from did not have much of a physics system at all, and over the course of development, many features were added, such as Raycasting, Linear and Angular Motion, Collision Detection and Response.

Raycasting was used by the enemy actors to determine whether the player was considered 'Visible' to an enemy, and had a line of sight to it. This would inform the AI's behaviour, covered later.

The linear and angular motion implemented several functions to perform operations such as integrating linear and angular acceleration and velocity for a given set of forces for each time step, allowing forces to be applied to objects, and for the objects to then move in a realistic manner as a result of these forces.

Collision detection between spheres and Axis Aligned bounding boxes was implemented, with collisions returning collision information such as pointers to both objects involved in the collision and the location and orientation of the contact point or area between the two objects, and by what penetration distance the objects have intersected.

The impulse method was used for collision resolution, where the linear and angular velocities of the two `PhysicsObject`s were changed without integrating the acceleration first. This uses a coefficient of restitution to determine how much velocity is affected by the loss of kinetic energy due to deformation or conversion to thermal energy.

### AI

#### State Machines

The enemy actors in the game make use of AI and pathfinding features, and the enemies operate using a simple state machine, with three states: Patrol, Chase Player and Target Player. Transitions between these states occur when a condition is met, and the state machine is in the correct state to be able to transition to another.

Patrol - The enemy will choose a random reachable location inside the maze, and move towards it.

Chase Player - When the player is in sight of the enemy (a raycast to the player is not blocked by any collider), the enemy moves in a straight line towards the player.

Target Player - When the player picks up the heist goal item and starts heading back to the starting location, all enemies will pathfind towards the player, regardless of if the player is visible, as the alarms have been triggered. 

A state machine is also used for the simple menu system in the game, and to implement different behaviour depending on the current game state - pregame, in progress or game over.

#### Pathfinding

The A Star algorithm is used with a grid based navigation mesh for the enemies' pathfinding. The game level is loaded using the methods described below, and the tiles considered solid - doors and walls - are areas that the enemies are unable to walk through, whereas all other tiles are not solid.

In game, Debug lines are drawn on screen showing the current path taken by each enemy. Green lines show the full path that will be taken by the enemy, and a light blue line shows the forces being applied to the enemy to move it towards the next waypoint in the path.

### Networking

Some basic networking features were added into the codebase as part of the module work, though they did not ultimately make their way into the final game due to time constraints.

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

- Friction between physics objects should be implemented, so that the actors in the game do not slide around on the floor when moving. This would also give the player greater control over the player character.

- Complex shaped collision volumes, such as capsules or oriented bounding volumes.

- Behaviour trees could be added to the enemy AI in order to give them more complex behaviours and gameplay interaction.

- The addition of collision layers and trigger volumes would allow for a wider variety of gameplay features, and would improve some present features, such as powerups which the player does not collide with, making them lose all momentum.

- Physics objects could each have a unique coefficient of restitution, with each collision pair using a calculated value for its CoR, as an average of the two colliding objects' CORs.

### 
