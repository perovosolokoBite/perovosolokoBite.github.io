---
layout: home
title: "Writeups HTB"

---

# ¡Bienvenido a mis Writeups de HackTheBox!

## Máquinas Resueltas

# Writeups disponibles:
- [Voleur Writeup](/_writeups/voleur)



<ul class="writeup-list">
{% for writeup in site.writeups %}
  <li>
    <a href="{{ writeup.url }}">{{ writeup.title }}</a>
    <span class="difficulty {{ writeup.difficulty | downcase }}">{{ writeup.difficulty }}</span>
  </li>
{% endfor %}
</ul>
