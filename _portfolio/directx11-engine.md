---
title: "DirectX11 Game Engine"
excerpt: "Game engine made with the DirectX11 API"
header:
    overlay_color: "#000"
    overlay_filter: "0.5"
    overlay_image: /assets/images/game-engine-teaser.png
    teaser: /assets/images/game-engine-teaser.png
gallery:
  - url: /assets/images/toonshader.png
    image_path: /assets/images/toonshader.png
    alt: "Toon Shader"
    title: "Toon"
  - url: /assets/images/midterm-shaders.png
    image_path: /assets/images/midterm-shaders.png
    alt: "Lambert, Specular, Half Lambert Shaders"
    title: "Lambert, Specular, Half Lambert"
  - url: /assets/images/gameengine.gif
    image_path: /assets/images/gameengine.gif
    alt: "Game Engine"
    title: "Game Engine"
---

Worked with DirectX11 to build a custom game engine. 

Includes:
- Multithreading job manager
- Multiple render targets
- 3D Collision detection
- Skeletal animation
- Normal mapping and mipmapping
- Shaders such as Valve's Half Lambert, bloom, skinning, and toon

Multithreading was incorporated together with a job manager to take care of tasks in the engine after profiling big offenders. As a result, animation and rendering tasks are delegated to the job manager.

{% include gallery %}