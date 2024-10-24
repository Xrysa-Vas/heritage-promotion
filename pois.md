---
layout: page
title: POIs List
permalink: /pois/
---

<h2>Τα Εφετεία της Ελλάδας</h2>
<p>Click για περσσότερες πληροφορίες</p>

<!-- Bootstrap CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">

<!-- Bootstrap JS (at the end of body) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>


<!-- Carousel Structure -->
<div id="courtCarousel" class="carousel slide" data-bs-ride="carousel">
  <div class="carousel-indicators">
    {% for p in site.pois %}
      <button type="button" data-bs-target="#courtCarousel" data-bs-slide-to="{{ forloop.index0 }}" {% if forloop.first %}class="active"{% endif %} aria-current="true"></button>
    {% endfor %}
  </div>

  <div class="carousel-inner">
    {% for p in site.pois %}
      <div class="carousel-item {% if forloop.first %}active{% endif %}" data-wikidatum="{{ p.wikidatum }}">
        <a href="{{ p.url | relative_url }}">
          <img id="image-{{ p.wikidatum }}" class="d-block w-100" alt="{{ p.title }}">
          <div class="carousel-caption d-none d-md-block">
            <h5>{{ p.title }}</h5>
          </div>
        </a>
      </div>
    {% endfor %}
  </div>

  <button class="carousel-control-prev" type="button" data-bs-target="#courtCarousel" data-bs-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Previous</span>
  </button>
  <button class="carousel-control-next" type="button" data-bs-target="#courtCarousel" data-bs-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Next</span>
  </button>
</div>

<!-- Add the necessary Bootstrap CSS and JS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>

<!-- Custom CSS to control image size in carousel -->
<style>
.carousel-item img {
  height: 500px; /* Adjust this height as needed */
  object-fit: cover;
  width: 100%;
}
</style>

<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script>
$(document).ready(function() {
  $('.carousel-item').each(function() {
    const wikidatum = $(this).data('wikidatum');
    get_wikidatum(wikidatum);
  });
});

function get_wikidatum(id) {
  let url = `https://www.wikidata.org/w/api.php?action=wbgetentities&ids=${id}&format=json&languages=en|el&origin=*`;
  $.getJSON(url, function(data) {
    const image = get_json_value(['entities', id, 'claims', 'P18', 0, 'mainsnak', 'datavalue', 'value'], data);
    
    // Populate the image for the carousel item
    if (image) {
      get_thumbnail(image, 1000).then(thumbnailUrl => {
        $(`#image-${id}`).attr('src', thumbnailUrl);
      });
    }
  });
}

function get_thumbnail(photoname, size) {
  return new Promise((resolve, reject) => {
    if (!photoname) return resolve('');  // Return empty if there's no image
    photoname = photoname.replace(/ /g, '_');
    const url = `https://api.wikimedia.org/core/v1/commons/file/File:${photoname}`;
    $.getJSON(url, function(data) {
      let thumbname = get_json_value(['thumbnail', 'url'], data);
      if (thumbname) {
        thumbname = thumbname.replace(/\/\d+px-/, `/${size}px-`);
        resolve(thumbname);
      } else {
        reject('Thumbnail not found');
      }
    }).fail(() => reject('Error fetching thumbnail'));
  });
}

// Helper function to access nested JSON data
function get_json_value(json_key, data) {
  try {
    while (json_key.length > 1) {
      data = data[json_key[0]];
      json_key = json_key.slice(1);
    }
    return data[json_key[0]];
  } catch (err) {
    console.error(err, json_key, JSON.stringify(data));
    return null;
  }
}
</script>


/* Optional: Set a fixed height for the carousel */
.carousel-item img {
  height: 500px; /* Adjust the height as needed */
  object-fit: cover; /* Ensures the image covers the entire carousel area */
}



<!-- Fetch Title and Image from Wikidata -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script>
$(document).ready(function() {
  $('.court-card').each(function() {
    const wikidatum = $(this).data('wikidatum');
    get_wikidatum(wikidatum);
  });
});

function get_wikidatum(id) {
  let url = `https://www.wikidata.org/w/api.php?action=wbgetentities&ids=${id}&format=json&languages=en|el&origin=*`;
  $.getJSON(url, function(data) {
    const title = get_json_value(['entities', id, 'sitelinks', 'elwiki', 'title'], data);
    const image = get_json_value(['entities', id, 'claims', 'P18', 0, 'mainsnak', 'datavalue', 'value'], data);

    // Populate the title and image for each court card
    if (title) {
      $(`#title-${id}`).text(title);
    }

    if (image) {
      get_thumbnail(image, 300).then(thumbnailUrl => {
        $(`#image-${id}`).css('background-image', `url(${thumbnailUrl})`);
      });
    }
  });
}

function get_thumbnail(photoname, size) {
  return new Promise((resolve, reject) => {
    if (!photoname) return resolve('');  // Return empty if there's no image
    photoname = photoname.replace(/ /g, '_');
    const url = `https://api.wikimedia.org/core/v1/commons/file/File:${photoname}`;
    $.getJSON(url, function(data) {
      let thumbname = get_json_value(['thumbnail', 'url'], data);
      if (thumbname) {
        thumbname = thumbname.replace(/\/\d+px-/, `/${size}px-`);
        resolve(thumbname);
      } else {
        reject('Thumbnail not found');
      }
    }).fail(() => reject('Error fetching thumbnail'));
  });
}

// Function to access nested JSON data
function get_json_value(json_key, data) {
  try {
    while (json_key.length > 1) {
      data = data[json_key[0]];
      json_key = json_key.slice(1);
    }
    return data[json_key[0]];
  } catch (err) {
    console.error(err, json_key, JSON.stringify(data));
    return null;
  }
}
</script>
