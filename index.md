---
layout: home
title: Τα Εφετεία της Ελλάδας
---

<!-- Add inline CSS to change background color -->
<style>
  body {
    background: #f8f9fa !important;
    background-color: #f8f9fa !important; /* Green background color */
  }

  h1 {
    text-align: center;
    margin-top: 20px;
    font-size: 36px;  /* Adjust the font size of the title */
    color: black;  /* Title color */
  }

  footer {
    background-color: #333; /* Dark background for footer */
    color: #fff; /* White text for footer */
    text-align: center;
    padding: 20px 0;
    margin-top: 40px;
  }

  footer p {
    margin: 0;
    font-size: 14px;
  }

    .carousel-item img {
    width: 100%;  /* Full width */
    height: 400px; /* Set a fixed height (adjust as necessary) */
    object-fit: cover; /* Ensures the image covers the entire area and scales appropriately */
  }
</style>

<style>
  .carousel-caption h5 {
    color: #000000; /* Black title text */
  }

  .carousel-caption p {
    color: #333333; /* Dark gray subtitle text */
  }
</style>

<div id="carouselExampleIndicators" class="carousel slide" data-bs-ride="carousel">
  <ol class="carousel-indicators">
    <li data-bs-target="#carouselExampleIndicators" data-bs-slide-to="0" class="active"></li>
    <li data-bs-target="#carouselExampleIndicators" data-bs-slide-to="1"></li>
  </ol>
  <div class="carousel-inner">
    <div class="carousel-item active">
      <a href="syros.html">
        <img src="assets/syros.jpg" class="d-block w-100" alt="Εφετείο Σύρου">
      </a>
      <div class="carousel-caption d-none d-md-block">
        <h5>Εφετείο Σύρου</h5>
      </div>
    </div>
    <div class="carousel-item">
      <a href="athens.html">
        <img src="assets/athens.jpg" class="d-block w-100" alt="Εφετείο Αθηνών">
      </a>
      <div class="carousel-caption d-none d-md-block">
        <h5>Εφετείο Αθηνών</h5>
      </div>
    </div>
  </div>
  <a class="carousel-control-prev" href="#carouselExampleIndicators" role="button" data-bs-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="carousel-control-next" href="#carouselExampleIndicators" role="button" data-bs-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>
<footer>
  <p>Ο ιστοχώρος αυτός αναπτύχτηκε από τις Βασιλείου Χρυσάνθη, Καββαδία Ειρήνη και Παναγιώτου Αφροδίτη
  στα πλαίσια εργασίας του μαθήματος Ψηφιακές Εφαρμογές για την Αγροτική Παραγωγή και το Περιβάλλον
  του ΠΜΣ Ψηφιακές Εφαρμογές και Καινοτομία του Ιονίου Πανεπιστημίου. </p>
</footer>

<!-- Add Bootstrap CDN -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
