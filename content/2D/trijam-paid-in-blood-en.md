+++
title = 'Trijam #226 - Paid in Blood'
date = 2024-01-21T14:15:00Z
draft = true
tags = ['2D','Unity','PC','Game Jam']
ShowToc = true
+++

## Project Summary

'Paid in Blood' was my entry to [Trijam #226](https://itch.io/jam/trijam-226), a game jam with the twist of being given only three hours to create your game! This does not include planning, compiling or uploading your game, only time spent actively creating the game.

The theme for the jam was 'You gain a little, you lose a little'. The idea I settled on for the game was a small roguelite, where the players health is used as a currency to unlock upgrades.

<img title="" src="https://img.itch.zone/aW1hZ2UvMjE0NDcyMS8xMjY0OTQ4Ni5wbmc=/original/dPCMgV.png" alt="">



## Play Game

[Paid In Blood on Itch.io](https://zoid-burger.itch.io/paid-in-blood)

Required Device: Any device with a mouse and keyboard

<img title="" src="https://img.itch.zone/aW1hZ2UvMjE0NDcyMS8xMjY0OTQ4OC5wbmc=/original/%2BNRXq2.png" alt="">

## Game Mechanics

After clearing a room, the player is given the option to open a chest containing a random permanent power up, at the cost of some health. These upgrades include attack damage, attack speed, fire rate, number of projectiles per shot, and more.

Clearing a room of enemies without taking damage restores a portion of the player's health, rewarding players who are skilled with the opportunity to spend their health points on more upgrades.

## Development

Development of this game was incredibly rushed, even moreso than a typical game jam, but heavy use of third party art made the game possible to finish in time. 

The layout of the map is randomised every time, though each individual room is of course very similar, as the time constraints did not allow too much variation there.

The rooms in the level are always connected using a grid like layout, with rooms either being above, below, to the left or right of each other. For each possible configuration of doors in a room, between one and four doors in each position, prefabricated rooms were designed, which would be used to construct a system of interconnected paths around the map.

The algorithm used to generate the map works as follows:

- Place a random room to start

- Keep track of all rooms containing doors without a room to connect to

- Generate a random compatible room connected to a random available door

- Prioritise generating rooms with more than one door, until the target number of rooms has been reached

- Add single door rooms to cap off any remaining incomplete paths

## Gamejam Results

There were [35 entries](https://itch.io/jam/trijam-226/results) submitted to the jam, and Paid in Blood was ranked third by user ratings! However, the games in first and second place were not eligible to win, as their creators had gone over the time limit - something which is allowed, but makes you ineligible for first place. Therefore, Paid in Blood was given first place!

### Future Work

Ideas for future work to develop the game further include:

- More variation in room design, with different layouts and enemy spawns

- A wider variety of possible upgrades such as added projectile effects (ignite enemies, overpenetrate targets etc)

- Rare, higher cost chests, with a better chance of giving the player the more powerful upgrades like multi shot

- Potential bad outcomes from opening chests, to add a further element of risk on top of losing health.

- Something to happen after all rooms are cleared, such as a boss fight.

### 
