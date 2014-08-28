---
published: true
---

## Creatividad con CSS3, canvas, y webGL

Durante estos días de "vacaciones", he dedicado unas a realizar algunos experimentos de programación gráfica, o programación creativa, directamente sobre el navegador.

Creo que todo empezó cuando me topé con [esta imagen](http://upload.wikimedia.org/wikipedia/commons/thumb/8/88/Pythagoras_tree_1_1_13_Summer.svg/1280px-Pythagoras_tree_1_1_13_Summer.svg.png), el árbol de Pitágoras, que es una figura basada en la apliación del teorema de Pitágoras. Me inspiró para intentar realizar alguna imagen recursiva (o fractal) en Javascript y directamente sobre el navegador. Ya había hecho alguna imagen de este tipo antes, pero no directamente para navegador, si no en Pascal o C.

Aquí están mis resultados:

- Un árbol binario (canvas): http://codepen.io/jllodra/pen/jCiox
- Un árbol de pitágoras (three.js): http://codepen.io/jllodra/pen/aqLlg
- Una versión sui-generis del árbol de pitágoras (three.js): http://codepen.io/jllodra/full/efsDd/

![](/_posts/images/Screenshot%202014-08-28%2016.39.40.png)

El principio general de estos algoritmos es simple: un árbol es un tronco + otro árbol más pequeño (a la izquierda) + otro árbol más pequeño (a la derecha). Estos otros árboles más pequeños son a su vez un tronco, y tienen también dos árboles más pequeños; así hasta alcanzar el nivel de profundidad que deseemos.

Luego me inspiró una "ilusión" óptica que vi en youtube, se trata de un movimiento lineal que nuestro cerebro interpreta como circular, ya que para él es más fácil, el vídeo en cuestión [es este](https://www.youtube.com/watch?v=pNe6fsaCVtI).

Bajo la excusa de aprender lenguajes como SCSS, SASS, y JADE, traté de realizar algunas animaciones sin utilizar Javascript (o haciendo un uso anecdótico):

- La ilusión del movimiento circular: http://codepen.io/jllodra/pen/dwJck
- Un texto scroller oldschool: http://codepen.io/jllodra/pen/BvpwJ
- Una especie de pulpo giratorio: http://codepen.io/jllodra/full/AceDp/
- Un pulpo más divertido: http://codepen.io/jllodra/full/nKzla

Todas estas animaciones se basan en la utilización de "keyframes" css, y transformaciones sencillas como translate, rotate, scale, y skew.

Espero que os gusten. Recordad que podéis encontrarme en twitter como @josep_llodra.
¡Un saludo!