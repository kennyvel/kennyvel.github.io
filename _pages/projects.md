---
permalink: /projects/
title: "Game Projects"
toc: true
toc_sticky: true
author_profile: false

---

## Tiled Heroes

Tiled Heroes is a strategy game playable on PC and mobile devices. The player has to use the few units provided to them to eliminate every enemy in the level, for a few levels. 

This was a project that I worked on as the final project for a mobile games class in college. Unlike all the other assignments in the class before this one, I had both complete control over what I wanted to create, and no guidance besides a list of criteria for grading.

For this game, I implemented:
- Levels with different tile maps using Tiled
- Unit movement, attacks, and counterattacking
- Movement and attack range grid display
- Buttons for undoing movements and ending the turn
- Simplified enemy unit turns
- Input for mobile devices and PC

The more I've coded for games throughout college, the more I wish I was at least decent at creating art, as making my own sprites for the units completely from scratch would have been really cool. Definitely something to learn for the future!

## ITP 438

The first half of the class covered some basics for Unreal Engine 4. During the second half of the semester, I had to create a multiplayer first person shooter game made in Unreal Engine 4 by building off of weekly deliverables.

Some features that I had to implement include:
- Default and grenade launcher firing modes
- Sprays/decals
- Ammo pickups
- Killstreak announcements
- Game chat (Team and All)
- Death animations
- Respawning
- Steam integration using Spacewar
- Ready check for starting the game

While networking was kind of a pain to learn, debug, and implement at first, this is probably my favorite project/assignment to have worked on. It felt very rewarding to see things change in the game across multiple screens and player perspectives like it would for any online game I've grown up playing. 

Below is a playlist containing videos that showcase the work that I submitted for grading in the class. 

## DirectX11 API Game Engine

Worked with DirectX11 to build multiple features in a simplified game engine. 

Engine subsystems include:
- Rendering
- Audio
- Collisions
- Physics
- Game world models
- Animation.

Multithreading was also incorporated together with a job manager to take care of tasks, largely rendering and animation tasks, in the engine after profiling big offenders.

For rendering, HLSL was used to create the shaders for the engine such as phong, half lambert, lambert, and bloom mask shaders to name a few. A Z-buffer was utilized to reduce overdrawing and was tweaked to support drawing transparent objects.

## ITP 380

Multiple games made using SDL/SDL2 with starter code provided for some from class. Over the course of the semester, games built off of code from games made previously.  

The games done as part of the class include Pong, Asteroids, Frogger, Mario, Pac-Man, Zelda, Star Fox Tunnel, Mario Kart, and Runner’s Edge (a first person 3D platformer). Another game, Robotron 2084, was also recreated and simplified using the same code base built up from this class for the final exam of another class (ITP 481) later in the future. Taking ITP 380 was my first exposure to programming for video games and helped me learn some of the basics for game programming, writing quality code, and some other mathematical applications.

Throughout the labs, we focused on using a component model for things like movement, sprites, and collisions while also having the ability to override functions if need be in our game object class, Actor

Here is a short video that shows clips of the assignments done in class, with the exception of Portal being a recent replacement to Runner's Edge:

<video width="875" height="500" controls="controls">
  <source src="/assets/videos/HypeTrailer5.mp4" type="video/mp4">
</video>