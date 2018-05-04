Introducció a D3
Crear pàgina HTML en blanc
Mostrar que amb Sublime puc fer "html + TAB”
Afegir <meta charset="utf-8"> al head

Afegir al head el tag de D3
<script src="https://d3js.org/d3.v5.min.js"></script>

Obrir Developer Tools i fer un d3.version
Crear un <p>, i sel·leccionar-lo des de la consola
d3.select("p”)

Mostrar que D3 retorna un Array
Repetir la sel·lecció i guardar-la a una variable
var paragraph = d3.select("p")

Canviar l’style del paràgraf
paragraph.style("background-color", "lightblue")

Ensenyar que el codi font no ha canviat, però si inspeccionem el DOM, sí que haurà canviat
Fer reload, i veure que s’ha perdut tot.
Mostrar method chaining
var paragraph = d3.select("p")
paragraph.style("background-color", "lightblue").style("color", "green")

Explicar que cada crida de cada funció retorna la selecció

Crear un nou paràgraf
d3.select("body”).append("p”).text("Hola bon dia! Sóc un paràgraf creat en D3!")
