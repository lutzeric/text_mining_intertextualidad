Análisis de sinopsis del Festival Internacional de Cine Independiente de Buenos Aires (BAFICI)

Agrupamiento de palabras según su relación sintáctica y semántica utilizando el argoritmo de k-means.

Trabajo perteneciente a la materia "Text mining" de Laura Alonso Alemany - FaMAF UNC. 2021.
Procedimiento
Procesamiento del corpus
Separación del corpus en oraciones y en palabras.
    Análisis morfosintáctico y funcionalidad de cada palabra.
    Eliminación de oraciones con menos de 10 palabras.
    Lematización de palabras.
    Conteo de ocurrencias totales de cada palabra.
    Creación de diccionario de palabras.
        Agregado de palabras al diccionario, aquellas que no sean números, puntuaciones o desconocidas. Cada palabra contiene un diccionario.
        Agregado al diccionario de cada palabra:
            Palabras de contexto.
            Part-of-speech tag.
            Morfología de tag.
            Funcionalidad.
            Triplas de dependencia.
        Eliminación de stopwords de los diccionarios de las palabras.
        Eliminación de palabras poco frecuentes en el corpus, de los diccionarios de las palabras.
        Eliminación de palabras poco frecuentes como contexto, de los diccionarios de las palabras.
    Vectorización de las palabras.
    Normalización de la matriz (número de ocurrencias totales de la columna sobre ocurrencias por cada fila).

Clustering
