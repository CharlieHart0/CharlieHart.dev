+++
title = 'Descending Dudes - Postgraduate 3D Team Project'
date = 2024-19-03T13:41:00Z
publishDate =  2024-19-03T13:41:00Z
draft = true
tags = ['3D','cpp','PC','University','Physics','AI','Team Project','Featured']
ShowToc = true
+++

This project is a 3D platformer game, in which the player has to reach the end of an obstacle course and achieve the fastest time they can. We created the game as a team of a few students working as a 'Mini-Studio' at Newcastle University.

Our project was created using our own engine in C++, and was based on [the 'Goat Heist' game](https://charliehart.dev/3d/goat-game-en/) I created as a previous coursework project; We expanded the codebase significantly, to add a wider range of features to a new game in the engine.

## Youtube Video

## Team Organisation and Communication

Throughout the project, we used many tools to assist in communication and organisation:

- **Git**, for Version Control and developing several features in parallel, making use of branches.

- **Jenkins**, for automated building of our project and reporting of issues when a push was made to Git.

- **Microsoft Teams**, for team communication, and the Teams add-on "Tasks by Planner", for creating kanban boards to track tasks.

### Mini Code Reviews

In order for a new feature to be merged into the main branch of our repository, we performed 'Mini Code Reviews', where the member merging the branch would sit down with another member of the team and read through their code, explaining what it does, and ensuring it was ready to be merged.

I also performed these reviews several times upon the request of other team members, when they required assistance to debug their code, and this really sped up the time it took to fix bugs in our program, maximising the time available to work on other features.

## Development

In this section, I'll cover the areas of the game's codebase I worked on.

### Axial Constraints

I added optional axial constraints to physics objects in the engine, per axis, for both linear motion and angular motion. 

For example, an object with constrained linear motion on the  X,Y and Z axis cannot move linearly (change position) as a result of physics calculations, but can rotate.

The constraint state of a physics object is stored as an eight bit value, where the lowest 6 bits represent a true or false constraint in each of the 3 axis of linear and angular motion.

<img title="" src="https://i.imgur.com/SQiGWR0.png" alt="">

Before calculating the new position of an object in the physics tick, a check is performed, and if any axial constraints are applied to the object, the relevant linear or angular velocity component is set to zero.

#### Usage

Axial Constraints were used for both the player and enemies, to ensure that they were unable to rotate on any axis other than Y, therefore constraints on the X and Z angular motion were applied.

### Trigger Volumes

Trigger Volumes are another feature I added to the codebase. Trigger Volumes are physics objects which are not 'solid', that is, no collision resolution is applied to a collision involving one or more Trigger Volumes. However, they will run one or more functions when an object begins to and finishes colliding with the volume.

As creating a Trigger Volume is as simple as setting the `m_isTrigger` flag on an object to true, a Trigger Volume can be made of any physics object with any existing collider shape already implemented.

#### Usage

##### Checkpoints and Respawn Triggers

Trigger Volumes were used for both Checkpoints and Respawn Triggers in this game. When a player enters a Checkpoint trigger, the player's current position and orientation are stored. 

When the player enters a Respawn trigger, their position and orientation are set to these stored values, and their velocity, both linear and angular, are set to zero.

This allowed us to set exactly where we deemed 'Out of Bounds' for the player, and was a much better method than what was used previously, resetting the player's position to a set value when the players y position was below a certain threshold. 

##### Player Detection Boxes

'Player Detection Boxes' are a type of Trigger Volume that tracks the player objects currently inside the volume, in a `std::set<PlayerCharacter*>`. These are used by the rhino enemies, to determine the closest player to them, within their patrolled area.

*Note that the game did not end up having multiplayer functionality, but at the time this feature was added, multiplayer was still a planned feature.*

### Rhino Enemy

### Map editor/importer

## Future Work

- Scene nodes and children parent objects

- Physics objects which are static, make physics calculations simpler, currently they are ridigbodies with full constraints applied.

- OBBs are not working correctly

### 
