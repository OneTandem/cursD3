# Progrmació de visualitzacions per la web
## Introducció a D3
https://www.safaribooksonline.com/library/view/an-introduction-to/9781491906323/oreillyvideos2023599.html

* Crear pàgina HTML en blanc
* Mostrar que amb Sublime puc fer "html + TAB”
* Afegir charset al head
```<meta charset="utf-8">```

* Afegir al head el tag de D3
```<script src="https://d3js.org/d3.v5.min.js"></script>```

* Obrir Developer Tools i fer un d3.version
* Crear un ```<p>```, i sel·leccionar-lo des de la consola
```d3.select("p”)```

* ```d3.select``` ens permet sel·leccionar 1 element. ```d3.selectAll``` sel·lecciona __TOTS__ els elements

* D3 retorna un Array!
* Repetir la sel·lecció i guardar-la a una variable
```var paragraph = d3.select("p")```

* Canviar l’style del paràgraf
```paragraph.style("background-color", "lightblue")```

  * Ensenyar que el codi font no ha canviat, però si inspeccionem el DOM, sí que haurà canviat

* Fer reload, i veure que s’ha perdut tot.
* Mostrar method chaining
```
var paragraph = d3.select("p")
paragraph.style("background-color", "lightblue").style("color", "green")
```

* Explicar que cada crida de cada funció retorna la selecció

* Crear un nou paràgraf i assignar-li text
```
d3.select("body")
	.append("p")
	.text("Hola bon dia, sóc un paràgraf")
```

* Crear variable amb el body
```var body = d3.select("body")```

* Crear un h1

```
body.append("h1")
var h1 = body.select("h1").text("Títol!")
```

   * Veure que ara, el que retorna D3 és l'element text, de manera que el method chaining retorna coses diferents en funció del que fem

* Comentar diferència entre .style i .attr

* Afegir classe a h1 ```h1.attr("class", "header_blau")```

## Data joins

* 
```
body.selectAll("p")
	.data(["Hello", "Goodbye"])
	.enter()
	.append("p")
		.text(d => {return d;})

```

![alt text](http://www.cs171.org/2016/assets/material/lab5/cs171-data-join.png?raw=true "D3 joins")

* Comentar demo de joins amb text: https://bl.ocks.org/mbostock/3808218


* Les dades amb les que treballa D3 sempre són un array. Pot ser un array de moltes coses diferents
```
var dataset = [10, 20, 30, 40, 50]
 ```

## SVG

* SVG és molt similar a HTML
* Veure exemple de codi SVG: https://github.com/alignedleft/scattered-scatterplot/blob/master/03_svg.html
  * Comentar que tots els tags són autocontinguts, menys el __text__

![alt text](https://www.vanseodesign.com/blog/wp-content/uploads/2015/03/wpid-svg-coordinate-system.png "Sistema de coordenades de SVG")

* Explorar i canviar codi SVG
* Comentar ordre dels elements a SVG. SVG no té profunditat
 * Generació del mateix SVG utilitzant D3: https://github.com/alignedleft/scattered-scatterplot/blob/master/04_svg_with_d3.html

 * Comentar-lo linia a linia i mostrar com __text__ s'afegeix de manera diferent perquè no és un tag autocontingut

* Definitem un estil per assegurar-nos visualment de que tot funciona bé
```
<style type="text/css">
	svg {
		background-color: lightgray;
	}
</style>
```

 * Creem el nostre element SVG
```
var svg = d3.select("body")
	.append("svg")
		.attr("width", 600)
		.attr("height", 600);
```

