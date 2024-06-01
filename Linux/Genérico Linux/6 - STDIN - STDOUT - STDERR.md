En Linux, los términos "stdout", "stdin" y "stderr" se refieren a los flujos de entrada y salida que utilizan los programas para interactuar con el sistema operativo y con otros programas.
**STDOUT**
Esto sirve para mostrar por pantalla de forma normal la ejecución de un comando, por ejemplo un "Hola, mundo!" que se imprime en la salida estándar (stdout):
```bash
echo "Hola, mundo!"
```
**STDIN**
Recibe información por parte del usuario. Por ejemplo, un comando de Linux que espera que el usuario ingrese una cadena de texto y la almacena en la variable "nombre":
```bash
read -p "Ingresa tu nombre: " nombre
```
![[Pasted image 20230216222020.png]]
**STDERR**
Esto es el resultado de ejecutar un comando erróneamente. Por ejemplo si ejecutamos un comando para buscar un archivo y no lo encuentra, pues nos mostrará un mensaje de error en la salida de error estándar (stderr):
```bash
ls archivo_inexistente
```
![[Pasted image 20230216222243.png]]
**REDIRECCIÓN DEL OUTPUT AL /DEV/NULL**
De todos modos, podemos redirigir los errores de STDERR al 2>/dev/null, de tal forma que no veremos mensajes de error de esta forma:
![[Pasted image 20230814182511.png]]
Por otra parte, podemos redirigir cualquier tipo de tráfico al /dev/null de esta forma:
![[Pasted image 20230814182608.png]]
