Plataphorma
===========

Meteor pet project created to teach my students the following Meteor functionality: 

* Collection's publish/subscribe 
* Deps.autorun 
* Meteor.methods/call 
* Integration of non-Meteor code in compatibility folder (HTML5 games Alien Invasion and Froot Wars)
* Usage of allow to control client access to collections

![ScreenShot](/screenshot.png)


Plataphorma offers the possibility to run 2 different HTML5 games: Alien Invasion and Froot Wars. 

On the right side of the screen the best players of each game are shown, updated in real time each time a signed in player finishes a game. If no game is selected, the best players overall are shown.

On the left side of the screen a chatroom for the current game is available. Only signed in users can post messages. If no game is selected a general chatroom is shown.

The original code of the two HTML5 games integrated in this project is available here:
* Alien Invasion: https://github.com/cykod/AlienInvasion
* Froot Wars: http://www.wrox.com/WileyCDA/WroxTitle/Professional-HTML5-Mobile-Game-Development.productCd-1118301323,descCd-DOWNLOAD.html

Bootstrap style (file bootstrap.min.css) provided by http://bootswatch.com


Running the project
-------------------

A live version of this code is running here: http://plataphorma.meteor.com

To run the project locally, clone the repo and run ```meteor``` inside it. You can see in .meteor/packages that this Meteor project uses these packages:
* ```meteor remove autopublish```
* ```meteor remove insecure```
* ```meteor add bootstrap```
* ```meteor add accounts-ui```
* ```meteor add accounts-password```

1)Click en el boton de juego
El codigo es el siguiente:
Template.choose_game.events = {
    'click #AlienInvasion': function () {
	$('#gamecontainer').hide();
	$('#container').show();
	var game = Games.findOne({name:"AlienInvasion"});
	Session.set("current_game", game._id);
    },
    'click #FrootWars': function () {
	$('#container').hide();
	$('#gamecontainer').show();
	var game = Games.findOne({name:"FrootWars"});
	Session.set("current_game", game._id);
    },
    'click #none': function () {
	$('#container').hide();
	$('#gamecontainer').hide();
	Session.set("current_game", "none");
    }
}

Se captura el evento Click y dependiendo de donde se pinche se le asigna a la variable game un juego u otro o none.
Con session.set se le asigna a la variable current_game el id del juego que hayas elegido.

2)Se escribe un mensaje en el chat sin estar autenticado.

En la linea 87 de client.js se comprueba si estas registrado. Si no lo estas se ejecuta $("#login-error").show(); que es un div que esta en la linea 60 de index.html y saca una alerta que te dice que tienes que estar registrado.

3)Se escribe un mensaje en el chat estando autenticado

En el mismo punto que en la pregunta anterior si estas registrado entonces se coge el id, se coge el mensaje y se añade a la tabla Messages. Por tanto, al modificar la tabla se recargara el navegador se llama al template que hay en la linea  71 del client.js y ya aparecera el nuevo mensaje.

4)Se termina la partida con una puntuacion mayor sin estar autenticado

En la linea 47 de server.js se define la funcion matchFinish en la cual se comprueba si hay un userId que es una variable que meteor crea cuando estas autenticado por tanto como no existe no se cumple la condicion y no se guarda nada en la tabla Matches.

5)Se termina la partida con una puntuacion mayor estando autenticado

En la misma funcion de la pregunta anterior, en este caso si estamos autenticados por tanto, la variable userId tendra un valor y se guardara en la coleccion Matches el user, la fecha, los puntos y el id del juego para que luego las puntuaciones salgan en su juego, no salgan puntuaciones de alieninvasion en frootwars por ejemplo.

6)¿Que sale en la consola cuando te autenticas?

sale 
--
[11:50:19.916] "current user: null"
--
[12:11:10.190] "current user: 8GxZEKws7X8Gcia8r"

Esto es debido a que es una fuente reactiva por tanto cuando te autenticas se ejecuta el Deps.autorun de la linea 57 del client.js primero sale null porque no se le ha asignado nada a currentUser, despues con la sentencia "currentUser = Meteor.userId();" le damos valor a la variable y nos saca el valor de esta por la consola.

7)¿Que sale en la consola cuando te sales?

En este caso como damos a sing out por el mismo motivo que antes se ejecutra el deps.autorun pero en este caso en el primer console.log la variable currentUser tiene el valor del usuario todavia despues con la sentencia "currentUser = Meteor.userId();" se actualiza la variable que como hemos dado a sing out es null por tanto lo que sale en la consola es:

[12:16:23.583] "current user: 8GxZEKws7X8Gcia8r"
[12:16:23.583] "current user: null"






