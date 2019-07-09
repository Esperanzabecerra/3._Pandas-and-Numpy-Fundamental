# 1. Introduction to NumPy

- Una de las razones por las que el lenguaje Python es extremadamente popular es que facilita la escritura de programas. Debido a que Python es un lenguaje de alto nivel , no tiene que preocuparse por cosas como asignar memoria en su computadora o elegir cómo se realizan ciertas operaciones con el procesador de su computadora. En contraste, cuando usa lenguajes de bajo nivel como C , define exactamente cómo se administrará la memoria y cómo el procesador ejecutará sus instrucciones. Esto significa que la codificación en un lenguaje de bajo nivel lleva más tiempo; Sin embargo, tienes más capacidad para optimizar tu código para correr más rápido.

Tipo de idioma	    Ejemplo	   Tiempo necesario para escribir el programa.	Control sobre el rendimiento del programa.
Nivel alto	        Python	                    Bajo	                                         Bajo
Nivel bajo	          C	                        Alto	                                         Alto

* Al elegir entre un lenguaje de alto y bajo nivel, usted hace una compensación entre poder trabajar rápidamente y crear programas que se ejecutan de manera rápida y eficiente. Afortunadamente, hay dos bibliotecas de Python que nos ofrecen lo mejor de ambos mundos: NumPy y pandas . Juntos, pandas y NumPy proporcionan un poderoso conjunto de herramientas para trabajar con datos en Python porque nos permiten escribir código rápidamente sin sacrificar el rendimiento.

* Aprenderemos sobre NumPy y los conceptos fundamentales que hacen de Python uno de los lenguajes de programación más populares para la ciencia de datos. A medida que aprendemos, analizaremos conjuntos de datos del mundo real, comenzando con los datos de viaje en taxi de la ciudad de Nueva York.

# Entendiendo la Vectorización

- En los dos primeros cursos, utilizamos listas de listas para representar conjuntos de datos. Si bien las listas de listas son suficientes para trabajar con conjuntos de datos pequeños, no son muy buenas para trabajar con conjuntos de datos más grandes. La biblioteca NumPy resuelve este problema.

- Veamos un ejemplo donde tenemos dos columnas de datos. Cada fila contiene dos números que deseamos sumar. Usando solo Python, podríamos usar una lista de estructura de listas para almacenar nuestros datos, y usar los bucles para iterar sobre esos datos:

* my_numbers = [ [6, 5], [1, 3], [5, 6], [1, 4], [3, 7], [5, 8], [3, 5], [8, 5]] Representación de listas de listas
* sum = []                              Reprensentación de código de Python para cada columna
* for row in my_numbers:
*     row_sum = row[0] + row[1]
*     sums.append(row_sum)
- En cada iteración de nuestro bucle, el intérprete de Python convierte nuestro código en un código de bytes , y el código de bytes le pide al procesador de nuestra computadora que sume los dos de cada columna números: Proe ejemplo 6+5=11 

- Nuestra computadora tomaría ocho ciclos de procesador para procesar las ocho filas de nuestros datos.

- La biblioteca NumPy aprovecha una función del procesador denominada Datos múltiples de instrucción única (SIMD) para procesar los datos más rápido. SIMD permite que un procesador realice la misma operación, en múltiples puntos de datos, en un solo ciclo de procesador: Osea que hace las sumas al mismo tiempo no una por una.

* Bytecode: también denominado código portátil o p-code , es una forma de conjunto de instrucciones diseñada para una ejecución eficiente por un intérprete de software . A diferencia del código fuente legible para el ser humano , los códigos de bytes son códigos numéricos compactos, constantes y referencias (normalmente direcciones numéricas) que codifican el resultado del análisis del compilador y el análisis semántico de cosas como tipo, alcance y profundidad de anidamiento de objetos de programa.

- Como resultado, la versión NumPy de nuestro código solo tomaría dos ciclos de procesador, ¡una aceleración de cuatro veces! Este concepto de reemplazo de bucles con operaciones aplicadas a múltiples puntos de datos a la vez se denomina vectorización .

- La estructura de datos central en NumPy que hace posible la vectorización es la matriz ndarray o n-dimensional . En programación, la matriz describe una colección de elementos, similar a una lista. La palabra n-dimensional se refiere al hecho de que ndarrays puede tener una o más dimensiones. Comenzaremos trabajando primero con ndarrays unidimensionales (1D).

- Para usar la biblioteca NumPy, primero debemos importarla a nuestro entorno Python. NumPy se importa comúnmente usando el alias np:
* import numpy as np

-Luego, podemos convertir directamente una lista a un ndarray usando el numpy.array()constructor . Para crear un ndarray 1D, podemos pasar en una sola lista:
* data_ndarray = np.array([5, 10, 15, 20])
* type(data_ndarray)
* numpy.ndarray

* Utilizamos la sintaxis en np.array() lugar numpy.array() de nuestro import numpy as npcódigo. Cuando introducimos una nueva sintaxis, siempre usaremos el nombre completo para describirlo, y deberá sustituirlo en la taquigrafía, según corresponda.

- Practiquemos creando ndarrays 1D a continuación.
* import numpy as np
* data_ndarray = np.array([10, 20, 30])

