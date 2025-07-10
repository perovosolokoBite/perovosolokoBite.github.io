---
title: "Voleur - Writeup"
date: 2025-07-07
tags: htb windows active-directory
---

# Voleur - HackTheBox Writeup

## Reconocimiento Inicial

### Escaneo con Nmap
```bash
nmap -sC -sV -p- 10.10.11.76





2222/tcp  open  ssh
5985/tcp  open  winrm

Explotación
Acceso inicial por SSH

Credenciales encontradas:

    Usuario: svc_ldap

    Password: M1XyC9pW7qT5Vn




ssh svc_ldap@dc.voleur.htb -p 2222

Escalada de Privilegios
Binario SUID vulnerable




$ find / -perm -4000 2>/dev/null
/opt/vpn

$ /opt/vpn --config exploit.sh


Conclusión

Máquina de dificultad media que enseña:

    Enumeración de Kerberos

    Escalada mediante binarios SUID




## 📁 Estructura recomendada de archivos

.
├── index.md # Tu página principal
├── _config.yml # Configuración del sitio
├── writeups/ # Carpeta para tus writeups
│ └── voleur.md # Este sería tu archivo
└── assets/ # Imágenes/videos
├── ssh-access.png
└── logo.png




## ✨ Creando tu `index.md` (página principal)

```markdown
---
layout: home
---

# ¡Bienvenido a mis Writeups de HTB!

![Logo](assets/logo.png)

## Máquinas resueltas:

### Windows
- [Voleur](/writeups/voleur) - Active Directory, Kerberos

### Linux
- [Secret](/writeups/secret) - JWT, Docker escape

## Sobre mí
Pentester apasionado por compartir conocimiento...





🔥 Sintaxis Esencial de Markdown
Elemento	Sintaxis	Resultado
Títulos	# H1 ## H2 ### H3	Tamaños de encabezado
Negrita	**texto**	texto
Cursiva	*texto*	texto
Código en línea	`código`	código
Bloque de código	```bash [código] ```	Código con sintaxis resaltada
Enlaces	[texto](https://ejemplo.com)	texto
Imágenes	![alt](ruta/imagen.png)	Muestra la imagen
Listas	- Item 1 - Item 2	• Item 1
• Item 2

















---
title: "Voleur - Writeup"
date: <FECHA_ACTUAL>
tags: [htb, windows, active-directory, kerberos]
---

# Voleur - HackTheBox Writeup

## 1. Reconocimiento Inicial

### Escaneo con Nmap
```bash
nmap -sC -sV -p- <IP_OBJETIVO>

Resultados clave:
text

<COPIA_AQUÍ_TUS_RESULTADOS_NMAP_REALES>

2. Enumeración de Servicios
Kerberos (Puerto 88)

Enumeramos usuarios con kerbrute:
bash

./kerbrute userenum -d <DOMINIO> --dc <DC_IP> <ARCHIVO_USUARIOS>

Usuarios válidos encontrados:

    <USUARIO1>
    <USUARIO2>

SMB (Puerto 445)

Listamos recursos compartidos:
bash

smbclient -L //<IP_OBJETIVO>/ -N

3. Explotación
Acceso Inicial por SSH
bash

ssh <USUARIO>@<IP_OBJETIVO> -p <PUERTO_SSH>
Password: <CONTRASEÑA>

4. Escalada de Privilegios
Binario SUID Vulnerable
bash

$ find / -perm -4000 2>/dev/null
<RUTA_BINARIO>

$ <COMANDO_EXPLOIT>

5. Conclusión

Máquina de dificultad <NIVEL> que enseña:

    <LECCIÓN1>

    <LECCIÓN2>

    <LECCIÓN3>

text


### 🔧 Lugares a personalizar (reemplaza estos valores):

1. `<FECHA_ACTUAL>`: Fecha en formato `YYYY-MM-DD` (ej: `2025-07-07`)
2. `<IP_OBJETIVO>`: La IP de la máquina (ej: `10.10.11.76`)
3. `<COPIA_AQUÍ_TUS_RESULTADOS_NMAP_REALES>`: Tu salida real de Nmap
4. `<DOMINIO>`: Dominio objetivo (ej: `voleur.htb`)
5. `<DC_IP>`: IP del controlador de dominio (ej: `dc.voleur.htb`)
6. `<ARCHIVO_USUARIOS>`: Archivo de usuarios usado (ej: `users.txt`)
7. `<USUARIO1>, <USUARIO2>`: Usuarios válidos que encontraste
8. `<USUARIO>`: Usuario para acceso SSH (ej: `svc_ldap`)
9. `<PUERTO_SSH>`: Puerto SSH (ej: `2222`)
10. `<CONTRASEÑA>`: Contraseña usada (ej: `M1XyC9pW7qT5Vn`)
11. `<RUTA_BINARIO>`: Ruta del binario vulnerable (ej: `/opt/vpn`)
12. `<COMANDO_EXPLOIT>`: Comando de explotación completo
13. `<NIVEL>`: Dificultad (ej: `media`)
14. `<LECCIÓN1>, <LECCIÓN2>, <LECCIÓN3>`: Aprendizajes clave

### 📁 Estructura final de carpetas:

perovosolokoBite.github.io/
├── writeups/
│ └── voleur.md # Este archivo
└── assets/
└── images/
├── nmap-scan.png # Tus capturas
└── exploit.png
text


### 🚀 Para publicar:

1. **Crea el archivo**:
```bash
cd ~/HTB/gitjub
mkdir -p writeups assets/images
nano writeups/voleur.md


