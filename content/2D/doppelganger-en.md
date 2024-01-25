+++
title = 'Mini Jam 117- Double-Barrelled Doppelgänger'
date = 2024-01-16T14:12:00Z
draft = false
tags = ['2D','Unity','PC','Game Jam','C Sharp']
ShowToc = true
+++

## Project Summary

Double-Barrelled Doppelgänger (DBD) was my entry to [Mini Jam 117](https://itch.io/jam/mini-jam-117-ghosts), a 72 hour gamejam that occurs every two weeks on Itch.io. The theme was 'Ghosts', as the jam took place in October 2022, near halloween.

The additional limitation for the jam's entries was 'Shotgun as a Mechanic', and so DBD features a noisemaker toy shotgun as one of it's core mechanics. 

{{< youtube AIe6Uh9CKh4 >}}

## Play Game

[Double-Barreled Doppelgänger on Itch.io](https://zoid-burger.itch.io/double-barreled-doppelganger)

Required Device: Windows PC

## Game Mechanics

In DBD, you play as a ghost in a haunted mansion. You must escape from a team of ghostbusters who are trying to kill you, and who run towards the player to suck them into their vacuum packs.

As the ghost, you have no weapons or tools besides a single toy shotgun, which is able to make loud noises to scare the ghostbusters away, but will not kill them. You also have the ability to turn invisible for a short period of time.

The player has to both lure and scare away the ghostbusters, in order to defeat them using environmental hazards such as spikes.

## Development

In DBD, there are hidden trigger volumes which cause events to occur, such as torches lighting up to show the player hints as they progress through the levels. In order for this to work, I used [UnityActions](https://docs.unity3d.com/ScriptReference/Events.UnityAction.html), a form of delegation in the Unity game engine. Each trigger zone can be assigned a `UnityAction` to call when the player enters the zone, and this `UnityAction` can in turn call several functions. This was essential in order to create trigger zones with varying behaviour in the short time frame I had to develop the game.

<img title="" src="https://img.itch.zone/aW1hZ2UvMTc0OTYxMC8xMDI5MzYxNy5wbmc=/347x500/snNmMr.png" alt="">

Realtime lighting was used in the 2D scenes of the game, so that the player character's torch could be used to explore the levels, and fit the halloween theme. This presented a few challenges, such as ambient lighting. The ambient lighting in the game was originally much brighter, but had to be darkened so that the other light sources in the game cast strong shadows, as this was one of the concepts that I really wanted in the game. 

So that the player was still visible, a very weak point light was attached to the player, as the cone of light from the torch does not include the player's position.

## Gamejam Results

[192 entries](https://itch.io/jam/mini-jam-117-ghosts/results) were submitted to Mini Jam 117, and DBD came in fourth place! Though I had experience working on personal projects before, this was my first time competing in a gamejam, and I was very happy to recieve many positive and constructive reviews from other contestants on Itch.io

### 
