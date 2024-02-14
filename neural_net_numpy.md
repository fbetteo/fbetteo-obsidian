 #### Padding
 X_pad = np.pad(X, ((0,0),(pad,pad), (pad,pad), (0,0) ), 'constant', constant_values=0)


#### Keras 
Ver notebooks/ejercicios del curso 4 de Andrew Ng.
* sequential() -> permite generar una red secuencial es decir donde cada capa sigue a la anterior de manera directa y única. Sin ramificaciones, cosas multiples ni nada. Está bueno para cosas mas bien simples-
* Functional API. -> Permite redes mucho más custom, pensando cada capa como parte de un grafo donde las cosas no tienen que ser secuenciales hacia adelante.