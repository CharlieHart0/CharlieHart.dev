+++
title = 'Descending Dudes - Postgraduate 3D Team Project'
date = 2024-03-19T13:41:00Z
publishDate =  2024-03-19T13:41:00Z
draft = false
tags = ['3D','C++','PC','University','Physics','AI','Team Project','Featured']
ShowToc = true
+++

This project was created for my University team project, and is a 3D platformer game, in which the player has to reach the end of an obstacle course and achieve the fastest time they can. We created the game as a team of a few students working as a 'Mini-Studio' at Newcastle University.

Our project was created using our own engine in C++, and was based on [the 'Goat Heist' game](https://charliehart.dev/3d/goat-game-en/) I created as a previous coursework project; We expanded the codebase significantly, to add a wider range of features to a new game in the engine.

## Video

{{< youtube 25KI7QYORl8 >}}

## Screenshots

Level 1 - Gate Gallop

<img title="" src="https://i.imgur.com/YyG7VYY.png" alt="">

Level 3 - Obstacle Course

<img title="" src="https://i.imgur.com/ZKbJJzd.png" alt="">

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

#### Usages

##### Checkpoints and Respawn Triggers

Trigger Volumes were used for both Checkpoints and Respawn Triggers in this game. When a player enters a Checkpoint trigger, the player's current position and orientation are stored. 

When the player enters a Respawn trigger, their position and orientation are set to these stored values, and their velocity, both linear and angular, are set to zero.

This allowed us to set exactly where we deemed 'Out of Bounds' for the player, and was a much better method than what was used previously, resetting the player's position to a set value when the players y position was below a certain threshold. 

##### Player Detection Boxes

'Player Detection Boxes' are a type of Trigger Volume that tracks the player objects currently inside the volume, in a `std::set<PlayerCharacter*>`. These are used by the rhino enemies, to determine the closest player to them, within their patrolled area.

```cpp
// PlayerDetectorBox.h
class PlayerDetectorBox :
    public TriggerBox
{
public:

    void onCollisionBegin(GameObject* t_otherObject) {
        if (t_otherObject->GetType() == ObjectType::Player) {
            m_playersDetected.insert((PlayerCharacter*)t_otherObject);
        }
    }

    void onCollisionEnd(GameObject* t_otherObject) {
        if (t_otherObject->GetType() == ObjectType::Player) {
            m_playersDetected.erase((PlayerCharacter*)t_otherObject);
        }
    }

    std::set<PlayerCharacter*>* getPlayers() { return &m_playersDetected; }

protected:
    std::set<PlayerCharacter*> m_playersDetected;
};
```

*Note that the game did not end up having multiplayer functionality, but at the time this feature was added, multiplayer was still a planned feature.*

### Rhino Enemy

The rhino enemies in the game are state machine controlled, and use the following logic:

- `Idle state` - remain still until a player enters the rhino's player detection volume, then enter `Targeting state`.

- `Targeting state` - smoothly orients itself towards the nearest player inside the detection volume, and enters `Attacking state` a set time after entering the `Targeting state`.

- `Attacking state` - force is applied in the direction the rhino is pointing, to make it charge in the targeted direction. The rhino does not change its targeted direction after entering this state, so it charges in a straight line. If the rhino leaves it's player detection volume, for example by charging out of it, the rhino will smoothly return to the volume and revert to the `Idle state`.

### Map editor/importer tool

In order to design levels for the game, a graphical map editor was required. Hard-coding the positions, sizes and orientations of each element in a level would take far too long, and is not an intuitive way to work.

I designed a Map Exporter tool for the Unity engine, in C#, which would convert a scene designed in Unity's graphical editor, into a `.json` file, containing information such as :

- Object positions, sizesand  orientations

- Object Types (Geometry, Player, Rhino, Checkpoint etc)

- Render Meshes

- Collider types

- Physics Object inverse masses

and much more information, where applicable to a given object.

The graphical map editor in Unity, showing level 4 of the game:

<img title="" src="https://i.imgur.com/oU4ZAge.png" alt="">

I then wrote a corresponding tool in C++, making use of [The Rapidjson Library](https://rapidjson.org/), to read a map file in `.json` format, and spawn the objects in the game world. 

It can also load a map file in at a given offset position, so it is possible to add a stage to the game inbetween existing stages, only the offset position would need to be changed, on the line calling the `loadMapFromJSON()` function.

```cpp
// MapImporter.h (Snippet)
...
static void loadMapFromJSON(std::string t_fileName, GameWorld& t_world,
    Vector3 t_offset = Vector3(0, 0, 0));
...
```

This tool allowed us to spend less time on level design - something not being assessed in this coursework - and more time on implementing other features into our game and engine.

### Debug UI Elements

Another team member created a Debug UI in the game using ImGUI, which can be accessed by pressing the `I` key.

I added several elements to this UI, displaying information such as:

- Frame rate in fps, and Frame time in ms

- Physics tick time in ms

- Render tick time in ms

- A representation of the world axes in the viewport
  
  <img title="" src="https://i.imgur.com/Kw0F82n.png" alt="">

## Future Work

- An object tree, where an object can have several child objects that move as the parent moves, would be useful to enable more complex gameplay.

- The physics system could be optimised, by not checking for collisions between objects pairs, when both are fully constrained.

- It would be convenient to be able to load a map from a file in any orientation. Currently, it can only be loaded in the orientation it was designed in.

- Implementation of networking and multiplayer to the game. This would require a redesign of several existing features, and may take some time.

### 
