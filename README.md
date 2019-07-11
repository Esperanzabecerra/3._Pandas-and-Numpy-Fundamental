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








# 3. Introduction to Pandas

# 4. Exploring Data with Pandas: Fundamentals

# 5. Exploring Data with Pandas: Intermediate

# 6. Data Cleaning Basics

# 7. Guiaded Proyect: Exploring Ebay Car Sales Data

