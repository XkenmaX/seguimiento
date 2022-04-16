# FORMULARIO.
### ¿Qué son los formularios web?
Los formularios web son uno de los principales puntos de interacción entre un usuario y un sitio web o aplicación. Los formularios permiten a los usuarios la introducción de datos, que generalmente se envían a un servidor web para su procesamiento y almacenamiento (consulta Enviar los datos de un formulario más adelante en el módulo), o se usan en el lado del cliente para provocar de alguna manera una actualización inmediata de la interfaz (por ejemplo, se añade otro elemento a una lista, o se muestra u oculta una función de interfaz de usuario).

```html
<form>
  <div class="mb-3">
    <label for="exampleInputEmail1" class="form-label">Email address</label>
    <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
    <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
  </div>
  <div class="mb-3">
    <label for="exampleInputPassword1" class="form-label">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword1">
  </div>
  <div class="mb-3 form-check">
    <input type="checkbox" class="form-check-input" id="exampleCheck1">
    <label class="form-check-label" for="exampleCheck1">Check me out</label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

![](https://s3.amazonaws.com/cdn.freshdesk.com/data/helpdesk/attachments/production/16045088524/original/-yshb9zlxAAWIRa5J-_9Ri1R9Uh83G7v5A.png?1569843182)

### Diseñar tu formularirio.
Antes de comenzar a escribir código, siempre es mejor dar un paso atrás y tomarte el tiempo necesario para pensar en tu formulario. Diseñar una maqueta rápida te ayudará a definir el conjunto de datos adecuado que deseas pedirle al usuario que introduzca. Desde el punto de vista de la experiencia del usuario, es importante recordar que cuanto más grande es tu formulario, más te arriesgas a frustrar a las personas y perder usuarios. Tiene que ser simple y conciso: solicita solo los datos que necesitas.


