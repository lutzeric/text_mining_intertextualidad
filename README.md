# Análisis de la intertextualidad de películas del Buenos Aires Festival Internacional de Cine Independiente (BAFICI) a través de sus sinopsis

El BAFICI es un festival de cine que se celebra desde 1999 en la Ciudad de Buenos Aires donde se muestra cine independiente de diversos géneros, países y temáticas. Desde la edición 2008, la programación está disponible para consultar en internet. Esta información consta, para cada película, de una ficha técnica (quiénes trabajaron en la dirección, producción, guion, fotografía, sonido, etc.), junto con el título, duración, año, países donde se produjo, formato y color. Además, casi todas las películas cuentan con una **sinopsis**, una descripción escrita por una persona especializada acerca de la película. La sinopsis no necesariamente es una crítica y no tiene ningún tipo de puntuación, sino que es más bien una descripción de la trama, la dirección u otro aspecto que le haya parecido interesante a la persona que la escribió. Cada película tiene una única sinopsis (o, en raros casos, ninguna) y el estilo de escritura varía bastante entre las personas que las escriben. Por ello, es común ver sinopsis que hablan sobre otras películas, personajes o directorxs, mientras que otras son más cerradas en sí mismas, sin una conexión explícita con otras películas. Las que más me interesan en este trabajo son las primeras, aquellas sinopsis que presentan conexiones explícitas con otras películas. A esta relación se la llama *intertextualidad* y es lo que trataré de visualizar en este trabajo.

Además, algunas sinopsis cuentan con la firma de la persona que la escribió (o la cita de la fuente como puede ser un diario), mientras que otras no.
Al menos desde el 2010, cada edición consta de entre 300 y 450 películas. También desde ese año, cada película cuenta con una página web propia, donde están su ficha y su sinopsis. 

Este trabajo busca hacer un análisis de las sinopsis en cuanto a cómo se conectan las unas a las otras, cómo se pueden agrupar, y cuánto nos hablan (explícita o implícitamente) de quiénes la dirigieron, los países donde se rodó, el año o su duración.

En este notebook se muestra el proceso desde la extracción de los datos mediante scrapeo de páginas web, hasta la visualización de la conexión entre las sinopsis mediante un grafo.

El objetivo del trabajo es extraer sinopsis de películas para analizar, agrupar y ver cómo se relacionan entre sí. Particularmente, me interesa saber si las sinopsis pueden conectarse entre sí a través de su intertextualidad y si sus agrupamientos nos dicen algo sobre, por ejemplo, las personas que las dirigieron y los países donde se produjeron.
El corpus elegido son sinopsis de cuatro ediciones del BAFICI (2010-2013), disponibles en las páginas web del Gobierno de la Ciudad: https://festivalesanteriores.buenosaires.gob.ar/bafici/home13/web/es/bafici/pasteditions.html
Trabajo perteneciente a la materia "Text mining" de Laura Alonso Alemany, FaMAF, Universidad Nacional de Córdoba. 2021.

## Procedimiento
### Extracción del corpus
1. Extracción automática con bibliotecas request y beautifulSoup para cada edición por separado.
    * Variables de interés de cada película: 
        - título
        - dirección
        - países
        - año
        - duración
        - sinopsis

2. Creación de archivos .txt con la información para cada edición.
3. Extracción de los .txt y agrupamiento en un sola base de datos.
4. Limpieza de los datos:
    * Eliminación de las firmas (o citas) de las personas que escribieron las sinopsis, en caso de haber.
    * Eliminación de palabras muy frecuentes y poco informativas: *película*, *personaje*, *director*, etc.
    * Eliminación de *stopwords* del español, leídas del archivo *spanish.txt*.
    * Eliminación de signos de puntuación, de interrogación, de exclamación, etc.
    * Eliminación de espacios antes y después de las palabras.
    * Eliminación de sinopsis de menos de 250 caracteres.
    * Eliminación de películas con datos faltantes.

### Clustering
1. Vectorización de las palabras con sklearn TfidfVectorizer, incluyendo bi y trigramas y excluyendo palabras con frecuencia de más de 0,8.
2. Prueba con un rango de número de clusters con suma de cuadrados de las distancias de K-means (sklearn Kmeans).
3. Elección del número óptimo mediante el método del codo (visualmente).
4. Análisis cualitativo de la pureza de los clusters en cuanto a directorxs y países mediante gráficos de boxplot.
5. Visualización de los clusters con nubes de palabras (wordcloud)

### Grafos (aún sin comenzar)
