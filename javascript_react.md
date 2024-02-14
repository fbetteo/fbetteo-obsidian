[[Typescript]]
# javascript 

best practice es poner el <script></script> de js al final del body despues de todo el contenido (al menos en html)

Vi cosas basicas que no requieren tomar notas so far

`{items.length === 0 ? <p> No item found </p> : null}`
Esto seria.. si lenght = 0, devuelve no item found, else null
# React


Library  para generar componentes en javascipt.
Permite returnear html, llamando dinamicamente a variables de javascript dentro de {}

Uno puede ir a getboostraped y buscar lo que va dentro del componente. Al copiarlo hay que cambiar todos los class por className porque class esta protegido en react o typescripts no se

si uno quiere devolver varias cosas, tipo un h1 y un ul, no puede porque javascript te permite devolver un solo elemento.
Se lo puede envolver todo dentro de un div y listo
sino..
#### Fragments
lo importas de react y remplazas el div con Fragment (creo que es mejor para el renderizado de la pagina)
Parece que en vez de importarlo uno puede poner <> y </> y listo


### Rendeing

Si uno quiere generar un componente que deuvelve una lista, los objetos de la lista se pueden hardcodear....
O "loopear".
Generas el array con los valores. 
`items = [aa,bb,cc]`
`items.map(item => <li key={item}>{item}</li>)`

Esa key deberia ser un id, es para que se pueda identificar, aca no pasa nada con poner item pero mejor algun id unico
y pones el items.map(bla bla) dentro de brackets en el return  

## Event

Aca vemos le damos una accion al ser clickeado (loguear el item en ese caso). Tengo que ver como se hacen cosas mas complejas como pegarle a api y etc
`items.map(item => <li key={item} onClick={() => console.log(item)}>{item}</li>)`

lo que se hace generalmente es sacar esa funcion de ahi y declararla antes para que quede mas prolijo

`const handleClick = (event: MouseEvent) => console.log(event);`

## State managing

Primero le agrega un classname al objeto de la lista, "active" que viene de bootstrap para highlightear. Hcae una funcion que dice que si al clickear el selectedIndex es igual al index clickeado, se active. Ademas prueba una fucnoin para que onClick, el selectedIndex se actualice al recien clickeado, pero no funciona por una cuestion de scope  y ahi viene el useState

useStates es un hook. Le decis que va a tener informacion que puede cambiar en el tiempo.

useState(-1) devuelve un array con el nombre de la variable y una funcion para setear el nuevo valor
hace asi
`const [selectedIndex, setSelectedIndex] = useState(-1)`
y despues en el return, en onClick le pasa `{ setSelectedIndex(index) }` lo cual actualiza el estado con el valor index.

## Props
 Agregar data a los componentes.
 Creo una interface de tipo Props y la paso como parametro del componente.
 Guardo los items a mostrar en la app principal y paso los valores como parametros
 ListGroup items={items} heading="Cities"
Y el componente en vez de esperar una interface (que se podria) lo modifica para recibir las properties de la interface y no tener que andar haciendo props.item, props.heading, etc

`function ListGroup({items, heading}: Props`


Handling Event
onSelect


## children

