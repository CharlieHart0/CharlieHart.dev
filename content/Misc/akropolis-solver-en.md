+++
title = 'Akropolis Board Game Solver'
date = 2024-04-03T16:23:00Z
publishDate = 2024-04-03T16:23:00Z
draft = true
tags = ['Misc','C++']
ShowToc = true
+++

## Github Repository

## Context

Akropolis is a board game in which players place cards made up of hexagonal tiles together to create a city, aiming to get as high a score as possible at the end of the game, when the scores are calculated. I created a program in C++ to evaluate the scores of randomly generated cities, in order to determine what would be the theoretical highest score a player could achieve in a perfect game.

Cards, made of three hexagonal tiles, must be placed such that they expand the player's city - either next to an existing card or on top of existing cards, if all hexagons directly below the placed card are occupied. Cards placed higher up contribute more points to the score, and only tiles visible from above contribute to the final score at the end of the game.

### Tiles

The possible hexagonal tiles that make up a card are:

- Quarries (not important in this project)

- Districts, which come in several flavours (Housing, Markets, Temples, Barracks and Gardens, each of which have their own distinct colour) and contribute to the player's score, provided that their conditions (see below) are met.

- Plazas, which have a number of stars on them, and come in the same colours as districts, provide a multiplier to the points earned by the equivalent tile type.

### Scoring

At the end of the game, the following district types contribute to the player score if the following conditions are met:

- Housing districts - only if the district is a part of the largest contiguous block of housing districts in the city

- Market districts - only if the district does not border another market district (markets don't like competition)

- Temple districts - only if the district is bordered on all six sides by any other tiles

- Barrack districts - only if the district has at least one exposed edge on the outside of the city

- Garden districts - always contribute to the player's score.

If a district is higher up, it contributes more points to the score, for example a district on the bottom layer contributes one point, a district on the second layer contributes two points, and so on. 

After the number of points from a type of district has been calculated, it is multiplied by the number of stars that plaza tiles have contributed to the player's city in that colour. Having no plazas of that colour in your city means that the you will gain no points from that type of district.

Different colours of plaza have different numbers of stars on them, for example, housing plazas have only one star, while garden plazas have three, meaning gardens have the potential to score much more per tile than houses. The elevation of plazas does not effect score.

## Development

- hex grid
  
  - grid coords

- hex tiles
  
  - tiletype

- city scoring
  
  - types of scoring

- random city generation

- multithreading

## Results

## Future Work

- 
