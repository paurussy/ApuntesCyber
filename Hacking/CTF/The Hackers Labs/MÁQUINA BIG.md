Antes de nada tenemos que crear una red en virtualbox:
![[Pasted image 20240514175154.png]]
Y ahora ya podremos arrancarla, por lo que hacemos el escaneo de nmap:
![[Pasted image 20240514175847.png]]
Esto es lo que corre por el puerto 80:
![[Pasted image 20240514180146.png]]
Viendo el código fuente nos encontramos con el usuario juicy:
![[Pasted image 20240514180219.png]]
Haciendo fuzzing web, nos encontramos con los siguientes directorios:
![[Pasted image 20240514180409.png]]
De esta imágenes, la big2 esconde algo:
![[Pasted image 20240514180436.png]]

