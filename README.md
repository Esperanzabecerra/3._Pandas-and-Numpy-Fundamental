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

 # Selección y corte de filas y elementos de ndarrays

- A continuación, veamos una comparación entre trabajar con ndarrays y la lista de listas para seleccionar una o más filas de datos:
*                                     Metodo de lista de listas             Metodo de NumPy
* Seleccionando una sola fila 1         sel_lol=data_lol[1]                 sel_np=data_np[1]
* Seleccionando múltiples filas 2-4     sel_lol=data_lol[2:]                sel_np=data_np[2:]
* La misma sintaxis de lista de listas producida en ndarray 1D y 2D

- Como se muestra arriba, podemos seleccionar filas en ndarrays de manera muy similar a las listas de listas. En realidad, lo que estamos viendo es una especie de atajo. Para cualquier matriz 2D, la sintaxis completa para seleccionar datos es:

* ndarray[row_index,column_index]
* #O si quieres seleccionar todo las columnas para un conjunto de filas:
* ndarray[row_index]

- Donde row_indexdefine la ubicación a lo largo del eje de fila y column_indexdefine la ubicación a lo largo del eje de columna.

- Al igual que las listas, la división de matrices es desde el primer índice especificado hasta, pero sin incluir, el segundo índice especificado. Por ejemplo, para seleccionar los elementos en el índice 1, 2y 3, tendríamos que utilizar la rebanada [1:4].

- Así es como seleccionamos un solo elemento de un ndarray 2D:
*                                          Metodo de lista de listas                Metodo de NumPy
* Selecciono 1 elemento columna3 fila2       sel_lol=data_lol[1][3]                sel_np=data_np[1,3]
* Con una lista de listas, utilizamos dos pares separados de corchetes consecutivos. Con NumPy ndarray, usamos un solo par de corchetes con ubicaciones de filas y columnas separadas por comas.

EJERCICIO:     Desde el taxindarray:
* row_0 = taxi[0]                   Seleccione la fila en el índice 0. Asignarlo a row_0.
* rows_391_to_500 = taxi[391:501] Seleccione cada columna para las filas en los índices 391a 500inclusivo. Asignarlos a rows_391_to_500.
* row_21_column_5 = taxi[21,5]    Seleccione el elemento en el índice de fila 21y el índice de columna 5. Asignarlo a row_21_column_5.

# Selección de columnas y ndarrays de corte personalizado

Continuemos aprendiendo cómo seleccionar una o más columnas de datos:

* Realizar en ndarray 1D:               Metodo de lista de listas             Metodo de NumPy
* Seleccionando una sola columna 3      sel_lol=[]                            sel_np=data_np[:,3]
*                                       for row in data_lol:               
*                                           col4=row[3]
*                                           sel_lol.append(col4)

* Realizar en ndarray 2D:               Metodo de lista de listas             Metodo de NumPy
* Seleccionando multiples columnas 1,2. sel_lol=[]                            sel_np=data_np[:,1:3]
*                                       for row in data_lol:               
*                                           col23=row[1,3]
*                                           sel_lol.append(col23)


* Realizar en ndarray 2D:               Metodo de lista de listas             Metodo de NumPy
* Seleccionando multiples columnas      sel_lol=[]                            cols = [1,3,4]
* con especificación de columnas        for row in data_lol:                  sel_np=data_np[:,cols]
* no consecutivas columnas 1,3.4            cols=[row[1], row[3], row[4]]
*                                           sel_lol.append(cols)

- Con una lista de listas, necesitamos usar un bucle for para extraer columnas específicas y agregarlas de nuevo a una nueva lista. Con ndarrays, el proceso es mucho más simple. Nuevamente usamos corchetes individuales con ubicaciones de filas y columnas separadas por comas, pero usamos dos puntos ( :) para las ubicaciones de las filas, lo que nos da todas las filas.

- Si queremos seleccionar un segmento parcial 1D de una fila o columna, podemos combinar un solo valor para una dimensión con un sector para la otra dimensión:

* Realizar en ndarray 1D:                                Metodo de lista de listas             Metodo de NumPy
* Seleccionando la fila 2 desde la columna 1 a la 3.     sel_lol= data_lol[2],[1:4]            sel_np=data_np[2,1:4]

* Realizar en ndarray 1D:                                Metodo de lista de listas             Metodo de NumPy
* Seleccionando la columna 4 desde la fila 1 a la 4.     sel_lol= []                           sel_np=data_np[1:,4]
*                                                        rows=data_lol[1:]
*                                                        for r in rows:
*                                                            col5= r[4]
*                                                            sel_lol.append(col5)

- Por último, si queremos seleccionar un sector 2D, podemos usar sectores para ambas dimensiones:

* Realizar en ndarray 2D:                                 Metodo de lista de listas             Metodo de NumPy
* Seleccionando las columnas 0,1,2 desde la fila 1 ala 3. sel_lol= []                           sel_np=data_np[1:4,:3]
*                                                         rows=data_lol[1:4]
*                                                         for r in rows:
*                                                             new_row= r[:3]
*                                                             sel_lol.append(new_row)

- EJERCICIO:
* columns_1_4_7 = taxi[:,[1,4,7]]                Seleccionar todas las filas de las columnas a índices 1, 4 y 7
* row_99_columns_5_to_8 = taxi[99,5:9]           Seleccione las columnas en los índices 5para 8incluir para la fila en el índice 99.
* rows_100_to_200_column_14 = taxi[100:201,14]   Seleccione las filas en los índices 100para 200incluir para la columna en el índice 14.

# Vector matematica
- Como vimos en las dos últimas pantallas, los ndarrays NumPy nos permiten seleccionar datos con mucha más facilidad. Más allá de esto, la selección que hacemos es mucho más rápida cuando se trabaja con operaciones vectorizadas porque las operaciones se aplican a múltiples puntos de datos a la vez.

- Cuando hablamos por primera vez de operaciones vectorizadas, usamos el ejemplo de agregar dos columnas de datos. Con los datos en una lista de listas, tendríamos que construir un bucle for y agregar cada par de valores de cada fila individualmente:

- En ese momento, solo hablamos sobre cómo las operaciones vectorizadas hacen que esto sea más rápido; sin embargo, las operaciones vectorizadas también hacen que nuestro código sea más fácil de ejecutar. Así es como realizaríamos la misma tarea anterior con operaciones vectorizadas:

* # Convertir la lista de listas a un ndarray
* my_numbers = np.array(my_numbers)

* # Seleccionar cada una de las columnas el resultado de cada uno será un ndarray 1d
* col1 = my_numbers[:,0]
* col2 = my_numbers[:,1]

* # Sumar o agregar las dos columnas
* sums = col1 + col2

- Podríamos simplificar esto aún más si quisiéramos:
* sums = my_numbers[:,0] + my_numbers[:,1]
- Aquí hay algunas observaciones clave sobre este código:
* Cuando seleccionamos cada columna, usamos la sintaxis ndarray[:,c]donde cestá el índice de columna que queríamos seleccionar. Como vimos en la pantalla anterior, los dos puntos seleccionan todas las filas.
* Para agregar los dos ndarrays 1D, col1y col2, simplemente usamos el operador de suma ( +) entre ellos.

* El resultado de agregar dos ndarrays 1D es un ndarray 1D de la misma forma (o dimensiones) que el original. En este contexto, los ndarrays también pueden llamarse vectores , un término tomado de una rama de las matemáticas llamada álgebra lineal. Lo que acabamos de hacer, sumar dos vectores juntos, se llama suma de vectores.

* Esto es lo que pasó detrás de las escenas: Lo que hicimos fue un 2D la dividimos en dos columnas y formamos dos 1D luego sumamos la fila 1 de la columna 1 con la fila 1 de la columna 2 y obtuvos el resultado en una sola columna.

# Matemáticas vectoriales continuado

- En la última pantalla, usamos la suma de vectores para agregar dos columnas, o vectores, juntos. Podemos usar cualquiera de los operadores numéricos estándar de Python con vectores, incluyendo:

* vector_a + vector_b - Adición
* vector_a - vector_b - Resta
* vector_a * vector_b - Multiplicación (esto no está relacionado con la multiplicación de vectores utilizada en el álgebra lineal).
* vector_a / vector_b - división

- Cuando realizamos estas operaciones en dos vectores 1D, ambos vectores deben tener la misma forma.

- Veamos otro ejemplo de nuestro conjunto de datos de taxi. Aquí están las primeras cinco filas de dos de las columnas en el conjunto de datos:

* Distancia de viaje	       trip_length
*        21.00	               2037.0
*        16.29	               1520.0
*        12.70	               1462.0
*        8.70	                 1210.0
*        5.56                	 759.0

- Usemos estas columnas para calcular la velocidad de viaje promedio de cada viaje en millas por hora. La fórmula para calcular millas por hora es:  MILES PER HOUR = DISTANCE IN MILES / LENGTH IN HOURS

* Como aprendimos en la segunda pantalla de esta misión, trip_distanceya se expresa en millas, pero trip_lengthse expresa en segundos. Primero, nos convertiremos trip_lengthen horas:

* trip_distance = taxi[:,7]
* trip_length_seconds = taxi[:,8]
* trip_length_hours = trip_length_seconds / 3600 
* # 3600 segundos en una hora       En este caso, dividimos cada valor en el vector por un solo número , 3600, en lugar de otro vector.
* trip_mph = trip_distance_miles / trip_length_hours      Usa la división de vectores para dividir

#  Cálculo de estadísticas para ndarrays 1D

- En la última pantalla, creamos trip_mph, un ndarray 1D de la velocidad promedio de milla por hora de cada viaje en nuestro conjunto de datos. A continuación, exploraremos más estos datos y calcularemos los valores mínimo, máximo y medio para trip_mph.

- Para calcular el valor mínimo de un ndarray 1D, usamos el ndarray.min()método vectorizado , así:
* mph_min = trip_mph.min()
* print(mph_min)           0.0

- El valor mínimo en nuestro trip_mphndarray es 0.0, para un viaje que no recorre ninguna distancia. Numpy ndarrays tienen métodos para muchos cálculos diferentes. Algunos métodos clave son:

* ndarray.min() para calcular el valor mínimo
* ndarray.max() para calcular el valor máximo
* ndarray.mean() para calcular el valor medio o medio
* ndarray.sum() para calcular la suma de los valores
-Puede ver la lista completa de métodos ndarray en la  https://docs.scipy.org/doc/numpy-1.14.0/reference/arrays.ndarray.html#calculation
- Puede ver la lista completa de Python operacione: https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex
* Cuando vea la sintaxis ndarray.method_name(), sustitúyala ndarraypor el nombre de su ndarray (en este caso trip_mph) como se muestra a continuación:                ndarray.method_name()   =>   trip_mph.method_name()   
* trip_mph Usemos lo que acabamos de aprender para calcular la velocidad máxima y media (promedio) de nuestro ndarray.
EJERCICIO:
* mph_min = trip_mph.min()
* mph_max = trip_mph.max()      Usa el ndarray.max()método para calcular el valor máximo de trip_mph.
* mph_mean = trip_mph.mean()    Usa el ndarray.mean()método para calcular el valor promedio de trip_mph.

#  Cálculo de estadísticas para ndarrays 1D Continuando

- Al observar el resultado del código en la pantalla anterior, podemos observar:

* Velocidad media (media) del viaje (redondeada): 32 mph
* Velocidad máxima de viaje (redondeada): 82,000 mph
* Una velocidad de viaje de 82,000 mph no es posible en el tráfico de Nueva York, ¡es casi 20 veces más rápido que el avión más rápido del mundo! Esto podría deberse a un error en los dispositivos que registran los datos, o quizás a errores cometidos en algún lugar del flujo de datos.

- Antes de ver otros métodos de matriz, revisemos la diferencia entre métodos y funciones. Las funciones actúan como segmentos independientes de código que normalmente toman una entrada, realizan algún procesamiento y devuelven alguna salida. Por ejemplo, podemos usar la len()función para calcular la longitud de una lista o el número de caracteres en una cadena .

* my_list = [21,14,91]
* print(len(my_list))               3

- En contraste, los métodos son funciones especiales que pertenecen a un tipo específico de objeto . Esto significa que, por ejemplo, cuando trabajamos con objetos de lista, existen funciones o métodos especiales que solo se pueden usar con listas. Por ejemplo, podemos usar el list.append()método para agregar un elemento al final de una lista. Si intentamos usar ese método en una cadena , obtendremos un error:
* my_string.append(' is the best!')

- En NumPy, a veces hay operaciones que se implementan como métodos y funciones, lo que puede ser confuso. Veamos algunos ejemplos:

* Cálculo	                                          Representación de la función	            Método de representación
* Calcular el valor mínimo de trip_mph     	        np.min(trip_mph)	                        trip_mph.min()
* Calcular el valor máximo de trip_mph	            np.max(trip_mph)	                        trip_mph.max()
* Calcular el valor medio promedio detrip_mph       np.mean(trip_mph)	                        trip_mph.mean()
* Calcular el valor medio de la mediana detrip_mph	 np.median(trip_mph)                     	No hay un método de ndarray mediana.

* Para recordar la terminología correcta, todo lo que comienza con np(por ejemplo, np.mean()) es una función y todo lo que se expresa con un nombre de objeto (o variable) primero (por ejemplo trip_mph.mean()) es un método. Cuando ambos existen, depende de usted decidir cuál usar, pero es mucho más común usar el enfoque del método.

#  Cálculo de estadísticas para ndarrays 2D

- A continuación, calcularemos estadísticas para ndarrays 2D. Si usamos el ndarray.max()método en un ndarray 2D sin ningún parámetro adicional, devolverá un solo valor, al igual que con una matriz 1D:

- Pero, ¿y si quisiéramos encontrar el valor máximo de cada fila? Deberíamos usar el axis parámetro y especificar un valor de 1para indicar que queremos calcular el valor máximo para cada fila.

* 2D ndarray fila tiene los datos(1,0,1,1)    ndarray.max(axis=1)   Result =1

- Si queremos encontrar el valor máximo de cada columna, usaríamos un axisvalor de 0:
* 2D ndarray columna tiene los datos(1,0,0,3)    ndarray.max(axis=0)   Result = 3

- Usemos lo que hemos aprendido para verificar los datos en nuestro conjunto de datos de taxi. Para recordarnos cómo se ven los datos, veamos las primeras cinco filas de las columnas con los índices del 9 al 13:

* fare_amount	    fees_amount	   tolls_amount	   tip_amount	   cantidad total
*     52.0	         0.8	          5.54	          11.65	        69.99
*     45.0	         1.3	          0.00	          8.00	        54.3
*     36.5	         1.3	          0.00	          0.00	        37.8
*     26.0         	 1.3	          0.00	          5.46	        32.76
*     17.5	         1.3	          0.00	          0.00	        18.8

- Usted puede haber notado que la suma de los cuatro primeros valores en cada fila debe ser igual al último valor, total_amount:
Cantidad total = fare_amount + fees_amount + tolls_amount + tip_amount

- En el próximo ejercicio, comprobaremos estos valores. Solo revisaremos las primeras cinco filas taxipara que podamos verificar más fácilmente los resultados.

- EJERCICIO:
* # Comparemos con las primeras 5 filas
* taxi_first_five = taxi[:5]

* # select these columns: fare_amount, fees_amount, tolls_amount, tip_amount
* fare_components = taxi_first_five[:,9:13]

* # Suma las columnas de componentes   Compruebe que la suma de cada fila en fare_componentsigual al valor en la total_amountcolumna.
* fare_sums = fare_components.sum(axis=1)   Usa el ndarray.sum()método para calcular la suma de cada fila en fare_components. Asigna el resultado a fare_sums.

* # select the total_amount column       Extraer la columna 14 en taxi_first_five. Asignar a fare_totals.
* fare_totals = taxi_first_five[:,13]

* # compare the summed columns to the fare_totals
* print(fare_totals)     Imprimir fare_totalsy fare_sums. Utilice el inspector de variables para verificar que los resultados coincidan.
* print(fare_sums)

# 2. Boolean Indexing with NumPy

# Leyendo archivos CSV con NumPy

- Sin embargo, ¿qué pasaría si también quisiéramos averiguar cuántos viajes se realizaron en cada mes? ¿O cuál aeropuerto es el más ocupado? Para ello, aprenderemos una nueva técnica: la indexación booleana . Antes de sumergirnos en qué es la indexación booleana y cómo puede ayudarnos, volvamos a familiarizarnos con nuestros datos.

- A continuación hay información sobre las columnas seleccionadas del conjunto de datos:

* pickup_year: El año del viaje.
* pickup_month: El mes del viaje (enero es 1, diciembre es 12).
* pickup_day: El día del mes del viaje.
* pickup_location_code: El aeropuerto o barrio donde comenzó el viaje.
* dropoff_location_code: El aeropuerto o barrio donde terminó el viaje.
* trip_distance: La distancia del viaje en millas.
* trip_length: La duración del viaje en segundos.
* fare_amount: La tarifa base del viaje, en dólares.
* total_amount: El monto total cobrado al pasajero, incluidas todas las tarifas, peajes y propinas.

- Puede encontrar información sobre todas las columnas en el diccionario de datos. https://s3.amazonaws.com/dq-content/290/nyc_taxi_data_dictionary.md

- Ahora que entendemos un poco mejor a NumPy, aprendamos a usar la numpy.genfromtxt()función para leer archivos en ndarrays NumPy. Aquí está la sintaxis simplificada para la función, y una explicación de los dos parámetros:

* np.genfromtxt(filename, delimiter=None)

* filename: Un argumento posicional, generalmente una cadena que representa la ruta al archivo de texto que se leerá.
* delimiter: Un argumento con nombre, que especifica la cadena utilizada para separar cada valor.
* En este caso, como tenemos un archivo CSV, el delimitador es una coma. Así es como leímos en un archivo llamado data.csv:
* data = np.genfromtxt('data.csv', delimiter=',')

- EJERCICIO: Vamos a leer nuestro nyc_taxis.csvarchivo en NumPy siguiente.

* import numpy as np                                        Importe la biblioteca NumPy y asigne al alias np.
* taxi = np.genfromtxt('nyc_taxis.csv', delimiter=',')      Utilice la numpy.genfromtxt()función para leer el nyc_taxis.csvarchivo en NumPy. Asigna el resultado a taxi.
* taxi_shape = taxi.shape                                   Usa el ndarray.shapeatributo para asignar la forma de taxia taxi_shape.

# Leyendo archivos CSV con NumPy continua

- Es posible que no hayas notado en la última misión que convertimos todos los valores a flotantes antes de convertir la lista de listas en un ndarray. Esto se debe a que las ndarrays NumPy solo pueden contener un tipo de datos .

- No tuvimos que completar este paso en la última ejecución, porque cuando se numpy.genfromtxt()lee en un archivo, intenta determinar el tipo de datos del archivo mirando los valores.

- Podemos usar el ndarray.dtypeatributo para ver el tipo de datos interno que se ha utilizado.
* print(taxi.dtype)  float64

- NumPy eligió el float64tipo, ya que permitirá leer la mayoría de los valores de nuestro CSV. Puede pensar que el float64tipo de NumPy es idéntico al floattipo de Python (el "64" se refiere al número de bits utilizados para almacenar el valor subyacente).

- Si se revisan los resultados del último ejercicio, podemos ver que taxicontiene casi todos los números excepto por un valor que no hemos visto antes: nan.

* [[   nan    nan    nan ...,    nan    nan    nan] [  2016      1      1 ...,  11.65  69.99      1][  2016      1      1 ...,      8   54.3      1]..., [  2016      6     30 ...,      5  63.34      1][  2016      6     30 ...,   8.95  44.75      1] [  2016      6     30 ...,      0  54.84      2]]

- NaN es un acrónimo de No es un número ; significa literalmente que el valor no se puede almacenar como un número. Es similar a (y a menudo se lo denomina a) un valor nulo, como la Noneconstante de Python .

- NaN se ve con más frecuencia cuando falta un valor, pero en este caso, tenemos valores de NaN porque la primera línea de nuestro archivo CSV contiene los nombres de cada columna. NumPy no puede convertir valores de cadena como pickup_yearen el float64tipo de datos.

- Por ahora, necesitamos eliminar esta fila de encabezado de nuestro ndarray. Podemos hacerlo de la misma manera que lo haríamos si nuestros datos se almacenaran en una lista de listas:

* taxi = taxi[1:]

* Alternativamente, podemos pasar un parámetro adicional,, skip_headera la numpy.genfromtxt()función. El skip_headerparámetro acepta un número entero, el número de filas desde el inicio del archivo a omitir. Tenga en cuenta que debido a que este número entero debe ser el número de filas y no el índice, omitir la primera fila requeriría un valor de 1, no 0.
- EJERCICIO:
* taxi= np.genfromtxt('nyc_taxis.csv', delimiter=',', skip_header=1)       Utilice la numpy.genfromtxt()función para volver a leer el nyc_taxis.csvarchivo en NumPy, pero esta vez, omita la primera fila. Asigna el resultado a taxi.
* taxi_shape = taxi.shape                                                  Asigna la forma de taxia taxi_shape.

