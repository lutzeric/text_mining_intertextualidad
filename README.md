# Análisis de sinopsis del Festival Internacional de Cine Independiente de Buenos Aires (BAFICI)

Agrupamiento de palabras según su relación sintáctica y semántica utilizando el argoritmo de k-means.
Trabajo perteneciente a la materia "Text mining" de Laura Alonso Alemany - FaMAF UNC. 2021.

## Procedimiento (en construcción)
### Procesamiento del corpus
1. Separación del corpus en oraciones y en palabras.
2. Creación de diccionario de palabras.
    * Agregado al diccionario de cada palabra:
        - Palabras de contexto.
        - Part-of-speech tag.
        - Morfología de tag.
        - Funcionalidad.
        - Triplas de dependencia.
    * Eliminación de stopwords de los diccionarios de las palabras.
    * Eliminación de palabras poco frecuentes en el corpus, de los diccionarios de las palabras.
    * Eliminación de palabras poco frecuentes como contexto, de los diccionarios de las palabras.
7. Vectorización de las palabras.

### Clustering
1. Elección de número de clusters.
2. Centroides aleatorios.
3. Algoritmo de k-means usando la distancia coseno para crear los clusters.

## Procedimiento en detalle
Utilizamos la herramienta Scapy para separar en oraciones, en palabras y etiquetar cada palabra con su POS tag, morfología del tag y funcionalidad en la oración.

      nlp = spacy.load('es', vectors=False, entity=False)
      doc = nlp(dataset)
      
## Resultados
k = 50
ante', 'sin', 'de', 'y', 'luego', 'poder', 'según', 'finalmente', 
