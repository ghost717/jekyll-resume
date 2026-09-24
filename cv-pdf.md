---
layout: default
title: CV — wersja do druku
permalink: /cv-pdf/
---

<nav class="print-actions" aria-label="Nawigacja i drukowanie">
  <a href="{{ '/' | relative_url }}">← Wróć do CV</a>
  <button type="button" onclick="window.print()">Drukuj / Zapisz jako PDF</button>
</nav>

<article class="print-cv">
  {% assign cv = site.data.json %}
  {% assign person_name = cv | where: "key", "name" | first %}
  {% assign role = cv | where: "key", "role" | first %}
  {% assign location = cv | where: "key", "location" | first %}
  {% assign contact = cv | where: "key", "contact" | first %}
  {% assign about = cv | where: "key", "about" | first %}

  <header>
    <h1>{{ person_name.value | escape }}</h1>
    {% if role.value.first %}
      <p class="cv-role">{{ role.value | join: " · " | escape }}</p>
    {% else %}
      <p class="cv-role">{{ role.value | escape }}</p>
    {% endif %}
    <ul class="cv-contact">
      {% if location %}<li>{{ location.value | escape }}</li>{% endif %}
      {% for item in contact.value %}
        <li><a href="{{ item.url | escape }}">{{ item.value | escape }}</a></li>
      {% endfor %}
    </ul>
  </header>

  {% if about %}
    <section>
      <h2>Profil</h2>
      {% for item in about.value %}<p>{{ item | escape }}</p>{% endfor %}
    </section>
  {% endif %}

  {% assign sections = "experience,selected_projects,stack,strengths,education,languages" | split: "," %}
  {% for section_key in sections %}
    {% assign section = cv | where: "key", section_key | first %}
    {% if section %}
      <section>
        <h2>
          {% case section_key %}
            {% when "experience" %}Doświadczenie
            {% when "selected_projects" %}Wybrane projekty
            {% when "stack" %}Technologie
            {% when "strengths" %}Kompetencje
            {% when "education" %}Wykształcenie
            {% when "languages" %}Języki
          {% endcase %}
        </h2>
        {% if section.value.first.key %}
          {% for item in section.value %}
            <div class="cv-entry">
              <h3>{{ item.key | escape }}</h3>
              <p>{{ item.value | escape }}</p>
            </div>
          {% endfor %}
        {% else %}
          <ul class="cv-list">
            {% for item in section.value %}<li>{{ item | escape }}</li>{% endfor %}
          </ul>
        {% endif %}
      </section>
    {% endif %}
  {% endfor %}
</article>
