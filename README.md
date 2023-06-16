# Actividad Integradora 5.3 Resaltador de sintaxis paralelo 
### Sergio Zuckermann
### Santiago Tena
---

## Ejecución del programa
    :timer.tc(fn -> PythonTokenizer.parallel_highlight("python")end)

Primeras ejecuciones:

3 Archivos de diferente tamaño de manera secuencial:

![](/images/secuencial.png)

3 Archivos de diferente tamaño de manera paralela:
![](/images/paralelo.png)

Segundas ejecuciones:

3 Archivos de diferente tamaño de manera secuencial:

![](/images/secuencial2.png)

3 Archivos de diferente tamaño de manera paralela:
![](/images/paralelo2.png)

Cálculo del speed up: $S_p=\frac{T_1}{T_p}$

Donde:

$S_p$ es el speedup obtenido usando _p_ procesadores

$T_1$ es el tiempo que tarda en ejecutarse la versión secuencial del programa.

$T_p$ es el tiempo que tarda en ejecutarse la versión paralela del programa utilizando _p_ procesadores.

Para el tiempo secunecial sumaré los 3 diferentes tiempos de la segunda vez que se ejecutaron. Dando un total de 52,936 $\mu$ _s_ o 0.052936 _s_

$S_p=\frac{0.052936_1}{0.049179_p}=1.07639439598$
--
---

## Complejidad del algoritmo

1. La función `read_write({in_filename, out_filename})` realiza las siguientes operaciones:
   - Lee un archivo de entrada línea por línea (`File.stream!()`).
   - Aplica la función `tokeni/1` a cada línea (`Enum.map(&tokeni/1)`).
   - Une todas las líneas en un solo string (`Enum.join("")`).
   - Escribe el string resultante en un archivo de salida (`File.write!(out_filename, generate_html(data))`).

   La complejidad de esta función depende del tamaño del archivo de entrada. Si consideramos que el número de líneas es `n` y el tamaño promedio de cada línea es `m`, entonces la complejidad sería aproximadamente O(n * m).

2. La función `parallel_highlight(directory)` realiza las siguientes operaciones:
   - Genera una lista de archivos en el directorio especificado (`Path.wildcard()`).
   - Mapea cada archivo a una tupla con el nombre del archivo y el nombre de salida (`Enum.map(&gen_tuple/1)`).
   - Realiza una impresión en consola de la lista de tuplas (`IO.inspect()`).
   - Ejecuta de forma paralela la función `read_write` para cada tupla (`Enum.map(&Task.async(fn -> read_write(&1) end))`).
   - Espera a que todas las tareas paralelas se completen (`Enum.map(&Task.await(&1))`).

   La complejidad de esta función depende del número de archivos en el directorio y del tamaño de cada archivo. Si consideramos que el número de archivos es `k` y el tamaño promedio de cada archivo es `m`, entonces la complejidad sería aproximadamente O(k * m).

3. La función `tokeni(code)` realiza el análisis de cada línea de código y aplica resaltado de sintaxis utilizando expresiones regulares. Para cada línea, se realiza un análisis de patrones con expresiones regulares y se construye una cadena resultante. La complejidad de esta función depende del tamaño de cada línea de código. Si consideramos que el tamaño de la línea es `m`, entonces la complejidad sería aproximadamente O(m).

4. La función `generate_html(tokens)` genera el archivo HTML utilizando una cadena de plantilla. La complejidad de esta función depende del tamaño de la cadena `tokens`. Si consideramos que el tamaño de `tokens` es `m`, entonces la complejidad sería aproximadamente O(m).

La complejidad del algoritmo en general depende del tamaño del archivo de entrada, el número de archivos en el directorio y el tamaño de cada línea de código. Si consideramos el tamaño promedio de línea de código como `m` y el número de líneas o archivos como `n` o `k`, respectivamente, entonces la complejidad total sería aproximadamente O(n * m) para la función `read_write` y O(k * m) para la función `parallel_highlight`. La complejidad total del algoritmo sería aproximadamente O(n + m + k).

## Reflexión

Como podemos observar en las pruebas si existe una diferencia de tiempo al momento de ejecutar los programas. Por un lado El tiempo de ejecución secuencial fue de _0.052936s_ y el tiempo de la ejecución paralela fue de _0.049179s_. Cabe mencionar que el tiempo de ejecución paralela no mide el tiempo que le toma al usuario ejecutar los programas, lo cual suma considerables segundos al tiempo total, por lo que se puede observar una obvia razón para usar la programación paralela sobre la secuencial en casos como los de este ejercicio. 

La programación paralela puede ofrecer beneficios significativos en términos de eficiencia y rendimiento, pero también presenta desafíos éticos que deben abordarse de manera responsable. Al considerar estas implicaciones éticas desde las etapas iniciales de desarrollo, podemos trabajar para aprovechar los beneficios de la programación paralela de manera equitativa y sostenible, promoviendo un impacto positivo en la sociedad.

Algunos de los problemas éticos que podría surgir son los siguientes: acceso equitativo, desigualdad y exclusión, étia laboral, impacto medioambiental, responsabilidad y trasnparencia. Como todas las tendencias nuevas vienen con ellas diferentes implicaciónes éticas. Sin embargo, existen más beneficios de los cuales podría disfrutar la sociedad de estas tecnologías que implicaciones éticas.