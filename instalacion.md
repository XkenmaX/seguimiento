@@ -0,0 +1,26 @@
#  instalacion

###  ## Requisitos del sistema
Antes de comenzar, debe conocer los requisitos relacionados para su entorno:

PHP 7.1 o superior.
Sesiones de PHP (o use el modo sin estado para OAuth2 y OpenID Connect)
Extensión Curl (o puede usar un adaptador de flujo)
Extensión JSON
##  Instalación
La forma recomendada de instalar ```php
conexión social/autorización```
es a través de Composer.
Si no tiene Composer instalado, descargue el [ composer.phar ](https://getcomposer.org/composer.phar "composer.phar") ejecutable o use el instalador.
```php
$ curl -sS https://getcomposer.org/installer | php
```
Ejecutar ```php
composer.phar requiere socialconnect/autor``` 
agregue un nuevo requisito en su composer.json.

        {
          "exigir": {
            "conexión social/autenticación": "^3.0"
          }
        }
