---
layout: default
title: Music
permalink: /music/
---

# Music

{% assign music_files = site.static_files | where_exp: "file", "file.path contains 'assets/music/'" %}
{% assign audio_files = music_files | where_exp: "file", "file.extname == '.mp3' or file.extname == '.m4a' or file.extname == '.wav' or file.extname == '.ogg' or file.extname == '.oga' or file.extname == '.flac' or file.extname == '.aac'" %}
{% assign audio_files = audio_files | sort: "name" %}

{% if audio_files.size > 0 %}
<div class="music-library">
  {% for track in audio_files %}
    {% assign ext = track.extname | downcase %}
    {% case ext %}
      {% when ".mp3" %}
        {% assign mime = "audio/mpeg" %}
      {% when ".m4a" %}
        {% assign mime = "audio/mp4" %}
      {% when ".wav" %}
        {% assign mime = "audio/wav" %}
      {% when ".ogg" or ".oga" %}
        {% assign mime = "audio/ogg" %}
      {% when ".flac" %}
        {% assign mime = "audio/flac" %}
      {% when ".aac" %}
        {% assign mime = "audio/aac" %}
      {% else %}
        {% assign mime = "" %}
    {% endcase %}
    {% assign nice_name = track.basename | replace: "-", " " | replace: "_", " " | capitalize %}
    <article class="music-track">
      <h2>{{ nice_name }}</h2>
      <audio controls preload="metadata">
        <source src="{{ track.path | relative_url }}"{% if mime != "" %} type="{{ mime }}"{% endif %}>
        Your browser does not support the audio element. You can <a href="{{ track.path | relative_url }}">download {{ nice_name }}</a> instead.
      </audio>
      <p><a href="{{ track.path | relative_url }}">Download {{ nice_name }}</a></p>
    </article>
  {% endfor %}
</div>
{% else %}
<p>No tracks found yet. Add audio files (mp3, m4a, wav, ogg, oga, flac, or aac) to <code>assets/music/</code> and they will appear here automatically.</p>
{% endif %}
