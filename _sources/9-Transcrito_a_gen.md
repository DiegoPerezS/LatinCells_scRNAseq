## Generación de mapas de transcrito a gen

### 6: Mapeo de transcrito a gen

El método de cuantificación estándar en scRNA-seq es la cuantificación a nivel de gen, esto quiere decir todas las posibles variables transcripcionales de un gen serán cuantificadas como una unidad. Las técnicas de scRNA-seq basadas en gotas sólo secuencian un extremo de cada transcrito, no teniendo una cobertura total de la molécula, haciendo imposible separar las diferentes isoformas de los transcritos.

Para generar la cuantificación a nivel de gen, se debe generar una tabla de conversión para que el software pueda hacer el análisis, en este caso Alevin. Para hacer esta tabla de conversión es necesaria la anotación del genoma, en este caso, es el archivo GTF que ingresamos al principio del tutorial. Los nombres de los transcritos deben ser iguales a los utilizados en el archivo FASTA del transcriptoma, esto quiere decir que se debe trabajar con una única base de datos, en este caso, ENSEMBL, pero también se podría escoger utilizar dichos archivos de NCBI u otra base de datos de preferencia. La razón de que los nombres deban ser consistentes entre el archivo GTF y el archivo FASTA, es que luego se generará un índice (_index_) del transcriptoma, este índice es utilizado para alinear y cuantificar las lecturas de forma más eficiente.

En caso de utilizar _spike-ins_ (transcritos conocidos en una concentración conocida utilizados para calibrar las cuantificaciones en ensayos de hibridización de RNA), se deben agregar tanto al transcriptoma como al mapa de transcrito a gen.

Utilizar herramienta **“GTF2GeneList”** (1.52.0+galaxy0) con los siguientes ajustes:

1. _Ensembl GTF file:_ Data input 'gtf_input' (gff)
2. _Feature type for which to derive annotation:_ transcript
3. _Field to place first in output table:_ transcript_id\
<sup>Nombre de la columna que corresponda al identificador (ID) de los transcritos</sup>
4. _Suppress header line in output?:_ Yes\
<sup>La siguiente herramienta, Alevin, no necesita el encabezado de la tabla</sup>
5. _Comma-separated list of field names to extract from the GTF (default: use all fields):_ transcript_id,gene_id\
<sup>Esto ordena las columnas de los identificadores en IDs de transcritos y IDs de genes</sup>
6. _Append version to transcript identifiers?:_ Yes\
<sup>Son necesarios al trabajar con transcriptoma</sup>
7. _Flag mitochondrial features?:_ No
8. _Provide a cDNA file for extracting annotations and/ or possible filtering?:_ Yes
    * _FASTA-format cDNA/transcript file:_ Data input 'fasta_input' (fasta or fasta.gz)
    * _Annotation field to match with sequences:_ transcript_id
    * _Parse the FASTA headers for annotation info?:_ No
    * _Filter the cDNA file to match the annotations?:_ Yes
9. _Additional options_
    * _Configure Output: ‘feature_annotation’_
        * _Label:_ Transcrito a gen
        * _Rename dataset:_ Transcrito a gen
    * _Configure Output: ‘fasta_output’_
        * _Label:_ archivo fasta
        * _Rename dataset:_ archivo fasta filtrado

### 7: Generación de índice y cuantificación

A modo general, el proceso de alineamiento necesita de un índice que puede ser generado a partir de un genoma o un transcriptoma dependiendo del estudio (en este caso estamos trabajando con un transcriptoma). Este índice es una conversión del formato FASTA en el cual se encuentra el transcriptoma a un archivo en formato binario que es mucho más fácil de leer y buscar información. Este archivo binario no es legible para un humano, por lo que no podemos abrir el archivo y revisarlo visualmente para comprobar qué contiene. El índice, al ser generado desde el genoma de referencia de la especie de estudio, puede ser actualizado si se lanza una nueva versión, o si por el contrario, queremos contrastar nuestros resultados de alineamiento con otra versión del genoma que provenga de otra base de datos.

Utilizar herramienta **“Alevin”** (1.10.1+galaxy0)* con los siguientes ajustes:\
<sup> * Al buscar Alevin podríamos no encontrarlo directamente, si tiene este problema, debe buscarlo dentro de la pestaña “Single Cell” </sup>