# Arreglos Booleanos

- En la última misión, aprendimos cómo indexar, o seleccionar, datos de ndarrays. En esta misión, nos centraremos en el método más poderoso, la matriz booleana. Una matriz booleana , como su nombre indica, es una matriz de valores booleanos. Los arreglos booleanos a veces se llaman vectores booleanos o máscaras booleanas .

- Puede recordar que el tipo booleano (o bool) es un tipo de Python integrado que puede ser uno de dos valores únicos:
* True
* False
- También puede recordar que hemos usado valores booleanos cuando trabajamos con operadores de comparación de Python como ==(igual) >(mayor que), <(menor que), !=(no igual). A continuación hay un par de ejemplos de operaciones booleanas simples:
Esta tabla resume las operaciones de comparación:  https://docs.python.org/3.4/library/stdtypes.html#comparisons

*         Operación	           Sentido
*            <	               estrictamente menos que
*            <=	               menor o igual
*            >	               estrictamente mayor que
*            > =	             mayor que o igual
*            ==	               igual
*            !=	               no es igual
*            is	               identidad de objeto
*            is not	           identidad de objeto negado

* EJEMPLO:  print(type(3.5) == float) True

- Cuando exploramos las matemáticas vectoriales en la primera misión, aprendimos que una operación entre un ndarray y un solo valor da como resultado un nuevo ndarray:

* print(np.array([2,4,6,8]) + 10)    [12 14 16 18]    La + 10operación se aplica a cada valor en la matriz.

- Ahora, veamos qué sucede cuando realizamos una operación booleana entre un ndarray y un solo valor:

* print(np.array([2,4,6,8]) < 5)    [ True  True False False]
* Se produce un patrón similar: cada valor de la matriz se compara con cinco. Si el valor es inferior a cinco, Truese devuelve. De lo contrario, Falsese devuelve.

EJERCICIO:
* a = np.array([1, 2, 3, 4, 5])         b = np.array(["blue", "blue", "red", "blue"])    c = np.array([80.0, 103.4, 96.9, 200.3])
* a_bool = (a < 3)              Evalúa si los elementos de la matriz ason menores que 3
* b_bool = (b == "blue")        Evalúa si los elementos de la matriz bson iguales a "blue".
* c_bool = (c > 100)            Evaluar si los elementos en la matriz cson mayores que 100.

# Indexación booleana con ndarrays 1D

- A continuación, aprenderemos cómo indexar (o seleccionar) mediante matrices booleanas, conocidas como indexación booleana. Para indexar usando nuestra nueva matriz booleana, simplemente la insertamos en los corchetes, como haríamos con nuestras otras técnicas de selección:

* c = np.array([80.0, 103.4, 96.9, 200.3])    c_bool= c > 100 

- La matriz booleana actúa como un filtro, de modo que los valores correspondientes a Trueformar parte del resultado y los valores correspondientes Falsese eliminan.

- Usemos la indexación booleana para confirmar el número de viajes en taxi en nuestro conjunto de datos del mes de enero. Primero, seleccionemos solo la pickup_monthcolumna, que es la segunda columna en el ndarray:
* pickup_month = taxi[:,1]

- A continuación, usamos una operación booleana para hacer una matriz booleana, donde el valor 1corresponde a enero:
* january_bool = pickup_month == 1

- Luego usamos la nueva matriz booleana para seleccionar solo los elementos pickup_monthque tienen un valor de 1:
* january = pickup_month[january_bool]

- Finalmente, usamos el .shapeatributo para averiguar cuántos artículos hay en nuestro januaryndarray, que es igual a la cantidad de viajes en taxi desde el mes de enero. Usaremos [0]para extraer el valor de la tupla devuelta por .shape:

* january_rides = january.shape[0]
* print(january_rides)              13481
* Hay 13,481 viajes en nuestro conjunto de datos desde el mes de enero. Practiquemos la indexación booleana y averigüemos la cantidad de viajes en nuestro conjunto de datos para febrero.

EJERCICIO: Calcule el número de paseos en el taxindarray que son de febrero:

* pickup_month = taxi[:,1]

* january_bool = pickup_month == 1
* january = pickup_month[january_bool]
* january_rides = january.shape[0]
* february_bool = pickup_month == 2    Cree una matriz booleana february_boolque evalúe si los elementos en pickup_monthson iguales a 2.
* february = pickup_month[february_bool]         Usa la february_bool matriz booleana para indexar pickup_month. Asigna el resultado a february.
* february_rides = february.shape[0]             Usa el ndarray.shape atributo para encontrar el número de elementos en february. Asigna el resultado a february_rides.

# Indexación booleana con ndarrays 2D

* Cuando trabaje con ndarrays 2D, puede usar la indexación booleana en combinación con cualquiera de los métodos de indexación que aprendimos en la misión anterior. La única limitación es que la matriz booleana debe tener la misma longitud que la dimensión que está indexando. 
- Debido a que una matriz booleana no contiene información sobre cómo se creó, podemos usar una matriz booleana hecha de una sola columna de nuestra matriz para indexar toda la matriz.

- Usemos lo que hemos aprendido para analizar la velocidad promedio de los viajes. En la misión anterior, calculamos que la velocidad máxima de viaje es de 82,000 mph, lo que sabemos que definitivamente no es exacto. Vamos a verificar si hay algún problema con los datos. Recordemos que calculamos la velocidad de viaje promedio de la siguiente manera:

# calculate the average speed
trip_mph = taxi[:,7] / (taxi[:,8] / 3600)

- A continuación, verificaremos los viajes con una velocidad promedio superior a 20,000 mph:
* # Crear una matriz booleana para viajes con promedio velocidades superiores a  20,000 mph
* trip_mph_bool = trip_mph > 20000
* # Usar la matriz booleana para seleccionar las filas para esos viajes y el  pickup_location_code, dropoff_location_code, trip_distance, y  trip_length columnas
* trips_over_20000_mph = taxi[trip_mph_bool,5:9]
* print(trips_over_20000_mph)
* [[     2      2     23      1] [     2      2   19.6      1] [     2      2   16.7      2] [     3      3   17.8      2] [     2      2   17.2      2][     3      3   16.9      3][     2      2   27.1      4]]

- Podemos ver en la última columna que la mayoría de estos son recorridos muy cortos, todos tienen trip_lengthvalores de 4 o menos segundos, que no concuerdan con las distancias de viaje, que son más de 16 millas.

- EJERCICIO: Usemos esta técnica para examinar las filas que tienen los valores más altos para la tip_amountcolumna.

* tip_amount = taxi[:,12]      
* tip_bool = tip_amount > 50        Cree una matriz booleana tip_bool, que determina qué filas tienen valores para la tip_amountcolumna de más de 50.
* top_tips = taxi[tip_bool, 5:14]   Utilice la tip_boolmatriz para seleccionar todas las filas taxicon valores de punta de más de 50y las columnas de los índices 5 a 13 inclusive. Asignar la matriz resultante a top_tips.

# Asignando valores en ndarrays
- A continuación, usaremos las mismas técnicas de indexación que ya hemos aprendido para modificar valores dentro de una ndarray. La sintaxis que usaremos (en pseudocódigo) es:

* ndarray[location_of_values] = new_value

- Echemos un vistazo a lo que parece en el código real. Con nuestra matriz 1D, podemos especificar una ubicación de índice específica:
* a = np.array(['red','blue','black','blue','purple'])
* a[0] = 'orange'
* print(a)             ['orange', 'blue', 'black', 'blue', 'purple']

- O podemos asignar múltiples valores a la vez:
* a[3:] = 'pink'
* print(a)             ['orange', 'blue', 'black', 'pink', 'pink']

- Con un ndarray 2D, al igual que con un ndarray 1D, podemos asignar una ubicación de índice específica:
* ones = np.array([[1, 1, 1, 1, 1],
*                 [1, 1, 1, 1, 1],
*                 [1, 1, 1, 1, 1]])
* ones[1,2] = 99
* print(ones)   [[ 1,  1,  1,  1,  1],
*  [ 1,  1, 99,  1,  1],
*  [ 1,  1,  1,  1,  1]]

- También podemos asignar una fila entera ...
* ones[0] = 42
* print(ones)     [[42, 42, 42, 42, 42], [ 1,  1, 99,  1,  1], [ 1,  1,  1,  1,  1]]

- ... o una columna entera:
* ones[:,2] = 0
* print(ones)    [[42, 42, 0, 42, 42], [ 1,  1, 0,  1,  1], [ 1,  1, 0,  1,  1]]

- EJERCICIO: Practiquemos una asignación de matriz con nuestro conjunto de datos de taxi.  Para ayudarlo a practicar sin hacer cambios a nuestra matriz original, hemos utilizado el ndarray.copy()método para hacer taxi_modifieduna copia de nuestro original para estos ejercicios.

* # Esto crea una copia de nuestro taxi ndarray
* taxi_modified = taxi.copy()
* taxi_modified[28214,5] = 1                                   El valor en el índice de columna 5(pickup_location) del índice de fila 28214es incorrecto. Use la asignación para cambiar este valor a 1en el taxi_modifiedndarray.
* taxi_modified[:,0] = 16                                      La primera columna (índice 0) contiene valores de año como números de cuatro dígitos en el formato AAAA ( 2016ya que todos los viajes en nuestro conjunto de datos son de 2016). Use la asignación para cambiar estos valores al formato YY ( 16) en el taxi_modifiedndarray.
* taxi_modified[1800:1802,7] = taxi_modified[:,7].mean()       Los valores en el índice de columna 7(trip_distance) del índice de filas 1800y 1801son incorrectos. Use la asignación para cambiar estos valores en el taxi_modifiedndarray al valor medio para esa columna.

# Asignación utilizando matrices booleanas

- Los arreglos booleanos se vuelven muy poderosos cuando los usamos para la asignación. Veamos un ejemplo:
* a2 = np.array([1, 2, 3, 4, 5])
* a2_bool = a2 > 2
* a2[a2_bool] = 99
* print(a2)       [ 1  2 99 99 99]

- La matriz booleana controla los valores a los que se aplica la asignación, y los otros valores permanecen sin cambios. Veamos cómo funciona este código:

* a = np.array([1, 2, 3, 4, 5])
* a[a > 2] = 99 

- Observe en el diagrama anterior que usamos un "atajo": insertamos la definición de la matriz booleana directamente en la selección. Este "atajo" es la forma convencional de escribir la indexación booleana. Hasta ahora, hemos estado asignando una variable intermedia primero para que el proceso sea claro, pero de aquí en adelante, usaremos este método de "acceso directo" en su lugar.

- EJERCICIO: Nuevamente usamos el ndarray.copy()método para hacer taxi_copyuna copia de nuestro original para este ejercicio.
* # Esto crea una copia de nuestro taxi ndarray
* taxi_copy = taxi.copy()
* total_amount = taxi_copy[:,13]           Seleccione la decimocuarta columna (índice 13) en taxi_copy. Asígnele a una variable nombrada total_amount.
* total_amount[total_amount < 0] = 0       Para las filas donde el valor de total_amountes menor que 0, use la asignación para cambiar el valor a 0.

# Asignación utilizando matrices booleanas continuación

- A continuación, veremos un ejemplo de asignación utilizando una matriz booleana con dos dimensiones:

* b = np_array([[1, 2, 3],[4 , 5, 6],[7, 8, 9]])
* b[b > 4 ] = 99

- La b > 4 operación booleana produce una matriz booleana 2D que luego controla los valores a los que se aplica la asignación. También podemos usar una matriz booleana 1D para realizar la asignación en una matriz 2D:

- La c[:,1] > 2 operación booleana compara solo los valores de una columna y produce una matriz booleana 1D. Luego usamos esa matriz booleana como el índice de la fila para la asignación, y 1como el índice de la columna para especificar la segunda columna. Nuestra matriz booleana solo se aplica a la segunda columna, mientras que todos los demás valores permanecen sin cambios.

La sintaxis de pseudocódigo para este código es la siguiente, primero utilizando una variable intermedia:

* bool = array[:, column_for_comparison] == value_for_comparison
* array[bool, column_for_assignment] = new_value

- y luego todo en una línea:

* array[array[:, column_for_comparison] == value_for_comparison, column_for_assignment] = new_value

- EJERCICIO: Practiquemos este patrón utilizando nuestro conjunto de datos de taxi: Hemos creado una nueva copia de nuestro conjunto de datos de taxi, taxi_modifiedcon una columna adicional que contiene el valor 0de cada fila.

* En nuestra nueva columna en el índice 15, asigne el valor 1si pickup_location_code(el índice de la columna 5) corresponde a una ubicación del aeropuerto, dejando el valor de 0otra manera realizando estas tres operaciones:
* # Crear una nueva columna rellena con `0`.
* zeros = np.zeros([taxi.shape[0], 1])
* taxi_modified = np.concatenate([taxi, zeros], axis=1)
* print(taxi_modified)      
 
* taxi_modified[taxi_modified[:, 5] == 2, 15] = 1            Para las filas en las que el valor del índice de columna 5es igual a 2(Aeropuerto JFK), asigne el valor 1al índice de columna 15.
* taxi_modified[taxi_modified[:, 5] == 3, 15] = 1            Para las filas en las que el valor del índice de columna 5es igual a 3(Aeropuerto de LaGuardia), asigne el valor 1al índice de columna 15.
* taxi_modified[taxi_modified[:, 5] == 5, 15] = 1            Para las filas donde el valor del índice de columna 5es igual a 5(Aeropuerto de Newark), asigne el valor 1al índice de columna 15.
 
# Desafío: ¿Cuál es el aeropuerto más popular?

- Los desafíos están diseñados para ayudarte a practicar las técnicas que has aprendido en esta misión. Le brindamos varios consejos para ayudarlo, pero primero intente completar el desafío sin los consejos, si puede. No se desanime si estos pasos desafiantes intentan hacer un buen trabajo, ¡trabajar con datos es un proceso iterativo!

- En este desafío, queremos averiguar qué aeropuerto es el destino más popular en nuestro conjunto de datos. Para hacerlo, usaremos la indexación booleana para crear tres matrices filtradas y luego veremos cuántas filas hay en cada matriz.

- Para completar esta tarea, necesitaremos verificar si la dropoff_location_codecolumna (índice de columna 6) es igual a uno de los siguientes valores:

* 2: JFK Aeropuerto
* 3: LaGuardia Airport
* 5: Aeropuerto de Newark.

EJERCICIO:
1. Usando el taxindarray original , calcule cuántos viajes tuvo el Aeropuerto JFK como destino: Use la indexación booleana para seleccionar solo las filas donde la dropoff_location_codecolumna (índice de columna 6) tiene un valor que corresponde a JFK. Asigna el resultado a jfk. Calcule cuántas filas hay en la nueva jfkmatriz y asigne el resultado a jfk_count.
2. Calcule cuántos viajes desde el taxiaeropuerto de Laguardia tenían como destino: Utilice la indexación booleana para seleccionar solo las filas donde la dropoff_location_codecolumna (índice de columna 6) tiene un valor que corresponde a Laguardia. Asigna el resultado a laguardia. Calcule cuántas filas hay en la nueva laguardiamatriz. Asigna el resultado a laguardia_count.
3. Calcule cuántos viajes de taxi tenían como destino el Aeropuerto de Newark:
Seleccione solo las filas donde la dropoff_location_codecolumna tiene un valor que corresponde a Newark y asigne el resultado a newark.
4. Calcule cuántas filas hay en la nueva newarkmatriz y asigne el resultado a newark_count. Después de ejecutar el código, inspeccionar los valores de jfk_count, laguardia_county newark_countpara ver qué aeropuerto tiene la mayoría de bajadas.

* jfk = taxi[taxi[:,6] == 2]    
* jfk_count = jfk.shape[0]

* laguardia = taxi[taxi[:,6] == 3]
* laguardia_count = laguardia.shape[0]

* newark = taxi[taxi[:,6] == 5]
* newark_count = newark.shape[0]

# Desafío: Calcular estadísticas para viajes en datos limpios

- Nuestros cálculos en la pantalla anterior muestran que Laguardia es el aeropuerto más común para las devoluciones en nuestro conjunto de datos.
- Nuestro segundo y último desafío consiste en eliminar los datos potencialmente malos de nuestro conjunto de datos y luego calcular algunas estadísticas descriptivas sobre los datos "limpios" restantes.

- Comenzaremos utilizando la indexación booleana para eliminar cualquier fila que tenga una velocidad promedio para el viaje de más de 100 mph (160 kph), lo que debería eliminar los datos cuestionables con los que hemos trabajado en las últimas dos misiones. Luego, usaremos métodos de matriz para calcular la media de columnas específicas de los datos restantes. Las columnas que nos interesan son:

* trip_distance, en el índice de la columna 7
* trip_length, en el índice de la columna 8
* total_amount, en el índice de la columna 13

EJERCICIO: El trip_mphn darray se ha proporcionado para usted.
1. Cree un nuevo ndarray, que cleaned_taxicontenga solo filas para las cuales los valores trip_mphsean menores que 100.
2. Calcula la media de la trip_distancecolumna de cleaned_taxi. Asigna el resultado a mean_distance.
3. Calcula la media de la trip_lengthcolumna de cleaned_taxi. Asigna el resultado a mean_length.
4. Calcula la media de la total_amountcolumna de cleaned_taxi. Asigna el resultado a mean_total_amount.
* trip_mph = taxi[:,7] / (taxi[:,8] / 3600)
* cleaned_taxi = taxi[trip_mph < 100]
* mean_distance = cleaned_taxi[:,7].mean()
* mean_length = cleaned_taxi[:,8].mean()
* mean_total_amount = cleaned_taxi[:,13].mean()

# 3. Introduction to Pandas

# Entendiendo Pandas y NumPy
- En las últimas dos misiones, exploramos cómo la biblioteca NumPy facilita el trabajo con datos. Debido a que podemos trabajar fácilmente en múltiples dimensiones, nuestro código es mucho más fácil de entender. Al utilizar operaciones vectorizadas en lugar de bucles, nuestro código se ejecuta más rápido con datos más grandes.

- Aunque NumPy proporciona estructuras y herramientas fundamentales que facilitan el trabajo con datos, hay varias cosas que limitan su utilidad:

* La falta de soporte para los nombres de columnas nos obliga a encuadrar preguntas como operaciones de matrices multidimensionales.
* El soporte para un solo tipo de datos por ndarray hace que sea más difícil trabajar con datos que contienen datos numéricos y de cadena.
* Hay muchos métodos de bajo nivel, pero hay muchos patrones de análisis comunes que no tienen métodos precompilados.

- La biblioteca de pandas proporciona soluciones a todos estos puntos de dolor y más. Pandas no es tanto un reemplazo para NumPy como una extensión de NumPy. El código subyacente para pandas usa la biblioteca NumPy ampliamente, lo que significa que los conceptos que has estado aprendiendo te serán útiles a medida que comiences a aprender más sobre pandas.

- La estructura de datos primaria en pandas se llama un marco de datos . Los marcos de datos son el equivalente de los pandas de un ndarray Numpy 2D, con algunas diferencias clave:

* Los valores de los ejes pueden tener etiquetas de cadena , no solo las numéricas.
* Los marcos de datos pueden contener columnas con varios tipos de datos : incluidos enteros, flotantes (decimales) y cadenas(nombres como Japo, China).

# Introducción a los datos

- A medida que aprendemos pandas, trabajaremos con un conjunto de datos de la lista Global 500 de la revista Fortune , que clasifica a las 500 corporaciones más importantes del mundo por ingresos. El conjunto de datos fue compilado originalmente aquí ; sin embargo, modificamos el conjunto de datos original para hacerlo más accesible.  https://data.world/chasewillden/fortune-500-companies-2017

- El conjunto de datos es un archivo CSV llamado f500.csv. Aquí hay un diccionario de datos para algunas de las columnas en el CSV:

* company: Nombre de la compania.
* rank: Rango global 500 para la empresa.
* revenues: Ingresos totales de la compañía para el año fiscal, en millones de dólares (USD).
* revenue_change: Cambio porcentual en los ingresos entre el año fiscal actual y el anterior.
* profits: Utilidad neta del ejercicio, en millones de dólares (USD).
* ceo: Director Ejecutivo de la compañía.
* industry: Industria en la que opera la empresa.
* sector: Sector en el que opera la empresa.
* previous_rank: Rango global de 500 para la compañía para el año anterior.
* country: País en el que tiene su sede la empresa.

- Similar a la convención de importación para NumPy ( import numpy as np), la convención de importación para pandas es:

* import pandas as pd

