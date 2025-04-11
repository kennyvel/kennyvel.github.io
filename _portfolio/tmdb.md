---
title: "TMDB Movie Search"
excerpt: "Website for searching for movies by name"
header:
    overlay_color: "#000"
    overlay_filter: "0.5"
    overlay_image: /assets/images/tmdb-teaser.png
    teaser: /assets/images/tmdb-teaser.png
    actions:
    - label: "<i class='fab fa-github'></i> GitHub Repo"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/kennyvel/tmdb-page"
    - label: Website
      url: "https://kennyvel.github.io/tmdb-page/"
gallery:
  - url: assets/images/tmdb.gif
    image_path: assets/images/tmdb.gif
    alt: "Default page"
  - url: assets/images/tmdb-search.gif
    image_path: assets/images/tmdb-search.gif
    alt: "Search results"
---

This project was done in class to learn more about using APIs and the data from them to dynamically create and display DOM elements. 

For this project, I implemented:
- jQuery AJAX requests to get movie data for movies currently in theaters and by name for the user's searches
- Mobile-friendly layouts using Bootstrap's size breakpoints
- An initial display of movies currently in theaters
- Displaying movies search results

{% include gallery %}