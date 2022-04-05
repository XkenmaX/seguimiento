# FUNCIONES.

## Por qué surge OAuth.
Surge para paliar la necesidad que se establece del **envío continuo de credenciales entre cliente y servidor.**

Si nos planteamos desarrollar una API REST y una aplicación cliente que pueda consumir de nuestros servicios, si queremos tener un mínimo de mecanismos de seguridad, antes de OAuth2 necesitábamos que el usuario nos enviara las credenciales.
Por ejemplo, si queremos que nuestra aplicación sea capaz de generar contenido para Twitter y que aparezca en el timeline de un usuario, tendríamos que pedirle el usuario y contraseña al mismo para poder hacer este proceso, y la verdad es que no es algo razonable ni aceptable.
De esta forma, la aplicación no tiene porqué tener nuestras credenciales de Twitter, pero le permitiríamos hacer algo en Twitter por nosotros.

## Roles.
Dentro de OAuth 2.0 encontramos diferentes roles que van a participar en el proceso:

 - Dueño del recurso (Owner).
 - Cliente (Client).
 - Servidor de recursos protegidos (Resource Server).
 - Servidor de autorización (Authorization Server).

### Dueño del recurso.
El propietario del recurso es el usuario que da autorización a una determinada aplicación para acceder a su cuenta y poder hacer algunas cosas en su nombre.

El conjunto de cosas que puede hacer la aplicación en su nombre se denomina alcance, y podría ser, por ejemplo, solamente acceso de lectura y no poder crear ningún tipo de elemento de ningún nuevo recurso en su nombre.
Un ejemplo de esto son los widgets que nos permiten integrar Twitter o Facebook dentro de una web, pero que no nos permiten generar nuevo contenido.

**Se le llama dueño del recurso porque, si bien la API no es suya, los datos que maneja sí lo son.**

### Cliente.
El cliente sería la aplicación que desea acceder a esa cuenta de usuario.

El cliente puede ser una aplicación web, una aplicación móvil, una aplicación de escritorio, una aplicación para smartTV, un dispositivo de IoT, otra API que consumiera de esta API, etcétera.
### Servidor de recursos.
El servidor de recursos será la API propiamente, es decir, el servidor que aloja el recurso protegido al cual queremos acceder.

Puede que también forme parte de la misma aplicación que el propio servidor de autenticación.
### Servidor de autorización.
El servidor de autorización es el responsable de gestionar las peticiones de autorización.
Verifica la identidad de los usuarios y emite una serie de tokens de acceso a la aplicación cliente.
En muchas ocasiones, podemos implementar el servidor de autorización nosotros mismos, o podemos delegar en un tercero y tener solamente un servidor de recursos.


## Flujo de protocolo abstracto.

Ahora que tienes una idea de cuáles son los roles de OAuth, veamos un diagrama de cómo interactúan generalmente entre sí:
![](https://assets.digitalocean.com/articles/translateddiagrams32918/Abstract-Protocol-Flow-Spanish@2x.png)
A continuación se encuentra una explicación más detallada de los pasos en el diagrama:

La aplicación solicita autorización para acceder a los recursos de servicio del usuario
Si el usuario autoriza la solicitud, la aplicación recibe la autorización
La aplicación solicita al servidor de autorización (API), presentando la autenticación de su identidad y la autorización otorgada La aplicación solicita al servidor de autorización (API) un token de acceso presentando la autenticación de su propia identidad y la autorización otorgada
Si la identidad de la aplicación es autenticada y la autorización es válida, el servidor de autorización (API) emite un token de acceso a la aplicación. La autorización finaliza
La aplicación solicita el recurso al servidor de recursos (API) y presenta el token de acceso para autenticarse
Si el token de acceso es válido, el servidor de recursos (API) provee el recurso a la aplicación
El flujo real de este proceso variará dependiendo del tipo autorización que esté en uso, sin embargo, esta es la idea general. Examinaremos diferentes tipos de autorizaciones en una sección posterior.

## Registro de la aplicación
Antes de utilizar OAuth, debes registrar tu aplicación con el servicio. Esto se hace a través de un formulario de registro en la parte del “desarrollador” o “API” del sitio web del servicio, en el cual proporcionarás la siguiente información (y posiblemente detalles de tu aplicación):


- Nombre de la aplicación
- Sitio web de la aplicación
- Redirect URI o Callback URL

Redirect URI es donde el servicio reorientará al usuario después de que se autorice (o deniegue) su solicitud y, por consiguiente, la parte de su aplicación que manejará códigos de autorización o tokens de acceso.

## Identificador del cliente y secreto de cliente.
Una vez esté registrada tu aplicación, el servicio emitirá «credenciales del cliente» en forma de un identificador de cliente y un secreto de cliente. El identificador de cliente es una cadena pública que utiliza la API de servicio para identificar la aplicación y para generar las URL de autorización que se presentan a los usuarios.


## Obtención de la autorización.
En el “flujo de protocolo abstracto” presentado anteriormente, los primeros cuatro pasos abarcan la obtención de una autorización y el token de acceso. El tipo de otorgamiento de la autorización depende del método utilizado por la aplicación para solicitar dicha autorización y de los tipos de autorización soportados por la API. OAuth 2 define cuatro tipos de autorización, cada uno de los cuales es útil en casos distintos:
- **Código de autorización**: usado con aplicaciones del lado del servidor
- **Implícito**: utilizado con aplicaciones móviles o aplicaciones web (aplicaciones que se ejecutan en el dispositivo del usuario)
- **Credenciales de contraseña del propietario del recurso**: utilizado con aplicaciones confiables, como aquellas pertenecientes al servicio
- **Credenciales del cliente**: usadas con el acceso API de aplicaciones

## Tipo de otorgamiento: Código de autorización.

El tipo de otorgamiento más usado es el código de autorización, ya que ha sido optimizado para aplicaciones del lado del servidor, en donde el código fuente no está expuesto públicamente y se puede mantener la confidencialidad del secreto de cliente. Este es un flujo basado en la reorientación (redirection), que significa que la aplicación debe ser capaz de interactuar con el agente de usuario (i.e. el navegador web del usuario) y recibir códigos de autorización API que se enrutan a través del agente de usuario.

![](https://assets.digitalocean.com/articles/translateddiagrams32918/Authorization-Code-Flow-Spanish@2x.png)