- En el script.pyeditor de código para esta pantalla, ya hemos importado pandas y utilizamos la pandas.read_csv()función para leer el CSV en un marco de datos y asignarlo al nombre de la variable f500. Aprenderemos read_csv()más adelante en este curso, pero por ahora, todo lo que necesita saber es que maneja automáticamente la lectura y el análisis de la mayoría de los archivos CSV.

- Al igual que las ndarrays de NumPy, los marcos de datos de los pandas tienen un .shapeatributo que devuelve una tupla que representa las dimensiones de cada eje del objeto. Usaremos eso y Python's type() functionpara inspeccionar el f500marco de datos.

- EJERCICIO: 1. Utilice la type()función de Python para asignar el tipo de f500a f500_type. 2. Usa el DataFrame.shapeatributo para asignar la forma de f500a f500_shape. 3. Después de ejecutar el código, utilice la variable inspector de mirar las variables f500, f500_typey f500_shape.    f500_typetipo ( <clase 'tipo'> )  pandas.core.frame.DataFrame  f500_shapetupla ( <clase 'tupla'> ) (500, 16)
* import pandas as pd
* f500 = pd.read_csv('f500.csv',index_col=0)
* f500.index.name = None
* f500_type = type(f500)
f500_shape = f500.shape

 # Introduciendo DataFrames

- El código que escribimos en la pantalla anterior nos permite saber que nuestros datos tienen 500 filas y 16 columnas, y se almacenan como una estructura de datos primaria, pandas.core.frame.DataFrame objecto simplemente un marco de datos.

- Recuerde que una de las características que mejoran los pandas para trabajar con datos es su compatibilidad con las columnas de cadena y las etiquetas de fila:

* Los valores de los ejes pueden tener etiquetas de cadena, no solo las numéricas .
* Los marcos de datos pueden contener columnas con varios tipos de datos: incluidos enteros, flotantes y cadenas.

- Vamos a verificar esto a continuación. Para ver las primeras filas de nuestro marco de datos, podemos usar el DataFrame.head()método . Por defecto, devolverá las primeras cinco filas de nuestro marco de datos. Sin embargo, también acepta un parámetro entero opcional, que especifica el número de filas:    
* f500.head(3)

- Del mismo modo, podemos usar el DataFrame.tail()método para mostrarnos las últimas filas de nuestro marco de datos:
* f500.tail(3)

EJERCICIO: Al igual que en las misiones anteriores, la f500variable que creamos en la pantalla anterior está disponible aquí.
* f500_head = f500.head(6)   Usa el head()método para seleccionar las primeras 6 filas . Asigna el resultado a f500_head. CABEZA=HEAD
* f500_tail = f500.tail(8)   Usa el tail()método para seleccionar las últimas 8 filas . Asigna el resultado a f500_tail. COLA=TAIL

 # Introduciendo DataFrames Continuación
 
- Otra característica que hace que los pandas sean mejores para trabajar con datos es que los marcos de datos pueden contener más de un tipo de datos:

* Los valores de los ejes pueden tener etiquetas de cadena, no solo numéricas
* Los marcos de datos pueden contener columnas con varios tipos de datos: incluidos enteros, flotantes y cadenas.

- Podemos usar el DataFrame.dtypesatributo (similar al ndarray.dtypeatributo de NumPy ) para devolver información sobre los tipos de cada columna. Veamos un ejemplo usando una selección de datos almacenados usando el nombre de la variable f500_selection.

* print(f500_selection.dtypes)
* rank          int64
* revenues      int64
* profits     float64
* country      object
* dtype: object

- Podemos ver tres tipos de datos diferentes, o dtypes .

- Puede reconocer el float64tipo de letra de nuestro trabajo en NumPy. Pandas usa dtypes NumPy para columnas numéricas, incluyendo integer64. También hay un tipo que no hemos visto antes, objectque se usa para las columnas que tienen datos que no encajan en ningún otro tipo de dtypes. Esto casi siempre se usa para columnas que contienen valores de cadena.

- Cuando importamos datos, los pandas intentarán adivinar el tipo de letra correcto para cada columna. En general, los pandas hacen un buen trabajo con esto, lo que significa que no debemos preocuparnos por especificar tipos de datos cada vez que comenzamos a trabajar con datos.

- Si quisiéramos una descripción general de todos los tipos de datos utilizados en nuestro marco de datos, junto con su forma y otra información, podríamos usar el DataFrame.info()método . Tenga en cuenta que DataFrame.info()imprime la información, en lugar de devolverla, por lo que no podemos asignarla a una variable.

EJERCICIO:  Utilice el DataFrame.info()método para mostrar información sobre el f500marco de datos.   * f500.info()

# Seleccionando una columna de un DataFrame por etiqueta

- En el último ejercicio, usamos el DataFrame.info()método para mostrarnos el número de entradas en nuestro índice (que representa el número de filas), una lista de cada columna con su dtype y el número de valores no nulos, así como un resumen de Los diferentes tipos de datos y uso de memoria.

- Debido a que nuestros ejes en pandas tienen etiquetas, podemos seleccionar datos utilizando esas etiquetas, a diferencia de NumPy, donde necesitábamos saber la ubicación exacta del índice. Para ello, podemos utilizar el DataFrame.loc[]método . La sintaxis del DataFrame.loc[]método es:

* df.loc[row_label, column_label]

- Tenga en cuenta que usamos paréntesis ( []) en lugar de paréntesis ( ()) cuando seleccionamos por ubicación.

- A lo largo de nuestras misiones pandas, verás que se dfusa en ejemplos de código como una abreviatura para un objeto de marco de datos. Utilizamos esta convención porque también se usa ampliamente en la documentación oficial de los pandas; es importante acostumbrarse a leerla.

- Veamos un ejemplo a continuación. Volveremos a trabajar con solo una selección de datos almacenados como * f500_selection : Seleccionemos una sola columna especificando una sola etiqueta :

* f500_selection.loc[:,"rank"] Estoy seleccionando una columna espepecifica de un conjunto de datos
* Observe que usamos :para especificar que deseamos seleccionar todas las filas. También tenga en cuenta que el nuevo marco de datos tiene las mismas etiquetas de fila que el original.

- También podemos usar el siguiente método abreviado para seleccionar una sola columna:

* rank_col = f500_selection["rank"]
* print(rank_col)
* Walmart                     1
* State Grid                  2
* Sinopec Group               3
* China National Petroleum    4
* Toyota Motor                5
* Name: rank, dtype: int64

- Este estilo de selección de columnas es muy común. Lo utilizaremos a lo largo de nuestras misiones Dataquest.

EJERCICIO: Practiquemos el uso de esta técnica para seleccionar una columna específica de nuestro f500marco de datos.

* industries = f500["industry"]         Seleccione la industrycolumna. Asigne el resultado al nombre de la variable industries.
* industries_type = type(industries)    Utilice la type()función de Python para asignar el tipo de industriesa industries_type.

# Introducción a la serie

- En la última pantalla, observamos que cuando selecciona solo una columna de un marco de datos, obtiene un nuevo tipo de pandas: un objeto de serie . Series es el tipo de pandas para objetos unidimensionales. Cada vez que veas un objeto pandas 1D, será una serie. Cada vez que vea un objeto pandas 2D, será un marco de datos.

- De hecho, puedes pensar en un marco de datos= DataFrame como una colección de objetos de series, que es similar a cómo los pandas almacenan los datos detrás de la escena. 

# Selección de columnas de un marco de datos=DataFrame por etiqueta Continuación

- A continuación, vamos a aprender cómo seleccionar varias columnas. A continuación, utilizamos una lista de etiquetas para seleccionar columnas específicas:

* f500_selection.loc[:,["country","rank"]]
* f500_selection[["country","rank"]]
* Debido a que el objeto devuelto es bidimensional, sabemos que es un marco de datos , no una serie. Nuevamente, en lugar de df.loc[:,["col1","col2"]], también puede usar df[["col1", "col2"]]para seleccionar columnas específicas.

- Terminemos con el uso de un objeto de división con etiquetas para seleccionar columnas específicas:

* f500_selection.loc[:,"rank":"profits"]

- Nuevamente obtenemos un objeto de marco de datos, con todas las columnas desde la primera hasta la última , e incluso , la última columna de nuestra división. También tenga en cuenta que no hay acceso directo para seleccionar segmentos de columna.

- A continuación un resumen de las técnicas que hemos aprendido hasta ahora:

*  Seleccionar por etiqueta	    Sintaxis explícita	         Taquigrafía común
*  Una sola columna	            df.loc[:,"col1"]	           df["col1"]
*  Lista de columnas           	df.loc[:,["col1", "col7"]]	 df[["col1", "col7"]]
* Rebanada de columnas	        df.loc[:,"col1":"col4"]	

EJERCICIO: Practiquemos el uso de estas técnicas para seleccionar columnas específicas de nuestro f500marco de datos.

* countries = f500["country"]                     Seleccione la countrycolumna. Asigne el resultado al nombre de la variable countries.
* revenues_years =  f500.loc[:,["revenues","years_on_global_500_list"]]          En orden, seleccione las columnas revenues  y years_on_global_500_list.
* ceo_to_sector =  f500.loc[:, "ceo":"sector"]   En orden, seleccione todas las columnas de ceohasta e incluyendo sector. 

# Selección de filas de un marco de datos por etiqueta

- Ahora que hemos aprendido cómo seleccionar columnas por etiqueta, aprendamos cómo seleccionar filas usando las etiquetas del eje de índice : LAS ETIQUETAS DEL EJE DEL INDICE SON COMO EL EJE VERTICAL DE UNA TABLA COMO LOS NOMBRES DE LAS FILAS

- Utilizamos la misma sintaxis para seleccionar filas de un marco de datos como lo hacemos para las columnas:

* df.loc[row_label, column_label]

- Volveremos a utilizar una selección de nuestros datos, almacenados como la variable * f500_selection :

- Seleccione una sola fila
* single_row = f500_selection.loc["Sinopec Group"]
* print(type(single_row))
* print(single_row)
* class 'pandas.core.series.Series'
* RESPUESTA
* rank             3
* revenues    267518
* profits     1257.9
* country      China
* Name: Sinopec Group, dtype: object

- Tenga en cuenta que el objeto devuelto es una serie porque es unidimensional. Como esta serie tiene que almacenar valores enteros, flotantes y de cadena, los pandas usan el tipo de objectdty, ya que ninguno de los tipos numéricos podría atender a todos los valores.

- Seleccione una lista de filas

* list_rows = f500_selection.loc[["Toyota Motor", "Walmart"]]
* print(type(list_rows))
* print(list_rows)
* RESPUESTA
* class 'pandas.core.frame.DataFrame'
*               rank  revenues  profits country
* Toyota Motor     5    254694  16899.3   Japan
* Walmart          1    485873  13643.0     USA

- Seleccione un objeto slice con etiquetas

Para la selección utilizando cortes, podemos utilizar el acceso directo a continuación. Esta es la razón por la que no podemos usar este acceso directo para las columnas, porque está reservado para su uso con filas:

* slice_rows = f500_selection["State Grid":"Toyota Motor"]
* print(type(slice_rows))
* print(slice_rows) 
* RESPUESTA
* class 'pandas.core.frame.DataFrame'
*                           rank  revenues  profits country
* State Grid                   2    315199   9571.3   China
* Sinopec Group                3    267518   1257.9   China
* China National Petroleum     4    262573   1867.5   China
* Toyota Motor                 5    254694  16899.3   Japan

-EJERCICIO: Seleccionando datos de f500:
1. Crea una nueva variable toyota, con: * Sólo la fila con índice Toyota Motor.  * Todas las columnas.
2. Crea una nueva variable drink_companies, con: * Filas con indicios Anheuser-Busch InBev, Coca-Colay Heineken Holding, en ese orden.
* Todas las columnas.
3. Crea una nueva variable, middle_companiescon:* Todas las filas con indicios de Tata Motorsa Nationwide, inclusive. * Todas las columnas de ranka country, inclusive.
* toyota = f500.loc['Toyota Motor']
* drink_companies = f500.loc[["Anheuser-Busch InBev", "Coca-Cola", "Heineken Holding"]]
* middle_companies = f500.loc["Tata Motors":"Nationwide", "rank":"country"]

# Series vs Dataframes

- En el último par de pantallas, creamos objetos de serie y de marco de datos cuando seleccionamos datos de nuestro f500marco de datos . Tómese un minuto para revisar estos ejemplos antes de continuar:

*    ORIGINAL DATAFRAME                CODE                                                       RESULT
* Selecciono la columna D        single_col = df["D"]                single_col es un objeto de serie  Obtengo una sola columna D
* Selecciono la fila v           single_row = df.loc["v"]            single_row es un objeto de serie  Obtengo una sola fila v
* Selecciono la columna A,C,D    mult_cols = df[["A", "C", "D"]]     mult_cols es un objeto de DataFrame  Obtengo tres columnas A,C,D
* Selecciono la fila v,w,x       mult_row = df.loc[["v", "w", "x"]]  mult_row es un objeto de DataFrame  Obtengo tres filas v, w, x

# Método del valor de las cuentas

- Dado que las series y los marcos de datos son dos objetos distintos, tienen sus propios métodos únicos. Veamos un ejemplo de un método de serie a continuación: el Series.value_counts()método . Este método muestra cada valor no nulo único en una columna y sus cuentas en orden.

- Primero, seleccionaremos solo una columna del f500marco de datos:
* sectors = f500["sector"]
* print(type(sectors))      class 'pandas.core.series.Series'

- A continuación, sustituiremos "Series" Series.value_counts()con el nombre de nuestra sectorsserie, como a continuación:

* sectors_value_counts = sectors.value_counts()
* print(sectors_value_counts)
* RESPUESTA
* Financials                       118
* Energy                            80
* Technology                        44
* Motor Vehicles & Parts            34
* Wholesalers                       28
* Health Care                       27
* Food & Drug Stores                20
* Transportation                    19
* Telecommunications                18
* Retailing                         17
* Food, Beverages & Tobacco         16
* Materials                         16
* Industrials                       15
* Aerospace & Defense               14
* Engineering & Construction        13
* Chemicals                          7
* Media                              3
* Household Products                 3
* Hotels, Restaurants & Leisure      3
* Business Services                  3
* Apparel                            2
* Name: sector, dtype: int64

- En la serie resultante, podemos ver cada valor único no nulo en la columna y sus recuentos.

- Veamos qué sucede cuando intentamos utilizar el Series.value_counts()método con un marco de datos. Primero, seleccionaremos las columnas sectory industrypara crear un marco de datos llamado sectors_industries:
* sectors_industries = f500[["sector", "industry"]]
* print(type(sectors_industries))

- Luego, trataremos de usar el value_counts()método:
* si_value_counts = sectors_industries.value_counts()
* print(si_value_counts)      < class 'pandas.core.frame.DataFrame' >

- Dado que value_counts()es un método solo de serie , obtenemos el siguiente error:
* AttributeError: 'DataFrame' object has no attribute 'value_counts'

EJERCICIO:  Ya hemos guardado una selección de datos f500en un marco de datos llamado f500_sel. 1. Encuentre los conteos de cada valor único en la countrycolumna en el f500_selmarco de datos. * Seleccione la countrycolumna en el f500_selmarco de datos. Asígnele a una variable nombrada countries. * Utilice el Series.value_counts()método para devolver los recuentos de valores countries. Asigna los resultados a country_counts.

* countries = f500_sel["country"]
* country_counts = countries.value_counts()

#  Selección de artículos de una serie por etiqueta

- En el último ejercicio, practicamos utilizando el Series.value_counts()método. A continuación, encontremos los recuentos de cada valor único en la countrycolumna para todo el f500marco de datos:

* countries = f500["country"]
* country_counts = countries.value_counts()

* USA             132
* China           109
* Japan            51
* Germany          29
* France           29
* Britain          24
* South Korea      15
* Netherlands      14
* Switzerland      14
* Canada           11
* Spain             9
* Brazil            7
* Australia         7
* India             7
* Italy             7
* Taiwan            6
* Russia            4
* Ireland           4
* Sweden            3
* Singapore         3
* Mexico            2
* Israel            1
* Turkey            1
* Norway            1
* Thailand          1
* Belgium           1
* Venezuela         1
* Luxembourg        1
* Denmark           1
* Indonesia         1
* Malaysia          1
* Saudi Arabia      1
* Finland           1
* U.A.E             1
* Name: country, dtype: int64

- Sin embargo, ¿qué pasa si quisiéramos seleccionar sólo el recuento para la India? ¿O las cuentas solo para los países de América del Norte?

- Al igual que con los marcos de datos, podemos utilizar Series.loc[]para seleccionar elementos de una serie utilizando etiquetas individuales, una lista o un objeto de sector. También podemos omitir loc[]y usar accesos directos de paréntesis para los tres:

*    Seleccionar por etiqueta	               Sintaxis explícita	            Convención de taquigrafía
*  Solo artículo de la serie	                s.loc["item8"]	                    s["item8"]
*  Lista de artículos de la serie	          s.loc[["item1","item7"]]	          s[["item1","item7"]]
*  Rebanada de artículos de la serie.	      s.loc["item2":"item4"]	            s["item2":"item4"]

- EJERCICIO: De la serie de pandas countries_counts: 1. Seleccione el elemento en la etiqueta de índice India. Asigne el resultado al nombre de la variable india. 2. Con el fin, seleccionar los elementos con etiquetas de índice USA, Canaday Mexico. Asigne el resultado al nombre de la variable north_america.

* countries = f500['country']
* countries_counts = countries.value_counts()
* india = countries_counts["India"]
* north_america = countries_counts[["USA","Canada","Mexico"]]

# Resumen del desafío
- Veamos un resumen de todos los diferentes métodos de selección de etiquetas que hemos aprendido en esta misión:

*       Seleccionar por etiqueta	                       Sintaxis explícita	          Convención de taquigrafía
* Columna única desde el marco de datos                df.loc[:,"col1"]	                     df["col1"]
* Lista de columnas del marco de datos	               df.loc[:,["col1","col7"]]	           df[["col1","col7"]]
* Rebanada de columnas de dataframe	                   df.loc[:,"col1":"col4"]	
* Una sola fila desde el marco de datos	               df.loc["row4"]	
* Lista de filas del marco de datos	                   df.loc[["row1", "row8"]]	
* Rebanada de filas de dataframe	                     df.loc["row3":"row5"]	               df["row3":"row5"]
* Solo artículo de la serie	                           s.loc["item8"]	                       s["item8"]
* Lista de artículos de la serie	                     s.loc[["item1","item7"]]	             s[["item1","item7"]]
* Rebanada de artículos de la serie.	                 s.loc["item2":"item4"]	               s["item2":"item4"]

EJERCICIO: Seleccionando datos de f500: 1. Crea una nueva variable big_movers, con: * Las filas con índices Aviva, HP, JD.com, y BHP Billiton, en ese orden. * Las columnas ranky previous_rank, en ese orden.  2. Crea una nueva variable, bottom_companiescon: * Todas las filas con índices desde National Grida AutoNation, inclusive. * El rank, sectory country columnas.

* big_movers = f500.loc[["Aviva", "HP", "JD.com", "BHP Billiton"], ["rank","previous_rank"]]
* bottom_companies = f500.loc["National Grid":"AutoNation", ["rank","sector","country"]]

# 4. Exploring Data with Pandas: Fundamentals

# Introducción a los datos
* En esta misión, aprenderemos otra manera en que los pandas hacen que los trabajos con datos sean más fáciles. Tiene muchos métodos y funciones incorporados para tareas comunes de exploración y análisis. A medida que aprendamos esto, también exploraremos cómo los pandas utilizan muchos de los conceptos que aprendimos en las misiones NumPy, incluidas las operaciones vectorizadas y la indexación booleana.

- A continuación, usemos los métodos DataFrame.head()y DataFrame.info()para volver a familiarizarnos con los datos.
EJERCICIO: Ya hemos leído el conjunto de datos en un marco de datos de pandas y lo hemos asignado a una variable nombrada f500.
* f500_head = f500.head(10)         Usa el DataFrame.head()método para seleccionar las primeras 10 filas en f500.
* f500.info()                       Utilice el DataFrame.info()método para mostrar información sobre el marco de datos.

# Operaciones Vectorizadas

- Debido a que los pandas están diseñados para funcionar como NumPy, se admiten muchos conceptos y métodos de Numpy. Recuerde que una de las formas en que NumPy facilita el trabajo con datos es con operaciones vectorizadas , o operaciones aplicadas a múltiples puntos de datos a la vez:

- La vectorización no solo mejora el rendimiento de nuestro código, sino que también nos permite escribir código más rápidamente. Como pandas es una extensión de NumPy, también admite operaciones vectorizadas. Veamos un ejemplo de cómo funcionaría esto con una serie de pandas:

