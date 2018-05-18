# Programación de visualizaciones para la web
  * [Prerequisitos](#prerequisitos)
  * [Introducción a D3](#introducci-n-a-d3)
  * [Data binding](#data-binding)
    + [Primer gráfico: Barra horizontales](#primer-gr-fico--barra-horizontales)
  * [SVG](#svg)
    + [Creación de un gráfico de barras](#creaci-n-de-un-gr-fico-de-barras)
    + [Creación de un scatter plot](#creaci-n-de-un-scatter-plot)
    + [Creación de un linechart](#creaci-n-de-un-linechart)
  * [Sistemas de fuerzas](#sistemas-de-fuerzas)
  * [Ejemplos reales](#ejemplos-reales)
    + [Visualización de datos de series](#visualizaci-n-de-datos-de-series)
    + [Creative coding](#creative-coding)
  * [Webs de referencia](#webs-de-referencia)

## Prerequisitos

Para poder trabajar con D3, necesitaremos disponer de un servidor web local. Les siguientes webs explican como hacerlo: 
* https://developer.mozilla.org/en-US/docs/Learn/Common_questions/set_up_a_local_testing_server
* https://gist.github.com/jgravois/5e73b56fa7756fd00b89

### Introducción a HTML
https://www.w3schools.com/html/html_intro.asp
### Introducción a CSS
https://www.w3schools.com/css/css_intro.asp

## Introducción a D3

* Crear página HTML en blanco
* Sublime Text: "html + TAB”
* Añadir charset al head
```<meta charset="utf-8">```

* Añadir al head el tag de D3
```
<script src="https://d3js.org/d3.v5.min.js"></script>
```

* Abrir Developer Tools y vaidar la versión de D3
```
d3.version
```

Añadiremos a nuestro código un estil de CSS
```
<style type="text/css">
	.blue {
		background-color: lightblue;
	}
</style>
```

* Crear un ```<p>```, y seleccionarlo des de la consola
```
d3.select("p")
```

* ```d3.select``` nos permite seleccionar un elemento. ```d3.selectAll``` selecciona __TODOS__ los elementos

* ```d3.select``` nos devuelve un array

* Repetir la sel·lecció y guardarla a una variable
```
var paragraph = d3.select("p")
```

* Canviar el style del párrafo
```
paragraph.style("background-color", "lightblue")
```

  * El código fuente no ha cambiado, pero el DOM si

* Con reload, volvemos a lo mismo que contiene el fichero HTML.
* Mostrar "method chaining"
```
var paragraph = d3.select("p")
paragraph.style("background-color", "lightblue").style("color", "green")
```

* Para que el "method chaining", cada método debe devolver un objeto. Debemos dominar qué hace cada método para ser conscientes de qué hacemos cuando encadenamos llamadas

* Crear un nuevo párrafo y asignarle un texto
```
d3.select("body")
	.append("p")
	.text("Hola bon dia, soy un párrafo")
```

* Crearemos una variable con el "body", y aquí iremos añadiendo nuevos elementos
```
var body = d3.select("body")
```

* Crear un h1
```
body.append("h1")
var h1 = body.select("h1").text("Títol!")
```
   * La función h1 nos devuelve una selección que contiene el elemento h1

* Diferencias entre .style y .attr

* Añadiremos la clase ```blue``` a nuestro h1 y veremos que se aplica el estilo que habíamos definido previamente
```
h1.attr("class", "blue")
```

## Data binding

* Asignaremos un array de datos a una selección mediante la función ```data```. Esto en D3 se le llama hacer _data binding_ 
```
var pes = body.selectAll("p")
	.data(["Hello", "Goodbye"])
```

* Si inspeccionamos el valor de ```pes``` en la consola, veremos que tienen un atributo ```__data__``` associado
* A nuestra selección le pondremos un valor entre los tags ```p``` con la función ```text()``` creando una función que es capaz de recuperar el dato asociado a la selección
```
pes.text(function(d) { return d;})
```

Des de la aparición de ES6, podemos utilizar 'arrow functions' que simplifican el código
```
pes.text(d => {return d;})
```

* ```d``` es la convención para referirnos a los datos asociados a cada elemento seleccionado

* Como ya disponíamos de dos ```<p>```, hemos cambiado su texto

* Podemos también acceder al índice de cada uno de nuestros elementos seleccionados. Generalmente se utiliza la convención ```i```

```
body.selectAll("p")
	.data(["Hello", "Goodbye"])
		.text((d, i) => {return 'Element: ' + i + ' Valor: ' + d;})
```

* Cambiaremos, de manera dinámica, el texto de cada párrafo

```
body.selectAll("p")
	.style("text-decoration", (d, i) => {
		return (i%2 == 0) ? 'overline' : 'line-through'
	})
	.text((d, i) => {return 'Element: ' + i + ' Valor: ' + d;})
```

* Pero, qué pasa si tenemos más datos que elementos existentes en mi DOM?

* Las funciones ```enter()``` y ```exit()``` no permiten decidir qué hacer en esos casos

![](http://www.cs171.org/2016/assets/material/lab5/cs171-data-join.png?raw=true "D3 joins")

* ```enter()``` nos devolverá un Array con tantas posiciones como elementos debemos añadir al DOM para cuadrar el número de elementos seleccionados con los datos

```
body.selectAll("p")
	.data(["Hello", "Goodbye", "Tercer element!"])
	.enter()
	.append("p")
		.text((d) => {return d;})

```

* Y si tenemos menos elementos?

```
body.selectAll("p")
	.data(["Only one element!"])
		.text((d, i) => {return d;})
```

* Con este código D3 ha cambiado solo el valor del primer elemento, pero seguimos teniendo 3 ```p``` en el DOM.

* Con la función ```exit()``` podremos marcarlos en rojo

```
body.selectAll("p")
	.data(["Només un element!"])
		.text((d, i) => {return d;})
	.exit()
		.style("color", "red")
```

* O podemos eliminarlos
```
body.selectAll("p")
	.data(["Només un element!"])
		.text((d, i) => {return d;})
	.exit()
		.remove()
```

* Update pattern: [Código demo 02_update_pattern.html](src/02_update_pattern.html)

* La función ```merge``` nos permitirá trabajar con los elementos visibles: enter + update. En este código añadiremos también una transición. [Código demo 03_update_pattern2.html](src/03_update_pattern2.html)

* Podemos ver también como, con el segundo parámetro de la función ```data```, podemos identificar los elementos que añadiremos al DOM de manera única para poder así reutilizarlos: [Código demo 04_update_pattern_keyjoin.html](04_update_pattern_keyjoin.html)

* Comentar demo de joins amb text
  + https://bl.ocks.org/mbostock/3808218
  + https://bl.ocks.org/mbostock/3808221
  + https://bl.ocks.org/mbostock/3808234

### Primer gráfico: Barra horizontales
* Partir del [template vacio](src/01_empty_template.html)
* Crear un ```div#viz``` donde podnremos nuestra visualización

```
var data = [100, 150, 250];

d3.select("#viz")
	.selectAll("div")
	.data(data)
	.enter()
	.append("div")
		.attr("class", "bar")
		.style("width", (d) => { return d + "px"; })
		.style("background-color", "steelblue")
		.style("min-height", "20px");		
```

* Le añadiremos texto a nuestras barras y veremos que podemos hacer una selección mediante la clase asignada a nuestros elementos utilizando un punto
```
d3.selectAll(".bar")
	.text((d) => { return d; })
```

* En este caso estamos utilizando los propios valores para definir su anchura, pero a menduo querremos definir el tamaño de nuestra visualización y que ésta se adapte en función de los valores
* Les [escalas](https://github.com/d3/d3-scale) nos ayudan a mapear valores entre un rango de valores en base al dominio de los mismo

![](http://robdodson.me/content/images/2014/12/d3scale1.png "Mapeo entre valores de nuestro dominio y el rango de valores que esperamos")
```
var scale = d3.scaleLinear()
	.domain([0, d3.max(data)])
	.range([0, 400]);
```
* ```d3.max()``` es una de le múltiples [funciones](https://github.com/d3/d3-array) que D3 nos da para trabajar con nuestros datos

* Ahora podemos cambiar nuestro código de modo que el width de cara barra dependa de la escala. [Demo barchart](src/05_barchart.html)

* __Ejercicio 01__: Crea una visualización de un gráfico de barras donde, mediante la consola, puedas pasarle nuevos datos para que se actualice automáticamente
  * Nivel máster: utiliza transiciones para que las barras hagan una transición del valor actual al nuevo y que las que desaparezcan se "desvanezcan"

## SVG

* SVG es muy similar a HTML
* Ejemplo de código SVG: https://github.com/alignedleft/scattered-scatterplot/blob/master/03_svg.html
  * Todos los tags son autocontenidos excepto __text__

![](https://www.vanseodesign.com/blog/wp-content/uploads/2015/03/wpid-svg-coordinate-system.png "Sistema de coordenadas de SVG")

* Explorar y cambiar el código SVG
* Los elementos que forman parte de un SVG se ordenand en profundidad en función de su orden de aparición. SVG no tiene profundidad (z-index)
 * Podemos crear el mismo código SVG utilizando D3: https://github.com/alignedleft/scattered-scatterplot/blob/master/04_svg_with_d3.html

* __Ejercicio 02__: Crear el siguiente SVG en D3
```
<svg height="1000" width="800">
  <circle cx="25" cy="25" r="20" stroke="black" stroke-width="2" fill="red" />
  <rect x="10" y="50" width="100" height="50" fill="blue" stroke-width="2" stroke="black" opacity="0.5" rx="20" ry="20"/>
  <ellipse cx="200" cy="80" rx="100" ry="50" fill="yellow" stroke="purple" stroke-width="2" />
   <line x1="10" y1="100" x2="200" y2="200" stroke="green" stroke-width="5" />
  <polygon points="200,10 250,190 160,210" style="fill:lime;stroke:purple;stroke-width:1" />
  <polyline points="20,20 40,25 60,40 80,120 120,140 200,180"
  style="fill:none;stroke:black;stroke-width:3" />
  <path d="M150 0 L75 200 L225 200 Z" />
  <text x="0" y="315" fill="red" transform="rotate(330 20,40)">I love SVG</text>
</svg>
```
![](img/ex02_.png "Imagen resultante del código SVG")

* Vamos a ver [como crear varios círculos](src/06_svg.html)

  * __Ejercicio 03__: Crear dos lineas de círculos que se adapten en función de los valores pasados a la función ```update``` por consola. La primera linea debe mostrar el tamaño de los círculos utilizando la escala cuadrática, y la segundo la escala lineal (d3.scaleLinear) Crear dues "linies" de cercles que responguin a l'hora amb dades noves. Una linia de cercles ha de mostrar el radi amb l'escala ```d3.scaleSqrt``` i l'altra amb ```d3.scaleLinear```
  * Hint: Es aconsejable crear dos grupos (```g```) para separar las dos lineas de círuclos
``` 
var gSqrt = svg.append("g");
var gLinear = svg.append("g")
	.attr("transform", "translate(0,50)");
```
 ![](img/ex03.png "Sistema de coordenadas de SVG")

### Convención de márgenes en D3
https://bl.ocks.org/mbostock/3019563

### Creación de un gráfico de barras
  * Creamos un barchart en SVG: https://github.com/alignedleft/d3-book/blob/master/chapter_06/13_making_a_bar_chart_rects.html
  * Asignamos a cada barra su posición correcta: https://github.com/alignedleft/d3-book/blob/master/chapter_06/14_making_a_bar_chart_offset.html
  * Aprovecharemos toda la anchura para crear el gráfico: https://github.com/alignedleft/d3-book/blob/master/chapter_06/16_making_a_bar_chart_widths.html
  * [Cambiaremos la altura en base al valor del dato](src/07_barchart_svg.html)
  * Tenemos que corregir la posición ```y``` y la altura de las barras en función del sistema de coordenadas de SVG: https://github.com/alignedleft/d3-book/blob/master/chapter_06/17_making_a_bar_chart_heights.html
  * Utilizaremos una escala para calcular las altura: [Código](src/08_barchart_scale.html)
  * Pondremos una etiqueta con el color de cada barra: https://github.com/alignedleft/d3-book/blob/master/chapter_06/20_making_a_bar_chart_labels.html
  * Centramos la etiqueta con el color de cada barra: https://github.com/alignedleft/d3-book/blob/master/chapter_06/21_making_a_bar_chart_aligned.html
  * Añadimos el eje de las X: [Código](src/09_barchart_axis.html)

* Ejemplo de un barchart vertical: https://bl.ocks.org/mbostock/3885304

### Creación de un linechart
* Ejemplo de de linechart que carga datos externos: https://github.com/alignedleft/d3-book/blob/master/chapter_11/02_line_chart_axes.html

   * __Ojo!__: este código está hecho en D3v4. Para poder utilizarlo con D3v5 hay que cambiar esta linea:
   ```
   d3.csv("mauna_loa_co2_monthly_averages.csv", rowConverter,function(data) {
   ```
   por esta
   ```
   d3.csv("mauna_loa_co2_monthly_averages.csv", rowConverter).then(function(data) {
   ```

   * Este ejemplo utiliza el dataset que se puede obtener des de i que cargaremos utilizando la función ```d3.csv```: https://raw.githubusercontent.com/alignedleft/d3-book/master/chapter_11/mauna_loa_co2_monthly_averages.csv
   * Los linecharts tiene la peculiaridad de que un único elemento visual, la linea, representa muchos valores. Es por eso que en lugar de hacer el binding de datos con __data__, lo haremos con __datum__
   * Para crear una linea necesitamos utilizar la función ```d3.line()```, que es la que se encarga de generar un __path__ de SVG
      * Creación de un __path__: https://www.w3schools.com/graphics/svg_path.asp
   * Podemos manipular nuestros datos para evitar los cortes que aparecen en los datos: https://github.com/alignedleft/d3-book/blob/master/chapter_11/04_line_chart_adjusted.html

### Creación de un scatter plot

* Ahora vamos a crear un scatter plot
  * Creación de un scatter plot con ejes de coordenadas [Código](src/10_scatter_plot.html)
  * Añadirenos un evento para poder manipular el elemento seleccionado: [Código](src/11_scatterplot_events.html)
  * Utilizaremos el algoritmo de Voronoi para facilitar la selección de nodos en el scatter [Código](src/12_scatterplot_voronoi.html)

* __Ejercicio 04__: Añade un valor random a la generación de datos del scatter plot y mapealo utilizando el radio de los círculos
* __Ejercicio 05__: Añade un slider y cambia el tamaño de todos los puntos en base al valor del mismo
```
<input type="range" min="1" max="100" value="50" class="slider" id="myRange">
```

## Sistemas de fuerzas
* Force directed layout: https://bl.ocks.org/mbostock/4062045

## Ejemplos reales

### Visualización de datos de series
* Demo: http://www.vpascual.org/onetandem/series/
* Código: https://github.com/OneTandem/SeriesViz/blob/master/index.html

### Creative coding
* https://bl.ocks.org/vpascual/cdb2156b88539792d02cfaaab10efbaf

## Webs de referencia
* Página con ejemplos hechos en D3: https://bl.ocks.org/
* Ejemplos del creador de D3: https://bl.ocks.org/mbostock
* Página para poder editar y testear ejemplos de D3 en el navegador: http://blockbuilder.org
* Notepads interactivos: https://beta.observablehq.com/
* Curso corto y básico de D3: https://www.safaribooksonline.com/library/view/an-introduction-to/9781491906323/oreillyvideos2023599.html
* Código del libro "Interactive Data Visualization for the Web": https://github.com/alignedleft/d3-book


