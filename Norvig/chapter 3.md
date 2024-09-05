
Cuando un agente tiene que encontrar el camino correcto (porque no es obvio) se lo llama **problem-solving agent** y el proceso computacional es **search**

Para search necesitas:
* una serie de estados posibles
* un estado inicial
* uno o varios estado objetivo (goal)
* acciones disponibles del agente para cada estado
* modelo de transicion. Te dice a que estado te lleva cada accion
* una funcion de costo de cada accion.


Cada estado tiene posibles acciones. Hay que elegir con cual seguir explorando. Es la esencia de search.


* Graph search: si chequea redundancia de paths
* Tree-like search si no hace ese chequeo.

#### Best first search

Enfoque generico, explorar el nodo que menos cueste ir. 
Hay que tener en cuenta o mantener cierta memoria de los estados a los que ya fuiste porque podes caer en un infinite loop. Chequear tambien por caminos redundantes (que te hacen pasar por lugares que ya pasaste,etc)


#### Metricas de performance

* Completitud: el algoritmo encuentra siempre alguna solucion si la hay o se da cuenta que no hay si no la hay?
* Cost optimality: Encuentra la solucion de menor costo?
* Time complexity: Cuanto le lleva encontrar la solucion? En tiempo o en cantidad de estados/acciones
* Space complexity: cuanta memoria requiere?

Para ser completo un algoritmo tiene que ser **sistematico** en el sentido de poder explorar un espacio infinito para poder llegar a cualquier goal state desde cualquier initial state.


### Breadth First Search
Cuando todas las acciones tienen el mismo costo, esta es una estrategia apropiada.
Explorar primero hacia los costados y luego profundiza.
Encuentra el minimo porque como se expande de la misma manera para todos lados, cuando encuentra uno es el de menor costo. Ya que exploro todos los caminos "paralelos" antes. Aca juega el punto de que cada accion debe valer lo mismo para asegurar que el primero encontrado es el de menor costo porque ya exploraste todo lo de profundidad menor.

Siempre es completo.

Space y time complexity crecen exponencialmente con la profundidad de la mejor solucion. En general esto explota mas rapido en terminos de memoria.


### Dijkstra (uniform cost search)

Si los costos por accion no son iguales, una opcion es usar best first donde la funcion de costo es el costo total desde el estado inicial al estado actual.
Dijsktra se expande en olas de mismo costo, mientras que breadth first se expande en olas de misma profundidad.

Es completo y cost optimal

La complejidad depende de la solucion optima y en un mal escenario puede ser peor que lo exponencial de Breadth First. Este algortimo puede explorar muchos caminos de poco costo que no llevan a ningun lado antes de ir por los caminos utiles.


### Depth First
Este expande siempre el nodo mas profundo de la frontera.
No es cost optimal, devuelve la primera solucion que encuentra.

Si es un arbol con estados finitos, es completo. 
Si hay ciclos puede quedar en loop infinito asi que hay implementaciones que chequean eso.
Si los estados son infinitos no es completo porque puede quedarse explorando un path infinito para siempre.

Cuando se usa? Para "tree-like" search problems, puede ser util porque requiere mucha menos memoria.
Hay ciertas areas donde se ahorra mucha memoria comparado con Breadth first (ver libro pg 80)

Backtracking es una implementacion que usa aun menos memoria.

Hay otras implementaciones que ponen un limite de profundidad y constraints asi pero hay que usarlas con cierto domain knowledge porque podes hacer que nunca encuentre la solucion.

**Iterative Deepening Search** es un intermedio quizas entre depth y breadth. Va probando iterativamente distintas profundidades.
Tiene tradeoffs de memoria y espacio. Ver bien

>In general, iterative deepening search is the preferred uninformed search method when the search state space is larger than can fit in memory and the depth of the solution is not known.

### Bidirectional search

Haces la busqueda desde los dos extremos en simultaneo. Necesitas dos fronteras y dos tablas de reached state, etc

**3.4.5 tiene una tabla comparando las metricas**


## Informed (Heuristic) Search Strategies

Implica proveer cierta información acerca de donde se encuentran los goals al algortimo.
Esta información suele ser una heurística h(n) acerca del costo hasta el goal desde cada estado. (Una estimación sencilla)

### Greedy Best First Search

Expandis primero el nodo de menor h(n) es decir, aquel que bajo tu heuristica es el de menor costo hasta el Goal. Trata de acercarse lo más posible siempre y por eso puede llevarte a camino No optimos.


### A* search

Uno de los algoritmos más conocidos de este estilo. La funcion de evaluación es f(n) = g(n) + h(n) donde g(n) es el costo desde el estado inicial hasta n y h(n) es el estimado (heuristica) hasta el goal.
Elige el mejor camino estimado teniendo en cuenta el pasado tambien. 
A* es completo y puede ser cost optimal si la heurística es **admissible** (una que nunca sobre estima el costo de llegar al goal, es decir, es optimista)

Mas alla de poder ser completo, cost optimal y eficiente puede tener problemas de memoria con crecimiento exponencial en el caso de haber muchos paths equivalentes (hay ej en pg 90), que haria que tuviera que visitar todos.


Existen por ellos "satisfying solutions". Si aceptamos soluciones no optimas pero suficientemente buenas se pueden explorar muchos menos nodos y evitar problemas de memoria y tiempo. Con heuristica inadmisible tenemos un caso asi. 

Otra alternativa es Weigthed A* , donde se le pone mas peso a h(n).  f(n) = g(n) + W * h(n).
Esto generalmente devuelve soluciones no optimas pero mas rapido y segun el libro, no suele estar tan alejado de lo optimo por mas que teoricamente puede ser mucho peor.

Existen **bounded** y **unbounded** cost search donde se le pone o no limite a la diferencia respecto al optimo. Ejemplo unbounded -> speedy search. Pero no da muchisimo detalle.