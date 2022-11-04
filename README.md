![HenryLogo](https://d31uz8lwfmyn8g.cloudfront.net/Assets/logo-henry-white-lg.png)
​
# Modelo de Machine Learning para el mercado inmobiliario colombiano.
​Por Ariel Serafini

​
El objetivo de este proyecto era implementar un modelo de clasificación que permita clasificar el precio de las propiedades en venta, utilizando los datos que se han puesto a su disposición correspondientes al año 2020. La categorización de las propiedades constó en dividirlas entre las consideradas baratas o caras, considerando como criterio el valor promedio de los precios. 


​
## Exploratory Data Analysis & Data Preprocessing
​
Se tenían a disposición dos datasets, cada uno con información detallada de ​propiedades a la venta en el territorio colombiano. Uno de ellos destinado a entrenar el modelo y el otro a ponerlo a prueba. 
En principio, se hizo un análisis exploratorio con tal de verificar que no había propiedades que no deberían pertenecer al dataset. Es decir, se verificó que todas las propiedades estén en Colombia, el tipo de operación sea la venta y que los precios estén expresados en una única moneda.
En este último apartado es donde se encontró cierta discrepancia: algunas propiedades detallaban su precio en pesos colombianos y otras en dólares estadounidenses. Se realizó una conversión de USD a COP con una cotización de $5069, valor en la fecha de autoría de este informe.

Los valores NaN no sufrieron ninguna modificación ni eliminación, a excepción de los de la columna de precios que fueron quitados ya que no aportaban información esencial para poder determinar la categorización de la propiedad. Los valores iguales a cero fueron mantenidos para hacer más fácil la posterior integración del OneHotEncoder.

Se decidió eliminar las columnas que no aportaban datos relevantes que afecten el precio del inmueble, como el Id, las fechas de los avisos o los datos geográficos.

Analizando el dataset, se encontró que estaba notoriamente desbalanceado. Es decir, había un mayor porcentaje de propiedades "baratas" que "caras". En uno de los modelos planteados aquí se realizó un equilibrado.

Por último, se utilizó el OneHotEncoder para transformar datos de texto a números. Esto es escencial ya que los modelos de ML sólo operan con números


## Machine Learning

Se optó por un modelo de árbol Histogram-based Gradient Boosting Classification. La particularidad de este estimador es que permite trabajar con datos NaN.


## Métrica a utilizar
​
Como método de evaluación del desempeño del modelo, se utilizó la métrica de Exhaustividad (Recall) para las propiedades caras, a partir de la matriz de confusión (Confusion Matrix). 
​
$$ Recall=\frac{TP}{TP+FN}$$
​
Donde $TP$ son los verdaderos positivos y $FN$ los falsos negativos.

Adicionalmente, se incluye la Accuracy como métrica de control.

El modelo que mejor valor obtuvo fue el equilibrado.

​
## Descripción de las dimensiones
- id - Identificador del aviso. No es único: si el aviso es actualizado por la inmobiliaria (nueva versión del aviso) se crea un nuevo registro con la misma id pero distintas fechas: de alta y de baja.
- ad_type - Tipo de aviso (Propiedad, Desarrollo/Proyecto).
- start_date - Fecha de alta del aviso.
- end_date - Fecha de baja del aviso.
- created_on - Fecha de alta de la primera versión del aviso.
- lat - Latitud.
- lon - Longitud.
- l1 - Nivel administrativo 1: país.
- l2 - Nivel administrativo 2: usualmente provincia.
- l3 - Nivel administrativo 3: usualmente ciudad.
- l4 - Nivel administrativo 4: usualmente barrio.
- l5 - Nivel administrativo 5.
- l6 - Nivel administrativo 6.
- rooms - Cantidad de ambientes.
- bedrooms - Cantidad de dormitorios (útil en el resto de los países).
- bathrooms - Cantidad de baños.
- surface_total - Superficie total en m².
- surface_covered - Superficie cubierta en m².
- price - Precio publicado en el anuncio.
- currency - Moneda del precio publicado.
- price_period - Periodo del precio (Diario, Semanal, Mensual)
- title - Título del anuncio.
- description - Descripción del anuncio.
- property_type - Tipo de propiedad (Casa, Departamento, PH).
- operation_type - Tipo de operación (Venta).
- geometry - Puntos geométricos formados por las coordenadas latitud y longitud. 
​