1. _Select a reference transcriptome from your history or use a built-in index?:_  Use one from the history
    * _Salmon index:_ 
		* _Kmer length:_ 31
3. _Single or paired-end reads?:_ Paired-end\
<sup>Hacer coincidir archivo de salida de Read 1 con Mate 1 y Read 2 con Mate 2. De no trabajar en un flujo de trabajo, seleccionar manualmente estos archivos en la configuración de la herramienta</sup>
    * _Specify the strandedness of the reads:_ Infer automatically (A)
4. _Type of single-cell protocol_: DropSeq Single Cell protocol
5. _Extra output files_ (marcar casilla de las siguientes opciones):
    * Salmon Quant log file
    * Features used by the CB classification and their counts at each cell level (--dumpFeatures)
        * **Cuidado:** “_Per cell level parsimonioes Umi graph (-dumpUmiGraph)_” genera un archivo para cada célula. Esto no es necesario para este tutorial
6. _Advanced options:_
    * _Dump cell v transcripts count matrix in MTX format:_ Yes
7. _Aditional options:_\
<sup> **En esta sección puede configurar qué archivos de salida desea generar, así como los nombres de estos archivos para una identificación más eficaz luego de que concluya el Flujo de Trabajo**</sup>
    * _Configure Output: ‘quants_mat_mtx’_
        * _Label:_ matriz_conteo_genes
    * _Configure Output: ‘quants_mat_cols_txt_
        * _Label:_ IDs_genes
    * _Configure Output: ‘quants_mat_rows_txt_
        * _Label: _Codigos_de_barra
    * _Configure Output: ‘raw_cb_frequency_txt_
        * _Label:_ raw_cb_frequency_txt
    * _Configure Output: ‘salmon_quant_log’_
        * _Label:_ salmon_quant_log

### 9: QC – droplets

Desde este paso nos referiremos al Control de Calidad como QC (sigla en inglés para _Quality Control_). Este primer control de calidad es para evaluar la formación de gotas con una única célula en el experimento. Pese a que esta es el ideal al realizar un experimento de secuenciación de célula única, muchas veces se pueden obtener gotas con más de una célula, o gotas sin ninguna célula, conteniendo sólo debrís. Es por esto que en el control de calidad se evalúa la calidad de las gotas mediante una clasificación por ranking según abundancia de las UMIs en relación a los códigos de barra presentes en cada gota.  

Utilizar herramienta **“Droplet barcode rank plot”** (1.6.1+galaxy2) con los siguientes ajustes:

1. _Input MTX-format matrix?:_ No
2. _A two-column tab-delimited file, with barcodes in the first column and frequencies in the second:_ Archivo de salida de Alevin “raw_cb_frequency_txt”
3. _Number of bins used in barcode count frequency distribution:_ 50
4. _Above-baseline multiplier to calculate roryk threshold:_ 1.5
5. _Label to place in plot title:_ Barcode rank plot (raw barcode frequencies)
6. _Configure output: ‘plot_file’_
    * _Label:_ QC – droplets – frecuencias de códigos de barra en bruto
    * _Rename dataset:_ QC – droplets – frecuencias de códigos de barra en bruto

