+++
title = 'Programming the fun out of a videogame puzzle'
date = 2024-01-18T16:23:00Z
draft = true
tags = ['Misc','C++','PC']
ShowToc = true
+++

# 

This project aimed to programatically solve a puzzle in the video game 'Destiny 2' and was the first program I created in C++ that was not a university project.

The puzzle is presented to the player as follows:

- There are 16 different alien glyphs on the wall in front of the player.

- 16 'hints' are then made to the player, which will show three glyphs, with each glyph either being matched to a triangle or a circle.
  
  <img title="" src="https://i.imgur.com/yC4lvMc.png" alt="">
  
  A complete hint: two glyphs are shown matched to a circle, and one to a triangle.

- Each of these 16 'hints' will contain 2 lies and 1 truth. Only one of the three glyphs will be shown with the correct matching symbol in each hint. The other two will be displayed with the incorrect of the two symbols, and must be the opposite of what is shown.

- The player must then determine the true value assigned to each glyph, either triangle or circle.

## Development

## Future Work
