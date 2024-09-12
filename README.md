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