Antes de analizar el gráfico que obtuvimos, primero mostraremos cómo interpretarlo. El análisis de control de calidad se divide en dos gráficos, el de la izquierda representa el conteo de UMIs para códigos de barra individuales (eje y) versus el ranking (eje x), el cual va de más alto a más bajo desde izquierda a derecha. Se espera observar una disminución abrupta de UMIs para códigos de barra individuales, esto indica la división entre gotas que contengan células y las que no contienen células como se puede observar en el gráfico de abajo, para el cual se utilizaron datos obtenidos desde el [Atlas de expresión de célula única](https://www.ebi.ac.uk/gxa/sc/experiments/E-MTAB-6653/results/tsne)). Por otro lado, a la derecha encontramos un gráfico de densidad, en este gráfico se comparte el eje Y del gráfico de la derecha, siendo el conteo de UMIs para códigos de barra individuales, mientras que el eje x muestra la cantidad de células para cada conteo de UMIs, podemos pensar en este gráfico como un histograma en vertical, que representa la cantidad de células en función de los UMIs. En ambos gráficos tenemos los diferentes puntos de corte representados con diferentes colores, el punto de inflección calculado por dropletsutils en rojo, una bajada abrupta del conteo en base al ranking, llamado “knee” por “rodilla” en inglés, y el punto de corte del ranking de roryk en azul. 

![aspectos_generales](Imagenes/10_galaxy.png)

El resultado que se obtiene a partir del set de datos de entrenamiento usado en este tutorial no es muy informativo, como podemos observar en la siguiente imagen, debido a que sólo se tiene una fracción de las lecturas de secuenciación que se encontrarían normalmente en un caso real de análisis. Al revisar el gráfico obtenido podemos ver que no se presentan los hitos característicos, como la caída abrupta del ranking de UMIs, o el aumento marcado de densidad de células en el ranking superior de UMIs, producto del uso del set de datos de entrenamiento con una fracción de las lecturas.

![aspectos_generales](Imagenes/11_galaxy.png)

Por esto, incluimos el gráfico obtenido con el set de datos completo, el cual además de tener una mayor cantidad de lecturas, demora significativamente más en ser analizado utilizando este Flujo de Trabajo. En el set de datos completo podemos ver los hitos característicos de este análisis, y aunque no son tan marcados como en el set de datos que usamos de ejemplo anteriormente, se logran observar con bastante claridad. 

![aspectos_generales](Imagenes/12_galaxy.png)

La finalidad de este control de calidad es poder obtener la información para seleccionar las células en base al ranking de UMIs y la densidad. Para esto, podemos utilizar cualquiera de los tres puntos de corte que nos muestra el gráfico, ya que estos indican el mínimo necesario para considerarse una gota con una célula única, dependiendo de la estadística por la que se obtuvo cada punto de corte. Estas estadísticas pueden obtenerse como lo hicimos en este paso, utilizando el análisis de dropletUtils, utilizando el análisis de Alevin, que mostraremos en el siguiente paso, o incluso otras formas de analizar estos datos con otros programas.


### 10: QC  - droplets – Alevin

En el primer gráfico utilizamos el análisis automático que hace la herramienta dropletUtils, y en este paso utilizaremos el procesamiento de datos que entrega Alevin en el paso **7: Generación de índice y cuantificación**, para ilustrar las diferencias entre las formas de analizar la información entre estos dos softwares.

Utilizar herramienta **“Droplet barcode rank plot”** (1.6.1+galaxy2) con los siguientes ajustes:

1. _Input MTX-format matrix?:_ Yes
2. _Matrix-market format matrix file, with cells by column (overrides –barcode-frequencies if supplied):_ Archivo de salida de Alevin “matriz_conteo_genes”
3. _For use with –mtx-matrix: force interpretation of matrix to assume cells are by row, rather than by column (default):_ Yes
4. _Label to place in plot title:_ Barcode rank plot (Alevin-processed)
5. _Number of bins used in barcode count frequency distribution:_ 50
6. _Above-baseline multiplier to calculate roryk threshold:_ 1.5
7. _Label to place in plot title:_ Barcode rank plot (raw barcode frequencies)
8. _Configure output: ‘plot_file’_
    * _Label:_ QC – droplets – frecuencias de códigos de barra - Alevin
    * _Rename dataset:_ QC – droplets – frecuencias de códigos de barra - Alevin

En la siguiente imagen podemos ver el gráfico correspondiente al set de entrenamiento, en donde, a diferencia del análisis automático de dropletUtils, podemos distinguir los diferentes hitos característicos de este gráfico de ranking de UMIs y densidad. 

![aspectos_generales](Imagenes/13_galaxy.png)

Podemos observar incluso el set de datos completo, en donde vemos aún más marcados todos los hitos en comparación al gráfico obtenido en el análisis anterior. 

![aspectos_generales](Imagenes/14_galaxy.png)

Finalmente, a modo de comparación final, dejamos el análisis del set de datos de ejemplo que usamos en un principio para explicar los componentes de estos gráficos, en donde, de la misma forma que en los gráficos anteriores, los hitos se observan de forma mucho más clara utilizando los datos procesados por Alevin.

![aspectos_generales](Imagenes/15_galaxy.png)

Si bien ahora podemos identificar mucho mejor las diferentes características en las distribuciones de estos gráficos, también debemos tener cuidado, ya que Alevin excluye cualquier código de barras individual asignado a una célula que tenga menos de 10 UMIs. Al hacer esto, la distribución de datos se interrumpe drásticamente, y en algunos sets de datos más complejos, podría significar un problema a la hora de filtrar los datos en los pasos posteriores. 

Generalmente en experimentos con características relativamente simples podemos ver la caída en el ranking de UMIs bastante marcada, pero para algunas poblaciones, como lo vimos en el set de datos de entrenamiento, pueden presentar dificultades. Por ejemplo, algunas subpoblaciones de células pequeñas podrían no ser distinguidas aparte de gotas vacías, basándose únicamente en el conteo por código de barras. Incluso algunas librerías generan varias “knees” para múltiples subpoblaciones de células. 

Con este tipo de datos no comunes, el método de emptyDrops ha ganado popularidad, ya que puede lidiar muy bien con este tipo de datos, conservando los códigos de barra con conteos altos de UMIs, pero también agrega los códigos de barra cuyos conteos de UMIs pueden ser estadísticamente distinguidos de los códigos de barra que presenten conteos de UMIs de RNA ambiental producto de la preparación de las muestras (incluso si el conteo es similar). Para poder correr el análisis con emptyDrops (o cualquier herramienta de preferencia), primero tenemos que correr Alevin nuevamente, pero sin que este aplique sus propios umbrales de corte.


### 8: Alevin - sin procesar

En este paso ocuparemos Alevin, esta vez ajustando manualmente los parámetros para que no procese los datos como en el caso anterior.

Utilizar herramienta **“Alevin”** (1.10.1+galaxy0)* con los siguientes ajustes:\
<sup> * Al buscar Alevin podríamos no encontrarlo directamente, si tiene este problema, debe buscarlo dentro de la pestaña “Single Cell” </sup>

1. _Select a reference transcriptome from your history or use a built-in index?:_  Use one from the history
    * _Salmon index:_ 
    * _Kmer length:_ 31
2. _Single or paired-end read_s?:_ Paired-end 
3. _Single or paired-end reads?:_ Paired-end 
<sup>Hacer coincidir archivo de salida de Read 1 con Mate 1 y Read 2 con Mate 2. De no trabajar en un flujo de trabajo, seleccionar manualmente estos archivos en la configuración de la herramienta</sup>
    * _Specify the strandedness of the reads:_ Infer automatically (A)
4. _Type of single-cell protocol:_ DropSeq Single Cell protocol
5. _Extra output files_ (marcar casilla de las siguientes opciones):
    * _Salmon Quant log file_
    * _Features used by the CB classification and their counts at each cell level (--dumpFeatures)_
    * **Cuidado:** _“Per cell level parsimonioes Umi graph (-dumpUmiGraph)”_ genera un archivo para cada célula. Esto no es necesario para este tutorial
6. _Advanced options:_
    * _Dump cell v transcripts count matrix in MTX format:_ Yes
    * _“Fraction of cellular barcodes to keep”:_ ‘1’\
<sup>nos quedamos con todos los códigos de barras</sup>
    * _“Minimum frequency for a barcode to be considered”:_ 3\
<sup>Con este filtro nos deshacemos de todos los códigos de barra con una frecuencia menor a 3, lo que es un filtro muy bajo y que al mismo tiempo nos evitará procesar lo que es casi certeramente códigos de barra vacíos</sup>
7. _Aditional options:_
    * _Configure Output: ‘quants_mat_mtx’_
        * _Label:_ matriz_conteo_genes_sp
    * _Configure Output: ‘quants_mat_cols_txt_
        * _Label:_ IDs_genes_sp
    * _Configure Output: ‘quants_mat_rows_txt_
        * _Label:_ Codigos_de_barra_sp
    * _Configure Output: ‘salmon_quant_log’_
        * _Label:_ salmon_quant_log_sp

### 11: Cambio de formato - Alevin a 10X

Debido a que la matriz de salida del análisis realizado con Alevin no es el indicado para seguir con el análisis de emptyDrops, debemos hacer un reformateo de los datos a unos más similares a los obtenidos con tecnología 10X. 

Utilizar herramienta **“salmonKallistoMtxTo10x”** (0.0.1+galaxy6) con los siguientes ajustes:

1.  _mtx-format matrix:_ matriz_conteo_genes_sp\
<sup>Archivo de salida de Alevin, paso 8</sup>
2. _Tab-delimited genes file:_ IDs_genes_sp\
<sup>Archivo de salida de Alevin, paso 8</sup>
3. _ Tab-delimited barcodes file_: Codigos_de_barra_sp\
<sup>Archivo de salida de Alevin, paso 8</sup>
4. _Configure Output: ‘genes_out’_
    * _Label:_ Tabla_genes
    * _Rename dataset:_ Tabla_genes
5. _Configure Output: ‘barcodes_out’_
    * _Label:_ Tabla_codigos_de_barra
    * _Rename dataset:_ Tabla_codigos_de_barra
6. _Configure Output: ‘matrix_out’_
    * _Label:_ Matriz
    * _Rename dataset:_ Matriz

El archivo de salida de este paso es una matriz con la orientación correcta para que las herramientas que usaremos puedan leerla sin problemas. La limitación actual de estos datos es que por ahora sólo podemos identificar los genes por medio del el ID asignado por ENSEMBL, por lo que en los siguientes pasos vamos a completar la información con los nombres de los genes en formato abreviado, o como se conoce en algunas bases de datos, el “_gene_symbol”.

### 5: Obtención de anotación

Para obtener la anotación de los genes utilizaremos el genoma en formato GTF. En este archivo podemos encontrar toda la información de cada gen, como coordenadas genómicas, transcritos, productos, etc. 

Utilizar herramienta **“GTF2GeneList”** (1.52.0+galaxy0) con los siguientes ajustes:

1. _Feature type for which to derive annotation:_ gene
2. _Field to place first in output table:_ gene_id
3. _Suppress header line in output?:_ Yes
4. _Comma-separated list of field names to extract from the GTF (default: use all fields):_ gene_id,gene_name,mito
5. _Append version to transcript identifiers?:_ Yes
6. _Flag mitochondrial features?:_ Yes\
<sup>Esto rellenará automáticamente los parametros de esta sección. Estos parámetros son integrados por acrónimos que servirán como palabras clave para la clasificación de cromosomas y biotipos mitocondriales</sup>
7. _Provide a cDNA file for extracting annotations and/ or possible filtering?:_ No\
<sup>No es necesario para esta ocasión, ya que tenemos el archivo FASTA obtenido previamente al inicio del Flujo de Trabajo</sup>\
<sup> **Se debe checkear que el formato de salida del archivo sea “tabular”, de no ser así, esto puede ser cambiado en “Configure Output: ‘feature_annotation’ - Change datatype” y especificar dicho formato.** </sup>
8. _Configure Output: ‘feature_annotation’_
    * _Label:_ Anotación de genes
    * _Rename dataset:_ Anotación de genes

Al inspeccionar esta tabla, podemos ver la información que obtuvimos con esta extracción de datos:

<table>
  <tr>
   <td>Column 1
   </td>
   <td>Column 2
   </td>
   <td>Column 3
   </td>
  </tr>
  <tr>
   <td>ENSMUSG00000102693
   </td>
   <td>4933401J01Rik
   </td>
   <td>False
   </td>
  </tr>
  <tr>
   <td>ENSMUSG00000064842
   </td>
   <td>Gm26206
   </td>
   <td>False
   </td>
  </tr>
  <tr>
   <td>ENSMUSG00000051951
   </td>
   <td>Xkr4
   </td>
   <td>False
   </td>
  </tr>
  <tr>
   <td>ENSMUSG00000102851
   </td>
   <td>Gm18956
   </td>
   <td>False
   </td>
  </tr>
  <tr>
   <td>ENSMUSG00000103377
   </td>
   <td>Gm37180
   </td>
   <td>False
   </td>
  </tr>
  <tr>
   <td>ENSMUSG00000104017
   </td>
   <td>Gm37363
   </td>
   <td>False
   </td>
  </tr>
</table>

### 12: Juntar tablas de genes y anotación

La información que obtuvimos en el paso anterior no nos sirve si no está integrada en nuestro set de datos que analizaremos posteriormente, es por esto que vamos a juntar la tabla de genes que obtuvimos en el paso 11 y la tabla que obtuvimos en el paso anterior.

Utilizar herramienta **“Join two Datasets”** con los siguientes ajustes:

1. _Join:_ Tabla_genes 
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección “_Join_” en el módulo.
2. _Using column:_ 1
3. _with:_ Anotación de genes
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección “_with_” en el módulo.
4. _and column:_ Column: 1
5. _Keep lines of first input that do not join with second input:_ Yes
6. _Keep lines of first input that are incomplete:_ Yes
7. _Fill empty columns:_ No
8. _Keep the header lines:_ No
9. _Configure Output: ‘feature_annotation’_
    * _Label:_ Tabla_genes_anotada_raw
    * _Rename dataset:_ Tabla_genes_anotada_raw

### 13: Filtrado de datos de anotación

Al inspeccionar la tabla que obtuvimos en el paso anterior, podemos encontrar repeticiones de IDs de genes, por lo que necesitamos eliminar esta redundancia de información.

Utilizar herramienta **“Cut columns from a table”** con los siguientes ajustes:

1. _Cut columns:_ c1,c4,c5
2. _Delimited by:_ Tab
3. _From:_ Tabla_genes_anotada_raw 
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en el la sección “_From_” en el módulo.
4. _Configure Output: ‘out_file1’_
    * _Label:_ Tabla_genes_anotada
    * _Rename dataset:_ Tabla_genes_anotada

### 14: Creación objeto SCE

Finalmente hemos llegado al paso de hacer el análisis de emptyDrops. Aquí procesaremos las tablas que generamos anteriormente para obtener el análisis de control de calidad utilizando el método de emptyDrops. Para comenzar con esto, primero debemos generar un objeto con los 3 set de datos, Tabla_genes_anotadas, Tabla_codigos_de_barra, y Matriz. 

Utilizar herramienta **“DropletUtils Read10x”** (1.0.4+galaxy0) con los siguientes ajustes:

1. _“Expression matrix in sparse matrix format (.mtx)”:_ Matrix table
2. param-file _“Gene Table”:_ Annotated Gene Table
3. param-file _“Barcode/cell table”:_ Barcode table
4. _“Should metadata file be added?”:_ No
5. _Configure Output: ‘output_rds’_
    * _Label:_ Objeto SCE
    * _Rename dataset:_ Objeto SCE

### 15: Análisis con emptyDrops

En este paso filtraremos las gotas que sean de baja calidad, quedándonos con la información útil para los análisis posteriores.

Utilizar herramienta **“DropletUtils emptyDrops”** (1.0.4+galaxy0) con los siguientes ajustes:

1. _SingleCellExperiment rdata object:_ Objeto SCE
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección “_SingleCellExperiment rdata object_” en el módulo.
2. _Should barcodes estimated to have no cells be removed from the output object?:_ Yes
3. _Configure Output: ‘output_rdata’_
    * _Label:_ Objeto_vaciado
    * _Rename dataset:_ Objeto_vaciado
4. _Configure Output: ‘output_txt’_
    * _Label:_ Archivo_tabular_vaciado
    * _Rename dataset:_ Archivo_tabular_vaciado
5. Los siguientes ajustes deberían ser los valores por defecto, están incluidos en este tutorial únicamente con la finalidad de mantenerlos de forma explícita y obtener resultados similares, ya que siempre hay un factor que podría cambiar los resultados, producto de las iteraciones de algoritmos que se hacen en los diferentes módulos.
    * _UMI count lower bound:_ 100
    * _Number of iterations:_ 1000
    * _FDR filter for removal of barcodes with no cells:_ 0.01
    * _Should results be returned for barcodes with totals less than or equal to the value of ‘lower’?:_ No

Al llevar a cabo el análisis con un set de datos de entrenamiento, es normal tener un bajo número de células luego del filtrado.

### 16: Creación de objeto AnnData

Este es el último paso de la primera parte de este tutorial, y consiste en cambiar el formato de salida, SCE, a un formato de objeto que se usa comúnmente en los análisis subsecuentes de secuenciación de célula única, AnnData, que es una variante del formato hdf5.

Utilizar herramienta **“SCEasy Converter”** (0.0.7+galaxy2/0.0.5+galaxy1) con los siguientes ajustes:

1. _Convert From / To:_ SingleCellExperiment to AnnData
2. _ Input object in sce,rds,rdata.sce format:_ Objeto_vaciado 
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección “_Input objetc_” en el módulo.
3. _Configure Output: ‘output_anndata’_
    * _Label:_ Objeto_anndata
    * _Rename dataset:_ Objeto_anndata
