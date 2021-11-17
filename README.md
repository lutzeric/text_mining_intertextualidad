# Análisis de sinopsis del Festival Internacional de Cine Independiente de Buenos Aires (BAFICI)

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
    * Eliminación de espacios antes y después de las palabras
5. Vectorización de las palabras.

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
