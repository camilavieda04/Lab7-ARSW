# Lab7-ARSW
## Integrantes
- Juan David Navarro
- Sarah Camila Vieda 

## Parte I 
1. Haga que la aplicación HTML5/JS al ingresarle en los campos de X y Y, además de graficarlos, los publique en el tópico: /topic/newpoint. Para esto tenga en cuenta (1) usar el cliente STOMP creado en el módulo de JavaScript y (2) enviar la representación textual del objeto JSON (usar JSON.stringify). 

``` javascript
	 publishPoint: function(px,py){
            var pt=new Point(px,py);
            console.info("publishing point at "+pt);
            addPointToCanvas(pt);
            stompClient.send("/topic/newpoint", {}, JSON.stringify(pt));
        },
```
2. Dentro del módulo JavaScript modifique la función de conexión/suscripción al WebSocket, para que la aplicación se suscriba al tópico "/topic/newpoint" (en lugar del tópico /TOPICOXX). Asocie como 'callback' de este suscriptor una función que muestre en un mensaje de alerta (alert()) el evento recibido. Como se sabe que en el tópico indicado se publicarán sólo puntos, extraiga el contenido enviado con el evento (objeto JavaScript en versión de texto), conviértalo en objeto JSON, y extraiga de éste sus propiedades (coordenadas X y Y). Para extraer el contenido del evento use la propiedad 'body' del mismo, y para convertirlo en objeto, use JSON.parse

``` javascript
 stompClient.subscribe("/topic/newpoint", function (eventbody) {
                var theObject=JSON.parse(eventbody.body);
                alert("x=" +theObject.x+ " Y=" + theObject.y);
```
3. Compile y ejecute su aplicación. Abra la aplicación en varias pestañas diferentes (para evitar problemas con el caché del navegador, use el modo 'incógnito' en cada prueba).

![3](https://user-images.githubusercontent.com/48154086/77126604-b6affb80-6a17-11ea-8b64-7e6c471cc176.PNG)

4. Ingrese los datos, ejecute la acción del botón, y verifique que en todas la pestañas se haya lanzado la alerta con los datos ingresados.

A continuación se muestra la evidencia al ingresar las coordenadas X = 100, Y = 100 en una de las ventanas, se lanza la alerta con estas coordenadas y en la pestaña de incógnito también se lanza esta alerta con estas mismas coordenadas.

![1](https://user-images.githubusercontent.com/48154086/77126396-ef9ba080-6a16-11ea-8e99-aac4d1cce8d0.PNG)


## Parte II

1. Haga que el 'callback' asociado al tópico /topic/newpoint en lugar de mostrar una alerta, dibuje un punto en el canvas en las coordenadas enviadas con los eventos recibidos. Para esto puede dibujar un círculo de radio 1.

``` javascript
 stompClient.subscribe("/topic/newpoint", function (eventbody) {
                var theObject=JSON.parse(eventbody.body);
                addPointToCanvas(theObject);
            });
```

2. Ejecute su aplicación en varios navegadores (y si puede en varios computadores, accediendo a la aplicación mendiante la IP donde corre el servidor). Compruebe que a medida que se dibuja un punto, el mismo es replicado en todas las instancias abiertas de la aplicación.

A continuación mostramos la evidencia de que al dibujar un punto en una de las pestañas en la otra pestaña también se dibuja el punto simultaneamente. 

![2](https://user-images.githubusercontent.com/48154086/77126538-6d5fac00-6a17-11ea-9c08-f83b9d146b99.PNG)
