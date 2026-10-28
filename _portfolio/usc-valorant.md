---
title: "USC Valorant Match Archiver"
excerpt: "Website for interacting with a collegiate Valorant match database"
order: 6
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

This project was done as a final project for a web development class using HTML/CSS, Javascript, PHP, and MySQL. I wanted to create a website that could be used for archiving Valorant match results for the USC Valorant team that I was a part of at the time. Collegiate matches were usually not tracked outside of Valorant's in-game career page, unless they were high profile enough for the website, vlr.

For this application, I implemented:
- Home and Team pages for introducing the players on the team and purpose of the site
- Form page for match submissions
- Paginated results page that shows all matches stored in the database
- Functionality for viewing, adding, deleting, and editing individual match results
- Match display format inspired by vlr that shows the series score, and a dropdown containing map scores and agents played by each team
- Styles using Bootstrap and vanilla CSS

Here are some code snippets of parts I enjoyed coding the most:

### Entire match results page
The match results page code is fairly long, if you would like to see the entire match results page code, click [here](https://github.com/kennyvel/usc-valorant-matches/blob/main/match_results.php) to see it on GitHub.

### Pagination for match results page
Code responsible for enabling pagination.

```sql
$total_results = $results_matches->num_rows;
$results_per_page = 10;
$last_page = ceil($total_results / $results_per_page);

if (isset($_GET['page']) && trim($_GET['page']) != '') {
    $current_page = $_GET['page'];
} else {
    $current_page = 1;
}

if ($current_page < 1 || $current_page > $last_page) {
    $current_page = 1;
}

$start_index = ($current_page - 1) * $results_per_page;
```

```html
<div class="col-12 mt-5">
    <nav aria-label="Page navigation">
        <ul class="pagination justify-content-center">
            <li class="page-item <?php 
                if ($current_page <= 1) {
                    echo "disabled";
                }
              ?>">
                <a class="page-link" href="<?php 
                    $_GET['page'] = 1;
                    echo $_SERVER['PHP_SELF'] . "?" . http_build_query($_GET);
                  ?>">First</a>
            </li>
            <li class="page-item <?php 
                if ($current_page <= 1) {
                    echo "disabled";
                }
              ?>">
                <a class="page-link" href="<?php 
                    $_GET['page'] = $current_page - 1;
                    echo $_SERVER['PHP_SELF'] . "?" . http_build_query($_GET);
                  ?>">&laquo;</a>
            </li>
            <li class="page-item active">
                <a class="page-link" href="">
                    <?php echo $current_page; ?>
                </a>
            </li>
            <li class="page-item <?php 
                if ($current_page >= $last_page) {
                    echo "disabled";
                }
              ?>">
                <a class="page-link" href="<?php 
                    $_GET['page'] = $current_page + 1;
                    echo $_SERVER['PHP_SELF'] . "?" . http_build_query($_GET);
                  ?>">&raquo;</a>
            </li>
            <li class="page-item <?php 
                if ($current_page >= $last_page) {
                    echo "disabled";
                }
              ?>">
                <a class="page-link" href="<?php 
                    $_GET['page'] = $last_page;
                    echo $_SERVER['PHP_SELF'] . "?" . http_build_query($_GET);
                  ?>">Last</a>
            </li>
        </ul>
    </nav>
</div> <!-- .col -->   
```
{: style="max-height: 400px; overflow-y: auto; border: 1px solid #333; border-radius: 6px; margin-bottom: 24px;"}

### Match results dropdown button
button that shows a dropdown with the round scores and agents played by both teams for each map in a series.

```html
<button class="dropdown-button <?php echo $classToAdd; ?>" onclick="toggleMapScores(this)">
    <span class="button-text"><strong><?php echo $row['series_score']; ?></strong></span>
    <br>
    <span class="dropdown-icon">&#9662;</span>
</button>
```

```js
<script>
  function toggleMapScores(button) {
      var matchContainer = button.closest('.match');
      var mapScoresDiv = matchContainer.querySelector('.map-score-bar');

      var dropdownButton = button
      var dropdownIcon = button.querySelector('.dropdown-icon');

      if(mapScoresDiv.classList.contains('mod-expanded')) {
          mapScoresDiv.classList.add('mod-collapsed');
          dropdownIcon.innerHTML = "&#9662";
      }
      else {
          mapScoresDiv.classList.remove('mod-collapsed');
          dropdownIcon.innerHTML = "&#9652";
      }
      mapScoresDiv.classList.toggle('mod-expanded');
  }
</script>
```
{: style="max-height: 400px; overflow-y: auto; border: 1px solid #333; border-radius: 6px; margin-bottom: 24px;"}

{% include gallery %}