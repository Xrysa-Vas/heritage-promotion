---
layout: page
title: Τα Εφετεία της Ελλάδας 
permalink: /pois/

---

<style>
  body {
      background-color: #333333;
      color: #f1f1f1;
  }

  header, footer, .container, .content, .navbar, .nav-bar {
      background-color: #333333;
      color: #f1f1f1;
  }

  h1, h2, h3, h4, h5, h6 {
      color: #ffffff;
  }

  a {
      color: #1E90FF;
  }

  a:hover {
      color: #FFD700;
  }
</style>

<p>Click για περισσότερες πληροφορίες</p>

<!-- Bootstrap CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">

<style>
  /* Full-width and full-height carousel images */
  .carousel-item img {
    width: 100% !important;
    height: 80vh !important; /* Adjust height as needed */
    object-fit: cover; /* Ensures image covers entire area without stretching */
  }

  /* Centering carousel captions */
  .carousel-caption {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    background: rgba(0, 0, 0, 0.6); /* Dark background for readability */
    padding: 10px;
    border-radius: 5px;
  }

  /* Caption title styling */
  .carousel-caption h4 {
    color: #1E90FF; /* Dark blue text */
    font-size: 1.8rem;
    margin: 0;
  }

  /* Carousel indicators for dark background */
  .carousel-indicators [data-bs-target] {
    background-color: #f1f1f1;
  }

  /* Carousel navigation buttons for dark theme */
  .carousel-control-prev-icon,
  .carousel-control-next-icon {
    background-color: #f1f1f1; /* Light color for navigation icons */
  }

  /* Remove potential padding and margin on parent containers */
  .container, .content, .page {
    padding: 0;
    margin: 0;
    width: 100%;
  }
</style>

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
          <!-- Inline styles for testing full-width image display -->
          <img id="image-{{ p.wikidatum }}" class="d-block w-100" 
               style="width: 100% !important; height: 100vh !important; object-fit: cover !important;" 
               alt="{{ p.title }}">
          <div class="carousel-caption d-none d-md-block">
            <h5>{{ p.title }}</h5> <!-- Title with dark blue color -->
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

<!-- Add the necessary Bootstrap JS at the end of the body -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>

<!-- JQuery Script to Fetch Title and Image from Wikidata -->
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
