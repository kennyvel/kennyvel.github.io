---
title: "MedInsight"
excerpt: "Portal for storing and viewing lab tests"
header:
    overlay_color: "#000"
    overlay_filter: "0.5"
    overlay_image: /assets/images/medinsight-teaser.png
    teaser: /assets/images/medinsight-teaser.png
gallery:
  - url: assets/images/medinsight.gif
    image_path: assets/images/medinsight.gif
    alt: "Med Insight Demo"
  - url: assets/images/medinsight-graph.png
    image_path: assets/images/medinsight-graph.png
    alt: "Graph showcase"
---

Capstone group project done with 5 other classmates together with a stakeholder and mentor overseeing our progress with weekly meetings. The tech stack for the project involved React, MongoDB, Refine, Express, Mongoose, and Material UI. Other tools that were used were GitHub, Figma, Notion, Multer.js, Postman, PyPDF2, and Pytesseract.

The features of the project include:
- Google account authentication and login
- A lab report scrapper that supports tests from Quest Diagnostics and Fusion Lab
- Trend graphs for each test type, such as cholestoral for example
- Fields for user specified minimum and maximum test thresholds for personal goals that show on the graphs
- A MongoDB database for storing a user's test results
- Endpoints for interacting with the database

I personally edited endpoints and the graph component to make sure the graph would properly display the five latest results and the threshold values for each individual type of test. I also helped with the user authentication and adding them to the database, with the user's ID being their unique Google OAuth token. At the end, I documented the endpoints, scraper, and database for future development.

{% include gallery %}