* print(my_series)
* 0    1
* 1    2
* 2    3
* 3    4
* 4    5
* dtype: int64
* OTRO EJEMPLO
* my_series = my_series + 10
* print(my_series)
* 0    11
* 1    12
* 2    13
* 3    14
* 4    15
* dtype: int64

- Al igual que con NumPy, podemos usar cualquiera de los operadores numéricos estándar de Python con series, que incluyen:

* series_a + series_b - Adición
* series_a - series_b - Resta
* series_a * series_b - Multiplicación (esto no está relacionado con las multiplicaciones utilizadas en el álgebra lineal).
* series_a / series_b - división

- Recuerde que nuestro f500marco de datos incluye el rango actual y el año anterior de cada compañía en la lista de Fortune 500. Usemos operaciones vectorizadas para calcular los cambios en el rango de cada compañía.

- EJERCICIO: Resta los valores en la rankcolumna de los valores en la previous_rankcolumna. Asigna el resultado a rank_change.
* rank_change =  f500["previous_rank"] - f500["rank"]

# Métodos de exploración de datos en serie

- En la última pantalla, restamos los valores en la rankcolumna de la previous_rankcolumna. A continuación se muestran los primeros cinco valores del resultado:

* Walmart                     0
* State Grid                  0
* Sinopec Group               1
* China National Petroleum   -1
* Toyota Motor                3

- Podemos observar a partir de los resultados que el Grupo Sinopec y Toyota Motor aumentaron su rango en comparación con el año anterior, mientras que China National Petroleum cayó un lugar. Sin embargo, ¿qué pasaría si quisiéramos encontrar el mayor aumento o disminución en el rango?

- Al igual que NumPy, pandas admite muchos métodos de estadísticas descriptivas que pueden ayudarnos a responder estas preguntas. Éstos son algunos de los más útiles (con enlaces a la documentación):

* Series.max()
* Series.min()
* Series.mean()
* Series.median()
* Series.mode()
* Series.sum()

- Veamos un ejemplo a continuación:
* print(my_series)
* a    0
* b    1
* c    2
* d    3
* e    4
* dtype: int64
* print(my_series.sum()) 10

- EJERCICIO: 1. Usa el Series.max()método para encontrar el valor máximo para la rank_changeserie. Asigna el resultado a la variable rank_change_max. 2. Usa el Series.min()método para encontrar el valor mínimo para la rank_changeserie. Asigna el resultado a la variable rank_change_min. 3. Después de ejecutar su código, use el inspector de variables para ver cada una de las nuevas variables que creó.

* rank_change =  f500["previous_rank"] - f500["rank"]
* rank_change_max = rank_change.max()
* rank_change_min = rank_change.min()

# Método de descripción de la serie

- En la última pantalla, usamos los métodos Series.max()y Series.min()para determinar el mayor aumento y disminución en el rango:
* Mayor aumento en el rango: 226
* Mayor disminución en el rango: -500

- Sin embargo, de acuerdo con el diccionario de datos, esta lista solo debería clasificar a las compañías en una escala de 1 a 500. Incluso si la compañía ocupó el primer lugar en el año anterior se movió a 500 este año, el cambio de rango calculado sería -499. Esto indica que hay datos incorrectos en la rankcolumna o en la previous_rankcolumna.

- A continuación, aprenderemos otro método que puede ayudarnos a investigar más rápidamente este problema: el Series.describe()método . Este método nos dice cuántos valores no nulos están contenidos en la serie, junto con la media, el mínimo, el máximo y otras estadísticas que conoceremos más adelante en esta ruta.

- Veamos un ejemplo: 
* assets = f500["assets"]
* print(assets.describe())

* count    5.000000e+02
* mean     2.436323e+05
* std      4.851937e+05
* min      3.717000e+03
* 25%      3.658850e+04
* 50%      7.326150e+04
* 75%      1.805640e+05
* max      3.473238e+06
* Name: assets, dtype: float64

- Puede observar que los valores en el segmento de código anterior se ven un poco diferentes. Debido a que los valores para esta columna son demasiado largos para mostrarse con claridad, los pandas los han mostrado en notación electrónica , un tipo de notación científica :

* Notación original	   Fórmula expandida	      Resultado
*   5.000000E+02	     5.000000 * 10 ** 2	        500
*   2.436323E+05	     2.436323 * 10 ** 5	      243632.3

- Si usamos describe()en una columna que contiene valores no numéricos, obtenemos algunas estadísticas diferentes. Veamos un ejemplo:

* country = f500["country"]
* print(country.describe())

* count     500
* unique     34
* top       USA
* freq      132
* Name: country, dtype: object

- La primera estadística,, countes la misma que para las columnas numéricas, que nos muestra el número de valores no nulos. Las otras tres estadísticas son nuevas:

* unique: Número de valores únicos en la serie. En este caso, nos dice que hay 34 países diferentes representados en Fortune 500.
* top: Valor más común en la serie. Estados Unidos es el país que tiene la sede de la mayoría de las compañías Fortune 500
* freq: Frecuencia del valor más común. Exactamente 132 compañías de Fortune 500 tienen su sede en los Estados Unidos.

EJERCICIO: Vamos a utilizar este método para recopilar más información sobre la serie ranky previous_rank.

1. Devuelve una serie de estadísticas descriptivas para la rankcolumna en f500. * Seleccione la rankcolumna. Asígnele a una variable nombrada rank. * Utilice el Series.describe()método para devolver una serie de estadísticas para rank. Asigna el resultado a rank_desc.
2. Devuelve una serie de estadísticas descriptivas para la previous_rankcolumna en f500. * Seleccione la previous_rankcolumna. Asígnele a una variable nombrada prev_rank. * Utilice el Series.describe()método para devolver una serie de estadísticas para prev_rank. Asigna el resultado a prev_rank_desc. 3. Después de ejecutar su código, use el inspector de variables para ver cada una de las nuevas variables que creó. Intente identificar cualquier problema potencial con los datos antes de pasar a la siguiente pantalla.

* rank = f500["rank"]
* rank_desc = rank.describe()
* prev_rank = f500["previous_rank"]
* prev_rank_desc = prev_rank.describe()

# Método de Encadenamiento

- En el último ejercicio, utilizamos el Series.describe()método para explorar las columnas ranky previous_rank. Cuando revisó los resultados, es posible que haya notado algo extraño: el valor mínimo para la previous_rankcolumna es 0:

* prev_rank = f500["previous_rank"]
* print(prev_rank.describe())
* count    500.000000
* mean     222.134000
* std      146.941961
* min        0.000000
* 25%       92.750000
* 50%      219.500000
* 75%      347.250000
* max      500.000000
* Name: previous_rank, dtype: float64

- Sin embargo, esta columna solo debe tener valores entre 1 y 500 (inclusive), por lo que un valor 0no tiene sentido. Para investigar la posible causa de este problema, confirmemos el número de 0valores que aparecen en la previous_rankcolumna.

- Recuerde que en la última misión, aprendimos a usar el Series.value_counts()método para mostrar los conteos de los valores únicos en una columna:

* countries = f500["country"]
* countries_counts = countries.value_counts()

- En lugar de asignar la countriesserie a su propia variable, podemos omitir ese paso y usar el método directamente en el resultado de la selección de la columna:

* countries_counts = f500["country"].value_counts()

- Esto se denomina encadenamiento de métodos : una forma de combinar varios métodos en una sola línea.

En la última misión, también aprendimos a usar Series.loc[]para seleccionar solo un elemento de una serie por etiqueta. Por ejemplo, para seleccionar solo los recuentos China, usaríamos la siguiente línea de código:

* print(f500["country"].value_counts().loc["China"]) 109

- Desde aquí, verás el método de encadenamiento más a menudo en nuestras misiones. Al escribir el código, siempre evalúe si el encadenamiento de métodos hará que su código sea más difícil de leer. Si lo hace, siempre es preferible dividir el código en más de una línea.

- EJERCICIO: Usemos el Series.value_counts()método y a Series.loccontinuación para confirmar el número de 0valores en la previous_rankcolumna.  
- 1. Utilice Series.value_counts()y Series.locpara devolver el número de compañías con un valor de 0en la previous_rankcolumna en el f500marco de datos. Asigna los resultados a zero_previous_rank. 2. Después de ejecutar su código, use el inspector de variables para ver cada una de las nuevas variables que creó.

* zero_previous_rank = f500["previous_rank"].value_counts().loc[0]

#  Métodos de exploración de marco de datos

- En el último ejercicio, confirmamos que 33 empresas en el marco de datos tienen un valor de 0en la previous_rankcolumna. Dado que varias compañías tienen un 0rango, podemos concluir que estas compañías no tenían un rango en absoluto para el año anterior. Tendría más sentido para nosotros reemplazar estos valores con un valor nulo.

- Antes de corregir estos valores, exploremos el resto de nuestro marco de datos para asegurarnos de que no haya otros problemas de datos. Al igual que usamos métodos de estadísticas descriptivas para explorar series individuales, también podemos usar métodos de estadísticas descriptivas para explorar nuestro f500marco de datos.

- Dado que las series y los marcos de datos son dos objetos distintos, tienen sus propios métodos únicos. Sin embargo, hay muchas veces en que los objetos de serie y de marco de datos tienen un método del mismo nombre que se comporta de manera similar. A continuación se presentan algunos ejemplos:

* Series.max() y DataFrame.max()
* Series.min() y DataFrame.min()
* Series.mean() y DataFrame.mean()
* Series.median() y DataFrame.median()
* Series.mode() y DataFrame.mode()
* Series.sum() y DataFrame.sum()

- A diferencia de sus homólogos de serie, los métodos de marco de datos requieren un parámetro de eje para que sepamos qué eje se debe calcular. Si bien puede usar números enteros para referirse al primer y segundo ejes, los métodos de marco de datos de pandas también aceptan las cadenas "index"y "columns"para el parámetro de eje:

- Por ejemplo, si quisiéramos encontrar el valor mediano (medio) para las columnas revenuesy profits, podríamos usar el siguiente código:

* medians = f500[["revenues", "profits"]].median(axis=0)
* # También podemos usar .median(axis="index")
* print(medians)
* revenues    40236.0
* profits      1761.6
* dtype: float64

- De hecho, el valor predeterminado para el parámetro de eje con estos métodos es axis=0. ¡Podríamos haber usado el median()método sin un parámetro para obtener el mismo resultado!

- En este próximo ejercicio, le pediremos que consulte la documentación de uno de los métodos anteriores para completar la tarea. Aunque puede parecer intimidante al principio, es muy importante familiarizarse con la documentación, ¡es imposible memorizar todo en la biblioteca de pandas!

EJERCICIO: Utilice el DataFrame.max()método para encontrar el valor máximo solo para las columnas numéricas de f500(es posible que deba verificar la documentación). Asigna el resultado a la variable max_f500.

* max_f500 = f500.max(numeric_only=True)

# Método de descripción de marco de datos o DataFrame

- En el último ejercicio, usamos el DataFrame.max()método para devolver el máximo para todas las columnas numéricas en f500:

* f500.max(numeric_only=True)
* rank                            500.0
* revenues                     485873.0
* revenue_change                  442.3
* profits                       45687.0
* assets                      3473238.0
* profit_change                  8909.5
* previous_rank                   500.0
* years_on_global_500_list         23.0
* employees                   2300000.0
* total_stockholder_equity     301893.0
* dtype: float64

- En base a las descripciones de las columnas, el máximo para cada una de estas columnas parece razonable.

- Al igual que los objetos de serie, los objetos de marco de datos también tienen un DataFrame.describe()método que podemos usar para explorar el marco de datos más rápidamente. Le recomendamos que eche un vistazo a la documentación utilizando el enlace de la oración anterior para familiarizarse con algunas de las diferencias entre los dos métodos.

- Una diferencia es que debemos especificar manualmente si desea ver las estadísticas de las columnas no numéricas. Por defecto, DataFrame.describe()devolverá estadísticas solo para columnas numéricas. Si quisiéramos obtener solo las columnas de objetos, necesitamos usar el include=['O']parámetro:

* print(f500.describe(include=['O']))
* _            ceo    industry     sector  country  hq_location    website
* count        500         500        500      500          500        500
* unique       500          58         21       34          235        500
* top     Xavie...   Banks:...  Financ...      USA  Beijing,...  http:/...
* freq           1          51        118      132           56 

- Tenga en cuenta que, si bien el Series.describe()método devuelve un objeto de serie, el DataFrame.describe()método devuelve un objeto de marco de datos. Practiquemos usando el método de descripción de marco de datos a continuación.

- EJERCICIO: Devuelve un marco de datos de estadísticas descriptivas para todas las columnas numéricas en f500. Asigna el resultado a f500_desc.
* f500_desc = f500.describe()

# Asignación con pandas

- Después de revisar las estadísticas descriptivas para las columnas numéricas en f500, podemos concluir que no hay valores que parezcan inusuales además de los 0valores en la previous_rankcolumna. Anteriormente, llegamos a la conclusión de que las empresas con un rango de cero no tenían un rango en absoluto. A continuación, reemplazaremos estos valores con un valor nulo para indicar claramente que falta el valor.

- Aprenderemos cómo hacer dos cosas para poder corregir estos valores:

* Realizar tareas en pandas.
* Utilice la indexación booleana en pandas.

- Comencemos por aprender asignación, comenzando con el siguiente ejemplo:

* top5_rank_revenue = f500[["rank", "revenues"]].head()
* print(top5_rank_revenue)
* _                         rank  revenues
* Walmart                      1    485873
* State Grid                   2    315199
* Sinopec Group                3    267518
* China National Petroleum     4    262573
* Toyota Motor                 5    254694
OTRO EJEMPLO 
* top5_rank_revenue["revenues"] = 0
* print(top5_rank_revenue)
* _                        rank  revenues
* Walmart                      1         0
* State Grid                   2         0
* Sinopec Group                3         0
* China National Petroleum     4         0
* Toyota Motor                 5         0
- Al igual que en NumPy, las mismas técnicas que usamos para seleccionar datos podrían usarse para la asignación. Cuando seleccionamos una columna completa por etiqueta y usamos la asignación, asignamos el valor a cada elemento en esa columna.

- Al proporcionar etiquetas para ambos ejes, podemos asignarles un solo valor dentro de nuestro marco de datos.

* top5_rank_revenue.loc["Sinopec Group", "revenues"] = 999
* print(top5_rank_revenue)
* _                         rank  revenues
* Walmart                      1         0
* State Grid                   2         0
* Sinopec Group                3       999
* China National Petroleum     4         0
* Toyota Motor                 5         0

-EJERCICIO: Practiquemos la asignación de valores utilizando nuestro marco de datos completo de Fortune 500: 1. La compañía "Dow Chemical" ha nombrado un nuevo CEO. Actualice el valor donde se encuentra la etiqueta de la fila Dow Chemicaly para la ceocolumna Jim Fitterlingen el f500marco de datos.

* f500.loc["Dow Chemical","ceo"] = "Jim Fitterling"

# Usando la indexación booleana con objetos pandas

- Ahora que sabemos cómo asignar valores en pandas, estamos un paso más cerca de corregir los 0valores en la previous_rankcolumna.

- Si bien es útil poder reemplazar valores específicos cuando conocemos la etiqueta de la fila con anticipación, esto puede ser engorroso cuando necesitamos reemplazar muchos valores. En su lugar, podemos usar la indexación booleana para cambiar todas las filas que cumplan con los mismos criterios, al igual que hicimos con NumPy.

- Veamos dos ejemplos de cómo funciona la indexación booleana en pandas. Para nuestro ejemplo, trabajaremos con este marco de datos de personas y sus números favoritos:
* name      num
* Kylie     12
* Rahul      8
* Michael    5
* Sarah      8

- Vamos a ver qué personas tienen un número favorito de 8 . Primero, realizamos una operación booleana vectorizada que produce una serie booleana:
* num_bool =df["num"] ==8

- Podemos usar esa serie para indexar todo el marco de datos, dejándonos con las filas que corresponden solo a las personas cuyo número favorito es 8 

* result = df[num_bool]

- Tenga en cuenta que no usamos loc[]. Esto se debe a que las matrices booleanas utilizan el mismo acceso directo que los cortes para seleccionar a lo largo del eje del índice. También podemos usar la serie booleana para indexar solo una columna del marco de datos:

* result = df.loc[num_bool, "name"]

- En este caso, solíamos df.loc[]especificar ambos ejes.

A continuación, usemos la indexación booleana para identificar compañías que pertenecen a la industria de "Vehículos y piezas de motor" en nuestro conjunto de datos de Fortune 500.

- EJERCICIO: 1. Cree una serie booleana motor_bool, que compare si los valores en la industrycolumna del f500marco de datos son iguales a "Motor Vehicles and Parts". 2. Usa la motor_boolserie booleana para indexar la countrycolumna. Asigna el resultado a motor_countries.

* motor_bool = f500["industry"] == "Motor Vehicles and Parts"
*  motor_countries = f500.loc[motor_bool, "country"]

# Uso de matrices booleanas para asignar valores

- Ahora tenemos todo el conocimiento que necesitamos para arreglar los 0valores en la previous_rankcolumna:
* Realizar tareas en pandas.
* Utilice la indexación booleana en pandas.

- Veamos un ejemplo de cómo combinamos estas dos operaciones juntas. Para nuestro ejemplo, cambiaremos los 'Motor Vehicles & Parts'valores de la sectorcolumna a 'Motor Vehicles and Parts', es decir, cambiaremos el signo ( &) a and.

* Primero, creamos una serie booleana comparando los valores en la columna de sector para 'Motor Vehicles & Parts'
* ampersand_bool = f500["sector"] == "Motor Vehicles & Parts"

- A continuación, usamos esa serie booleana y la cadena "sector"para realizar la asignación.
* f500.loc[ampersand_bool,"sector"] = "Motor Vehicles and Parts"

- Al igual que vimos en la misión NumPy anteriormente en este curso, podemos eliminar el paso intermedio de crear una serie booleana y combinar todo en una sola línea. Esta es la forma más común de escribir código de pandas para realizar la asignación utilizando matrices booleanas:

* f500.loc[f500["sector"] == "Motor Vehicles & Parts","sector"] = "Motor Vehicles and Parts"

- Ahora podemos seguir este patrón para reemplazar los valores en la previous_rankcolumna. Reemplazaremos estos valores con np.nan. Al igual que en NumPy, np.nan

-Para facilitar la comparación de los valores en esta columna antes y después de nuestra operación, hemos agregado la siguiente línea de código al script.pycodebox:

* prev_rank_before = f500["previous_rank"].value_counts(dropna=False).head()

- Esto utiliza Series.value_counts()y Series.head()muestra los 5 valores más comunes en la previous_rankcolumna, pero agrega un dropna=Falseparámetro adicional , que impide que el Series.value_counts()método excluya los valores nulos cuando realiza su cálculo, como se muestra en la Series.value_counts()documentación. https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.value_counts.html

- EJERCICIO: 1. Use la indexación booleana para actualizar los valores en la previous_rankcolumna del f500marco de datos: * Ahora debería haber un valor de np.nandonde previamente había un valor de 0. * Depende de usted si asigna la serie booleana a su propia variable primero, o si completa la operación en una línea. 2. Cree una nueva serie de pandas prev_rank_afterusando la misma sintaxis que se usó para crear la prev_rank_beforeserie. 3. Después de ejecutar su código, use el inspector de variables para comparar prev_rank_beforey prev_rank_after.

* import numpy as np
* prev_rank_before = f500["previous_rank"].value_counts(dropna=False).head()
* f500.loc[f500["previous_rank"] == 0, "previous_rank"] = np.nan
* prev_rank_after = f500["previous_rank"].value_counts(dropna=False).head()

# Creación de nuevas columnas

- Es posible que haya notado que después de asignar valores de NaN, la previous_rankcolumna cambió dtype. Miremos más de cerca:

* print(prev_rank_after)
* NaN      33
* 471.0     1
* 234.0     1
* 125.0     1
* 166.0     1

- El índice de la serie que Series.value_counts()produce ahora nos muestra flotantes como en 471.0lugar de enteros. La razón detrás de esto es que pandas usa el tipo de entero NumPy, que no admite valores de NaN. Las pandas heredan este comportamiento, y en los casos en los que intentas asignar un valor de NaN a una columna entera, las pandas convertirán esa columna de forma silenciosa en un dtype flotante. Si está interesado en obtener más información sobre esto, hay una sección específica sobre valores de NaN enteros en la documentación de los pandas. https://pandas.pydata.org/pandas-docs/stable/user_guide/gotchas.html

- Ahora que hemos corregido los datos, vamos a crear la rank_changeserie de nuevo. Esta vez, lo agregaremos a nuestro f500marco de datos como una nueva columna.

- Cuando asignamos un valor o valores a una nueva etiqueta de columna, los pandas crearán una nueva columna en nuestro marco de datos. Por ejemplo, a continuación agregamos una nueva columna a un marco de datos llamado top5_rank_revenue:

