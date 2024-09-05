Comportamiento del agente depende "agent function" que mapea secuencia de inputs a acciones.

> As a general rule, it is better to design performance measures according to what one actually wants to be achieved in the environment, rather than according to how one thinks the agent should behave.

Ejemplo de la aspiradora automatica... el objetivo tiene que ser tener el piso limpio, y no por ejemplo, maximizar cantidades limpiadas. Maximizar cantidades limpiadas puede ser limpiar, ensuciar, limpiar de nuevo en loop... uno lo que quiere es una metrica del suelo limpio, quizas penalizado por consumo de energia y etc como para hacerlo eficiente.

Rational agent maximizes "expected performance"

Uno quiere que el rational agent sea autonomo, que aprenda del entorno para compensar la falta de informacion que tenga.

Si no se puede ver todo el entorno, probablemente el agente necesite mantener un estado interno para tener nocion del estado actual del mundo (que no se puede ver todo)

Hay distintos tipos de agentes, de mas sencillos a mas complejos
* simple reflex agents
* model based reflex agents
* goal based agents
* utility based agents
