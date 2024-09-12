# Problema

Recopilamos 6 conjuntos de datos de la plataforma Kaggle sobre el precio de vehículos usados ​​o de segunda mano. Los fabricantes de vehículos que recopilamos fueron los siguientes: Ford, Hyundai, Toyota, Audu, BMW y Mercedez-Benz

# Características del conjunto de datos

Manufacturer: Fabricantes de vehículos de las respectivas marcas mencionadas anteriormente.
Model: Modelo del vehículo.
Year: Año de fabricación del modelo.
Mileage: Número de millas recorridas por el vehículo.
EngineSizer: Tamaño del motor del vehículo.
Tax: Importe del impuesto por vehículo.
MPG: Coste del combustible en millas por galón.

# ¿Cuál es el precio del vehículo?

Desarrollar un modelo predictivo que permita estimar el precio de los vehículos de las marcas: Ford, Toyota, Hyundai, BMW, Audi y Mercedes-Benz, en base a las variables explicadas anteriormente.

# Descubrimientos importantes

![mean_price](https://user-images.githubusercontent.com/85312561/187807320-c551598b-4eae-4f27-a781-66e90d4d13ed.png)

Como se puede observar en marcas como Mercedes-Benz, Audi y BMW, tienen un precio medio más elevado, ya que son fabricantes de vehículos de alta gama.

![matrix plot](https://user-images.githubusercontent.com/85312561/187807773-39d1df0b-e2a0-408f-b1f2-f186d695380e.png)

* Generalmente entre más reciente sea el modelo, el precio aumentará, ya que los vehículos van mejorando con el tiempo.
* Cuanto más kilómetros recorra el vehículo, el precio baja, ya que el modelo se desgasta, siempre y cuando sea el mismo modelo.
* La cilindrada influye positivamente en el precio del vehículo, ya que el apartado técnico de los vehículos aumenta.

## Limpieza de datos

![unique_values](https://user-images.githubusercontent.com/85312561/187808831-3c54e23e-398b-4c15-8d00-c1b76ad96871.png)

Hay modelos de vehículos que se repiten menos de 10 veces, por lo que es mejor optar por eliminarlos, ya que pueden afectar al rendimiento del modelo, ya que estas observaciones aparecen muy poco.

Se optó por crear varios subconjuntos según el fabricante del vehículo con el fin de dar una mejor limpieza a los datos, para la variable precio, ya que es la variable a estimar.

![mpg](https://user-images.githubusercontent.com/85312561/187809290-6a363bdd-e9c4-41ed-98c0-5d7669950e30.png)

Creamos un rango superior para la variable MPG. Para encontrar un intervalo más exacto, decidimos calcularlo según el tipo de combustible del vehículo.
Tiene sentido que los vehículos híbridos tengan un MPG, ya que dependen de sus motores eléctricos. Tomamos el intervalo más grande para seleccionar todos aquellos valores que sean menores a esa cantidad.

## *Procesamiento de Datos*

Las variables de carácter nominal, como el modelo o el fabricante, requieren una transformación llamada One Hot Encoding. Se crean varias variables dummy en función del número de estas. En caso de que se cumpla la condición se asignará un 1, mientras que las demás columnas se rellenarán con un 0.

Como resultados llegamos a un número de 119 variables de entrada. Además de tener alrededor de 50.000 observaciones. El cual el mejor modelo que cumple con estas características es el XGBoost. En lugar de utilizar una red neuronal, ya que utilizarla requiere más conjuntos de datos. XGBoost no requiere ningún tipo de escalado, ya que aplica inecuaciones matemáticas sobre cada estimador.

## *XGBoost*

<img src = "https://miro.medium.com/max/560/1*85QHtH-49U7ozPpmA5cAaw.png">

Es un algoritmo que utiliza otros modelos más simples, generalmente árboles de decisión. Cada árbol se vuelve mejor según la proporción de la tasa de aprendizaje del usuario. Una tasa de aprendizaje baja permite el mayor uso de estimadores.

Además de evitar que el modelo sobreajuste los datos de entrenamiento, tiene la ventaja adicional de que el modelo se puede entrenar utilizando una GPU, acelerando la velocidad de entrenamiento.

#### *Parámetros comunes*
* max_depth: Profundidad máxima de cada árbol.
* n_estimators: Número de árboles de decisión.
* learning_rate: Margen de mejora de cada estimador.
* subsample: Número de muestras de entrenamiento. En lugar de seleccionar el 100% de los datos, puede utilizar el 80 o el 85% de los datos. Evitar que el modelo sobreajuste los datos de entrenamiento

### *Número de estimadores ideales*

Utilizamos el error cuadrático medio, que mide el error promedio entre el valor original y el predicho.

Evaluamos el MSE según el número de estimadores, teniendo cuidado de que la función de pérdida no disminuya únicamente para los datos de entrenamiento. A cada propuesta de modelo se le asigna una tasa de aprendizaje de 0,01. Para que el modelo se generalice mejor.

![diagrama de estimadores](https://user-images.githubusercontent.com/85312561/187816326-687aa2ce-8adc-423b-a8c4-d3590b2be2b7.png)

Utilizamos de 100 a 1000 estimadores para encontrar el número óptimo de árboles. Según la profundidad máxima del árbol, de esta manera nos aseguramos de que el modelo no se sobreajuste.

### *GridSearchCV*

Nos apoyamos en la función GridSearchCV que busca la mejor combinación de parámetros según la validación cruzada. Gracias a esta herramienta nos aseguramos de que el modelo se generalice bien.

Como modelo final de la siguiente combinación de parámetros:

* max_depth = 7
* learning_rate = 0.01
* n_estimators = 1000
* subsample = 0.85
* gamma = 10

### *Importancia de las características*

![plot_importance](https://user-images.githubusercontent.com/85312561/188053598-d9e75b84-7e25-4ce4-8929-8f80722e0c9b.jpeg)

* Si el vehículo tiene transmisión Manual, el precio del vehículo generalmente disminuye, es menos costoso con transmisión automática o semiautomática.
El tamaño del motor es una variable que tiene mucho peso. Ya que a mayor cilindrada, mayor capacidad técnica del auto.

* El MPG en algunos países es el principal productor, para la compra de un vehículo. Ya que habrá personas que busquen un auto económico en cuanto a consumo de combustible.

* El año de fabricación incluye en el precio, ya que si se trata de un vehículo del mismo modelo. Encarecerá el precio del auto, ya que será un modelo más reciente.

Estas son las variables que más influyen en el modelo. Las demás variables pueden ayudar a complementar el resultado.

Por ejemplo, en esta lista de las variables más importantes no aparece la variable kilometraje. Es bien sabido que generalmente un vehículo con más kilometraje tiende a bajar el precio.

## *Producción*

Por último, se desarrolla aún más una interfaz gráfica práctica, que permite a los usuarios ingresar nuevos datos de una manera amena. Posteriormente, utilizando Streamlit Cloud, lo enviamos a un servidor gratuito, que permite a los usuarios utilizarlo. Streamlit es una librería que permite crear fácilmente aplicaciones web que se ocupen del trabajo con datos.

# *Resumen*

Desarrollamos todos los procesos que requiere un proyecto de ciencia de datos. Limpiamos los datos con el fin de mejorar el rendimiento de nuestro modelo.

Optamos por utilizar XGBoost ya que este algoritmo funciona bastante bien para conjuntos de datos moderados. Además, las variables continuas no requieren ningún tipo de preprocesamiento y como si fuera poco, permite el entrenamiento a través de GPU. Esto se traduce en un entrenamiento mucho más eficiente de lo que haríamos con una CPU.

El modelo creado se utiliza para predecir nuevos valores, para verificar su rendimiento. Como estaba seguro de su rendimiento, desarrollé una aplicación para el modelo de Inteligencia Artificial. Con elin que los usuarios puedan ingresar datos de una mejor manera.
