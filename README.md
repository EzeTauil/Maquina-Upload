# Maquina-Upload 
## Resolviendo maquina-Upload de "El Pingüino de Mario" 

> _Paso N°:1 Scaneo con nmap. (Fase de reconocimiento)_

```bash
┌──(eze㉿kali)-[~]
└─$ nmap -A 172.17.0.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-20 13:43 -03
Nmap scan report for 172.17.0.2
Host is up (0.00012s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Upload here your file
|_http-server-header: Apache/2.4.52 (Ubuntu)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.41 seconds
```
## _Adjunto imagen:_
![img01](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/9af47708-694e-42e3-9381-2b7761f215d9)

## -----------------------------------------------------------------------------------------------------------------------------------

## Paso N°2: Buscamos alguna pista en la web.
### _Podemos ver que el puerto "80/tcp open http Apache httpd 2.4.57 ((Debian))" esta abierto, asi que ponemos "http://172.17.0.2" en el navegador para ver que muestra:_

