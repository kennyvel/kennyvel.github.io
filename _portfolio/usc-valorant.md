---
title: "USC Valorant Match Archiver"
excerpt: "Website for interacting with a collegiate Valorant match database"
header:
    overlay_color: "#000"
    overlay_filter: "0.5"
    overlay_image: /assets/images/usc-valorant-home-page.png
    teaser: /assets/images/usc-valorant-home-page.png
    actions:
    - label: "<i class='fab fa-github'></i> GitHub Repo"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/kennyvel/usc-valorant-matches"
    
gallery:
  - url: assets/images/usc-home-page.gif
    image_path: assets/images/usc-home-page.gif
    alt: "Home page"
  - url: assets/images/usc-team-page.gif
    image_path: assets/images/usc-team-page.gif
    alt: "Team page"
---

This project was done as a final project for a web development class using HTML/CSS, Javascript, PHP, and MySQL. I wanted to create a website that could be used for archiving Valorant match results for the USC Valorant team that I was a part of at the time. Collegiate matches were usually not tracked outside of Valorant's in-game career page, unless they were high profile enough for the website, vlr. Even then, the in-game tracker cant display a full list of custom matches, only the most recent ones.

For this application, I implemented:
- Home and Team pages for introducing the players on the team and purpose of the site
- Form page for match submissions
- Paginated results page that shows all matches stored in the database
- Functionality for viewing, adding, deleting, and editing individual match results
- Match display format inspired by vlr that shows the series score, and a dropdown containing map scores and agents played by each team
- Styles using Bootstrap and vanilla CSS

{% include gallery %}