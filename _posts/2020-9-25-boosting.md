---
layout: post
title: Boosting
---

En este articulo voy a dar una introduccion al aprendizaje por ensambles con principal foco en boosting. Partamos de la base de modelos sencillos como regresion lineal o logistica como estandares para la resolucion de problemas de regresion o clasificacion. Algo que nos era dificil con estos modelos era explicar comportamientos no lineales, uno debe hacer una ingeneriera de features para incorporar interacciones no lineales entre las variables. Un metodo de ajuste que funciona muy bien con las no linealidades es el arbol de decision.


## Arboles de decision:

Un arbol de decision es un metodo no parametrico de ajuste, es decir no busca una relacion funcional $ y=f(x)$ entre la variable target y las variables explicativas. Consiste en la particion iterativa del espacio de fases asignando un valor constante a cada region de manera tal de poder predecir el valor de nuevas observaciones que caigan en esta region. Su construccion es bastante intuitiva asi como su interpretacion visual. 



Un ejemplo de clasificacion en dos variables:

![](https://github.com/carabedo/carabedo.github.io/raw/main/assets/img/dt_0.png)


Iterativamente vamos particionando el espacio de fases de manera de elegir las regiones que mejor ajusten a los datos.

![](https://github.com/carabedo/carabedo.github.io/raw/main/assets/img/dt_1.png)



Veamos como se construye con un ejemplo de regresion en una variable, tenemos estos datos con un compartamiento fuertemente no lineal:

![](https://github.com/carabedo/carabedo.github.io/raw/main/assets/img/dt_2.png)

Se puede ver que una regresion lineal performa muy mal, ahora ajustemos estos datos con un arbol de decision de 2 niveles:

![](https://github.com/carabedo/carabedo.github.io/raw/main/assets/img/dt_5.png)

Los puntos rojos representan el ajuste por el arbol de decision, parecen existir 4 rectas horizontales que ajustan los datos, hemo particionado en 4 regiones la variable x. Para cada region se le asigno un valor de la variable y. Esto permitio representar de mejor manera el comportamiento no lineal de los datos. Pero, que signifca un arbol de dos niveles? Por que solo hay cuatro regiones?

Aqui el grafico del arbol de decision propiamente dicho, se ve el proceso en el que fue creado. Desde una raiz se van abriendo ramas y terminan en hojas. Las hojas son las regiones finales, hay un nivel por cada vez que se particionaron regiones.

![](https://github.com/carabedo/carabedo.github.io/raw/main/assets/img/dt_6.png)

En las hojas, entran nuestros datos, son las 4 regiones que vemos en el grafico del ajuste. Tanto como el nivel y la cantidad de hojas son hiperparametros de los modelos de arbol de decision. 

#### Como elegimos y armamos las regiones?

Este es un proceso iterativo, se utiliza alguna metricas para decidir en cada paso una particion binaria en alguna variable. Ademas es jerarquico, en cada iteraccion se hace una eleccion de variable y de threshold que maximice el error cuadratico medio.

¿Cómo construye la computadora un árbol de regresión?
El enfoque ideal sería que la computadora considere todas las particiones posibles del espacio de atributos. Sin embargo esto es computacionalmente inviable, por lo que en su lugar se utiliza un algorítmo voraz (greedy) de división binaria recursiva:

Comenzar en la raíz del árbol.
Para cada atributo, examinar cada punto de corte posible y elegir el atributo y punto de corte de manera que el árbol resultante de hacer la división tenga el menor error cuadrático medio (ECM).
Repetir el proceso para las dos ramas resultantes y nuevamente hacer una sola división (en cada rama) para minimizar el ECM.
Repitir este proceso hasta que se cumpla un criterio de detención.
¿Cómo sabe cuándo parar?

Podríamos definir un criterio de detención, como la profundidad máxima del árbol o el número mínimo de muestras en la hoja.
También podríamos hacer crecer el árbol grande y luego "podarlo" utilizando algún método de poda como "cost complexity pruning"
¿Como decidir que división es la mejor?

Una forma de decidir cual es la mejor división es calcular la ganancia en la reduccion del error cuadrático medio, si se aplica la división candidata.

$$
\Delta = ECM(\text{padre}) - \sum_{j \in \text{hijos}}\frac{N_j}{N}ECM(\text{hijo}_j)
$$

El objetivo es buscar la maxima  $\Delta$, donde  ECM es el Error Cuadrático Medio,  $N_j$  es el número de registros en el nodo hijo j  y  N  es el número de registros en el nodo padre.

#### Por que usar arboles de decision?

Los árboles son muy fáciles de explicar a las personas.
Parecen más cercanos a la forma en la que las personas toman decisiones 
Pueden ser representados gráficamente, pueden ser interpretados por no expertos fácilmente (especialmente si son pequeños)
Pueden manejar fácilmente predictores cualitativos sin necesidad de crear variables dummy.

#### Por que no usar arboles de decision?

En general no tienen el mismo nivel de precisión en la predicción comparados con otros enfoques para regresión y clasificación vistos previamente. 
Además pueden ser poco robustos. Un pequeño cambio en los datos puede generar un gran cambio el árbol final estimado. Sin embargo al agregar muchos árboles de decisión usando métodos como bagging, random forests, y  boosting la performance predictiva de los árboles puede mejorarse sustancialmente. 





## Por que Esambles?

El problema de un modelo con un solo arbol de decision es el overfitting, una solucion a esto es usar un ensamble de arboles. Si ajustamos 50 arboles de decision y luego promediamos sus resultados podremos evitar el overfitting. A esto se lo conoce como bagging, otra manera de realizar estos ensambles es boosting.
Boosting consiste en entrenar varios modelos de manera secuencial, entrenando un nuevo modelo sobre los datos que menor permormance tuvo el modelo anterior. 

## Adaboost


## Gradient boost


### Xgboost

### Lightbm

### Catboost

