1.1	Clasificar complejos 
O(1): es la búsqueda de uno en uno dentro en un conjunto de datos 
O(log n): Se debe cumplir que debe ser un sistema ordenado antes de realizar la búsqueda, se debe tener siempre en cuenta que se inicia por la mitad de el total de datos siempre realizando el corte si es por la parte superior si el dato es mayor o inferior  si el datos es pequeño

1.2 De la vida real al análisis
Guardar la ropa limpia en el clóset. Tomo una prenda, la doblo y la ubico en su cajón, y repito lo mismo con la siguiente. Cada prenda me cuesta más o menos el mismo tiempo, así que si tengo el doble de ropa me demoro el doble.
Si la entrada se hace 10 veces más grande (de 20 prendas paso a 200), el tiempo también se multiplica por 10. No se dispara ni se estanca: crece parejo con la cantidad, y eso es justo lo que significa O(n).
1.3 ¿Cuál escala mejor?
Recomendaría la Forma B, la O(n log n). Con n = 5 la Forma A puede ganar porque son tan pocos datos que la diferencia se siente en microsegundos y pesan más otras cosas (lo simple que sea el código, las operaciones internas de cada una). Pero eso solo se sostiene mientras n sea diminuto.
El problema es que O(n²) crece mucho más rápido. Con n = 1000, la Forma A hace del orden de 1.000.000 de operaciones y la Forma B unas 10.000: ya es 100 veces peor. Con n = 1.000.000 la diferencia se vuelve impagable. Como el enunciado dice que no sé qué tan grande será n en el futuro, prefiero la que aguanta el crecimiento, no la que gana en el caso chiquito.
1.4 Verdadero o falso
a. Falso. Big O no habla de segundos reales, habla de cómo crece el tiempo. Un O(1) puede tener una operación pesada por dentro y demorarse más que un O(n) que recorre solo 3 elementos. Lo que sí es cierto es que a medida que n crece, el O(1) termina ganando siempre.
b. Verdadero. Big O describe el comportamiento del crecimiento, no una medición en segundos. Por eso el mismo algoritmo es O(n) en un computador viejo y en uno nuevo, aunque los tiempos reales sean distintos.
c. Falso. Acceder por índice es O(1). El arreglo no se recorre: con la dirección base, el índice y el tamaño de cada casilla se calcula directamente en dónde está el dato. Pedir arreglo[5] cuesta lo mismo que pedir arreglo[0].
2.1 Diseña el arreglo
índice: 0 1 2 3 4 5
valor: [3.5] [4.2] [2.8] [5.0] [3.9] [4.5]
La tercera nota que ingresé es 2.8 y está en el índice 2. Esto pasa porque los índices arrancan en 0, entonces la posición "número 3" para una persona es el índice 2 para el computador.
Para llegar directo a la última nota sin recorrer nada:
ultima = notas[5]
o de forma más general, que sirve aunque cambie el tamaño:
ultima = notas[longitud(notas) - 1]
2.2 Direcciones de memoria
Base = 0x2000, cada valor ocupa 4 bytes.
notas[0] = 0x2000 + (0 × 4) = 0x2000
notas[3] = 0x2000 + (3 × 4) = 0x2000 + 12 = 0x200C
notas[5] = 0x2000 + (5 × 4) = 0x2000 + 20 = 0x2014
Acceder a notas[3] cuesta lo mismo que acceder a notas[0] porque el computador no va contando casillas hasta llegar. Solo hace una multiplicación y una suma con la fórmula, y con ese resultado va directo a esa dirección de RAM. Sea el índice 0, el 3 o el 500, siempre son las mismas dos operaciones, y por eso es O(1). Esto funciona gracias a que el arreglo está guardado seguido en memoria y todas las casillas ocupan lo mismo.
2.3 Búsqueda lineal vs. binaria
edades = [15, 18, 20, 23, 27, 31, 35, 40], busco el 31.
Búsqueda lineal (arranca en el índice 0 y va de uno en uno):
1.	¿15 es 31? No
2.	¿18 es 31? No
3.	¿20 es 31? No
4.	¿23 es 31? No
5.	¿27 es 31? No
6.	¿31 es 31? Sí, lo encontré en el índice 5.
Total: 6 comparaciones.
Búsqueda binaria (arranca por la mitad y va cortando):
Inicio = 0, fin = 7. Mitad = (0+7)/2 = 3, ahí está el 23.
1.	¿31 es 23? No, y como 31 es mayor, boto toda la mitad de abajo y me quedo con inicio = 4.
Inicio = 4, fin = 7. Mitad = (4+7)/2 = 5, ahí está el 31.
2.	¿31 es 31? Sí, lo encontré.
Total: 2 comparaciones.
La binaria solo sirve con el arreglo ordenado porque toda su lógica es poder botar la mitad de los datos de una. Cuando compara con el valor del centro y decide "me voy para arriba" o "me voy para abajo", está confiando en que todo lo que quedó de un lado es menor y todo lo del otro es mayor. Si el arreglo está desordenado esa suposición es falsa, entonces botaría una mitad donde tal vez sí estaba el dato y devolvería que no existe estando ahí.
Con 1 millón de elementos seguiría usando la binaria sin dudarlo. La lineal es O(n): en el peor caso son 1.000.000 de comparaciones. La binaria es O(log n): como en cada paso parte el problema a la mitad, con unas 20 comparaciones ya cubrió el millón entero (2^20 ≈ 1.048.576). La única condición es que el arreglo esté ordenado.
2.4 Insertar un elemento
Insertar al final habiendo espacio: es lo más barato que hay. Voy directo a la primera casilla libre, escribo el valor y actualizo el contador de cuántos elementos tengo. No toco a nadie más, así que es O(1).
Insertar al final sin espacio: toca crear un arreglo nuevo más grande, copiar los 5 valores viejos uno por uno al nuevo, y ahí sí meter el valor nuevo. Como hay que copiar todos los elementos, el costo depende de cuántos haya: es O(n).
Insertar el 21 en la mitad para que quede ordenado: primero busco dónde va. El 21 es mayor que 20 (índice 2) y menor que 23 (índice 3), entonces le toca el índice 3. Pero esa casilla está ocupada, así que tengo que correr un puesto a la derecha a todos los que están de ahí en adelante, y hacerlo empezando por el final para no sobrescribir datos: el 27 se pasa a la casilla siguiente, luego el 23, y ya con el índice 3 libre escribo el 21. Queda [15, 18, 20, 21, 23, 27]. Como en el peor caso (insertar al principio) hay que correr todos los elementos, es O(n). Y ojo que si además no había espacio, primero toca hacer el arreglo nuevo y copiar.
Comparando: insertar al final con espacio es O(1) porque es una sola operación sin importar el tamaño. Insertar en la mitad, o al final sin espacio, es O(n) porque el trabajo crece igual que la cantidad de datos. Esa es la debilidad grande de los arreglos: son buenísimos para leer (O(1)) pero caros para insertar y borrar en el medio.
