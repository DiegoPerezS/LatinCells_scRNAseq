## Concatenar set de datos

### 8: Concatenar set de datos

En este paso todos los archivos, que serían objetos AnnData formato h5ad serán concatenados en un solo objeto del mismo formato.

Utilizar herramienta **“Manipulate AnnData”** (0.10.3+galaxy0) con los siguientes ajustes:

1. _Annotated data matrix: _N701-400k 
    * Especificar solo en caso de no estar en la interfaz de Flujo de Trabajo.
    * En el flujo de trabajo se debe conectar este archivo de salida en la sección “_Annotated data matrix_” en el módulo.
2. _Function to manipulate the object:_ Concatenate along the observations axis
3. _Annotated data matrix to add:_ Conectar/Seleccionar todos los otros archivos. Recordar que el set de datos N701 ya fue ingresado a la herramienta anteriormente.
4. _Join method:_ Intersection of variables
5. _Key to add the batch annotation to obs:_ batch
6. _Separator to join the existing index names with the batch category:_ -
7. _Configure Output: ‘anndata’_
    * _Label: _AnnData Combinado
    * _Rename dataset: _AnnData Combinado

En los siguientes pasos se visualizará el objeto concatenado, para esto se utilizará repetitivamente la misma herramienta.
