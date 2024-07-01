---
permalink: /engine/
title: "Engine Project"
toc: true
toc_sticky: true
author_profile: false

---

## DirectX11 API Game Engine

Overview:

Worked with DirectX11 to build multiple features in a simplified game engine. 

Engine subsystems include:
- Rendering
- Audio
- Collisions
- Physics
- Game world models
- Animation.

Multithreading was also incorporated together with a job manager to take care of tasks in the engine after profiling big offenders.

For rendering, HLSL was used to create the shaders for the engine such as phong, half lambert, lambert, and bloom mask shaders to name a few. A Z-buffer was utilized to reduce overdrawing and was tweaked to support drawing transparent objects. 

