[[javascript_react]]


Superset of javascript

tsconfig.json: configuraciones de typescript. *target* es para definitr a que version de javascript tiene que transpilar por ej.

"?" despues del parametro lo hace opcional pero despues hay que manejarlo ne la funcoin, mejor poner un valor default 

tambien se puede usar para validar que un objeto tenga el valor.por ejemplo, agarrar el primer objeto de un array solo si existe. En vez de chequear con if que exista..
`lista?[0]`

"readonly" hace que no pueda ser modificado un valor en un objeto

`let employee: {`
	`readonly id: number,` 
	`name: string`
`} = { id:1, name:'Mosh'}`

employee.id = 4 va a tirar error


"|" para declarar que un parametro puede tener uno u otor tipo.
"&" (intersection). El parametro tiene que ser "ambos"

type es para definir un tipo custom.