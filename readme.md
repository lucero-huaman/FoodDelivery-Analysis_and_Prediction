Analisis de delivery y predicción de tiempo de entrega
=======================================================

 Los servicios de entrega o delivery son de gran importancia en la sociedad actual debido a la comodidad y accesibilidad que brindan a los consumidores. Al permitirles recibir productos y servicios directamente en su puerta, se ahorra tiempo y esfuerzo al evitar desplazamientos físicos. Además, el delivery amplía la variedad de opciones disponibles, permitiendo explorar diferentes alternativas desde la comodidad del hogar. Para garantizar la satisfacción del cliente y una experiencia fluida, es fundamental proporcionar un estimado del tiempo de entrega, ya que establece expectativas claras, ayuda en la planificación interna de las empresas y construye confianza al cumplir con los plazos acordados.  
 Para el siguiente trabajo se empleó el dataset [deliverytime.txt](https://github.com/lucero-huaman/FoodDelivery-Analysis_and_Prediction/blob/main/deliverytime.txt) que tiene como diccionario de datos a [link](https://github.com/lucero-huaman/FoodDelivery-Analysis_and_Prediction/blob/main/Diccionario%20de%20datos.png).

 ![](https://iili.io/HLiKs4a.png)

Carga de datos  Primero se importaron las bibliotecas necesarias para el manejo de datos y la generación de gráficas, posteriormente se importa el archivo csv.

 ![](https://iili.io/HL4A0iB.jpg)
 
Tratamiento de los datos
------------------------

 En esta sección se busco realizar un analisis más profundo de los datos y realizar correcciones o crear nuevos campos de ser necesarios.  
 Para poder identificar problemas con los datos se emplean comandos como .info y .describe que nos dan un vistazo general del estado actual de los datos.  
 Podemos visualizar que los tipos de datos son correctos, según la descripción proporcionada sobre cada columna. Adicionalmente, se imprimió la tabla para un contacto directo con los datos y se busco valores nulos y Nan, sin embargo, no se encontraron estos.  
 Tras los pasos realizados se pudo identificar la necesidad de conocer la distancia entre el lugar de entrega del pedido y el restaurante que lo proporcionará.

### Cálculo de la distancia

 Se aplica la Fórmula de Haversine para calcular la distancia entre dos puntos geográficos.  
 Haversine fórmula:   
 a = sin²(Δφ/2) + cos φ1 ⋅ cos φ2 ⋅ sin²(Δλ/2)  
 c = 2 ⋅ atan2( √a, √(1−a) )  
 d = R ⋅ c  
 Donde φ es latitud, λ es longitud, R es el radio de tierra (radio = 6,371km); Los ángulos han de estar en radianes.

 ![](https://iili.io/HL4ql24.jpg)
 
Análisis de correlación
-----------------------

 El análisis de correlación es una herramienta esencial para comprender y sacar el máximo provecho de los datos. Sirve para identificar patrones y hacer predicciones informadas, sin embargo, esto no implica causalidad.  
 En los resultados podemos observar que la mayor correlación es entre la latitud y longitud con la distancia, lo cual tiene sentido considerando que apartir de dichos datos se obtuvo la distancia, sin embargo, cada una individualmente no proporcionan la sufiente información, por otro lado tenemos una correlación negativa entre el rating y el tiempo de entrega, también existe una relación positiva entre la edad del repartidor junto con su tiempo de entrega.  
 Es importante que tengamos en cuenta que una correlación de 0 entre la distancia y el tiempo no implica que no haya una relacion entre estas variables, sino que existen distintos factores que influyen en este y que independientemente de la distancia el tiempo de entrega es altamente variable.

 ![](https://iili.io/HL4I9M7.jpg)
 
EDA
---

### Recuento por tipo de órdenes

 Se identifico que el tipo de orden más solicitada son los Snacks con 11533, seguido de los Platos clasicos con 11450, Bebidas con 11322 y Buffets con 11280. La cantidad de pedidos por órdenes son solicitados similares de veces.

 ![](https://iili.io/HL6qWWF.jpg)
 
 ### Recuento por tipo de vehiculos para la entrega

 Se identifico que el tipo de vehículo más usado es la Motocicleta con 26435 de veces, seguido del Scooter con 15276, Scooter eléctrico con 3814 y Bicicletas con 68. Las bicicletas son el tipo de vehículo menos usado por los repartidores en más de los 45000 pedidos realizados.

 ![](https://iili.io/HL65w12.jpg)
 
 ### Tiempo de entrega dependiendo del tipo de vehiculo y pedido

 Inesperadamente, en la gráfica se puede visualizar que la movilidad que tiene el rango de tiempo más estable de entregas es el Scotter, mientras que el más inestable es la Bicicleta, por otro lado la Motocicleta presenta más demora en las entregas. Podemos sugerir que factores que como tráfico retrasan la movilidad lo cual es una desventaja para las Motocicletas que únicamente pueden transitar por pista, a diferencia de los otros vehículos mencionados, además se debe considerar que repartidores que cuenten con motocicleta son asignados a realizar entregas de larga distancia.

 ![](https://iili.io/HL65vee.jpg) ![](https://iili.io/HLPgKeS.jpg)
 
 ### Relación entre la calificación del repartidor y el tiempo de entrega

 Como observamos en la correlación existe una correlación negativa entre la calificación del repartidor y el tiempo de entrega, esto quiere decir que a mayor sea la calificación del repartidor menor será el tiempo de entrega, esto implica que las rutas que tienen mayor conocimiento de rutas alternas que faciliten la llegada hacia el destino.

 ![](https://iili.io/HL65NgS.jpg)
 
 ### Relación entre la edad del repartidor y su calificación

 En la gráfica se puede observar que no hay una correlación considerable entre la edad del repartidor y su calificación, esto quiere decir que la edad no influye directamente en el rating del repartidor.

 ![](https://iili.io/HL65ed7.jpg)
 
 ### Relación entre la edad del repartidor y el tiempo de entrega

 Tal como se observó en el análisis de correlación existe una relación positiva entre la edad del repartidor y el tiempo de entrega.

 ![](https://iili.io/HL65k79.jpg)
 
 ### Relación entre la distancia y el tiempo de entrega

 Es importante que tengamos en cuenta que una correlación de 0 entre la distancia y el tiempo no implica que no haya una relacion entre estas variables, sino que existen distintos factores que influyen en este y que independientemente de la distancia el tiempo de entrega es altamente variable.

 ![](https://iili.io/HL6X6wQ.jpg)
 
SVM
---

 Se utiliza el algoritmo de Máquinas de Vectores de Soporte para realizar una predicción del tiempo estimado de entrega basado en diferentes características relacionadas con el repartidor y la entrega.  
 **Pasos:**  
 1- Se seleccionan las características que se utilizarán para entrenar el modelo y el objetivo que se quiere predecir (el tiempo de entrega). En este caso, las características incluyen la edad del repartidor, la calificación del repartidor, la distancia hacia el lugar de entrega, el tipo de orden y el tipo de vehículo utilizado. El objetivo es el tiempo que tomará la entrega.  
 2- Se divide el conjunto de datos en dos conjuntos: uno para entrenar el modelo (conjunto de entrenamiento) y otro para evaluar su rendimiento (conjunto de prueba).  
 3- Las características se escalan utilizando StandardScaler para que todas tengan una media de 0 y una desviación estándar de 1. Esto ayuda a mejorar la convergencia del modelo y asegurar que todas las características tengan el mismo peso.  
 4- Se crea una instancia del modelo SVR (Regresión de Vectores de Soporte) y se ajusta al conjunto de entrenamiento escalado. El modelo se entrena para encontrar una función que relacione las características con el tiempo estimado de entrega.  
 5- El código solicita al usuario que ingrese valores para las características del repartidor y el pedido con el fin de realizar una predicción del tiempo estimado de entrega.  
 6- Los valores ingresados por el usuario se escalan utilizando el mismo escalador utilizado durante el entrenamiento del modelo. Luego, el modelo SVM predice el tiempo estimado de entrega basado en estos inputs.  
 7- Finalmente, el código imprime el tiempo estimado de entrega calculado por el modelo SVM basado en los valores ingresados por el usuario.  
 El SVM es una técnica de aprendizaje supervisado que se puede utilizar tanto para problemas de clasificación como de regresión. En este caso, se utiliza como un modelo de regresión (SVR) para predecir una variable continua (el tiempo estimado de entrega) basada en características específicas relacionadas con el repartidor y el pedido. El SVM es útil para este tipo de problemas, ya que puede manejar datos no lineales y generalizar bien para datos nuevos, lo que lo hace adecuado para tareas de predicción

 ![Screenshot 29](https://iili.io/HL65P1V.jpg)
 
Conclusiones
------------

 Dentro del trabajo se trataron y analizaron los datos para predecir el tiempo estimado de entrega de un pedido, se pudo encontrar correlación negativa de 0.33 entre el tiempo y la calificación del repartidor, además de la influencia de la edad de este, así mismo, el tipo de vehículo usado para la entrega influye en este sobre todo si la distancia es amplia o no, se encontró que el vehículo con un rango de tiempo de entrega más estable independientemente del tipo de orden realizada es el Scooter clásico y eléctrico.  
 Para aumentar la precisión del modelo de predicción se sugiere la recopilación adicional de datos como: tráfico estimado, condiciones climáticas y festividades.

Visualizacion de los datos en Tableau
-------------------------------------

 Se recopiló los gráficos más importantes dentro de Tableau para facilitar la comprensión y manipulación de los datos proporcionados.

 [![](https://iili.io/HL67oB4.png)](https://public.tableau.com/app/profile/lucero.huam.n/viz/Fooddeliveryprediction/Dashboard1)