* top5_rank_revenue["year_founded"] = 0
* print(top5_rank_revenue)
* _                         rank  revenues  year_founded
* Walmart                      1         0             0
* State Grid                   2         0             0
* Sinopec Group                3       999             0
* China National Petroleum     4         0             0
* Toyota Motor                 5         0             0

- EJERCICIO: Vamos a crear una rank_changecolumna en nuestro f500 marco de datos a continuación. 
1. Agregue una nueva columna nombrada rank_changeal f500marco de datos restando los valores en la rankcolumna de los valores en la previous_rankcolumna. 
2. Utilice el Series.describe()método para devolver una serie de estadísticas descriptivas para la rank_changecolumna. Asigna el resultado a rank_change_desc. 
3. Después de ejecutar su código, use el inspector de variables para ver cada una de las nuevas variables que creó. Verifique que el valor mínimo de la rank_changecolumna ahora sea mayor que -500.


* f500["rank_change"] = f500["previous_rank"] - f500["rank"]
* rank_change_desc = f500["rank_change"].describe()

# Desafío: Top Performers por país

- Terminaremos esta misión con un reto. Al igual que los desafíos en las misiones anteriores, este desafío está diseñado para ayudarte a practicar las técnicas que has aprendido en esta misión. Hemos brindado algunos consejos, pero si es posible, intente completar el desafío sin ellos.

- En este desafío, calcularemos una estadística o atributo específico de cada uno de los tres países más comunes a partir de nuestro f500marco de datos. Hemos identificado los tres países más comunes utilizando el siguiente código:

* top_3_countries = f500["country"].value_counts().head(3)
* print(top_3_countries)
* USA      132
* China    109
* Japan     51
* Name: country, dtype: int64

- Al igual que el DataFrame.head()método, el Series.head()método devuelve los primeros cinco elementos de una serie de forma predeterminada, o un número diferente si proporciona un argumento, como el anterior.

- No se desanime si esto requiere algunos intentos para hacerlo bien. ¡Trabajar con datos es un proceso iterativo!

- EJERCICIO: 1. Cree una serie que industry_usacontenga los recuentos de las dos industrias más comunes para las empresas con sede en los Estados Unidos.
2. Cree una serie que sector_chinacontenga los recuentos de los tres sectores más comunes para las empresas con sede en China.
3. Cree un objeto flotante mean_employees_japan, que contenga el número medio (promedio) de empleados para las empresas con sede en Japón
* top_3_countries = f500["country"].value_counts().head(3)
* industry_usa = f500["industry"][f500["country"] == "USA"].value_counts().head(2)
* sector_china = f500["sector"][f500["country"] == "China"].value_counts().head(3)
* mean_employees_japan = f500["employees"][f500["country"] == "Japan"].mean()

# 5. Exploring Data with Pandas: Intermediate

# Introducción

- En esta misión, continuaremos trabajando con el conjunto de datos de Fortune Global 500 de 2017 a medida que aprendemos técnicas más avanzadas de selección y exploración.

- Hasta ahora, hemos proporcionado el código para leer el archivo CSV en pandas para usted. Para comenzar esta misión, aprenderemos cómo usar la pandas.read_csv()función para leer en archivos CSV.

- Echemos un vistazo a las primeras líneas de nuestro archivo CSV en su forma original. Para facilitar la lectura, solo mostramos las primeras cuatro columnas de cada línea:

* company,rank,revenues,revenue_change
* Walmart,1,485873,0.8
* State Grid,2,315199,-4.4
* Sinopec Group,3,267518,-9.1
* China National Petroleum,4,262573,-12.3
* Toyota Motor,5,254694,7.7

- EJERCICIO: Ya hemos leído el conjunto de datos en un marco de datos de pandas y lo hemos asignado a una variable nombrada f500. También reemplazamos todos los 0valores en la previous_rankcolumna con NaN, como hicimos en la misión anterior.

1. Seleccione la rank, revenuesy revenue_changecolumnas en f500. Luego, usa el DataFrame.head()método para seleccionar las primeras cinco filas. Asigna el resultado a f500_selection.
2. Utilice el inspector de variables para ver f500_selection. Compare los resultados con las primeras líneas de nuestro archivo CSV anterior.
3. Eche un vistazo a la documentación de la pandas.read_csv()función para intentar comprender los resultados. Si tienes problemas para entender, ¡no te preocupes! Explicaremos los resultados en la siguiente pantalla.

* import pandas as pd
* # read the data set into a pandas dataframe
* f500 = pd.read_csv("f500.csv", index_col=0)
* f500.index.name = None

* # replace 0 values in the "previous_rank" column with NaN
* f500.loc[f500["previous_rank"] == 0, "previous_rank"] = np.nan
* f500_selection = f500[['rank', 'revenues', 'revenue_change']].head()

# Leyendo archivos CSV con pandas
- Cuando comparamos el resultado de las primeras líneas de par en el CSV, se habrán dado cuenta de que las etiquetas de los ejes índice son en realidad los valores de la primera columna en el conjunto de datos, company:

* company,rank,revenues,revenue_change
* Walmart,1,485873,0.8
* State Grid,2,315199,-4.4
* Sinopec Group,3,267518,-9.1
* China National Petroleum,4,262573,-12.3
* Toyota Motor,5,254694,7.7

- Si revisamos la documentación de la read_csv()función , podemos ver que el index_colparámetro es un argumento opcional y deberíamos especificar qué columna usar como etiquetas de fila para el marco de datos. Cuando usamos un valor de 0, especificamos que queríamos usar la primera columna como etiquetas de fila.

- Veamos lo que la f500trama de datos se ve como si quitamos la segunda línea: f500.index.name = None.

* f500 = pd.read_csv("f500.csv", index_col=0)
* _                         rank  revenues  revenue_change
* company                                                    
* Walmart                      1    485873             0.8
* State Grid                   2    315199            -4.4
* Sinopec Group                3    267518            -9.1
* China National Petroleum     4    262573           -12.3
* Toyota Motor                 5    254694             7.7

- Observe que sobre las etiquetas de índice está el texto company, el nombre de la primera columna en el CSV. Las pandas utilizaron este valor como el nombre del eje para el eje del índice.

- Tanto la columna como los ejes de índice pueden tener nombres asignados a ellos. Sin embargo, originalmente utilizamos el siguiente código para acceder al nombre de los ejes de índice y configurarlo None, por lo que nuestro marco de datos no tenía un nombre para el eje de índice:

* f500.index.name = None

Volvamos al código restante:

* f500 = pd.read_csv("f500.csv", index_col=0)

- EJERCICIO: A continuación, veamos cómo se ve el marco de datos si lo usamos pandas.read_csv()sin el index_colparámetro.
1. Utilice la pandas.read_csv()función para leer el f500.csvarchivo CSV como un marco de datos de pandas. Asígnelo al nombre de la variable f500. * No utilice el index_colparámetro.
2. Use el siguiente código para insertar los valores de NaN en la previous_rankcolumna: f500.loc[f500["previous_rank"] == 0, "previous_rank"] = np.nan

* f500 = pd.read_csv("f500.csv")
* f500.loc[f500["previous_rank"] == 0, "previous_rank"] = np.nan

# Usando iloc para seleccionar por posición entera

- Hay dos diferencias con este enfoque:

- La company columna ahora se incluye como una columna regular, en lugar de usarse para el índice.
Las etiquetas de índice son ahora enteros a partir de 0.
Esta es la forma más convencional de leer en un marco de datos, y es el método que usaremos de aquí en adelante.

- Recuerde que cuando trabajamos con un marco de datos con etiquetas de índice de cadena , usamos loc[] para seleccionar datos:

* EJEMPLO: QUEREMOS SELECCIONAR LA FILA z DE LA COLUMNA A
* df.loc["z","A"]
* df.loc["y"] SELECCIONANDO LA FILA y

- En algunos casos, el uso de etiquetas para hacer selecciones hace que las cosas sean más fáciles, aunque en otros, las cosas son más difíciles.

- Al igual que en NumPy, también podemos usar posiciones enteras para seleccionar datos usando Dataframe.iloc[]y Series.iloc[]. Es fácil confundirse loc[]y iloc[]confundirse al principio, pero la forma más fácil es recordar la primera letra de cada método:

* l oc: l selección basada en etiqueta
* i loc: selección basada en posición entera

- El uso iloc[]es casi idéntico a la indexación con NumPy, con posiciones enteras que comienzan en 0ndarrays y listas de Python. Veamos cómo realizaríamos la selección anterior utilizando iloc[]:

* EJEMPLO: QUEREMOS SELECCIONAR LA FILA z DE LA COLUMNA A
* df.iloc[2,0]
* df.iloc["1"] SELECCIONANDO LA FILA y

- Como puedes ver, se DataFrame.iloc[] comporta de manera similar a DataFrame.loc[]. La sintaxis completa para DataFrame.iloc[], en pseudocódigo, es:

* df.iloc[row_index, column_index]

- EJERCICIO: Practiquemos usando el iloc[]siguiente.

* Seleccione solo la quinta fila del f500marco de datos. Asignar el resultado afifth_row
* Seleccione el valor en la primera fila de la companycolumna. Asigna el resultado a company_value.

* fifth_row = f500.iloc[4]
* company_value = f500.iloc[0,0] 

# Usando iloc para seleccionar por posición entera continua

- En la última pantalla, aprendimos cómo seleccionar una fila o un valor por posición de enteros usando DataFrame.iloc[]. Como recordatorio, la sintaxis completa de DataFrame.iloc[]pseudocódigo es:

* df.iloc [row_index, column_index]

- Digamos que queríamos seleccionar solo la primera columna de nuestro f500marco de datos. Para hacer esto, usamos :(dos puntos) para especificar todas las filas, y luego usamos el entero 0para especificar la primera columna:

* first_column = f500.iloc[:,0]
* print(first_column)

* 0                        Walmart
* 1                     State Grid
* 2                  Sinopec Group
* ...
* 497    Wm. Morrison Supermarkets
* 498                          TUI
* 499                   AutoNation
* Name: company, dtype: object

- Para especificar un sector de posición, podemos aprovechar el mismo acceso directo que usamos con las etiquetas. Así es como seleccionaríamos las filas entre las posiciones del índice de una a cuatro (inclusive):

* second_to_sixth_rows = f500[1:5]
* company  rank  revenues ... employees  total_stockholder_equity
* 1         State Grid     2    315199 ...    926067                    209456
* 2      Sinopec Group     3    267518 ...    713288                    106523
* 3  China National...     4    262573 ...   1512048                    301893
* 4       Toyota Motor     5    254694 ...    364445                    157210

- En el ejemplo anterior, la fila en la posición del índice 5no está incluida, como si estuviéramos rebanando con una lista de Python o NumPy ndarray. Recordemos que loc[]maneja rebanar de manera diferente:

* Con loc[], se incluye la porción final .
* Con iloc[], la porción final no está incluida.

- La siguiente tabla resume cómo podemos usar DataFrame.iloc[]y Series.iloc[]seleccionar por posición de entero:

* Seleccionar por posición entera	              Sintaxis explícita	   Convención de taquigrafía
* Columna única desde el marco de datos	           df.iloc[:,3]	
* Lista de columnas del marco de datos	        df.iloc[:,[3,5,6]]	
* Rebanada de columnas de dataframe	             df.iloc[:,3:7]	
* Una sola fila desde el marco de datos	           df.iloc[20]	
* Lista de filas del marco de datos	              df.iloc[[0,3,8]]	
* Rebanada de filas de dataframe	                 df.iloc[3:5]	                df[3:5]
* Artículos sueltos de la serie.	                  s.iloc[8]	                    s[8]
* Lista de elementos de la serie	                s.iloc[[2,8,1]]            	s[[2,8,1]]
* Rebanada de artículos de la serie.	             s.iloc[5:10]	                s[5:10]

- EJERCICIO: Practiquemos usando el DataFrame.iloc[] siguiente.
1. Seleccione las primeras tres filas del f500marco de datos. Asigna el resultado a first_three_rows.
2. Seleccione la primera y séptima filas y las primeras cinco columnas del f500marco de datos. Asigna el resultado a first_seventh_row_slice.
* first_three_rows = f500[:3]
* first_seventh_row_slice = f500.iloc[[0,6], :5]

# Usando métodos pandas para crear máscaras booleanas. Using pandas methods to create boolean masks

- En el último par de misiones, se utilizó Python operadores booleanos como >, <y ==la creación de máscaras booleanas para seleccionar subconjuntos de datos. También hay una serie de métodos pandas que devuelven máscaras booleanas útiles para explorar datos.

- Dos ejemplos son el Series.isnull()método y el Series.notnull()método . Se pueden usar para seleccionar filas que contengan valores nulos (o NaN) o filas que no contengan valores nulos para una columna determinada.

- Primero, usemos el Series.isnull()método para ver las filas con valores nulos en la revenue_changecolumna:

* rev_is_null = f500["revenue_change"].isnull()
* print(rev_is_null.head())
* 0    False
* 1    False
* 2    False
* 3    False
* 4    False
* Name: revenue_change, dtype: bool

- Vemos que el Series.isnull()resultado resultó en una serie booleana. Al igual que en NumPy, podemos usar esta serie para filtrar nuestro marco de datos f500:

