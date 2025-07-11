---
layout: home
title: "Inicio"
---

# ¡Bienvenido a mis Writeups de HTB!

## Máquinas Resueltas

{% for writeup in site.writeups %}
### [{{ writeup.title }}]({{ writeup.url }})
- **Dificultad**: {{ writeup.difficulty }}
- **Fecha**: {{ writeup.date | date: "%d/%m/%Y" }}
- **Temas**: {{ writeup.tags | join: ", " }}
{% endfor %}
