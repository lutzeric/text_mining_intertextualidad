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
4. Limpieza y filtrado de los datos:
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

Resultado de ejemplo de un grafo, donde cada nodo está coloreado según a qué edición (2010, 2011, 2012 o 2013) pertenece esa película:
![grafo_edicion](https://user-images.githubusercontent.com/79468958/148290586-811f03ca-6d8e-461f-94a2-e81812177110.png)

## Discusión
Este trabajo no busca ser exhaustivo, sino más bien ser una excusa para poner en práctica conceptos y herramientas vistas en la materia. Varias de las decisiones tomadas, sobre todo aquellas que tienen que ver con qué películas no fueron tenidas en cuenta para el análisis, son completamente arbitrarias y buscaron hacer el resultado final del trabajo más fácil de interpretar. Otra persona que quiera trabajar con este mismo corpus podrá tomar decisiones sustancialmente diferentes a las que yo tomé, y así obtener resultados diferentes. 
Dentro de las decisiones que tomé, dejo algunas que me parece importante destacar y discutir:
1. La eliminación de _stop words_ y palabras de uso frecuente en las sinopsis, ya que no tiene sentido a la hora de hacer los clusters tener palabras como "el", "y", "o", "película" o "directora" que son comunes a todas o a una gran cantidad de las sinopsis y no revelan información que permita identificar a una película en particular. Sí me parece razonable dejar palabras o frases comunes, pero no universales, como "drama", "documental", "blanco y negro", ya que pueden ser características interesantes para diferenciar las películas entre sí. 
2. La elección del número de clusters no siguió ningún criterio matemático, ya que el método del codo no mostró practicamente diferencias entre las cantidades de clusters. Habiendo probado otros números, decidí utilizar seis porque es un número manejable y porque no dejaba ningún cluster con una cantidad muy disparatada con respecto a los demás.
3. Descartar para los grafos películas cuyo título es una palabra o nombre común del español, como "A", "Centro" o "Pablo": me parece razonable dejarlas afuera, ya que termina habiendo conexiones espurias por muchas sinopsis que nombran esas palabras sin hacer relación a esas películas. El problema de esto no es solo quedarse sinalgunas posibles intertextualidades reales, sino que no me parece una decisión fácil de automatizar. Habría en todo caso que hacer una lista con nombres o palabras comunes, que no necesariamente sean *stop words* y filtrar así varias de esas películas. Quizás podría hacerse esto y ver si los resultados cambian significativamente a como están ahora. Como solución rápida, pero poco elegante y muy discutible, decidí filtrar aquellas películas cuyo título es menor a 7 letras.
4. No tomar en cuenta como intertextualidad a aquellas sinopsis que nombran solo el apellido de unx directorx (en vez de su nombre completo) o bien solo a unx directorx de una tupla de directorxs. Esta decisión obedece a dos razones: por un lado, evitar que haya conexiones espurias por apellidos comunes como "Ramirez" o "Smith", que pueden ser compartidos por varixs directorxs, y por otro lado, ahorrar tiempo en algo que no afecta al resultado cualitativo del trabajo. Desde ya, en un trabajo más serio esto se tendría que hacer cuidadosamente.
5. Descartar películas sin conexión intertextual con otras. Esto es porque el objetivo del trabajo es encontrar formas de conectar las películas entre sí, y porque al haber muchas películas sin conexión explícita con otras, es mucho más fácil visualizar el grafo si estas no son tenidas en cuenta.
6. No incorporar el peso a las conexiones en los grafos. Agregar el peso a las aristas según la cantidad de veces que se nombraba a un título o directorx es algo que probé y dejé de lado. Era algo que hacía más difícil la visualización del grafo sin agregarle información valiosa.
7. Dejar películas cuya única intertextualidad es hacia ella misma. Son pocas, pero tranquilamente podrían eliminarse.
8. No hacer análisis cuantitativos de la pureza de cada cluster o de la intertextualidad según el cluster o según la edición de cada película. Esto me parece interesante de hacer, pero por cuestiones de tiempo lo dejo para otra ocasión.
9. Sería interesante tomar algunas muestras al azar y verificar manualmente cuántas de las intertexualidades son correctas para futuros trabajos relacionados. También sería interesante analizar estadísticamente si pertenecer a un mismo cluster da mayores probabilidades de encontrar intertextualidad entre dos películas.

## Recursos consultados
- Scrapeo de páginas web: https://colab.research.google.com/drive/1L5nHiP75k9ZmpA-N2HL8Ud4Grlk7anUv?usp=sharing
- Procesamiento de texto y nubes de palabras: https://colab.research.google.com/drive/1BzNWdDqEkR27jzRSJO1unB-Aqnc52RU4?usp=sharing
- Clustering: https://colab.research.google.com/drive/15C8_ERuZPmNqx7jjXGU-INJ0ES8yFaD1?usp=sharing
- Grafos: https://towardsdatascience.com/structuring-text-with-graph-representations-41dd4f2a3ab3
