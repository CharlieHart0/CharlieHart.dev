+++
title = 'The Language Game'
date = 2024-01-12T15:57:00Z
draft = false
tags = ['2D','Unity','Android','Languages']
ShowToc = true
+++

<img title="" src="https://play-lh.googleusercontent.com/RcKFesX_kcjCwjzCw1kefi8ysdjfD6R6okb_oaJ0Ezvz-75ZGJxSY6gcff67_QG1fA=w526-h296" alt=""> 

## Project Summary

[The Language Game](https://play.google.com/store/apps/details?id=com.Zoidburger.LanguageGame) is a multiple choice quiz game made in Unity, inspired partly by the BBC quiz show "Richard Osman's House of Games". In the language round of the show, contestants are presented with a multiple choice question, with both the question and all four answers shown in a random foreign language. 

Expanding upon this concept, my game allows the player to also correctly guess the language in which the question is being asked, in order to gain bonus points. 

As someone with a hobby in linguistics and language learning, I enjoy learning vocabulary and recognising patterns in foreign languages, and so there are 17 different languages available in the game, so that everybody is able to play with a set of languages they are unfamiliar with.

<img title="" src="https://play-lh.googleusercontent.com/bRJIzRRSoSvC8tkTfkLBiSjTlBGk1vsAAaOu27BA-Vt8H2Osmni0LIZIa9ICHQFVrwE=w526-h296" alt=""> 

## Download

[Google Play Store Page](https://play.google.com/store/apps/details?id=com.Zoidburger.LanguageGame)

Required Device: Android device running Android 5.1 or higher

## Development

### Unity

#### Graphs

### Translation

As the game features 17 different playable languages, I had to look elsewhere for translations, for the questions, answers, and localisation in the game. 

I wrote a python script to search through a JSON file containing everything to be translated in English, and use Google's Translate API to retrieve translations for each element. 

Questions had to be carefully worded, as to remove as much ambiguity as possible, as many phrases do not translate directly into other languages.

Where possible, I had friends check the translations in their languages for errors, though some are likely to remain.

### Google Play Store

#### In-App Purchases

Although this game is free to play, I included an optional one time purchasable "Premium Version" upgrade for about GBP £2.00, which would disable adverts in game (shown once per 5 or so games), and provide a further 150 or so questions to the player, with more to be added every so often, for free to users who have bought the upgrade.

I purposefully made the upgrade optional, and opting to play the game for free does not worsen the player experience, and is intended for the few players who really enjoy the game and want more questions, after having played the base game enough to need more questions.

#### Publishing

This was my first game made for Android, and so I had to go through the developer registration process for the Google Play Store in order to publish the game.  

I also had to create pictures and icons for the game's Play Store page, something I had not done before. I used Canva, as it came with a handful of presets I could use to make a store listing that looked pretty good!

## Future Work

### Unity Localisation system

For all the menus and UI in the game, I used my own localisation system, reading the name of text objects in the scene, and reading from the language's JSON file to find the string(s) needed. While this does work, it is pretty limited, and cannot replace select substrings with their localised versions. In the future, I'd like to explore Unity's localisation system, to see what it offers.
