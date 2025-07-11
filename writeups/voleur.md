---
title: "Voleur - Writeup"
layout: page  # Usa el layout por defecto
permalink: /writeups/voleur  # Define la URL personalizada
collection: writeups  # Asigna a la colección
---
# Voleur - HackTheBox

## Resumen
- **OS**: Windows
- **Dificultad**: Media
- **Temas**: Kerberos, Active Directory, SUID

## Fases
1. Reconocimiento
2. Explotación
3. Escalada de Privilegios



2. Formato con sintaxis Markdown avanzada:

    Bloques de código con sintaxis:
```bash
nmap -sV 10.10.11.76
```


    Tablas:

    | Usuario | Contraseña       |
    |---------|------------------|
    | admin   | P@ssw0rd123!     |



    Alertas visuales:

> 💡 **Tip**: Usa pspy para detectar procesos.
> 
> ⚠️ **Advertencia**: ¡No ejecutes esto en producción!



3. Imágenes optimizadas:

    Usa capturas de pantalla claras.

    Compresión: convert captura.png -resize 800x captura-compressed.png

    Inserta con:

![Nmap Scan](assets/images/nmap-scan.png)

4. Detalles técnicos precisos:

    Comandos exactos que usaste.

    Errores que encontraste y cómo los solucionaste.

    Texto de salida de herramientas (si es relevante).

5. Conclusiones y aprendizajes:

## Lecciones Aprendidas
- Nunca exponer servicios sin autenticación.
- Verificar permisos de binarios SUID.
- La importancia de sincronizar tiempo en Kerberos.

6. Tags para SEO (en el front matter):
yaml
---
title: "Voleur HTB Writeup"
tags: [htb, windows, active-directory, kerberos, suid]
---
