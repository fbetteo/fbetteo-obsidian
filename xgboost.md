# Xgboost
#datascience 
### Xgboost como algoritmo

Xgboost es un algoritmo/librería que busca optimizar la metodología de Gradient Boosting. Extreme gradient boosting. Por ende, en el fondo es una versión nueva de la metodología anterior. A su vez le agrega parámetros de regularización que antes no estaban presentes.

### Gradient booster en general

Algoritmo basado en _weak learners via ensemble_. Esto significa que se generan muchos modelos débiles o simples y la predicción final se hace con todos de manera conjunta.  
En este caso son modelos que se van acumulando secuencialmente.

Cada weak learner es un modelo de tipo _árbol_, que funciona separando la data mediante cortes en las variables (categóricas o continuas). No hay parámetros como en un red neuronal o modelo lineal.  
La idea es optimizar la predicción mediante Gradient Descent pero no se puede hacer la derivada $\frac{\partial L}{\partial \theta}$ porque son árboles por lo tanto para cada observación podés quedarte con la dirección en la cual deberías modificar tu predicción (pero sin hablar de parámetros.).  
**El siguiente weak learner por lo tanto va a tener como variable Target el residuo del weak learner anterior.**

La nueva predicción va a ser la predicción del primero modelo + la del segundo, multiplicado por un learning rate para no overfitear. De eso, obtenemos un nuevo residuo y continuamos hasta algún criterio de parada (máxima cantidad de árboles o ganancia imperceptible)

### Referencias
https://xgboost.readthedocs.io/en/latest/  

https://maelfabien.github.io/machinelearning/GradientBoost/#gradient-boosting-steps  

https://ftvalentini.github.io/misc-notebooks/gradient-boosting.html#gradient_boosted_trees  

https://datascience.stackexchange.com/questions/16904/gbm-vs-xgboost-key-differences
