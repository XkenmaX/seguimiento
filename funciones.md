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

### Paso 1: Enlace de código de autorización.
Primero se le da al usuario un enlace de código de autorización similar al siguiente:
```html
   https://cloud.digitalocean.com/v1/oauth/authorize?response_type=code&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read
```

A continuación se presenta una explicación de los componentes del enlace:

- https://cloud.digitalocean.com/v1/oauth/authorize: El punto de conexión de la API de autorización
- client_id=client_id: el ID de cliente (cómo la API identifica la aplicación)
- redirect_uri=CALLBACK_URL: donde el servicio reorienta al agente-usuario después de que se otorgue un código de autorización
- response_type=code: especifica que tu aplicación está solicitando un código de autorización
- scope=read: especifica el nivel de acceso que la aplicación está solicitando

### Paso 2: El usuario autoriza a la aplicación.

Cuando el usuario hace clic en el enlace, debe primero iniciar sesión en el servicio para autenticar su identidad (a menos que ya haya iniciado sesión). Luego, el servicio solicitará autorizar o denegar el acceso de la aplicación a su cuenta. A continuación se presenta un ejemplo de solicitud de autorización:
![](https://assets.digitalocean.com/articles/oauth/authcode.png)

Esta captura específica de pantalla es de la autorización de DigitalOcean; y podemos ver que “Thedropletbook App” está solicitando autorización para el acceso de “lectura” a la cuenta de “manicas@digitalocean.com”.

### Paso 3: La aplicación recibe el código de autorización.

Si el usuario hace clic en “Authorize Application”, el servicio reorienta el agente-usuario al “redirect URI” de la aplicación, que se especificó durante el registro del cliente, junto con un código de autorización. La reorientación sería algo así (suponiendo que la aplicación es “dropletbook.com”):
```html
 https://dropletbook.com/callback?code=AUTHORIZATION_CODE
```

### Paso 4: La aplicación solicita token de acceso.
La aplicación solicita un token de acceso de la API, pasándole el código de autorización junto con los detalles de autenticación, incluido el secreto del cliente, a la terminal del token de la API. A continuación se presenta un ejemplo de una solicitud POST para la conexión del token de DigitalOcean:
```html
https://cloud.digitalocean.com/v1/oauth/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=authorization_code&code=AUTHORIZATION_CODE&redirect_uri=CALLBACK_URL
```

### Paso 5: La aplicación recibe el token de acceso.
Si la autorización es válida, la API enviará una respuesta a la aplicación, con el token de acceso (y, opcionalmente, un token de actualización). La respuesta completa se verá más o menos así:
```json
{"access_token":"ACCESS_TOKEN","token_type":"bearer","expires_in":2592000,"refresh_token":"REFRESH_TOKEN","scope":"read","uid":100101,"info":{"name":"Mark E. Mark","email":"mark@thefunkybunch.com"}}
```

¡Ahora la aplicación está autorizada! Ella puede utilizar el token para acceder a la cuenta del usuario a través de la API de servicio, limitada al alcance del acceso, hasta que el token caduque o se revoque. Si se generó un token de actualización, éste se puede usar para solicitar nuevos tokens de acceso cuando el token original ha caducado.

## Tipo de otorgamiento: Implicito.

El tipo de otorgamiento implícito se utiliza para aplicaciones móviles y aplicaciones web , donde no se garantiza la confidencialidad del secreto de cliente. El tipo de otorgamiento implícito también es un flujo basado en la reorientación, pero el token de acceso se entrega al agente-usuario para reenviarlo a la aplicación, por lo que puede estar expuesto al usuario y a otras aplicaciones en el dispositivo del usuario. Además, este flujo no autentica la identidad de la aplicación y depende del redirect URI para cumplir este propósito.
El flujo de otorgamiento implícito básicamente funciona de la siguiente manera: se le solicita al usuario que autorice la aplicación, luego el servidor de autorización pasa el token de acceso al agente-usuario, quien a su vez se lo pasa a la aplicación.
![](https://assets.digitalocean.com/articles/translateddiagrams32918/Implicit-Flow-Spanish@2x.png)

### Paso 1: Enlace de autorización implícita.
Con el tipo de solicitud implícita, se le presenta al usuario un enlace de autorización que solicita un token de la API. Este enlace se parece al enlace del código de autorización, excepto que está solicitando un token en lugar de un código (ten en cuenta el tipo de respuesta “token”):
```html
https://cloud.digitalocean.com/v1/oauth/authorize?response_type=token&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read
```
### Paso 2: El usuario autoriza a la aplicación.
Cuando el usuario hace clic en el enlace, primero debe iniciar sesión en el servicio para autenticar su identidad (a menos que previamente haya iniciado sesión). Luego, el servicio le solicitará que autorice o deniegue el acceso de la aplicación a su cuenta. A continuación se presenta un ejemplo de autorización de aplicación:
![](https://assets.digitalocean.com/articles/oauth/authcode.png)
Podemos ver que “Thedropletbook App” solicita autorización de acceso de “lectura” a la cuenta de “manicas@digitalocean.com”.

### Paso 3: El agente-usuario recibe el token de acceso con Redirect URI.
Si el usuario hace clic en “Authorize Application”, el servicio reorienta el usuario-agente al redirect URI de la aplicación e incluye un fragmento de URI que contiene el token de acceso. Se vería como algo así:
```html
https://dropletbook.com/callback#token=ACCESS_TOKEN
```

### Paso 4: El agente-usuario sigue al Redirect URI.
El agente-usuario sigue al redirect URI pero conserva el token de acceso.

### Paso 5: La aplicación envía el script de extracción de tokens de acceso.
La aplicación devuelve una página web que contiene una secuencia de comandos que puede extraer el token de acceso del redirect URI completo que ha conservado el usuario-agente.

### Paso 6: Token de acceso transferido a la aplicación.
El agente-usuario ejecuta el script proporcionado y pasa a la aplicación el token de acceso extraído.
¡Ahora la aplicación está autorizada! Ésta puede usar el token para acceder a la cuenta del usuario a través de la API de servicio, limitada al alcance del acceso, hasta que el token caduque o se revoque.

## Flujo de credenciales de contraseña.
Después de que el usuario proporcione sus credenciales a la aplicación, ésta solicitará un token de acceso desde el servidor de autorizaciones. La solicitud POST puede ser similar a lo siguiente:
```html
https://oauth.example.com/token?grant_type=password&username=USERNAME&password=PASSWORD&client_id=CLIENT_ID
```
Si se comprueban las credenciales del usuario, el servidor de autorización devuelve un token de acceso a la aplicación. ¡Ahora la aplicación está autorizada!

Nota: [DigitalOcean](https://es.wikipedia.org/wiki/DigitalOcean "DigitalOcean") actualmente no soporta el tipo de solicitud de credenciales de contraseña, así que el enlace apunta a un servidor de autorización imaginario, en “oauth.example.com”.


## Tipo de otorgamiento: Credenciales del cliente.
El tipo de otorgamiento de credenciales del cliente proporciona a la aplicación una forma de acceder a su propia cuenta de servicio. Si una aplicación desea actualizar su descripción registrada o redirigir el URI, o acceder a otros datos almacenados en su cuenta de servicio a través de la API serían ejemplos de cuándo podría ser útil este tipo de otorgamiento.

### Flujo de credenciales del Cliente.
La aplicación solicita un token de acceso enviando sus credenciales, su ID de cliente y su secreto de cliente, al servidor de autorización. Un ejemplo de solicitud POST podría ser similar a lo siguiente:

```html
https://oauth.example.com/token?grant_type=client_credentials&client_id=CLIENT_ID&client_secret=CLIENT_SECRET
```
Si se comprueban las credenciales de la aplicación, el servidor de autorización devuelve un token de acceso a la aplicación. ¡Ahora la aplicación está autorizada para usar su propia cuenta!
Nota: DigitalOcean no soporta actualmente el otorgamiento de credenciales de cliente, por lo que el enlace apunta a un servidor de autorización imaginario en “oauth.example.com”.

## Ejemplo de uso del token de acceso.
Una vez que la aplicación tiene un token de acceso, puede utilizarlo para acceder a la cuenta del usuario a través de la API, limitado al alcance del acceso, hasta que el token caduque o sea revocado.
A continuación se encuentra un ejemplo de una solicitud API, usando curl. Observa que éste incluye el token de acceso:
```json
curl -X POST -H "Authorization: Bearer ACCESS_TOKEN""https://api.digitalocean.com/v2/$OBJECT" 
```
Suponiendo que el token de acceso es válido, la API procesará la solicitud de acuerdo con sus especificaciones API. Si el token de acceso ha caducado o no es válido, la API devolverá un error de “invalid_request”.

## Flujo de actualización de token.
Hacer una solicitud desde el API, utilizando un token de acceso que ha caducado, generará un error de token inválido “Invalid Token Error”. Si cuando se emitió el token de acceso original se incluyó un token de actualización, entonces éste puede ser usado para solicitar un token de acceso nuevo desde el servidor de autorización.
Aquí se proporciona un ejemplo de una solicitud POST, usando un token de actualización para obtener un nuevo token de acceso:
```html
https://cloud.digitalocean.com/v1/oauth/token?grant_type=refresh_token&client_id=CLIENT_ID&client_secret=CLIENT_SECRET&refresh_token=REFRESH_TOKEN
```

#CREASION DE PAGINA.
```html
https://drive.google.com/drive/folders/1Utv_XqId96QX7De7A2NndJMhNWLysZ5n?usp=sharing
```

