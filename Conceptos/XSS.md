https://blog.hackmetrix.com/xss-cross-site-scripting/

# ¿Qué es?

El XSS (Cross Site Scripting) es una vulnerabilidad que permite a un atacante **insertar scripts o secuencias de código malicioso en el navegador web de un usuario**.

*Por lo tanto, el ataque se produce en el código del sitio web que se ejecuta en el navegador, y no en el servidor del sitio. 

Los ataques de tipo XSS pueden tener diversas consecuencias para los usuarios, entre ellas:

- Recopilación de datos personales
- Robo de credenciales (usuarios y contraseñas)
- Robo de cookies
- Redireccionamiento a sitios maliciosos
- Acceso al control del equipo de la víctima
- Cambio de la apariencia visual del sitio

# Tipo de ataques XSS
## XSS Reflejado

La secuencia de comandos reflejada entre sitios surge cuando una aplicación recibe datos maliciosos en una *solicitud HTTP* y los *incluye dentro de la respuesta inmediata*.


***Ejemplo***
Supongamos que un sitio web tiene una función de búsqueda (la cual no realiza ningún procesamiento de los datos) y el usuario proporciona un término en un parámetro de URL:
![](../images/Conceptos/XSS%201.png)

Si el usuario visita la URL construida por el atacante, entonces el script del atacante se ejecutará en el navegador del usuario. 

En ese momento, el script podrá realizar cualquier acción y acceder a los datos a los que el usuario tiene acceso.


## XSS Almacenado

Las secuencias de comandos almacenadas entre sitios (o XSS persistentes) surgen cuando una aplicación recibe datos de una fuente que no es de confianza y los *incluye en sus respuestas HTTP posteriores*.


***Ejemplo***
Supongamos que tenemos un sitio web en el que los usuarios pueden comentar los artículos de un blog. Estos comentarios se enviarán mediante una solicitud HTTP.

Suponiendo que la aplicación no realiza ningún otro procesamiento de los datos, un atacante podría crear una sentencia de código malicioso e insertarla (de forma encriptada) dentro de la solicitud. 

Para que la solicitud la guarde como un comentario en la base de datos.

Cualquier usuario que visite la publicación del blog ahora recibirá el script dentro de la respuesta de la aplicación:
![](../images/Conceptos/XSS%202.png)

Y la secuencia de comandos del atacante se ejecutará en el navegador del usuario.

## XSS Basado en DOM

El XSS basado en DOM surge cuando una aplicación que tiene código JavaScript del lado del cliente procesa datos de una fuente maliciosa, generalmente escribiendo los datos en el DOM.

El atacante puede modificar el DOM utilizando código JavaScript añadiendo nuevas etiquetas, modificando o eliminando otras, cambiando sus atributos HTML, añadiendo clases, cambiando el contenido de texto, entre otras muchas acciones más, incluyendo la inserción de código malicioso.