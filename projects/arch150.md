---
layout: default
title: ARCH150
permalink: /projects/arch150
weeks:
  - slug: week1
    title: Week 1 – Form Explorations
    description: Initial sketches and conceptual studies establishing the project direction.
  - slug: week2
    title: Week 2 – Iterations and Refinement
    description: Mid-course refinements that test materiality and spatial relationships.
  - slug: week3
    title: Week 3 – Final Presentation
    description: Final boards and renders prepared for critique.
---
{% assign arch_images = site.static_files | where_exp: "file", "file.path contains 'assets/images/arch150/'" %}
{% assign image_files = arch_images | where_exp: "file", "file.extname == '.jpg' or file.extname == '.jpeg' or file.extname == '.png' or file.extname == '.gif' or file.extname == '.webp' or file.extname == '.svg'" %}
{% assign image_files = image_files | sort: "path" %}

<style>
.arch150-intro {
  max-width: 800px;
  margin: 0 auto 2rem;
  text-align: center;
}

.arch150-week {
  margin-bottom: 3rem;
}

.arch150-week h2 {
  text-align: center;
  margin-bottom: 0.5rem;
}

.arch150-week p {
  max-width: 700px;
  margin: 0.5rem auto 1.5rem;
  text-align: center;
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.5rem;
}

.gallery-thumb {
  display: block;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 8px 20px rgba(0,0,0,0.25);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  cursor: zoom-in;
}

.gallery-thumb:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgba(0,0,0,0.35);
}

.gallery-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.gallery-empty {
  text-align: center;
  color: #bbb;
}

.lightbox {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.85);
  align-items: center;
  justify-content: center;
  padding: 2rem;
  box-sizing: border-box;
  z-index: 9999;
  cursor: zoom-out;
}

.lightbox:target {
  display: flex;
}

.lightbox img {
  max-width: 90vw;
  max-height: 85vh;
  border-radius: 12px;
  box-shadow: 0 16px 40px rgba(0,0,0,0.45);
  cursor: default;
}

.lightbox__close {
  position: absolute;
  top: 1.5rem;
  right: 1.5rem;
  color: #fff;
  font-size: 2rem;
  text-decoration: none;
}

@media (max-width: 640px) {
  .gallery-grid {
    gap: 1rem;
  }
}
</style>

# ARCH 150 - UMD
<div class="arch150-intro">
  <p>This is an explanation of how this was created, including a timeline.</p>
</div>

{% for week in page.weeks %}
  {% assign week_path = '/assets/images/arch150/' | append: week.slug | append: '/' %}
  {% assign week_images = image_files | where_exp: "file", "file.path contains week_path" %}
  {% assign week_id = week.slug | slugify: 'pretty' %}
  <section class="arch150-week" id="{{ week_id }}">
    <h2>{{ week.title }}</h2>
    {% if week.description %}
      <p>{{ week.description }}</p>
    {% endif %}

    {% if week_images.size > 0 %}
      <div class="gallery-grid">
        {% for image in week_images %}
          {% assign image_index = forloop.index0 | plus: 1 %}
          {% assign lightbox_id = week_id | append: '-' | append: image_index %}
          {% assign alt_text = image.basename | replace: '-', ' ' | replace: '_', ' ' | capitalize %}
          <a class="gallery-thumb" href="#{{ lightbox_id }}">
            <img src="{{ image.path | relative_url }}" alt="{{ alt_text }}">
          </a>
          <div class="lightbox" id="{{ lightbox_id }}">
            <a class="lightbox__close" href="#gallery-close" aria-label="Close">&times;</a>
            <img src="{{ image.path | relative_url }}" alt="{{ alt_text }}">
          </div>
        {% endfor %}
      </div>
    {% else %}
      <p class="gallery-empty">Add images to <code>assets/images/arch150/{{ week.slug }}/</code> to populate this section.</p>
    {% endif %}
  </section>
{% endfor %}

<div id="gallery-close"></div>

<script>
document.addEventListener('DOMContentLoaded', function () {
  const lightboxes = document.querySelectorAll('.lightbox');
  lightboxes.forEach(function (lightbox) {
    lightbox.addEventListener('click', function (event) {
      if (event.target === lightbox) {
        window.location.hash = '#gallery-close';
      }
    });
  });

  document.addEventListener('keydown', function (event) {
    if (event.key === 'Escape') {
      const activeLightbox = document.querySelector('.lightbox:target');
      if (activeLightbox) {
        event.preventDefault();
        window.location.hash = '#gallery-close';
      }
    }
  });
});
</script>
