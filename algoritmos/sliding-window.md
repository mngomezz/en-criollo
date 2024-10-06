# Ventana flotante (Sliding Window)

## Resumen

> Aclaracion: A modo de acortar la explicacion de ahora en mas cada vez que se hable de subarrays debe saberse que hablamos de subarrays **consecutivos**.

Este algoritmo (o tactica) es una forma mucho mas eficiente de resolver cualquier problema que trate sobre hallar **el mejor subarray de un array mas grande**. El ejemplo mas claro que siempre se da para practicar esta tactica es cuando tenemos un array de numeros y queremos obtener:
- El subarray de tamaño _k_ cuya suma sea mayor a cualquier otro subarray de tamaño _k_. Para esto aplicaremos la **ventana flotante fija**.
- El subarray mas pequeño cuya suma supere cierto numero _x_. Para esto aplicaremos la **ventana flotante variable**.

Para el primer problema propuesto la forma mas intuitiva de resolverlo es calcular la suma de todos los subarrays de tamaño _k_ posibles del array y luego quedarse con aquel mayor. Sin embargo, esto nos lleva a iterar _k_ veces sobre cada elemento de un array de tamaño _n_.

Para el segundo problema propuesto es aun peor ya que iteraremos para cada elemento del array de tamaño _n_ hasta _n_ veces para llegar a una suma superior a el numero _x_.

La tactica de la ventana flotante sugiere que para cada elemento del array no se itere _k_ veces si no que la suma se realice para el primer elemento del array (el primer subarray) y luego para obtener la suma de los siguientes subarrays solo **se resta el primer elemento y se agrega el siguiente**. De esta manera, con tan solo iterar 1 vez por el array uno ya obtiene aquel subarray mayor.

De esta manera, para hallar el mejor subconjunto de tamaño _k_ usando ventana flotante solo hacemos la suma inicial (primeros _k_ elementos) y luego iteramos _n-k_ veces y solo debemos restar el primer elemento del subarray, es decir, comienzo de la ventana flotante (_i-1_) y luego agregarle el siguiente elemento, es decir, proximo al final de la ventana (_i+k_). Asi, nos ahorramos las cuentas de cada subconjunto y al llegar al ultimo elemento ya obtendremos el mejor subconjunto.

Luego, para hallar el minimo subconjunto mayor a _x_ solo debemos hacer la suma inicial hasta hallar la cantidad de elementos necesarios hasta superar dicho numero. Una vez hallado ese numero vamos acortando la ventana del lado inferior (izquierdo) hasta que deje de ser mayor a _x_. Una vez sucede eso vamos integrando mas numeros del lado superior (derecho) del array y asi hasta llegar al final del conjunto.

## ¿Cuando usarlo?

Cuando se requiera obtener combinaciones consecutivas de un array de elementos y no se quiera iterar por todos los elementos. Las palabras claves para entender cuando usarlo son:

*Array, String, Sub Array, Sub String, Largest Sum, Maximum Sum, Minimum Sum*

Por tanto, vemos que los problemas que resuelve son **cuando queremos obtener la minima o maxima suma de elementos subyacentes en un array**

## Complejidad

**A REVISAR!**
Esto nos permite obtener una mejora de **_O(N)_** donde antes podiamos tener un **_O(N*k)_** 

## Ejemplos

![Ventana flotante paso inicial](../images/algoritmos/sliding-window-1.png)

![Ventana flotante paso intermedio](../images/algoritmos/sliding-window-2.png)

![Ventana flotante paso final](../images/algoritmos/sliding-window-3.png)

## Codigo

### Ventana flotante fija en Python

```python
def ventana_flotante_fija(array: list[int], k: int) -> list[int]:
    """Obtiene la mayor suma de las sublistas de tamaño k del array"""
    # Suma inicial de la subarray de elementos
    suma_actual = sum(array[:k])
    maximo = suma_actual

    # Obtenemos cada suma adicionando el siguiente valor (i+k-1) y removemos el ultimo (i-1)
    for i in range(0, len(array) - k):
        suma_actual = suma_actual - array[i]
        suma_actual = suma_actual + array[i+k]

        if suma_actual > maximo:
            maximo = suma_actual

    return maximo
```

### Ventana flotante dinamica en Python

```python
def ventana_flotante_dinamica(array: list[int], x: int) -> int:
    """Obtiene el tamaño minimo de la ventana cuya suma supere el numero x"""
    ventana_minima = float('inf')

    # Obtenemos el rango y suma de nuestra ventana flotante
    comienzo_de_ventana = 0
    final_de_ventana = 0
    suma_actual = 0

    # Extendemos el final de la ventana flotante hasta que sea mayor a x
    while final_de_ventana < len(array):
        suma_actual = suma_actual + array[final_de_ventana]
        final_de_ventana = final_de_ventana + 1

        # Contraemos el comienzo de la ventana flotante hasta que sea menor a x
        while comienzo_de_ventana < final_de_ventana and suma_actual >= x:
            suma_actual = suma_actual - array[comienzo_de_ventana]
            comienzo_de_ventana = comienzo_de_ventana + 1

            # Actualizamos tamaño minimo de ventana superior a x
            ventana_actual = final_de_ventana - comienzo_de_ventana + 1
            ventana_minima = min(ventana_minima, ventana_actual)

    return ventana_minima
```


## Implementaciones

**TODO: Mencionar como se usa en algoritmo de congestion TCP.**

**TODO: Mencionar como se usa en algoritmo de compresion LZ77.**

