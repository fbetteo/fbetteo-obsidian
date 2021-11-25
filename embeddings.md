# Embeddings
#datascience 
Como concepto el embedding es representar una variable no numérica como un vector de cierta dimensión a determinar.   
Ejemplo típico en NLP es representar una palabra como un vector de números. Los valores de los vectores se aprenden en entrenamiento e idealmente deberían representar atributos de la variable aunque no sea intuitivo a simple vista. Por ejemplo, la distancia (coseno/euclidea? - revisar) de estos vectores entre variables similares debería ser menor que entre variables disímiles.

La manera habitual de obtener estos vectores (o _entity embeddings_) es a través de entrenamiento supervisado de una red neuronal. Los embeddings son los pesos resultantes de la capa que une la variable con la capa destinada a esto.

En _Guo, Berkhan (2016)_ se muestra cómo generar embeddings para variables one hot encoded resulta en mejor generalización de la red que con las variables sparse. A su vez, estos embeddings pueden ser utilizados en otros modelos donde se use la variable que originalmente era one hot encoded, obteniendo mejores resultados.

Como ejemplo, cada variable X es uno de los valores one hot encoded de _dia de la semana_ (input layer roja). La output layer Y (verde) es el output de la tarea de clasificación que aplica a nuestro problema.  
Lo interesante es la capa azul hidden, que es fully connected con la input. Cada X va a tener una serie de pesos que la unen a la capa azul y que sólo van a estar activos cuando la X toma el valor 1. Cada uno de esos sets de pesos corresponde al embedding de ese valor, por lo tanto cada día de la semana tiene su propia representación mediante pesos, ya que cada peso va a ser el valor que llega a la capa azul (dado que las otras X no están activas). Comunmente la red neuronal tiene otras capas intermedias donde los embbedings pasan a ser inputs.

![[embbeding.png]]

# Referencias
