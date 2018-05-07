# Programació de visualitzacions per la web
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
```d3.select("p")```

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

## Data binding

* Assignem un array de dades a una sel·lecció = fem una sel·lecció i una __join__
```
var pes = body.selectAll("p")
	.data(["Hello", "Goodbye"])

```

* Podem veure com, si inspeccionem les ```<p>``` del DOM, ara tenen una variable ```__data__``` associada
* A la nostra sel·lecció li posarem un valor entre els tags amb la funció ```text()```

```
pes.text(function(d) { return d;})

```

Des de l'aparició d'ES6, es poden utilitzar 'arrow functions'
```
pes.text(d => {return d;})

```

* ```d```és la convenció per referir-nos a la data que ens entra

* Com que ja teniem dos ```<p>```, ens ha canviat el seu contingut

* Podem també accedir al número d'element amb el que estem treballant. Generalment s'utilitza la convenció ```i```

```
body.selectAll("p")
	.data(["Hello", "Goodbye"])
		.text((d, i) => {return 'Element: ' + i + ' Valor: ' + d;})

```

* Podriem, per exemple, canviar la manera com pintem els paràgrafs en funció del seu índex

```
body.selectAll("p")
		.style("text-decoration", (d, i) => {
			return (i%2 == 0) ? 'overline' : 'line-through'
		})
		.text((d, i) => {return 'Element: ' + i + ' Valor: ' + d;})

```

* Què passa si tinc més dades dels elements que tinc al DOM?

* Les funcions ```enter()``` o ```exit()```ens permeten decidir què fer en aquests casos

![alt text](http://www.cs171.org/2016/assets/material/lab5/cs171-data-join.png?raw=true "D3 joins")

* ```enter()``` ens retorna un Array amb tantes posicions com elements cal afegir al DOM per quadrar el número d'elements a les dades

```
body.selectAll("p")
	.data(["Hello", "Goodbye", "Tercer element!"])
	.enter()
	.append("p")
		.text((d) => {return d;})

```

* Què passa si en tenim menys?

```
body.selectAll("p")
	.data(["Només un element!"])
		.text((d, i) => {return d;})
```

* Ens ha canviat el valor del primer element, però seguim tenint 3 paràgrafs!

* Cal fer servir ```exit()```. Podem, per exemple, marcar-los en vermell

```
body.selectAll("p")
	.data(["Només un element!"])
		.text((d, i) => {return d;})
	.exit()
		.style("color", "red")
```

* O podem eliminar-los
```
body.selectAll("p")
	.data(["Només un element!"])
		.text((d, i) => {return d;})
	.exit()
		.remove()
```

* Update pattern: [Codi demo 02_update_pattern.html](src/02_update_pattern.html)

* La funció merge ens permetrà treballar amb els elements visibles: enter + update. També afegirem una transició. [Codi demo 03_update_pattern2.html](src/03_update_pattern2.html)

* A vegades, volem poder reutilitzar elements: [Codi demo 04_update_pattern_keyjoin.html](04_update_pattern_keyjoin.html)

* Comentar demo de joins amb text
  + https://bl.ocks.org/mbostock/3808218
  + https://bl.ocks.org/mbostock/3808221
  + https://bl.ocks.org/mbostock/3808234

### Primer gràfic: Gràfic de barres horitzonals
* Partir del template buit
* Crear un ```div#viz``` on posar la visualització

```
var data = [100, 150, 250];

d3.select("#viz")
	.selectAll("div")
	.data(data)
	.enter()
	.append("div")
		.style("width", (d) => { return d + "px"; })
		.style("background-color", "steelblue")
		.text((d) => { return d; })
```

* No sempre podrem fer servir els mateixos valors per a marcar el valor de les visualitzacions
* Les [escales](https://github.com/d3/d3-scale) ens ajuden a interpolar valors
```
var scale = d3.scaleLinear()
	.domain([0, d3.max(data)])
	.range([0, 400]);
```
* ```d3.max()``` és una de le múltiples [funcions](https://github.com/d3/d3-array) que D3 ens dona per treballar amb Arrays

* Ara ja podem canviar el nostre codi de manera que el width de cada barra depengui de l'escala. [Demo barchart](src/05_barchart.html)

* __Exercici__: Crea una visualització d'un gràfic de barres on, mitjançant la consola, puguis passar-li noves dades per a que s'actualitzi
  * Nivell màster: utilitza transicions per a que les barres s'adeqüin als nous valors amb una animació

## SVG

* SVG és molt similar a HTML
* Veure exemple de codi SVG: https://github.com/alignedleft/scattered-scatterplot/blob/master/03_svg.html
  * Tots els tags són autocontinguts, menys el __text__

![alt text](https://www.vanseodesign.com/blog/wp-content/uploads/2015/03/wpid-svg-coordinate-system.png "Sistema de coordenades de SVG")

* Explorar i canviar codi SVG
* Els elements que formen part d'un SVG s'ordenen en profunditat en funció del seu ordre d'apració. L'últim és el que està a sobre dels demés. SVG no té profunditat (z-index)
 * Generació del mateix SVG utilitzant D3: https://github.com/alignedleft/scattered-scatterplot/blob/master/04_svg_with_d3.html

 * Comentar-lo linia a linia i mostrar com __text__ s'afegeix de manera diferent perquè no és un tag autocontingut

* [Demo cercles en SVG](src/06_svg.html)

* __Exercici__: Crear dues "linies" de cercles que responguin a l'hora amb dades noves. Una linia de cercles ha de mostrar el radi amb l'escala ```d3.scaleSqrt``` i l'altra amb ```d3.scaleLinear```
  * Hint: Podem crear dos grups d'SVG per facilitar-nos el càlcul de coordenades de la següent manera
``` 
var gSqrt = svg.append("g");
var gLinear = svg.append("g")
	.attr("transform", "translate(0,50)");
```

## Càrrega i preparació de dades

* Els arrays de dades poden ser més complexes que simples valors de dades: https://github.com/alignedleft/scattered-scatterplot/blob/master/05_data_values.html


## Exemples reals

### Visualització de dades de sèries
* Demo: http://www.vpascual.org/onetandem/series/
* Codi: https://github.com/OneTandem/SeriesViz/blob/master/index.html

### Creative coding
* https://bl.ocks.org/vpascual/cdb2156b88539792d02cfaaab10efbaf

