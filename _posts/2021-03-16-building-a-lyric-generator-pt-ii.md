---
layout: post
title: "Escribe Letras de Canciones (usando Machine Learning)."
subtitle: 'pt. 2 - "Creando el modelo usando Keras y Tensorflow" '
background: '/img/posts/spotify_analysis/header.png'
---

En la parte 1 de este proyecto, hablamos sobre como obtener la data para entrenar nuestro modelo de Machine Learning. Ahora es momento de construirlo y entrenarlo.

Puede llegar a parcer abrumador la cantidad de conceptos de Machine Learning que necesitas saber para comenzar un proyecto, sin embargo no es así. Aunque exiten muchas ramas dentro de Machine Learning, cada una de ellas sirve para propositos esepecificos. Conocerlas todas te puede ayudar a tener un panorama general y un mejor entendimiento del tema, pero no es 100% necesario para comenzar a construir tus ideas.

Compañías como Google, Amazon y Microsoft, tienen un sin fin de recursos para aprender a usar Machine Learning. Incluso, estas mismas compañías han desarrollado librerias que abstraen toda la parte de código y te dejan experimentar modelos de Machine Learning sin necesidad de programar. Al final de este posteo voy a citar 2 de estos proyectos para que los conozcas.  

Mi recomendación es que, para aprender como operan los algoritmos y librerias de Machine Learning, comienzes a usarlas. Eso es lo que pretendo lograr con esta parte del proyecto. Que veas que más que saber programar código en Python, necesitas tener clara la idea de lo que quieres lograr. Cuando tienes la idea puedes buscar todos los recursos para construirla.  

## Proceso
**1.** El primer paso es importar *Tensorflow* y *Keras*. Tensorflow es una libreria creada por Google que permite desarrollar y entrenar modelos de Machine Learning. Keras es un framework que nos permite interactuar con Tensorflow de una manera más sencilla. Si estas usando Colab, ambas librerías vienen ya instaladas dentro del notebook por default. Si estas usando un jupyter notebook [aqui](https://www.tensorflow.org/install) puedes encontrar un tutorial de como descargar las librerías. 

Ya que tengamos las librerias, tenemos que seleccionar algunas funciones especificas de Keras. Estas funciones las usaremos para hacer *tokens*, entrenar el modelo y calificarlo. Puedes ver todas las funciones en la siguiente imagen:
![](/img/posts/music_predictor/ii_1.png)

**2.** Ya que tenemos nuestras funciones el primer paso es transformar nuestros datos en *tokens*. Esto significa que a cada palabra dentro de nuestras canciones les asignaremos un número. Esto nos ayuda a estandarizar los datos en un formato común y acomodarlos en una matriz.

Para hacer los tokens utilizamos la función `fit_on_texts()` que se encuentra dentro de la clase `Tokenizer()` de Keras. Como parametro le pasamos nuestro archivo *.txt* con todas las canciones. 
![](/img/posts/music_predictor/ii_4.png)
En este ejemplo en particular utilice 100 canciones de Alejandro Fernandez, un conocido cantante mexicano. Así se ve el objeto que genera la función:

![](/img/posts/music_predictor/ii_3.png)
Podemos observar que a cada palabra se le asocio un número.

**3.** Ya que tenemos nuestros tokens, podemos proceder a convertirlos en matrices. Estas matrices van a contener sequencias de palabras. Estas secuencias, a su vez, son las que vamos a usar para calculcar la probabilidad de que cierta palabra aparezca en un determinado momento.

Este concepto es una de las bases principales del algoritmo que usaremos y puedes leer más de el dando clic [aqui](https://en.wikipedia.org/wiki/N-gram).

El código para generar estas matrices luce asi:
![](/img/posts/music_predictor/ii_6.png)

**4.** Todas las pieces están listas para comenzar a generar el modelo que generará nuevas canciones. Usaremos la clase `Sequential()` de *Keras* y le agregaremos los parametros `Embedding`, `Bidirectional`, `Dense` y como *optimizer* usaremos la función *Adam*. Una vez que ajustemos nuestros parametros los compilaremos usando la función `compile()` y finalmente usaremos `fit()` para comenzar a entrenar el modelo.

El código luciría asi:
![](/img/posts/music_predictor/ii_8.png)

Mientras el modelo se va entrenando podrás ver el resultado de cada iteración:
![](/img/posts/music_predictor/ii_7.png)

**5.** Una vez que se termine de entrenar el modelo, podemos comenzar a utilizarlo para escribir canciones. Para hacerlo crearemos una función llamada `write_song()` que aceptara 2 parametros, `seed_text` y `next_words`. 
Esta función tomará los parametros y predicirá una canción con base en el texto que le dimos al parametro `seed_text`.

El código de la función se vería asÍ: 
![](/img/posts/music_predictor/ii_9.png)

Finalmente, ¡Hagamos algunas predicciones!. Para este ejemplo entrene el modelo utilizando todas las canciones de J Balvin.
![](/img/posts/music_predictor/ii_10.png)

Como vemos en el ejemplo, con solo agregar una palabra, nuestra función puede predecir las siguientes 20. Quizá algunas oraciones no tienen mucho sentido, pero con más entrenamiento, podríamos obtener mejores resultados.

Listo, ahora que tenemos este modelo vamos a hacer *deployment* del mismo para que usuarios puedan interactuar con el. Da [clic](https://germarr.github.io/2021/02/14/building-a-lyric-generator-pt-iii.html) aquí para ver la tercera parte de este proyecto y conocer el proceso.

Visita mi repo en Github, en dónde podrás descargar el código de este proyecto, dando click [aqui](https://github.com/germarr/).

## Recursos Adicionales
Para lograr esta sección del proyecto, me apoye mucho en los siguientes recursos:
* [**Predict Shakespeare with Cloud TPUs and Keras**](https://colab.research.google.com/github/tensorflow/tpu/blob/master/tools/colab/shakespeare_with_tpu_and_keras.ipynb#scrollTo=edfbxDDh2AEs). El equipo de Tensorflow creó este notebook en el cuál entrenan un modelo que escribe como Shakespear.
* [**Writing Hamilton Lyrics with Tensorflow/R**](https://www.kaggle.com/anasofiauzsoy/writing-hamilton-lyrics-with-tensorflow-r): En este notebook publicado en la plataforma *Kaggle* realizaron predicciones usando como base las letras del musical Hamilton. Aunque el código que utilizan es **R** los conceptos son iguales. También hay un [video](https://www.youtube.com/watch?v=Ny82iVL6vQ8&t=327s) de la chica que lo diseño en donde explica un poco su proceso.
* [**Song Generator**](https://www.mygreatlearning.com/blog/song-generator/): Este artículo me ayudo a conceptualizar de una manera más amplia los parametros que adicionamos a la clase `Sequence()` de Keras. Si te gustaría ajustar más el modelo te recomiendo que lo visites.

Adicional, si quieres probar plataformas de Machine Learning que no requieren mucho código o *set up* te recomiendo visitar los siguientes páginas:
* [**ml5**](https://ml5js.org/)
* [**teachable machines**](https://teachablemachine.withgoogle.com/)
