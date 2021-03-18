---
layout: post
title: "Escribe Letras de Canciones (usando Machine Learning)."
subtitle: 'pt. 1 - "Obteniendo los Datos" '
background: '/img/posts/spotify_analysis/header.png'
---

## ¿Qué datos necesitamos para este proyecto?
Necesitamos la mayor cantidad de letras de canciones posibles de un mismo cantante. Estos datos servirán como base para entrenar el modelo de Machine Learning que generará las nuevas letras.

## ¿De dónde los vamos a sacar?
Los datos los vamos a obtener de [**Genius**]("https://docs.genius.com/"). Este sitio web es muy popular porque puedes encontrar casi todo el catálogo de canciones (y letras) de un artista.

## ¿Cómo los vamos a obtener?
Vamos a utilizar el [API de Genius](https://genius.com/api-clients/new) para obtener la lista de canciones. Adicional, usaremos esta misma API para obtener los links desde dónde podemos ver la letra de la canción.

Ya que tengamos esta lista vamos a hacer *web scrapping* utilizando la librería de `html-requests` para obtener las letras.Para todo este proceso utilizaremos `Python` y `Jupyter Notebook`.

**Nota:** Hacer la configuración de los Jupyter Notebooks e importar las librerías de Python puede llegar a ser tardado. Si quieres comenzar rápidamente con este proyecto te recomiendo que uses [**Google Colab**](https://colab.research.google.com/notebooks/intro.ipynb). Esta plataforma de Google te permite usar *notebooks* muy parecidos a los de Jupyter. Colab también te da acceso a cierto número de GPU's. Todo esto sin tener que configurar absolutamente nada.


## Resumen:
* **1.** Crear una cuenta en Genius. Lo puedes hacer desde [**aqui**](https://genius.com/signup_or_login).
* **2.** Una vez que tengas tu cuenta, visita la página de [**Desarrolladores**](https://genius.com/developers) y da clic en [**Create an API Client**](https://genius.com/api-clients/new)
* **3.** La página te solicitará que registres una app. Para el propósito de este proyecto, no vamos a requerir autentificar usuarios, por lo que puedes colocar la información que quieras.
* **4.** Una vez agregada, la página te dará la opción de generar un *Client Access Token*. Genéralo y copialo.
* **5.**Dentro de tu *notebook* Crea una variable que se llame `client_access_token` y pega la información que copiaste como un *string*
![](/img/posts/music_predictor/i_2.png)
* **6.** Importa las librerias `urlencode`, `requests`, `enocde` y `pandas` a tu *notebook*. Las vamos a necesitar para utilizar el API de Genius.
![](/img/posts/music_predictor/i_1.png)
* **7.** Crear una variable que se llame `headers`. En esta variable se guardará un diccionario con un `key` que se llame "Authorization" y un *fstring* que tenga un valor `Bearer {client_access_token}`
![](/img/posts/music_predictor/i_3.png)
* **8.** Con esta API puedes obtener mucha información acerca de tus artistas favoritos. Nosotros estamos interesados en obtener todo su catálogo de canciones. Para hacer esto vamos a utilizar 3 recursos de esta API. `Search`, `Artists` y `Songs`
  - **8.1** `Search`: Para obtener todas las canciones necesitamos un parametro llamado `id`. Cada artista tiene uno distinto. Este parametro lo obtenemos llamando el recurso `Search` y buscando el `key` con el nombre `id`.
  - **8.2** Para facilitar el proceso realice una función llamada `search_artist_id` que acepta un parametro (`search_term`). Este parametro es el nombre del artista del cuál queremos el `id`
![](/img/posts/music_predictor/i_4.png)
  - **8.3** Ya que tenemos el `id` del artista, podemos obtener una lista de todas sus canciones. Para hacerlo usaremos el recurso `Songs`.
  - **8.4** De igual forma, para facilitar el proceso, genere una función llamada `all_artist_songs` que acepta como parametro el `id` del artista. Esta función nos regresa 2 resultados `full_df` que contiene un *dataframe* con todas las canciones en las cuáles el artista principal tiene presencia (como vocalista, colaboración, productor, etc) y `full_df_clean` que contiene un *dataframe* con las canciones que interpreta el artista principalmente.
![](/img/posts/music_predictor/i_5.png)
* **9.** El ultimo paso del proceso es utilizar la libreria `requests_html` para obtener las letras de las canciones. Para esta parte del proceo vamos a hacer *web_scraping*. Es importante mencionar que este proceso puede ser completamente distinto al momento en el que estes viendo este tutorial. Esto ocurre debido a que dependemos enteramente de como esta organizado el *HTML* del sitio web.
![](/img/posts/music_predictor/i_6.png)
  - **9.1** Si quieres saber más acerca de esta librería, puedes hacerlo visitando su sito [**aqui**](https://requests.readthedocs.io/projects/requests-html/en/latest/)
  -  **9.2** Para conocer más acerca de *Web Scrapping* y sus implicaciones, te recomiendo leer este [**artículo**](https://medium.com/@tjwaterman99/web-scraping-is-now-legal-6bf0e5730a78) de Medium.
  - **9.3** Ahora creamos una función llamada `words_from_song(url)` que acepta como parametro una `url` de Genius que contenga la letra de una canción. En resumen esta función abrira la url, buscará la letra de la canción, la copiara y la dividira en oraciones. Estas oraciones serán la data que usaremos para entrenar nuestro modelo de Machine Learning. 
![](/img/posts/music_predictor/i_7.png)
  -  **9.4** Ya que tenemos nuestra data dividida en oraciones, creamos una función llamada `combine_all_sentences()` que acepta como parametro la lista de oraciones que genero la función `words_from_song()`. Esta función nos va a regresar todas las oraciones combinadas en una sola lista.
![](/img/posts/music_predictor/i_8.png)
  -  **9.5** Como ultimo paso vamos a exportar las canciones a formato *txt*. Para hacer esto creamos una función llamada `save_to_txt()` que acepta como parametro las lista de oraciones que genero la función `combine_all_sentences()`. Esta función genera el *txt* y lo guarda dónde le especifiquemos.
![](/img/posts/music_predictor/i_9.png)

¡Listo!, ahora tenemos las letras de todas las canciones en un formato que podrá consumir nuestro modelo de Machine Learning.

Para que conozcas como construir el modelo y entrenarlo, te invito a que visites la parte 2 de este proyecto dando click [aquí](https://germarr.github.io/2021/02/14/building-a-lyric-generator-pt-ii.html).

Visita mi repo en Github, en dónde podrás descargar el código de este proyecto, dando click [aqui](https://github.com/germarr/).
