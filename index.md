---
layout: home
title: "Writeups HTB"
---

# ¡Bienvenido a mis Writeups de HackTheBox!

## Máquinas Resueltas

{% for writeup in site.writeups %}
<div class="writeup-card">
  <h3>
    <a href="{{ writeup.url | relative_url }}">
      {{ writeup.title }}
    </a>
  </h3>
  <p><strong>Dificultad:</strong> {{ writeup.difficulty }}</p>
  <p><strong>Fecha:</strong> {{ writeup.date | date: "%d/%m/%Y" }}</p>
  <p><strong>Temas:</strong> {{ writeup.tags | join: ", " }}</p>
</div>
{% endfor %}