# Datos de NYC Taxi-Aeropuerto
- En la última pantalla, practicamos la creación de un ndarray 1D. Sin embargo, los ndarrays también pueden ser bidimensionales:
                                                   Número de dimensiones     Conocido como 
__________
l__l__l__l    3 columnas de cuadros con una fila              1             Formación de una dimensional, lista, vector, secuencia 

______________
l__l__l__l___l
l__l__l__l___l   4 columnas por 4 filas                        2            Dos dimensiones, matriz, tabla de lista de lista, 
l__l__l__l___l                                                              Hoja de cálculo
l__l__l__l___l

* A medida que aprendemos sobre ndarrays bidimensionales (2D), analizaremos los datos de los viajes en taxi publicados por la ciudad de Nueva York.  https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page

- Solo trabajaremos con un subconjunto de estos datos: aproximadamente 90,000 viajes en taxi amarillo desde y hacia los aeropuertos de la Ciudad de Nueva York entre enero y junio de 2016. A continuación, encontrará información sobre las columnas seleccionadas del conjunto de datos:

* pickup_year: El año del viaje.
* pickup_month: El mes del viaje (enero es 1, diciembre es 12).
* pickup_day: El día del mes del viaje.
* pickup_location_code: El aeropuerto o barrio donde comenzó el viaje.
* dropoff_location_code: El aeropuerto o barrio donde terminó el viaje.
* trip_distance: La distancia del viaje en millas.
* trip_length: La duración del viaje en segundos.
* fare_amount: La tarifa base del viaje, en dólares.
* total_amount: El monto total cobrado al pasajero, incluidas todas las tarifas, peajes y propinas.

- Puede encontrar información sobre todas las columnas en el diccionario de datos del conjunto de datos. https://s3.amazonaws.com/dq-content/289/nyc_taxi_data_dictionary.md

* Nuestros datos se almacenan en un archivo CSV llamado nyc_taxis.csv.

- ¿Te parece familiar? Desplácese hacia arriba y compare esta tabla con el diagrama del ndarray 2D anterior. Puede imaginar ndarrays 2D como datos de almacenamiento como esta tabla.

- Para convertir el conjunto de datos en un ndarray 2D, primero usaremos el csvmódulo incorporado de Python para importar nuestro CSV como una "lista de listas". Luego, convertiremos la lista de listas a un ndarray. Volveremos a utilizar el numpy.array()constructor, pero para crear un ndarray 2D , pasaremos a nuestra lista de listas en lugar de una sola lista:

* # Nuestra lista de listas se almacena como data_list
* data_ndarray = np.array(data_list)

- EJERCICIO:  ¡Convirtamos nuestro taxi CSV en un ndarray NumPy!
* import csv
* import numpy as np

* #importar nyc_taxi.csv como listas de listas
* f = open("nyc_taxis.csv", "r")
* taxi_list = list(csv.reader(f))

* #eliminar la fila del encabezado
* taxi_list = taxi_list[1:]

* #Convertir todos los valores a flotadores
* converted_taxi_list = []
* for row in taxi_list:
*     converted_row = []
*     for item in row:
*         converted_row.append(float(item))
*     converted_taxi_list.append(converted_row)

* #Nuestra lista de listas se almacena como data_list      
* taxi = np.array(converted_taxi_list)                Agregue una línea de código usando el numpy.array()constructor para convertir la converted_taxi_listvariable en una ndarray NumPy.  Asigne el resultado al nombre de la variable taxi.

# Formas de Matriz
- Echemos un vistazo a los datos en la taxivariable de la pantalla anterior imprimiéndolos usando la print()función de Python :
* print(taxi) [[  2016      1      1 ...  11.65  69.99      1]
*  [  2016      1      1 ...      8   54.3      1]
*  [  2016      1      1 ...      0   37.8      2]
*  ...
*  [  2016      6     30 ...      5  63.34      1]
*  [  2016      6     30 ...   8.95  44.75      1]
*  [  2016      6     30 ...      0  54.84      2]]  Los puntos suspensivos (...) entre filas y columnas indican que hay más datos en nuestro Ndarray NumPy de los que se pueden imprimir fácilmente.

- Sin embargo, a menudo es útil conocer el número de filas y columnas en un ndarray. Cuando no podemos imprimir fácilmente toda la ndarray, podemos usar el ndarray.shapeatributo en su lugar:

* data_ndarray = np.array([[5, 10, 15], [20, 25, 30]])
* print(data_ndarray.shape)    (2, 3)
* El tipo de datos devuelto se llama una tupla. Las tuplas son muy similares a las listas de Python, pero no pueden modificarse.
-  La salida nos da algunos datos importantes:
* El primer número nos dice que hay 2 filas en data_ndarray.
* El segundo número nos dice que hay 3 columnas en data_ndarray.

- EJERCICIO: Confirmemos el número de filas y columnas en nuestro conjunto de datos a continuación.
* taxi_shape=taxi.shape       Asigna la forma de taxia taxi_shape. Imprime el resultado.
* print(taxi_shape)



# 2. Boolean Indexing with NumPy

# 3. Introduction to Pandas

# 4. Exploring Data with Pandas: Fundamentals

# 5. Exploring Data with Pandas: Intermediate

# 6. Data Cleaning Basics

# 7. Guiaded Proyect: Exploring Ebay Car Sales Data

