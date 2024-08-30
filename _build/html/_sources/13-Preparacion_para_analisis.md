## Preparación de datos para el análisis

Cabe destacar, que dependiendo del método en el que se hayan importado los sets de datos de entrenamiento de este tutorial, o de igual forma, los archivos que se hayan utilizado mediante los métodos para **Importar el historial**, podrían estar desordenados, como se menciona en la explicación de la **Parte 2**, por lo que debe tener cuidado de corregir los ajustes de estos pasos de acuerdo a su conveniencia. Además, esta metadata puede ser cualquiera que el usuario desee agregar, por lo que estos pasos son totalmente adaptables a cualquier experimento que se desee analizar.

### 12: Agregar metadata – Sexo – Pt 1

En este paso agregaremos la información del sexo de las muestras que estamos analizando, para esto tomaremos el número de la columna “batch”, que configuramos en el **Paso 8**, numeración 5. 

Esto lo podremos hacer conociendo el orden en el que se importaron los sets de datos y la tabla “Experimental_Design.tabular” que descargamos junto a estos. El número del orden de importación de los sets de datos va de 0 a 6 en este caso, partiendo desde el set de datos N701 hasta el N707 (si es que todo fue importado en el orden que corresponde), este número es el que fue asignado en la Columna 8, en esta tabla, podemos ver también que la primera fila corresponde al encabezado de la tabla y que nos confirma que corresponde a “batch”. Tomando en cuenta esto, si nosotros confeccionamos una tabla que contenga el diseño experimental y el orden en el que se importaron los sets de datos, obtendríamos algo como esto:

<table>
  <tr>
   <td>Index</td>
   <td>Batch</td>
   <td>Genotype</td>
   <td>Sex</td>
  </tr>
  <tr>
   <td>N701</td>
   <td>0</td>
   <td>wildtype</td>
   <td>male</td>
  </tr>
  <tr>
   <td>N702</td>
   <td>1</td>
   <td>knockout</td>
   <td>male</td>
  </tr>
  <tr>
   <td>N703</td>
   <td>2</td>
   <td>knockout</td>
   <td>female</td>
  </tr>
  <tr>
   <td>N704</td>
   <td>3</td>
   <td>wildtype</td>
   <td>male</td>
  </tr>
  <tr>
   <td>N705</td>
   <td>4</td>
   <td>wildtype</td>
   <td>male</td>
  </tr>
  <tr>
   <td>N706</td>
   <td>5</td>
   <td>wildtype</td>
   <td>male</td>
  </tr>
  <tr>
   <td>N707</td>
   <td>6</td>
   <td>knockout</td>
   <td>male</td>
  </tr>
</table>

Esta tabla no es necesaria, es sólo un ejemplo para poder visualizar de mejor forma a qué corresponde cada número en la columna “batch” y así poder trabajar correctamente con nuestros sets de datos.

Utilizar herramienta **“Replace Text in a specific column”** (9.3+galaxy0) con los siguientes ajustes:

1. _File to process: output of Inspect AnnData:_ Información de las células 
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección “_File to process”_ en el módulo.
2. _1: Replacement:_
    a. _in column:_ 8\ 
<sup>O la columna en donde esté “batch”</sup>
    b. _Find pattern:_ 0|1|3|4|5|6\
<sup>Aquí se colocan los valores de “batch” que contengan la información que deseamos agregar, en este caso, las muestras que corresponden a machos. En este parámetro se pueden ocupar expresiones regulares, en este caso el carácter “|” significa ó, lo que se traduciría en encontrar 0, ó 1, ó 3…</sup>
    c. _Replace with:_ male\
<sup>En este caso, es el sexo que corresponde con el número de “batch”</sup>
3. _2. Replacement:_
    a. _in column:_ 8
    b. _Find pattern:_ 2
    c. _Replace with:_ female
4. _3. Replacement:_
    a. _in column:_ 8
    b. _Find pattern:_ batch
    c. _Replace with:_ sex
5. _Configure Ouput: ‘outfile’_
    a. _Label:_ Agegar metadata – Sexo – Pt 1
    b. _Rename dataset:_ Agegar metadata – Sexo – Pt 1

### 14: Agregar metadata – Sexo – Pt 2

Utilizar herramienta **“Replace Text in a specific column”** (9.3+galaxy0) con los siguientes ajustes:

1. _File to process: output of Inspect AnnData:_ 
2. _Cut columns:_ c8
3. _Delimited by:_ Tab
4. _From:_ Agegar metadata – Sexo – Pt 1 
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección “_From_” en el módulo.
5. _Configure Ouput: ‘outfile’_
    1. _Label:_ Metadata – Sexo
    2. _Rename dataset:_ Metadata – Sexo

### 13: Agregar metadata – Genotipo – Pt 1

Ahora agregaremos la información correspondiente al genotipo, del mismo modo que agregamos la información en el **Paso 12**.

Utilizar herramienta **“Replace Text in a specific column”** (9.3+galaxy0) con los siguientes ajustes:

1. _File to process: output of Inspect AnnData:_ Información de las células 
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección “_File to process_” en el módulo.
2. _1: Replacement:_
    a. _in column:_ 8 
    b. _Find pattern:_ 0|3|4|5 
    c. _Replace with:_ wildtype
3. _2. Replacement:_
    a. _in column:_ 8
    b. _Find pattern:_ 1|2|6
    c. _Replace with:_ knockout
4. _3. Replacement:_
    a. _in column:_ 8
    b. _Find pattern:_ batch
    c. _Replace with:_ genotype
5. _Configure Ouput: ‘outfile’_
    a. _Label:_ Agegar metadata – Genotipo – Pt 1
    b. _Rename dataset:_ Agegar metadata – Genotipo – Pt 1

### 15: Agregar metadata – Genotipo – Pt 2

Utilizar herramienta **“Replace Text in a specific column”** (9.3+galaxy0) con los siguientes ajustes:

1. _File to process: output of Inspect AnnData:_ 
2. _Cut columns:_ c8
3. _Delimited by:_ Tab
4. _From:_ Agegar metadata – Genotipo – Pt 1 
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección “_From_” en el módulo.
5. _Configure Ouput: ‘outfile’_
    * _Label:_ Metadata – Genotipo
    * _Rename dataset:_ Metadata – Genotipo

### 16: Juntar Metadata

Ahora que tenemos la información de interés, que en este caso es Genotipo y Sexo, podemos juntar esta información. Cabe destacar, que para otros experimentos que en donde no aplique separar muestras por genotipo o sexo, podemos cambiar los parámetros de las herramientas utilizadas entre el **Paso 12 y 15**, o incluso agregar repeticiones de estos pasos para agregar más información.  

Utilizar herramienta **“Paste two files side by side”** con los siguientes ajustes:

1. _Paste:_ Metadata - Genotipo
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección _“Paste”_ en el módulo.
2. _and:_ Metadata - Sexo
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección _“and”_ en el módulo.
3. _Delimit by:_ Tab
4. _Configure Ouput: ‘out_file1’_
    * _Label:_ Metadata - Células
    * _Rename dataset:_ Metadata – Células

### 17: Integración de metadata a objeto AnnData

Al tener la metadata creada en una tabla no podríamos usarla en nuestros análisis, por esto es necesario agregarla a nuestro objeto AnnData que contiene toda la información de nuestras muestras de secuenciación de célula única.

Utilizar herramienta **“Manipulate AnnData”** (0.10.3+galaxy0) con los siguientes ajustes:

1. _Annotated data matrix:_ Objeto combinado
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección _“Annotated data matrix”_ en el módulo.
2. _Function to manipulate the object:_ Add new annotation(s) for observations or variables
3. _What to annotate?: _Observations (obs)
4. _Table with new annotations:_ Metadata - Células
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección _“Table with new annotations”_ en el módulo.
5. _Configure Ouput: ‘anndata’_
    * _Label:_ Objeto AnnData con metadata
    * _Rename dataset:_ Objeto AnnData con metadata

### 18: Identificación de muestras

Antes de seguir, es una buena idea cambiar el significado de el dato “batch” a algo que nos sea de utilidad de una forma más directa. Siendo que “batch” contiene el orden en el que fueron importados los datos de cada muestra, lo mejor sería poder identificar la muestra como tal. 

Utilizar herramienta **“Manipulate AnnData”** (0.10.3+galaxy0) con los siguientes ajustes:

1. _Annotated data matrix:_ Objeto combinado`
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección _“Annotated data matrix”_ en el módulo.
2. _Function to manipulate the object:_ Rename categories of annotation
3. _Key for observations or variables annotation:_ batch
4. _Comma-separated list of new categories:_ N701,N702,N703,N704,N705,N706,N707
<sup>Cuando esté utilizando datos propios, debe reemplazar estos nombres por los que correspondadan a sus muestras, en orden</sup>
5. _Configure Ouput: ‘anndata’_
    * _Label:_ Objeto AnnData con metadata corregida
    * _Rename dataset:_ Objeto AnnData con metadata corregida

### 19: Anotación de lecturas mitocondriales

Para terminar de preparar nuestro objeto AnnData preprocesado, necesitamos anotar los genes mitocondriales que ya habíamos definido en la **Parte 1**.

Utilizar herramienta **“AnnData Operations”** (7.9.3+galaxy0) con los siguientes ajustes:

1. _Input object in hdf5 AnnData format:_ Objeto AnnData con metadata corregida
>Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.\
>En el flujo de trabajo se debe conectar este archivo de salida en la sección _Input object in hdf5 AnnData format_ en el módulo.
2. _Format of output object:_ AnnData format
3. _Gene symbols field in AnnData:_ NA.
4. _Flag genes that start with these names:_
    * _Starts with:_ True
    * _Var name:_ mito
5. _Configure Ouput: ‘anndata’_
    * _Label:_ Objeto Anndata anotado
    * _Rename dataset:_ Objeto Anndata anotado
