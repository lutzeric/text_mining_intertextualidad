# Análisis de la intertextualidad de películas del Buenos Aires Festival Internacional de Cine Independiente (BAFICI) a través de sus sinopsis

El BAFICI es un festival de cine que se celebra desde 1999 en la Ciudad de Buenos Aires donde se muestra cine independiente de diversos géneros, países y temáticas. Desde la edición 2008, la programación está disponible para consultar en internet. Esta información consta, para cada película, de una ficha técnica (quiénes trabajaron en la dirección, producción, guion, fotografía, sonido, etc.), junto con el título, duración, año, países donde se produjo, formato y color. Además, casi todas las películas cuentan con una **sinopsis**, una descripción escrita por una persona especializada acerca de la película. La sinopsis no necesariamente es una crítica y no tiene ningún tipo de puntuación, sino que es más bien una descripción de la trama, la dirección u otro aspecto que le haya parecido interesante a la persona que la escribió. Cada película tiene una única sinopsis (o, en raros casos, ninguna) y el estilo de escritura varía bastante entre las personas que las escriben. Por ello, es común ver sinopsis que hablan sobre otras películas, personajes o directorxs, mientras que otras son más cerradas en sí mismas, sin una conexión explícita con otras películas. Las que más me interesan en este trabajo son las primeras, aquellas sinopsis que presentan conexiones explícitas con otras películas. A esta relación se la llama *intertextualidad* y es lo que trataré de visualizar en este trabajo, a través de conexiones en grafos dirigidos. Además, quiero ver si las sinopsis se pueden agrupar en *clusters* cuyas características sean fáciles de determinar sin necesidad de leer cada sinopsis.

En este notebook se muestra el proceso desde la extracción de los datos, el procesamiento, la creación de clusters y la visualización de la intertextualidad mediante grafos.

El corpus elegido son sinopsis de cuatro ediciones del BAFICI (2010-2013), disponibles en las páginas web del Gobierno de la Ciudad: https://festivalesanteriores.buenosaires.gob.ar/bafici/home13/web/es/bafici/pasteditions.html
Trabajo perteneciente a la materia "Text mining" de Laura Alonso Alemany, FaMAF, Universidad Nacional de Córdoba, 2021.

Datos a tener en cuenta: desde el 2010 al 2013 la información es fácil de extraer automáticamente descargando los .csv de la página del Gobierno de la Ciudad, o bien scrapeando cada página de cada película en los sitios de cada edición. Dedidí utilizar este último procedimiento y quedarme con esas cuatro ediciones. La información extraída es el título, directorxs, duración, año, países donde se produjo y la sinopsis.
Cada edición consta de entre 300 y 450 películas, totalizando alrededor de 1300 datos crudos para este corpus.
Algunas sinopsis cuentan con la firma de la persona que la escribió (o la cita de la fuente como puede ser un diario), mientras que otras no. Esta información no fue considerada como parte de la sinopsis. 
Las películas con datos faltantes fueron eliminadas. Aquellas (pocas) con sinopsis de menos de 250 caracteres también, ya que era muy improbable encontrar intertextualidad en ellas.



## Procedimiento general
El trabajo paso por paso, con su código y comentarios está en el notebook intertextualidad_bafici.ipynb https://github.com/lutzeric/text_mining_intertextualidad/blob/main/intertexualidad_bafici.ipynb

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
3. Extracción de los .txt y agrupamiento en una sola base de datos.
4. Limpieza de los datos:
    * Eliminación de las firmas (o citas) de las personas que escribieron las sinopsis, en caso de haber.
    * Eliminación de palabras muy frecuentes y poco informativas: *película*, *personaje*, *director*, etc.
    * Eliminación de *stopwords* del español, leídas de un archivo *spanish.txt*.
    * Eliminación de signos de puntuación, de interrogación, de exclamación, etc.
    * Eliminación de espacios antes y después de las palabras.
    * Eliminación de sinopsis de menos de 250 caracteres.
    * Eliminación de películas con datos faltantes.

### Clustering
1. Vectorización de las palabras con sklearn TfidfVectorizer, incluyendo bi y trigramas y excluyendo palabras con frecuencia de más de 0,8.
2. Prueba con un rango de número de clusters con suma de cuadrados de las distancias de K-means (sklearn Kmeans).
3. Elección del número óptimo mediante el método del codo (visualmente, aunque no se ven claras diferencias entre los números).
4. Análisis cualitativo de la pureza de los clusters en cuanto a directorxs y países mediante gráficos de boxplot.
5. Visualización de las palabras de cada cluster mediante nubes de palabras (biblioteca wordcloud)

### Grafos
1. Generación de nodos a partir de cada título con nombre de más de 6 caracteres, para evitar películas llamadas "A", "Pablo", "Centro", etc.
2. Armado de las relaciones dirigidas: un nodo apunta a otro si su sinopsis nombra el título o directorx/s de este último. Las repeticiones cuentan una sola vez. Se descartaron películas sin intertextualidad con otras.
3. Armado del gráfico, con la opción de colorear cada nodo según su edición o cluster al que pertenece.
