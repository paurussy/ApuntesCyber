https://blog.hackmetrix.com/broken-access-control/

# Qué es un *Access Control*

El Access Control es un mecanismo en el que se especifica qué información, funciones o sistemas serán accesibles para un usuario, grupo o rol en particular.

*Es decir, es una manera de controlar quién puede acceder a ciertos recursos, generalmente, mediante el uso de políticas para especificar los privilegios de acceso.


En las aplicaciones web, el control de acceso implica usar de mecanismos de protección como:

- Autenticación (verificar la identidad del usuario).
- Autorización (comprobar si el usuario tiene permiso de acceder a un recurso).


# Qué es un *Broken Access Control*

La vulnerabilidad Broken Access Control ocurre cuando una *falla o una ausencia de mecanismos de control de acceso* le permite a un usuario acceder a un recurso que está fuera de sus permisos previstos.

Debido a la cantidad de errores relacionados con el control de acceso, existen varias vulnerabilidades bajo la categoría de Broken Access Control, algunas de ellas son:

- [IDOR (Insecure Direct Object Reference)](https://blog.hackmetrix.com/insecure-direct-object-reference/)
- [CSRF (Client-Side Request Forgery)](https://blog.hackmetrix.com/csrf-cross-site-request-forgery/)
- CORS (Cross-Origin Resource Sharing) misconfiguration


# Caso Común

***Broken Access Control* en funciones administrativas**

Un sitio web permite a los administradores listar los correos electrónicos de sus usuarios desde una URL similar a la siguiente.

`https://sitio-inseguro.com/admin/listar_mails

Aquí podría haber dos posibles escenarios de Broken Access Control:

- En el caso de que un usuario no autenticado pueda acceder y obtener la lista de e-mails.

- En el caso de que un usuario autenticado que no es administrador logre acceder y obtener la lista de mails.

En ambos casos, la aplicación sería vulnerable.


# Como se ve un *Broken Access Control* en el código

Supongamos que un sitio web permite a sus usuarios registrarse y publicar artículos de blog. 

Durante ese registro, el usuario debe entregar datos como su correo electrónico y nombre, y los administradores del sitio pueden ver esta información desde un menú (solo accesible para administradores) que tiene la opción “Ver todos los mails”. Esta sería la URL resultante:

`https://sitio-inseguro.com/admin/ver_todos_los_mails.php

Y al acceder aquí se ejecuta el siguiente código:

```php
if (isset($_SESSION[‘loggedin’]) { 

cargar_emails(); 
} 
else { 

return_to_login(); 
}
```

El servidor efectivamente solicita al usuario que inicie sesión para acceder al listado de e-mails, sin embargo, *no comprueba su rol o sus permisos*. 

Por lo tanto, la aplicación es vulnerable ya que un usuario autenticado pero sin permisos puede acceder a una funcionalidad pensada únicamente para administradores.


# Solución

El servidor también debería comprobar qué roles o permisos posee el usuario al acceder al recurso. Para ello, sería necesario modificar el código para que contenga lo siguiente:

```php
if (isset($_SESSION[‘loggedin’]) && $_SESSION[‘isadmin’] == true)) {   
  
cargar_emails();   
}   
  
else {   
return_to_login();   
}
```

De esta forma, cualquier usuario que no esté autenticado o que no esté autorizado (que no sea administrador), será redirigido a la página de Iniciar sesión.