* rev_change_null = f500[rev_is_null]
* print(rev_change_null[["company","country","sector"]])
* _`                       company  country      sector
* 90                       Uniper  Germany      Energy
* 180  Hewlett Packard Enterprise      USA  Technology 

- Podemos confirmar que las dos compañías con valores faltantes para la revenue_changecolumna son Uniper, una compañía de energía alemana, y Hewlett Parkard Enterprise, una compañía de tecnología estadounidense. Usemos lo que hemos aprendido para encontrar los valores nulos en la previous_rankcolumna siguiente.

- EJERCICIO: Utilice el Series.isnull() método para seleccionar todas las filas f500que tengan un valor nulo para la previous_rankcolumna. Seleccionar sólo los company, ranky previous_rankcolumnas. Asigna el resultado a null_previous_rank.

* null_previous_rank = f500[f500["previous_rank"].isnull()][["company","rank", "previous_rank"]]

# Trabajar con etiquetas enteras
- En el último ejercicio, seleccionamos las filas con valores nulos en la previous_rankcolumna. A continuación se muestran las primeras dos filas:

*      Empresa	                   Rango	  previous_rank
* 48	 Grupo Legal y General	      49	         NaN
* 90	 Uniper	                      91	         NaN
* 123	 Tecnologías Dell	           124	         NaN
* Anterior, podemos ver que los ejes índice de etiquetas para esta selección son 48, 90y 123.

- Si quisiéramos seleccionar la primera compañía de nuestro nuevo null_previous_rankmarco de datos por posición de entero , podemos usar DataFrame.iloc[]:

* first_null_prev_rank = null_previous_rank.iloc[0]
* print(first_null_prev_rank)
* company          Legal & General Group
* rank                                49
* previous_rank                      NaN
* Name: 48, dtype: object

- Veamos que pasa cuando usamos en DataFrame.loc[]lugar de DataFrame.iloc[]:

* first_null_prev_rank = null_previous_rank.loc[0]              KeyError: 'the label [0] is not in the [index]'

- Recibimos un error que nos dice que the label [0] is not in the [index](el seguimiento real de este error es mucho más largo que esto). Recuerde que DataFrame.loc[]se utiliza para la selección basada en etiquetas :

* l oc: l selección basada en etiqueta
* i loc: selección basada en posición entera
* Debido a que no hay una fila con una 0etiqueta en el índice, obtuvimos el error anterior. Si quisiéramos seleccionar una fila usando loc[], tendríamos que usar la etiqueta entera para la primera fila - 48.

- Siempre piense detenidamente si desea seleccionar por etiqueta o posición de entero . Utilizar DataFrame.loc[]o en DataFrame.iloc[]consecuencia.

* iloc[1]  Usa la posición entera de la fila para seleccionar la segunda fila.                       df.iloc[1] SELECCIONAR FILA 2
* loc[1]   Utiliza la etiqueta de la fila para seleccionar la fila con una etiqueta de eje de 1.     df.loc[1]  SELECCIONAR FILA 1

- EJERCICIO: Asigne las primeras cinco filas del null_previous_rankmarco de datos a la variable seleccionando top5_null_prev_rankel método correcto de cualquiera de loc[]o iloc[].

*  null_previous_rank = f500[f500["previous_rank"].isnull()]
*  top5_null_prev_rank = null_previous_rank.iloc[:5]

# Alineación del índice de pandas

- Ahora que hemos identificado las filas con valores nulos en la previous_rankcolumna, usemos el Series.notnull()método para excluirlas de la siguiente parte de nuestro análisis.

* previously_ranked = f500[f500["previous_rank"].notnull()]

- Luego podemos crear una rank_changecolumna restando la rankcolumna de la previous_rankcolumna:

* rank_change = previously_ranked["previous_rank"] - previously_ranked["rank"]
* print(rank_change.shape)
* print(rank_change.tail(3))

* (467,)
* 496   -70.0
* 497   -61.0
* 498   -32.0
* dtype: float64

- Arriba, podemos ver que nuestra rank_changeserie tiene 467 filas. Como la última etiqueta de índice de enteros es 498, sabemos que nuestras etiquetas de índice ya no se alinean con las posiciones de enteros.

- Supongamos que ahora decidimos agregar la rank_changeserie al f500marco de datos como una nueva columna. Sus etiquetas de índice ya no coinciden con las etiquetas de índice f500, así que, ¿cómo podría hacerse esto?

- Otro aspecto poderoso de los pandas es que casi todas las operaciones se alinearán en las etiquetas de índice . Veamos un ejemplo: a continuación tenemos un marco de datos llamado foody una serie llamada alt_name:

*                fruit_veg        qty 
* tomato          fruit            4         
* carrot          veg              2
* lime            fruit            4                                     arugula     rocket
* corn            veg              1                                     eggplant    aubergine
* eggplant        veg              2                                     corn        maize
                       food                                                          alt_name
                       
- La foodtrama de datos y la alt_nameserie no sólo tienen un número diferente de elementos, pero también sólo tienen dos de las mismas etiquetas de índice - corny eggplant- y ellos están en diferentes órdenes. Si quisiéramos agregar alt_name una nueva columna en nuestro food marco de datos, podemos usar el siguiente código:

* food["alt_name"] = alt_name

- Cuando hagamos esto, los pandas ignorarán el orden de la alt_nameserie y se alinearán en las etiquetas de índice:

- Pandas también:

* Deseche cualquier elemento que tenga un índice que no coincida con el marco de datos (como arugula).
* Rellene las filas restantes con NaN.

- A continuación se muestra el resultado:

*                fruit_veg        qty     alt_name
* tomato          fruit            4         NaN
* carrot          veg              2         NaN
* lime            fruit            4         NaN                            
* corn            veg              1        maize                           
* eggplant        veg              2       aubergine                              
                       food                    
                       
- La biblioteca de pandas se alineará en el índice en cada oportunidad, sin importar si nuestras etiquetas de índice son cadenas o enteros; esto hace que trabajar con datos de diferentes fuentes o trabajar con datos cuando hemos eliminado, agregado o reordenado filas sea mucho más fácil de lo que sería de otra manera.                      

- EJERCICIO:
1. Utilice el Series.notnull() método para seleccionar todas las filas f500 que tengan un valor no nulo para la previous_rank columna. Asignar el resultado apreviously_ranked
2. Desde el previously_ranked marco de datos, resta la rankcolumna de la previous_rank columna. Asigna el resultado a rank_change.
3. Asignar los valores en la rank_changeque una nueva columna en la f500 trama de datos, "rank_change".

* previously_ranked = f500[f500["previous_rank"].notnull()]
* rank_change = previously_ranked["previous_rank"] - previously_ranked["rank"]
* f500["rank_change"] = rank_change

# Usando Operadores Booleanos

- La indexación booleana es una herramienta poderosa que nos permite seleccionar o excluir partes de nuestros datos en función de sus valores. Sin embargo, para responder preguntas más complejas, necesitamos aprender a combinar arreglos booleanos.

- Para recapitular, las matrices booleanas se crean utilizando cualquiera de los operadores de comparación estándar de Python : ==(igual), >(mayor que), <(menor que), !=(no igual).

- Combinamos matrices booleanas utilizando operadores booleanos . En Python, estos operadores booleanos son and, ory not. En pandas, los operadores son ligeramente diferentes:

* Pandas	       Equivalente de Python	              Sentido
* a & b	               a and b	              True si ambos a y b son True, si no False
* a | b	               a or b	                True si cualquiera a o b es True
* ~a	                 not a	                True si aes False, si no False

- Veamos un ejemplo usando f500_sel una pequeña selección de nuestro f500 marco de datos:

*       Company             Revenues    Country              * cols = ["company", "revenues", "country"] 
* 0     Walmart             485873      USA                  * f500_sel = f500[cols].head()
* 1     State Grid          315199      China
* 2     Sinopec Group       267518      China    
* 3     China Nation        262573      China
* 4     Toyota Motor        254694      Japan
                                 f500_sel
- Supongamos que quisiéramos encontrar las empresas f500_selcon más de 265 mil millones de ingresos que tienen su sede en China. Comenzaremos realizando dos comparaciones booleanas para producir dos matrices booleanas separadas (la columna de ingresos ya está en millones).

* 0   True            * 0   False               * over_265 = f500_sel["revenue"] > 265000
* 1   True            * 1   True                * china = f500_sel["country"] == "china"
* 2   True            * 2   True
* 3   False           * 3   True 
* 4   False           * 4   False  
  over_265               china
  
- Luego usamos el &operador para combinar las dos matrices booleanas usando la lógica booleana "and":

* 0   True     &      * 0   False     =     * 0   False      
* 1   True     &      * 1   True      =     * 1   True        *  combined = over_265 & china
* 2   True     &      * 2   True      =     * 2   True
* 3   False    &      * 3   True      =     * 3   False
* 4   False    &      * 4   False     =     * 4   False
  over_265               china                combined

- Por último, utilizamos la matriz booleana combinada para realizar la selección en nuestro marco de datos:

* final_cols = ["company","revenues"]   
* result = f500_sel.loc[combined, final_cols]
*                Company           Revenues    Country              
* 0   False     Walmart             485873      USA          Company           Revenues             
* 1   True  =>  State Grid          315199      China   =>  State Grid          315199
* 2   True  =>  Sinopec Group       267518      China   =>  Sinopec Group       267518 
* 3   False     China Nation        262573      China          result
* 4   False     Toyota Motor        254694      Japan          
   combined      f500_sel


- El resultado nos da dos compañías de las f500_selcuales ambas son chinas y tienen más de 265 mil millones en ingresos.

-EJERCICIO: Practiquemos una selección más compleja utilizando operadores booleanos.
1. Seleccione todas las compañías con ingresos de más de 100 mil millones y ganancias negativas del f500marco de datos. El resultado debe incluir todas las columnas.
* Seleccione las empresas con ingresos superiores a los 100 mil millones. Asigna el resultado a large_revenue.
* Seleccione las empresas con ganancias menores a 0. Asigne el resultado a negative_profits.
* Combinar large_revenue y negative_profits. Asigna el resultado a combined.
* Utilizar combined para filtrar f500. Asigna el resultado a big_rev_neg_profit.

* large_revenue =  f500["revenues"] > 100000
* negative_profits = f500["profits"] < 0
* combined = large_revenue & negative_profits
* big_rev_neg_profit =  f500[combined]

# Usando Operadores Booleanos Continúa

- En el último ejercicio, identificamos compañías que tienen más de 100 mil millones en ingresos y ganancias negativas:

- Al igual que cuando usamos una única matriz booleana para realizar la selección, no necesitamos usar variables intermedias. El primer lugar donde podemos optimizar nuestro código es mediante la combinación de nuestros dos arreglos booleanos en una sola línea, en lugar de asignarlos primero al intermedio large_revenuey las negative_profitsvariables:

* combined = (f500["revenues"] > 100000) & (f500["profits"] < 0)

- Observe que usamos paréntesis alrededor de cada una de nuestras comparaciones booleanas. Esto es muy importante: nuestra operación booleana fallará sin paréntesis . Por último, en lugar de asignar las matrices booleanas a combined, podemos insertar la comparación directamente en nuestra selección:

* big_rev_neg_profit = f500[(f500["revenues"] > 100000) & (f500["profits"] < 0)]

- Si realizar este paso final es una cuestión de gustos. Como siempre, su decisión debe basarse en lo que hará que su código sea más legible.

- Practiquemos una selección más compleja usando operadores booleanos a continuación:

* Pandas	       Equivalente de Python	              Sentido
* a & b	               a and b	              True si ambos a y b son True, si no False
* a | b	               a or b	                True si cualquiera a o b es True
* ~a	                 not a	                True si aes False, si no False

- EJERCICIO: 1. Seleccione todas las filas para empresas con sede en Brasil o Venezuela. Asigna el resultado a brazil_venezuela.
2. Seleccione las primeras cinco compañías en el sector de Tecnología que no tienen su sede en los EE f500. UU. Desde el marco de datos. Asigna el resultado a tech_outside_usa.

* filter_brazil_venezuela = (f500["country"] == "Brazil") | (f500["country"] == "Venezuela")
* brazil_venezuela = f500[filter_brazil_venezuela]

* filter_tech_outside_usa = (f500["sector"] == "Technology") & ~(f500["country"] == "USA")
* tech_outside_usa = f500[filter_tech_outside_usa].head()

#  Valores de clasificación

- Continuemos respondiendo preguntas más complejas sobre nuestro conjunto de datos. Supongamos que quisiéramos encontrar la compañía que emplea a la mayoría de las personas en China. Podemos lograr esto seleccionando primero todas las filas donde la countrycolumna es igual a China:

* selected_rows = f500[f500["country"] == "China"]

- Luego, podemos usar el DataFrame.sort_values()método para ordenar las filas en la employeescolumna. Para ello, pasamos el nombre de la columna al método:

* sorted_rows = selected_rows.sort_values("employees")
* print(sorted_rows[["company", "country", "employees"]].head())
* RESPUESTA
*  employees
* 204                         Noble Group   China       1000
* 458             Yango Financial Holding   China      10234
* 438  China National Aviation Fuel Group   China      11739
* 128                         Tewoo Group   China      17353
* 182            Amer International Group   China      17852

- De forma predeterminada, el sort_values()método ordenará las filas en orden ascendente , de menor a mayor. Para ordenar las filas en orden descendente , para que aparezca primero la empresa con el mayor número de empleados, podemos configurar el ascendingparámetro para False:

* sorted_rows = selected_rows.sort_values("employees", ascending=False)
print(sorted_rows[["company", "country", "employees"]].head())
* RESPUESTA
* _                       company country  employees
* 3      China National Petroleum   China    1512048
* 118            China Post Group   China     941211
* 1                    State Grid   China     926067
* 2                 Sinopec Group   China     713288
* 37   Agricultural Bank of China   China     501368

- Ahora, podemos ver que la compañía china que emplea a la mayoría de las personas es China National Petroleum. Vamos a buscar la empresa japonesa con más empleados a continuación.

- EJERCICIO:
1. Encuentre la compañía con sede en Japón con el mayor número de empleados.
* Seleccione solo las filas que tienen un nombre de país igual a Japan.
* Utilice DataFrame.sort_values()para ordenar esas filas por la employeescolumna en orden descendente .
* Utilice DataFrame.iloc[] para seleccionar la primera fila del marco de datos ordenado.
* Extraiga el nombre de la compañía de la etiqueta de índice companyde la primera fila. Asigna el resultado a top_japanese_employer.
2. Después de ejecutar su código, use el inspector de variables para ver el empleador principal para Japan.

* selected_rows = f500[f500["country"] == "Japan"]
* sorted_rows = selected_rows.sort_values("employees", ascending=False)
* top_japanese_employer = sorted_rows.iloc[0]["company"]

# Usando Loops = bucles con pandas

- En la última pantalla, confirmamos que la compañía japonesa que emplea a la mayoría de las personas es Toyota Motor.

- Supongamos que quisiéramos calcular la compañía que emplea a la mayoría de las personas en cada uno de los 34 países. El uso del método de la última pantalla sería muy ineficiente, por lo que confiaremos en una técnica que aún no hemos usado con pandas: bucles.

- Hemos evitado explícitamente el uso de bucles en pandas porque uno de los beneficios clave de los pandas es que tiene métodos vectorizados para trabajar con datos de manera más eficiente. Aprenderemos técnicas más avanzadas en cursos posteriores, pero por ahora, aprenderemos cómo usar los bucles para la agregación .

- La agregación es donde aplicamos una operación estadística a grupos de nuestros datos. Digamos que queríamos calcular el ingreso promedio para cada país en el conjunto de datos. Nuestro proceso podría verse así:

* Identificar cada país único en el conjunto de datos.
* Para cada país:   * Selecciona solo las filas correspondientes a ese país.
                    * Calcule el ingreso promedio para esas filas.

- Para identificar los países únicos, podemos utilizar el Series.unique()método . Este método devuelve una matriz de valores únicos de cualquier serie. Luego, podemos hacer un bucle sobre esa matriz y realizar nuestra operación. Esto es lo que parece:

* # Crear un diccionario vacio para almacenar los resultados
* avg_rev_by_country = {}
* # Crear una matriz de paises únicos
* countries = f500["country"].unique()
* # Use un bucle for para interar sobre los paises
* for c in countries:
* # Use la comparación booleana para seleccionar solo las filas que corresponden a un país especifico 
*     selected_rows = f500[f500["country"] == c]
* # Calcula el ingreso promedio  solo para esasa filas
*     mean = selected_rows["revenues"].mean()
* # Assigne el valor medio al diccionario, usando nombre del pais como clave 
*     avg_rev_by_country[c] = mean
    
- El diccionario resultante está debajo (hemos mostrado solo las primeras teclas):     
    
*     {'Australia': 33688.71428571428,
*  'Belgium': 45905.0,
*  'Brazil': 52024.57142857143,
*  'Britain': 51588.708333333336,
*  'Canada': 31848.0,
*  'China': 55397.880733944956,
*  'Denmark': 35464.0,
*  ...}

- EJERCICIO: En este ejercicio, vamos a producir el siguiente diccionario del empleador principal en cada país:

* {'Australia': 'Wesfarmers',
*  'Belgium': 'Anheuser-Busch InBev',
*  'Brazil': 'JBS',
*  ...
*  'U.A.E': 'Emirates Group',
*  'USA': 'Walmart',
*  'Venezuela': 'Mercantil Servicios Financieros'}
1. Crea un diccionario vacío, top_employer_by_countrypara almacenar los resultados del ejercicio.
2. Utilice el Series.unique() método para crear una matriz de valores únicos de la countrycolumna.
3. Utilice un bucle for para iterar sobre los países únicos de la matriz. En cada iteración:
* Seleccione solo las filas que tienen un nombre de país igual a la iteración actual.
* Utilice DataFrame.sort_values() para ordenar esas filas por la employeescolumna en orden descendente .
* Seleccione la primera fila del marco de datos ordenado.
* Extraiga el nombre de la compañía de la etiqueta de índice companyde la primera fila.
* Asigne los resultados al top_employer_by_country diccionario, utilizando el nombre del país como clave y el nombre de la empresa como valor.
4. Después de ejecutar su código, use el inspector de variables para ver el principal empleador de cada país.

* top_employer_by_country = {}

* countries = f500["country"].unique()
* for c in countries:
*     selected_rows = f500[f500["country"] == c]
*     sorted_rows = selected_rows.sort_values("employees", ascending=False)
*     top_employer = sorted_rows.iloc[0]
*     employer_name = top_employer["company"]
*     top_employer_by_country[c] = employer_name

# Desafío: Cálculo del rendimiento de los activos por país

- ¡Ahora es el momento de un desafío para juntar todo! En este desafío, agregaremos una nueva columna a nuestro marco de datos y luego realizaremos una agregación utilizando esa nueva columna.

- La columna que creamos contendrá una métrica llamada retorno de activos (ROA). ROA es una métrica específica de negocios que indica la capacidad de una empresa para obtener ganancias utilizando sus activos disponibles. 

* return on assets = profit / assets         https://www.inc.com/encyclopedia/return-on-assets-roa.html

- Una vez que hayamos creado la nueva columna, agregaremos por sector y encontraremos la compañía con el ROA más alto de cada sector. Al igual que los desafíos anteriores, brindaremos orientación en los consejos, pero si es posible, intente completarlos sin ellos.

- No se desanime si este desafío requiere algunos intentos para ser correcto. Trabajar de manera iterativa es una excelente manera de trabajar, y este desafío es más difícil que los ejercicios que ha completado anteriormente.

- EJERCICIO: 1. Cree una nueva columna roaen el f500marco de datos, que contenga la métrica de retorno de activos para cada compañía.
2. Agregue los datos por la sectorcolumna y cree un diccionario top_roa_by_sectorcon:
* Teclas de diccionario con el nombre del sector.
* Diccionario de valores con el nombre de la empresa con el valor de ROA más alto de ese sector.

* f500["roa"] = f500["profits"] / f500["assets"]

* top_roa_by_sector = {}
* for sector in f500["sector"].unique():
*     is_sector = f500["sector"] == sector
*     sector_companies = f500.loc[is_sector]
*     top_company = sector_companies.sort_values("roa",ascending=False).iloc[0]
*     company_name = top_company["company"]
*     top_roa_by_sector[sector] = company_name

# 6. Fundamentos De Limpieza De Datos

- Hasta ahora, hemos aprendido cómo seleccionar, asignar y analizar datos con pandas usando datos previamente limpiados. En realidad, los datos rara vez están en el formato necesario para realizar el análisis. Los científicos de datos suelen pasar más de la mitad de su tiempo limpiando datos , por lo que saber cómo limpiar datos "desordenados" es una habilidad extremadamente importante.

* https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/#47dd0c406f63

- En esta misión, aprenderemos los aspectos básicos de la limpieza de datos con pandas mientras trabajamos con laptops.csvun archivo CSV que contiene información sobre 1,300 computadoras portátiles. Las primeras cinco filas del archivo CSV se muestran a continuación:

* laptops = pd.read_csv("laptops.csv")          UnicodeDecodeError                        Traceback 

- Recibimos un error! (El mensaje de error se ha acortado). Este error hace referencia a UTF-8, que es un tipo de codificación . Las computadoras, en sus niveles más bajos, solo pueden entender binarios 0 y 1, y las codificaciones son sistemas para representar caracteres en binarios.

- Algo que podemos hacer si nuestro archivo tiene una codificación desconocida es probar las codificaciones más comunes:

* UTF-8
* Latin-1 (también conocido como ISO-8895-1)
* Windows-1251

- La pandas.read_csv() función tiene un encoding argumento que podemos usar para especificar una codificación:

* df = pd.read_csv("filename.csv", encoding="some_encoding")

- Como la pandas.read_csv()función ya intentó leer el archivo con UTF-8 y falló, sabemos que el archivo no está codificado con ese formato. Probemos la siguiente codificación más popular en el ejercicio.

- EJERCICIO: 
* import pandas as pd                                           Importar la biblioteca de pandas.
* laptops = pd.read_csv('laptops.csv', encoding='Latin-1')      Utilice la pandas.read_csv()función para leer el laptops.csvarchivo en un marco de datos laptops. Especifique la codificación utilizando la cadena "Latin-1".
* laptops.info()                             Utilice el DataFrame.info()método para mostrar información sobre el laptopsmarco de datos.

# Nombres de columnas de limpieza

- A continuación se muestra el resultado del DataFrame.info()método de la pantalla anterior:

* class 'pandas.core.frame.DataFrame'
* RangeIndex: 1303 entries, 0 to 1302
* Data columns (total 13 columns):
* Manufacturer                1303 non-null object
* Model Name                  1303 non-null object
* Category                    1303 non-null object
* Screen Size                 1303 non-null object
* Screen                      1303 non-null object
* CPU                         1303 non-null object
* RAM                         1303 non-null object
*  Storage                    1303 non-null object
* GPU                         1303 non-null object
* Operating System            1303 non-null object
* Operating System Version    1133 non-null object
* Weight                      1303 non-null object
* Price (Euros)               1303 non-null object
* dtypes: object(13)
* memory usage: 132.4+ KB

- Podemos ver que cada columna se representa como el objecttipo, lo que indica que están representadas por cadenas, no por números. Además, una de las columnas,Operating System Version

- Las etiquetas de las columnas tienen una variedad de letras mayúsculas y minúsculas, así como espacios y paréntesis, lo que hará que sea más difícil trabajar con ellas y leerlas. Un problema notable es que la" Storage"

- Podemos acceder al eje de columna de un marco de datos utilizando el DataFrame.columnsatributo . Esto devuelve un objeto de índice, un tipo especial de Ndarray NumPy, con las etiquetas de cada columna:

* print(laptops.columns)

Index(['Manufacturer', 'Model Name', 'Category', 'Screen Size', 'Screen',
       'CPU', 'RAM', ' Storage', 'GPU', 'Operating System',
       'Operating System Version', 'Weight', 'Price (Euros)'],
      dtype='object')

- No solo podemos usar el atributo para ver las etiquetas de las columnas, sino que también podemos asignar nuevas etiquetas al atributo:

* laptops_test = laptops.copy()
* laptops_test.columns = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M']
* print(laptops_test.columns)       Index(['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M'], dtype='object')

- EJERCICIO: A continuación, usemos el DataFrame.columnsatributo para eliminar los espacios en blanco de los nombres de columna.

1. Elimine cualquier espacio en blanco del inicio y final de cada nombre de columna.
* Cree una lista vacía nombrada new_columns.
* Use un bucle for para iterar a través del nombre de cada columna usando el DataFrame.columnsatributo. Dentro del cuerpo del bucle for: * Utilice el str.strip()método para eliminar los espacios en blanco desde el principio y el final de la cadena. * Agregue el nombre de columna actualizado a la new_columnslista.
* Asigne los nombres de columna actualizados al DataFrame.columnsatributo.

* new_columns= []
* for c in laptops.columns:
*     clean_c = c.strip()
*     new_columns.append(clean_c)
    
laptops.columns = new_columns

# Nombres de columnas de limpieza continuados

- En el último ejercicio, eliminamos espacios en blanco de los nombres de las columnas. A continuación se muestra el resultado:

* Index(['Manufacturer', 'Model Name', 'Category', 'Screen Size', 'Screen', 'CPU', 'RAM', 'Storage', 'GPU', 'Operating System', 'Operating System Version', 'Weight', 'Price (Euros)'], dtype='object')

- Sin embargo, las etiquetas de las columnas todavía tienen una variedad de letras mayúsculas y minúsculas, así como paréntesis, lo que hará que sea más difícil trabajar con ellas y leerlas. Terminemos de limpiar nuestras etiquetas de columna por:

* Reemplazo de espacios con guiones bajos.
* Eliminando caracteres especiales.
* Haciendo todas las etiquetas en minúsculas.
* Acortar cualquier nombre de columna larga.

- Podemos crear una función que use métodos de cadena de Python para limpiar nuestras etiquetas de columna, y luego usar nuevamente un bucle para aplicar esa función a cada etiqueta. Veamos un ejemplo:

* def clean_col(col):
*     col = col.strip()
*     col = col.replace("(","")
*     col = col.replace(")","")
*     col = col.lower()
*     return col

* new_columns = []
* for c in laptops.columns:
*     clean_c = clean_col(c)
*     new_columns.append(clean_c)

* laptops.columns = new_columns
* print(laptops.columns)

* Index(['manufacturer', 'model name', 'category', 'screen size', 'screen', 'cpu', 'ram', 'storage', 'gpu', 'operating system', 'operating system version', 'weight', 'price euros'], dtype='object')

1. Definió una función que:
* Se utilizó el str.strip()método para eliminar los espacios en blanco desde el principio y el final de la cadena.
* Utiliza el str.replace()método para eliminar paréntesis de la cadena.
* Utiliza el str.lower()método para hacer la cadena en minúscula.
* Devuelve la cadena modificada.
2. Usó un bucle para aplicar la función a cada elemento en el objeto de índice y asignarlo de nuevo al DataFrame.columnsatributo.
3. Impreso los nuevos valores para el DataFrame.columnsatributo.


- EJERCICIO: Usemos esta técnica para limpiar las etiquetas de las columnas en nuestro marco de datos, agregando algunas tareas adicionales de limpieza en el camino.

1. Defina una función que acepte un argumento de cadena y:
* Elimina cualquier espacio en blanco del inicio y final de la cadena.
* Reemplaza la subcadena Operating Systemcon la abreviatura os.
* Reemplaza todos los espacios con guiones bajos.
* Elimina paréntesis de la cadena.
* Hace que toda la cadena sea minúscula.
* Devuelve la cadena modificada.
2. Use un bucle para aplicar la función a cada elemento en el DataFrame.columnsatributo para el laptopsmarco de datos. Asigne el resultado de nuevo al DataFrame.columnsatributo.


* import pandas as pd
* laptops = pd.read_csv('laptops.csv', encoding='Latin-1')

* def clean_col(col):
*     col = col.strip()
*     col = col.replace("Operating System","os")
*     col = col.replace(" ","_")
*     col = col.replace("(","")
*     col = col.replace(")","")
*     col = col.lower()
*     return col

* new_columns = []
* for c in laptops.columns:
*     clean_c = clean_col(c)
*     new_columns.append(clean_c)
    
* laptops.columns = new_columns

# Convertir columnas de cadena a numérico

Anteriormente observamos que las 13 columnas tienen el tipo objectd type, lo que significa que están almacenadas como cadenas. Veamos las primeras filas de algunas de nuestras columnas:

* print(laptops.iloc[:5,2:5])

* _   category screen_size                              screen
* 0  Ultrabook       13.3"  IPS Panel Retina Display 2560x1600
* 1  Ultrabook       13.3"                            1440x900
* 2   Notebook       15.6"                   Full HD 1920x1080
* 3  Ultrabook       15.4"  IPS Panel Retina Display 2880x1800
* 4  Ultrabook       13.3"  IPS Panel Retina Display 2560x1600

- De estas tres columnas, tenemos tres tipos diferentes de datos de texto:

* category: Datos puramente de texto - no hay valores numéricos.
* screen_size: Datos numéricos almacenados como datos de texto debido al "carácter.
* screen: Una combinación de datos de texto puro con datos numéricos.

- Debido a que los valores en la screen_sizecolumna se almacenan como datos de texto, no podemos clasificarlos numéricamente. Por ejemplo, si quisiéramos seleccionar computadoras portátiles con pantallas de 15 "o más, no podríamos hacerlo.

- Vamos a convertir la screen_sizecolumna a numérica a continuación. Siempre que convertimos texto a datos numéricos, podemos seguir este flujo de trabajo de limpieza de datos:

* 1. Exploracion de datos en la columna   =>  2. Identificar patrones y casos especiales  =>  3. Remover los caracteles que no sean
* digitos  =>   4. Convertir la columna a dtype numerica =>  5. Renombrar la columna si se requiere

- 1. PASO: El primer paso es explorar los datos . Una de las mejores maneras de hacer esto es usar el Series.unique()método para ver todos los valores únicos en la columna:

* print(laptops["screen_size"].dtype)
* print(laptops["screen_size"].unique())

object ['13.3"', '15.6"', '15.4"', '14.0"', '12.0"', '11.6"', '17.3"', '10.1"', '13.5"', '12.5"', '13.0"', '18.4"', '13.9"', '12.3"', '17.0"', '15.0"', '14.1"', '11.3"']

2. PASO: Nuestro siguiente paso es identificar patrones y casos especiales . Podemos observar lo siguiente:

* Todos los valores en esta columna siguen el mismo patrón: una serie de dígitos y caracteres de período, seguidos de un carácter de comillas ( ").
* No hay casos especiales. Cada valor coincide con el mismo patrón.
* Tendremos que convertir la columna a un floatdtype, ya que el intdtype no podrá almacenar los valores decimales.

- Identifiquemos los patrones y casos especiales en la ram columna siguiente.

- EJERCICIO: 1. Utilice el Series.unique()método para identificar los valores únicos en la ramcolumna del laptopsmarco de datos. Asigna el resultado a unique_ram.  2. Después de ejecutar su código, use el inspector de variables para ver los valores únicos en la ramcolumna e identificar cualquier patrón.

* unique_ram = laptops["ram"].unique()

# 3. PASO: Eliminar caracteres que no sean dígitos

- En el último ejercicio, identificamos un patrón claro en la ramcolumna: todos los valores son enteros e incluyen el carácter GBal final de la cadena:

* ['8GB' '16GB' '4GB' '2GB' '12GB' '6GB' '32GB' '24GB' '64GB']

- Para convertir las columnas ramy las screen_sizecolumnas a tipos numéricos, primero tendremos que eliminar los caracteres que no sean dígitos .

* 1. Exploracion de datos en la columna   =>  2. Identificar patrones y casos especiales  =>  3. Remover los caracteles que no sean
* digitos  =>   4. Convertir la columna a dtype numerica =>  5. Renombrar la columna si se requiere

- La biblioteca de pandas contiene docenas de métodos de cadenas vectorizadas que podemos usar para manipular datos de texto, muchos de los cuales realizan las mismas operaciones que los métodos de cadenas de Python. La mayoría de los métodos de cadena vectorizada están disponibles usando el elemento de Series.stracceso , lo que significa que podemos acceder a ellos agregando strentre el nombre de la serie y el nombre del método:    Series.str.method_name()

- En este caso, podemos usar el Series.str.replace()método , que es una versión vectorizada del str.replace()método Python que usamos en la pantalla anterior, para eliminar todos los caracteres de comillas de cada cadena en la screen_sizecolumna:

* laptops["screen_size"] = laptops["screen_size"].str.replace('"','')
* print(laptops["screen_size"].unique())

* ['13.3', '15.6', '15.4', '14.0', '12.0', '11.6', '17.3', '10.1', '13.5', '12.5', '13.0', '18.4', '13.9', '12.3', '17.0', '15.0', '14.1', '11.3']

- Vamos a eliminar los caracteres que no son dígitos de la ramcolumna siguiente.

- EJERCICIO: 1. Utilice el Series.str.replace()método para eliminar la subcadena GBde la ramcolumna.
2. Utilice el Series.unique()método para asignar los valores únicos en la ramcolumna a unique_ram.
3. Después de ejecutar su código, use el inspector de variables para verificar sus cambios.

* laptops["ram"] = laptops["ram"].str.replace('GB','')
* unique_ram = laptops["ram"].unique()

# 4. PASO: Convertir columnas a tipos numéricos

- En la última pantalla, usamos el Series.str.replace()método para eliminar los caracteres sin dígitos de las columnas screen_sizey ram. Ahora, podemos convertir (o convertir) las columnas a un dtype numérico .

* 1. Exploracion de datos en la columna   =>  2. Identificar patrones y casos especiales  =>  3. Remover los caracteles que no sean
* digitos  =>   4. Convertir la columna a dtype numerica =>  5. Renombrar la columna si se requiere

- Para ello, utilizamos el Series.astype()método . Para convertir la columna a un dtype numérico, podemos usar into floatcomo parámetro para el método. Como el intdtype no puede almacenar valores decimales, convertiremos la screen_sizecolumna al floatdtype:

* laptops["screen_size"] = laptops["screen_size"].astype(float)
* print(laptops["screen_size"].dtype)
* print(laptops["screen_size"].unique())


* float64
* [13.3, 15.6, 15.4, 14. , 12. , 11.6, 17.3, 10.1, 13.5, 12.5, 13. , 18.4, 13.9, 12.3, 17. , 15. , 14.1, 11.3]

- Nuestra screen_sizecolumna es ahora el float64dtype. Vamos a convertir el dtype de la ramcolumna a numérico a continuación.

- EJERCICIO: 1. Usa el Series.astype() método para cambiar la ramcolumna a un integerdtype.
2. Utilice el DataFrame.dtypes atributo para obtener una lista de los nombres y tipos de laptops columna del marco de datos. Asigna el resultado a dtypes.
3. Después de ejecutar su código, use el inspector de variables para ver la dtypes variable y ver los resultados de su código.

* laptops["ram"] = laptops["ram"].str.replace('GB','')
* laptops["ram"] = laptops["ram"].astype(int)
* dtypes = laptops.dtypes

# 5. Renombrando columnas

- Ahora que hemos convertido nuestras columnas a dtypes numéricos, el paso final es cambiar el nombre de la columna . Este es un paso opcional y puede ser útil si los valores sin dígitos contienen información que nos ayuda a entender los datos.

* 1. Exploracion de datos en la columna   =>  2. Identificar patrones y casos especiales  =>  3. Remover los caracteles que no sean
* digitos  =>   4. Convertir la columna a dtype numerica =>  5. Renombrar la columna si se requiere

- En nuestro caso, los caracteres de la cita que eliminamos de la screen_sizecolumna indicaban que el tamaño de la pantalla era en pulgadas. Como recordatorio, aquí están los valores originales:

* ['13.3"', '15.6"', '15.4"', '14.0"', '12.0"', '11.6"', '17.3"', '10.1"', '13.5"', '12.5"', '13.0"', '18.4"', '13.9"', '12.3"', '17.0"', '15.0"', '14.1"', '11.3"']

- Para evitar que perdamos información y nos ayuda a comprender los datos, podemos usar el DataFrame.rename()método para cambiar el nombre de la columna de screen_sizea screen_size_inches.

- A continuación, especificamos el axis=1parámetro para que pandas sepa que queremos cambiar el nombre de las etiquetas en el eje de la columna:

* laptops.rename({"screen_size": "screen_size_inches"}, axis=1, inplace=True)
* print(laptops.dtypes)

* manufacturer           object
* model_name             object
* category               object
* screen_size_inches    float64
* screen                 object
* cpu                    object
* ram                    object
* storage                object
* gpu                    object
* os                     object
* os_version             object
* weight                 object
* price_euros            object
* dtype: object

- Tenga en cuenta que podemos usar inplace=Trueo asignar el resultado a la estructura de datos; ambos nos darán los mismos resultados. Vamos a cambiar el nombre de la ramcolumna siguiente y analizar los resultados.

- EJERCICIO: 1. Debido a que los GBcaracteres contenían información útil sobre las unidades (gigabytes) del ram del portátil, use el DataFrame.rename()método para cambiar el nombre de la columna de rama ram_gb.
2. Utilice el Series.describe()método para devolver una serie de estadísticas descriptivas para la ram_gbcolumna. Asigna el resultado a ram_gb_desc.
3. Después de ejecutar su código, use el inspector de variables para ver los resultados de su código.

* laptops["ram"] = laptops["ram"].str.replace('GB','').astype(int)
* laptops.rename({"ram": "ram_gb"}, axis=1, inplace=True)
* ram_gb_desc = laptops["ram_gb"].describe()

# Extraer valores de cuerdas

- A veces, puede ser útil extraer valores no numéricos desde dentro de las cadenas. Veamos los primeros cinco valores de la gpucolumna (unidad de procesamiento de gráficos):

* print(laptops["gpu"].head())
* 0    Intel Iris Plus Graphics 640
* 1          Intel HD Graphics 6000
* 2           Intel HD Graphics 620
* 3              AMD Radeon Pro 455
* 4    Intel Iris Plus Graphics 650
* Name: gpu, dtype: object

- La información en esta columna parece ser un fabricante (Intel, AMD) seguido de un nombre / número de modelo. Extraigamos al fabricante solo para que podamos encontrar los más comunes.

- Debido a que a cada fabricante le sigue un carácter de espacio en blanco, podemos usar el Series.str.split()método para extraer esta información:

* (laptops["gpu"].head().str.split())
* 0 [Intel, Iris, Plus, Graphics, 640]
* 1 [Intel, HD, Graphics, 6000]
* 2 [Intel, HD, Graphics,  620]
* 3 [AMD, Radeon, Pro, 455]
* 4 [Intel, Iris, Plus, Graphics, 650]

- Este método divide cada cadena en el espacio en blanco; el resultado es una serie que contiene listas individuales de Python. También tenga en cuenta que utilizamos paréntesis para encadenar el método en varias líneas, lo que hace que nuestro código sea más fácil de leer.

- Al igual que con las listas y ndarrays, podemos usar la notación de corchetes para acceder a los elementos en cada lista de la serie. Sin embargo, con la serie, utilizamos el elemento de stracceso seguido de [](paréntesis):

* print(laptops["gpu"].head().str.split().str[0])

- Arriba, solíamos 0 seleccionar el primer elemento en cada lista. A continuación se muestra el resultado:

* 0    Intel
* 1    Intel
* 2    Intel
* 3      AMD
* 4    Intel
* Name: gpu, dtype: object

- Usemos esta técnica para extraer también al fabricante de la cpu columna. Aquí están las primeras 5 filas de la cpu columna:

* print(laptops["cpu"].head())

* 0          Intel Core i5 2.3GHz
* 1          Intel Core i5 1.8GHz
* 2    Intel Core i5 7200U 2.5GHz
* 3          Intel Core i7 2.7GHz
* 4          Intel Core i5 3.1GHz
* Name: cpu, dtype: object

- EJERCICIO: En el código de ejemplo, extrajimos el nombre del fabricante de la gpucolumna y lo asignamos a una nueva columna gpu_manufacturer.

1. Extraiga el nombre del fabricante de la cpu columna. Asígnelo a una nueva columna cpu_manufacturer.
2. Usa el Series.value_counts() método para encontrar los conteos de cada fabricante en cpu_manufacturer. Asigna el resultado a cpu_manufacturer_counts.

* laptops["gpu_manufacturer"] = (laptops["gpu"].str.split().str[0])
* laptops["cpu_manufacturer"] = (laptops["cpu"].str.split().str[0])
* cpu_manufacturer_counts = laptops["cpu_manufacturer"].value_counts()

# Corrigiendo malos valores

- Si sus datos han sido raspados de una página web o si hubo un ingreso manual de datos involucrado en algún momento, puede terminar con valores inconsistentes. Veamos un ejemplo de nuestra oscolumna:

* print(laptops["os"].value_counts())

* Windows      1125
* No OS          66
* Linux          62
* Chrome OS      27
* macOS          13
* Mac OS          8
* Android         2
* Name: os, dtype: int64

- Podemos ver que hay dos variaciones del sistema operativo de Apple, macOS, en nuestro conjunto de datos: Mac OSy macOS. Una forma en que podemos arreglar esto es con el Series.map()método . El Series.map()método es ideal cuando queremos cambiar varios valores en una columna, pero lo usaremos ahora como una oportunidad para aprender cómo funciona el método.

- La forma más común de usar Series.map()es con un diccionario. Veamos un ejemplo usando una serie de frutas mal escritas:

* print(s)
* 0       pair
* 1     oranje
* 2    bananna
* 3     oranje
* 4     oranje
* 5     oranje
* dtype: object

- Crearemos un diccionario llamado correctionsy pasaremos ese diccionario como un argumento a Series.map():

* corrections = {"pair": "pear","oranje": "orange","bananna": "banana"}
* s = s.map(corrections)
* print(s)

* 0       pear
* 1     orange
* 2     banana
* 3     orange
* 4     orange
* 5     orange
* dtype: object

- Podemos ver que cada una de nuestras correcciones se hizo a través de nuestra serie. Una cosa importante para recordar Series.map()es que si un valor de su serie no existe como clave en su diccionario, convertirá ese valor a NaN. Veamos que pasa cuando ejecutamos el mapa una vez más:

* s = s.map(corrections)
* print(s)
* 0    NaN
* 1    NaN
* 2    NaN
* 3    NaN
* 4    NaN
* 5    NaN
* dtype: object

- Debido a que ninguno de los valores corregidos en nuestra serie existían como claves en nuestro diccionario, ¡todos los valores se convirtieron NaN! Es una ocurrencia muy común, especialmente cuando se trabaja en el cuaderno Jupyter, donde puede volver a ejecutar celdas fácilmente. Vamos a usar Series.map()para limpiar los valores en la oscolumna.

- EJERCICIO: Hemos creado un diccionario para que lo uses con el mapeo. Tenga en cuenta que hemos incluido la ortografía correcta e incorrecta de macOS como claves, de lo contrario terminaremos con valores nulos.

1. Utilice el Series.map()método con el mapping_dictdiccionario para corregir los valores en la oscolumna.

* mapping_dict = {'Android': 'Android','Chrome OS': 'Chrome OS','Linux': 'Linux','Mac OS': 'macOS','No OS': 'No OS','Windows': 'Windows','macOS': 'macOS'}
* laptops["os"] = laptops["os"].map(mapping_dict)

# Bajando valores perdidos

- En misiones anteriores, hemos hablado brevemente acerca de los valores perdidos y cómo tanto NumPy como los pandas los representan como valores nulos. En pandas, los valores nulos se indicarán con NaNo None.

- Recuerde que podemos usar el DataFrame.isnull()método para identificar valores faltantes, que devuelve un marco de datos booleano. Luego podemos usar el DataFrame.sum()método para contar los Truevalores de cada columna:

* print(laptops.isnull().sum())

* manufacturer            0
* model_name              0
* category                0
* screen_size_inches      0
* screen                  0
* cpu                     0
* ram_gb                  0
* storage                 0
* gpu                     0
* os                      0
* os_version            170
* weight_kg               0
* price_euros             0
* cpu_manufacturer        0
* screen_resolution       0
* cpu_speed               0
* dtype: int64

- Ahora está claro que solo tenemos una columna con valores nulos os_version, que tiene 170 valores faltantes.

- Hay algunas opciones para manejar los valores perdidos:

* Elimina las filas que tengan valores perdidos.
* Eliminar cualquier columna que tenga valores perdidos.
* Rellene los valores faltantes con algún otro valor.
* Deje los valores que faltan como están.

- Las primeras dos opciones a menudo se usan para preparar datos para algoritmos de aprendizaje automático, que no pueden usarse con datos que incluyen valores nulos. Podemos usar el DataFrame.dropna() método para eliminar o eliminar filas y columnas con valores nulos.

- El DataFrame.dropna() método acepta un axisparámetro, que indica si queremos soltar a lo largo de la columna o del eje de índice. Veamos un ejemplo:

* print(df)   APARECE NaN EN LA COLUMNA C FILA x,z HAY 4 COLUMNAS A,B,C,D Y 4 FILAS w,x,y,z

- El valor predeterminado para el axisparámetro es 0, por lo que df.dropna()devuelve un resultado idéntico a df.dropna(axis=0):

* print(df.dropna())      ELIMINA LAS DOS FILAS x,z QUEDAN LAS 4 COLUMNAS A,B,C,D Y 2 FILAS w,y

- Las filas con etiquetas xy zcontienen valores nulos, por lo que esas filas se eliminan. Veamos qué sucede cuando usamos axis=1 para especificar el eje de columna:

* print(df.dropna(axis=1)) ELIMINA LA COLUMNA C Y QUEDAN LOS DEMAS DATOS IGUALES

- Solo la columna con etiqueta Ccontiene valores nulos, por lo que, en este caso, solo se elimina una columna. Practiquemos usando DataFrame.dropna()para eliminar filas y columnas. 

- EJERCICIO: 1. Se usa DataFrame.dropna() para eliminar cualquier fila del marco de datos de las computadoras portátiles que tienen valores nulos. Asigna el resultado a laptops_no_null_rows.
2. Se usa DataFrame.dropna() para eliminar columnas del marco de datos de las computadoras portátiles que tienen valores nulos. Asigna el resultado a laptops_no_null_cols.

* laptops_no_null_rows = laptops.dropna(axis=0)
* laptops_no_null_cols = laptops.dropna(axis=1)

# Relleno de valores perdidos

- Si bien eliminar filas o columnas es el enfoque más fácil para lidiar con los valores faltantes, puede que no siempre sea el mejor enfoque. Por ejemplo, eliminar una cantidad desproporcionada de las computadoras portátiles de un fabricante podría cambiar nuestro análisis.

- Debido a esto, es una buena idea explorar los valores faltantes en la os_versioncolumna antes de tomar una decisión. Podemos usar Series.value_counts()para explorar todos los valores en la columna, pero usaremos un parámetro que no hemos visto antes:

* print(laptops["os_version"].value_counts(dropna=False))

* 10      1072
* NaN      170
* 7         45
* X          8
* 10 S       8
* Name: os_version, dtype: int64

- Como establecimos el dropnaparámetro en False, el resultado incluye valores nulos. Podemos ver que la mayoría de los valores en la columna son 10y los valores faltantes son los siguientes más comunes.

- También exploremos la oscolumna, ya que está estrechamente relacionada con la os_versioncolumna. Solo veremos las filas en las que os_versionfalta:

* os_with_null_v = laptops.loc[laptops["os_version"].isnull(),"os"]
* print(os_with_null_v.value_counts())

* No OS        66
* Linux        62
* Chrome OS    27
* macOS        13
* Android       2
* Name: os, dtype: int64

- Inmediatamente, podemos observar algunas cosas:

* El valor más frecuente es "No OS". Es importante tener en cuenta que si no hay un sistema operativo, no debería haber una versión definida en la os_versioncolumna.
* Trece de las computadoras portátiles que vienen con macOS no especifican la versión. Podemos usar nuestro conocimiento de MacOS para confirmar que os_versiondebe ser igual a X.

- En ambos casos, podemos completar los valores faltantes para que nuestros datos sean más correctos. Para el resto de los valores, probablemente sea mejor dejarlos como desaparecidos para que no eliminemos valores importantes.

- Podemos usar la asignación con una comparación booleana para realizar este reemplazo, como a continuación:

* laptops.loc[laptops["os"] == "macOS", "os_version"] = "X"

- Para filas con No OSvalores, reemplacemos el valor faltante en la os_versioncolumna con el valor Version Unknown.

- EJERCICIO: 
1. Utilice una matriz booleana para identificar las filas que tienen el valor No OSpara la oscolumna. Luego, use la asignación para asignar el valor Version Unknowna la os_versioncolumna para esas filas.
2. Usa la siguiente sintaxis para crear la value_counts_aftervariable:

* value_counts_after = laptops.loc[laptops["os_version"].isnull(), "os"].value_counts()

3. Después de ejecutar su código, use el inspector de variables para ver la diferencia entre value_counts_beforey value_counts_after.
RESPUESTA:
* value_counts_before = laptops.loc[laptops["os_version"].isnull(), "os"].value_counts()
* laptops.loc[laptops["os"] == "macOS", "os_version"] = "X"
* laptops.loc[laptops["os"] == "No OS", "os_version"] = "Version Unknown"
* value_counts_after = laptops.loc[laptops["os_version"].isnull(), "os"].value_counts()

# Desafío: limpiar una columna de cadena

- ¡Ahora es el momento de practicar lo que hemos aprendido hasta ahora! En este desafío, limpiaremos la weightcolumna. Veamos una muestra de los datos en esa columna:

* print(laptops["weight"].head())
* 0    1.37kg
* 1    1.34kg
* 2    1.86kg
* 3    1.83kg
* 4    1.37kg
* Name: Weight, dtype: object

- Su desafío es convertir los valores en esta columna a valores numéricos. Como recordatorio, aquí está el flujo de trabajo de limpieza de datos que puede utilizar:

* 1. Exploracion de datos en la columna   =>  2. Identificar patrones y casos especiales  =>  3. Remover los caracteles que no sean
* digitos  =>   4. Convertir la columna a dtype numerica =>  5. Renombrar la columna si se requiere

- Aunque parece que la weight columna podría necesitar sólo los kg caracteres eliminados de la final de cada cadena, hay un caso especial uno de los valores extremos con kgs, por lo que tendrá que eliminar tanto kgy kgs caracteres.

- En el último paso de este desafío, también le pediremos que utilice el DataFrame.to_csv()método para guardar los datos limpios en un archivo CSV. Es una buena idea guardar un archivo CSV cuando finalice la limpieza en caso de que desee realizar un análisis más adelante.

- Podemos usar la siguiente sintaxis para guardar un CSV:

* df.to_csv('filename.csv', index=False)

- De forma predeterminada, los pandas guardarán las etiquetas de índice como una columna en el archivo CSV. Nuestro conjunto de datos tiene etiquetas de enteros que no contienen ningún dato, por lo que no necesitamos guardar el índice.

- No se desanime si este desafío requiere algunos intentos para ser correcto. Trabajar de manera iterativa es una excelente manera de trabajar, y este desafío es más difícil que los ejercicios que ha completado anteriormente. Hemos incluido algunos consejos adicionales, pero le recomendamos que intente sin los consejos primero; ¡Úsalos solo si los necesitas!

- EJERCICIO: 1. Convertir los valores en la weightcolumna a valores numéricos.
2. Renombra la weightcolumna a weight_kg.
3. Utilice el DataFrame.to_csv()método para guardar el marco de datos de las computadoras portátiles en un archivo CSV laptops_cleaned.csv sin etiquetas de índice.

* laptops["weight"] = laptops["weight"].str.replace("kgs","").str.replace("kg","").astype(float)
* laptops.rename({"weight": "weight_kg"}, axis=1, inplace=True)
* laptops.to_csv('laptops_cleaned.csv',index=False)

# 7. Proyecto Guiado: Explorando Datos De Ventas De Automóviles De Ebay

# 1. Introducción
- En este proyecto guiado, trabajaremos con un conjunto de datos de autos usados ​​de eBay Kleinanzeigen , una sección de clasificados del sitio web alemán de eBay.

- El conjunto de datos se raspó originalmente y se cargó en Kaggle . Hemos hecho algunas modificaciones del conjunto de datos original que se cargó a Kaggle:  https://www.kaggle.com/orgesleka/used-cars-database/data

* Tomamos muestras de 50,000 puntos de datos del conjunto de datos completo, para garantizar que su código se ejecute rápidamente en nuestro entorno hospedado

* Hemos ensuciado un poco el conjunto de datos para asemejarnos más a lo que cabría esperar de un conjunto de datos raspado (la versión cargada en Kaggle se limpió para que sea más fácil trabajar con ella)

- El diccionario de datos provisto con datos es el siguiente:

* dateCrawled- Cuando este anuncio fue rastreado por primera vez. Todos los valores de campo se toman de esta fecha.
* name - Nombre del coche.
* seller - Si el vendedor es privado o un distribuidor.
* offerType - El tipo de listado.
* price - El precio en el anuncio para vender el coche.
* abtest - Si el listado está incluido en una prueba A / B.
* vehicleType - El tipo de vehículo.
* yearOfRegistration - El año en que se registró por primera vez el automóvil.
* gearbox - El tipo de transmisión.
* powerPS - La potencia del coche en PS.
* model - El nombre del modelo de coche.
* kilometer - Cuántos kilómetros ha conducido el coche.
* monthOfRegistration - El mes en que se registró por primera vez el automóvil.
* fuelType - Qué tipo de combustible utiliza el automóvil.
* brand - La marca del coche.
* notRepairedDamage - Si el coche tiene un daño que aún no se ha reparado.
* dateCreated - La fecha en que se creó el anuncio de eBay.
* nrOfPictures - El número de imágenes en el anuncio.
* postalCode - El código postal de la ubicación del vehículo.
* lastSeenOnline - Cuando el rastreador vio este último anuncio en línea.

- El objetivo de este proyecto es limpiar los datos y analizar los listados de autos usados incluidos. También se familiarizará con algunos de los beneficios exclusivos que el cuaderno jupyter ofrece para los pandas.

- Comencemos importando las bibliotecas que necesitamos y leyendo el conjunto de datos en pandas.

- EJERCICIO:

1. Comience escribiendo un párrafo en una celda de rebaja que presenta el proyecto y el conjunto de datos.
2. Importar las librerías pandas y numpy.
3. Lea el autos.csvarchivo CSV en pandas y asígnele el nombre de la variable autos.
* Inténtalo sin especificar ninguna codificación (que por defecto lo hará UTF-8)
* Si recibe un error de codificación, intente las dos codificaciones más populares ( Latin-1y Windows-1252) siguientes hasta que pueda leer el archivo sin errores.
4. Cree una nueva celda con solo la variable autosy ejecute esta celda.
* Una característica interesante del cuaderno jupyter es su capacidad para representar los primeros y los últimos valores de cualquier objeto pandas.
5. Utilice los métodos DataFrame.info()y DataFrame.head()para imprimir información sobre el autosmarco de datos, así como las primeras filas.
* Escribe una celda de rebaja que describa brevemente tus observaciones.

# 2. Nombres de columnas de limpieza

- Del trabajo que hicimos en la última pantalla, podemos hacer las siguientes observaciones:

* El conjunto de datos contiene 20 columnas, la mayoría de las cuales son cadenas.
* Algunas columnas tienen valores nulos, pero ninguna tiene más de ~ 20% de valores nulos.
* Los nombres de las columnas usan camelcase en lugar del snakecase preferido de Python , lo que significa que no podemos simplemente reemplazar espacios con guiones bajos.

* Camel case (estilizado como camelCase ; también conocido como gorras de camello o más formalmente como mayúsculas mediales ) es la práctica de escribir frases tales que cada palabra o abreviatura en el centro de la frase comience con una letra mayúscula , sin espacios intermedios ni puntuación. Ejemplos comunes incluyen " iPhone " y " eBay ". A veces también se usa en nombres de usuario en línea como "johnSmith", y para hacer más legibles los nombres de dominio de varias palabras , por ejemplo, en los anuncios.

* El caso de la serpiente (o snake_case ) es la práctica de escribir palabras compuestas o frases en las que los elementos están separados con un carácter de subrayado (_) y sin espacios, con la letra inicial de cada elemento generalmente en minúscula dentro del compuesto y la primera letra ya sea superior o en minúsculas, como en "foo_bar" y "Hello_world". Se usa comúnmente en el código de computadora para nombres de variables , nombres de funciones y, a veces, nombres de archivos de computadoras . [1]

* Al menos un estudio encontró que los lectores pueden reconocer los valores de los casos de serpientes más rápidamente que camelCase 

- Convirtamos los nombres de columna de camelcase a snakecase y reformulemos algunos de los nombres de columna basados ​​en el diccionario de datos para que sean más descriptivos.

- EJERCICIO: 

1. Utilice el DataFrame.columnsatributo para imprimir una matriz de los nombres de columna existentes.
2. Copie esa matriz y realice las siguientes ediciones en los nombres de las columnas:
* yearOfRegistration a registration_year
* monthOfRegistration a registration_month
* notRepairedDamage a unrepaired_damage
* dateCreated a ad_created
* El resto de los nombres de columna de camelcase a snakecase.
2. Asigne los nombres de columna modificados de nuevo al DataFrame.columnsatributo.
3. Se usa DataFrame.head()para ver el estado actual del autosmarco de datos.
4. Escribe una celda de rebaja que explique los cambios que hiciste y por qué.

# 3. Exploración inicial y limpieza.

- Ahora, hagamos una exploración básica de los datos para determinar qué otras tareas de limpieza deben realizarse. Inicialmente buscaremos: - Columnas de texto donde todos o casi todos los valores sean iguales. Estos a menudo se pueden eliminar ya que no tienen información útil para el análisis. - Ejemplos de datos numéricos almacenados como texto que se pueden limpiar y convertir.

- Los siguientes métodos son útiles para explorar los datos: - DataFrame.describe()(con include='all'para obtener columnas numéricas y categóricas) - Series.value_counts()y Series.head()si alguna columna necesita una mirada más cercana.

- EJERCICIO:
1. Se usa DataFrame.describe()para ver las estadísticas descriptivas de todas las columnas.
2. Escribe una celda de rebaja anotando:
* Cualquier columna que tenga en su mayoría un valor que sean candidatos a ser eliminados
* Cualquier columna que necesite más investigación.
* Cualquier ejemplo de datos numéricos almacenados como texto que necesita ser limpiado.
3. Si necesitas investigar más columnas, hazlo y escribe cualquier cosa adicional que hayas encontrado.
4. Probablemente haya encontrado que las columnas pricey odometerson valores numéricos almacenados como texto. Para cada columna:
* Elimina cualquier carácter no numérico.
* Convertir la columna a un dtype numérico.
* Utilice DataFrame.rename()para cambiar el nombre de la columna a odometer_km.


# 4. Explorando el odómetro y las columnas de precios = Exploring the Odometer=Cuenta kilometros and Price Columns

1. Desde la última pantalla, aprendimos que hay una serie de columnas de texto donde casi todos los valores son iguales ( sellery offer_type). También convertimos las columnas pricey odometeren tipos numéricos y cambiamos el nombre odometera odometer_km.

2. Continuemos explorando los datos, específicamente buscando datos que no se vean bien. Comenzaremos analizando las columnas odometer_kmy price. Aquí están los pasos que tomaremos:

- Analice las columnas utilizando valores mínimos y máximos y busque cualquier valor que parezca demasiado alto o bajo (valores atípicos) que podríamos eliminar.
- Vamos a utilizar:
* Series.unique().shape para ver cuántos valores únicos
* Series.describe() para ver min / max / mediana / media etc.
* Series.value_counts(), con algunas variaciones: * Encadenado a .head()si hay muchos valores. * Debido a que Series.value_counts() devuelve una serie, podemos usar Series.sort_index() con ascending= Trueo False para ver los valores más altos y más bajos con sus recuentos (también se pueden encadenar head()aquí).

* Al eliminar los valores atípicos, podemos hacerlo df[(df["col"] > x ) & (df["col"] < y )], pero es más fácil de usardf[df["col"].between(x,y)]

- EJERCICIO: 1. Para cada una de las columnas odometer_kmy price:
* Usa las técnicas anteriores para explorar los datos
* Si descubre que hay valores atípicos, elimínelos y escriba un párrafo de rebaja que explique su decisión.
* Después de haber eliminado los valores atípicos, haga algunas observaciones sobre los valores restantes.

# 5. Explorando las columnas de fecha

- Ahora pasemos a las columnas de fecha y entendamos el rango de fechas que cubren los datos.

- Hay 5 columnas que deben representar valores de fecha. Algunas de estas columnas fueron creadas por el rastreador, otras provenían del propio sitio web. Podemos diferenciarnos por referencia al diccionario de datos:

* - `date_crawled`: added by the crawler
* - `last_seen`: added by the crawler
* - `ad_created`: from the website
* - `registration_month`: from the website
* - `registration_year`: from the website

- En este momento, las date_crawled, last_seeny ad_createdlas columnas están identificadas como valores de cadena de pandas. Debido a que estas tres columnas se representan como cadenas, debemos convertir los datos en una representación numérica para que podamos entenderlos cuantitativamente. Las otras dos columnas se representan como valores numéricos, por lo que podemos usar métodos como Series.describe()para entender la distribución sin ningún procesamiento de datos adicional.

- Primero entendamos cómo se formatean los valores en las tres columnas de cadena. Todas estas columnas representan valores de marca de tiempo completos, así:

* autos[['date_crawled','ad_created','last_seen']][0:5]

*    date_crawled	            ad_created	          last_seen
* 0	2016-03-26 17:47:46	  2016-03-26 00:00:00	  2016-04-06 06:45:54
* 1	2016-04-04 13:38:56	  2016-04-04 00:00:00	  2016-04-06 14:45:08
* 2	2016-03-26 18:57:24	  2016-03-26 00:00:00	  2016-04-06 20:15:37
* 3	2016-03-12 16:58:10	  2016-03-12 00:00:00	  2016-03-15 03:16:28
* 4	2016-04-01 14:38:50	  2016-04-01 00:00:00	  2016-04-01 14:38:50

- Notará que los primeros 10 caracteres representan el día (por ejemplo 2016-03-12). Para comprender el rango de fechas, podemos extraer solo los valores de fecha, usarlos Series.value_counts()para generar una distribución y luego ordenarlos por el índice.

- Para seleccionar los primeros 10 caracteres en cada columna, podemos usar Series.str[:10]:

* print(autos['date_crawled'].str[:10])

* 0        2016-03-26
* 1        2016-04-04
* 2        2016-03-26
* 3        2016-03-12
* ...

- EJERCICIO: 
1. Utilizar el flujo de trabajo que acabamos de describir para calcular la distribución de los valores en el date_crawled, ad_createdy last_seencolumnas (todas las columnas de cadena) como porcentajes.
* Para incluir valores faltantes en la distribución y usar porcentajes en lugar de conteos, encadene el Series.value_counts(normalize=True, dropna=False) método.
* Para clasificar por fecha en orden ascendente (desde el más reciente al más reciente), encadene el Series.sort_index() método.
* Escriba una celda de reducción después de cada exploración de columna para explicar sus observaciones.
2. Utilizar Series.describe() para entender la distribución de registration_year.
* Escribe una celda de rebaja explicando tus observaciones.

# 6. Tratar con los datos del año de registro incorrecto

- Una cosa que se destaca de la exploración que hicimos en la última pantalla es que la registration_yearcolumna contiene algunos valores impares:

* El valor mínimo es 1000, antes de que se inventaran los coches.
* El valor máximo es 9999, muchos años en el futuro.

- Debido a que no se puede registrar un automóvil por primera vez después de ver el listado, cualquier vehículo con un año de registro superior a 2016 es definitivamente inexacto. Determinar el primer año válido es más difícil. De manera realista, podría estar en algún lugar en las primeras décadas de la década de 1900.

- Contemos el número de listados con autos que se encuentran fuera del intervalo 1900 - 2016 y veamos si es seguro eliminar esas filas por completo, o si necesitamos más lógica personalizada.

- EJERCICIO:

1. Decida cuáles son los valores más altos y más bajos aceptables para la registration_yearcolumna.
* Escribe una celda de rebaja explicando tu decisión y por qué.
2. Elimine los valores fuera de los límites superior e inferior y calcule la distribución de los valores restantes utilizando Series.value_counts(normalize=True).
* Escribe una celda de rebaja explicando tus observaciones.

# 7.  Explorando el precio por marca

- Una de las técnicas de análisis que aprendimos en este curso es la agregación. Cuando se trabaja con datos sobre automóviles, es natural explorar las variaciones en diferentes marcas de automóviles. Podemos utilizar la agregación para entender la brandcolumna.

- Si recuerdas en una misión anterior, exploramos cómo usar los bucles para realizar la agregación. Así es como se ve el proceso:

* - Identify the unique values we want to aggregate by
* - Create an empty dictionary to store our aggregate data
* - Loop over the unique values, and for each:
*     - Subset the dataframe by the unique values
*     - Calculate the mean of whichever column we're interested in
*     - Assign the val/mean to the dict as k/v.

- EJERCICIO: 

1. Explore los valores únicos en la brandcolumna y decida qué marcas desea agregar.
* Es posible que desee seleccionar los 20 primeros, o puede seleccionar aquellos que tienen más de un cierto porcentaje de los valores totales (por ejemplo,> 5%).
* Recuerde que Series.value_counts()produce una serie con etiquetas de índice, por lo que puede usar el Series.index atributo para acceder a las etiquetas, si lo desea.
2. Escriba un breve párrafo que describa los datos de la marca y explique qué marcas ha elegido agregar.
3. Cree un diccionario vacío para mantener sus datos agregados.
* Recorra las marcas seleccionadas y asigne el precio medio al diccionario, con el nombre de la marca como la clave.
* Imprima su diccionario de datos agregados y escriba un párrafo analizando los resultados.

# 8. Almacenamiento de datos agregados en un DataFrame

- En la última pantalla, agregamos todas las marcas para comprender el precio medio. Observamos que en las 6 mejores marcas, hay una diferencia de precios distinta.

* Audi, BMW y Mercedes Benz son más caros
* Ford y Opel son menos costosos
* Volkswagen está en el medio

- Para las 6 marcas principales, usemos la agregación para comprender el kilometraje promedio de esos autos y si hay algún vínculo visible con el precio promedio. Si bien nuestro instinto natural puede ser mostrar ambos objetos agregados de la serie y compararlos visualmente, esto tiene algunas limitaciones:

* es difícil comparar más de dos objetos agregados de la serie si queremos extenderlos a más columnas
* No podemos comparar más de unas pocas filas de cada objeto de la serie.
* solo podemos clasificar por el índice (nombre de la marca) de los dos objetos de la serie para que podamos hacer comparaciones visuales fácilmente

- En su lugar, podemos combinar los datos de ambos objetos de la serie en un único marco de datos (con un índice compartido) y mostrar el marco de datos directamente. Para hacer esto, necesitaremos aprender dos métodos de pandas:

* pandas series constructor          https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html
* pandas dataframe constructor       https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html

- Aquí hay un ejemplo del constructor de series que usa el brand_mean_pricesdiccionario:

* bmp_series = pd.Series(brand_mean_prices)
* print(bmp_series)

* audi             9336
* bmw              8332
* ford             3749
* mercedes_benz    8628
* opel             2975
* volkswagen       5402
* dtype: int64

- Las claves en el diccionario se convirtieron en el índice en el objeto de la serie. Luego podemos crear un marco de datos de una sola columna a partir de este objeto de serie. Necesitamos usar el columnsparámetro al llamar al constructor de marcos de datos (que acepta un objeto similar a una matriz) para especificar el nombre de la columna (o el nombre de la columna se configurará 0de forma predeterminada):

* df = pd.DataFrame(bmp_series, columns=['mean_price'])
* df

*                     mean_price
* bmw	                  8332
* mercedes_benz	        8628
* opel	                2975
* audi                	9336
* volkswagen	          5402
* ford	                3749

- EJERCICIO:
1. Use el método de bucle de la última pantalla para calcular el kilometraje promedio y el precio promedio para cada una de las marcas principales, almacenando los resultados en un diccionario.
2. Convierte ambos diccionarios a objetos de series, usando el constructor de series.
3. Cree un marco de datos a partir del primer objeto de la serie utilizando el constructor de marcos de datos.
4. Asigne las otras series como una nueva columna en este marco de datos.
5. Imprima bastante el marco de datos y escriba un párrafo analizando los datos agregados.

# 9. Próximos pasos

- En este proyecto guiado, practicamos la aplicación de una variedad de métodos de pandas para explorar y comprender un conjunto de datos en los listados de autos. Aquí hay algunos pasos a seguir para que usted considere:

1. Limpieza de datos próximos pasos:
* Identifique datos categóricos que usan palabras en alemán, tradúzcalos y asigne los valores a sus equivalentes en inglés
* Convierta las fechas para que sean datos numéricos uniformes, de modo que se "2016-03-21"convierta en el entero 20160321.
* Vea si hay palabras clave particulares en la columna de nombre que puede extraer como nuevas columnas

2. Análisis de los siguientes pasos:
* Encuentra las combinaciones de marca / modelo más comunes.
* Divida los odometer_kmgrupos y use la agregación para ver si los precios promedio siguen algún patrón basado en el kilometraje.
* ¿Cuánto más baratos son los automóviles con daños que sus homólogos no dañados?
