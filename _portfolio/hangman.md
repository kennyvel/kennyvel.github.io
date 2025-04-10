---
title: "Hangman"
excerpt: "Hangman-style game made with React"
header:
    overlay_color: "#000"
    overlay_filter: "0.5"
    overlay_image: /assets/images/hangman.png
    teaser: /assets/images/hangman.png
    actions:
    - label: "<i class='fab fa-github'></i> GitHub Repo"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/kennyvel/hangman"
    - label: "<i class='fa-solid fa-gamepad'></i> Play here!"
      url: "https://kennyvel.github.io/hangman/"
gallery:
  - url: /assets/images/hangman.gif
    image_path: /assets/images/hangman.gif
    alt: "Hangman browser game"
---

Made a hangman-style game with React to brush up on the basics of React and eventually start learning more about Next.js. 

For this project, I implemented:
- Conditional styling and rendering of game elements such as the game status and keyboard using clsx
- Game logic using two states for the current word and the player's guesses, with everything else being derived from these states
- A visually hidden section for game status updates via the screen reader for more accessibility
Feel free to view the code and play the game yourself with the links above!

{% include gallery %}

