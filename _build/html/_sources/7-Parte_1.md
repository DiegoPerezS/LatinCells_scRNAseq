# Generación de matrices de células únicas usando Alevin

Adaptado desde la página de entrenamiento en Galaxy:

[https://training.galaxyproject.org/training-material/topics/single-cell/tutorials/scrna-case_alevin/tutorial.html](https://training.galaxyproject.org/training-material/topics/single-cell/tutorials/scrna-case_alevin/tutorial.html)

En esta primera parte se tomarán los datos crudos en formato FASTQ y serán llevados a un formato de matriz de células y genes en formato AnnData, el cual será usado en el siguiente paso. Este análisis se enfoca en identificar y asignar los genes expresados en cada célula secuenciada en un formato de matriz en donde la primera columna corresponde a las células, la primera fila a los genes, y el contenido las lecturas asignadas para X gen en Y célula.

Para esto, se usará la aproximación basada en gotas (en lugar de la metodología basada en placa), ya que esta aproximación es la que más se diferencia de otras desarrolladas para secuenciación de RNA completo. La aproximación basada en gotas tiene tres componentes: **códigos de barra para cada célula, identificadores moleculares únicos (UMIs), y** **lecturas de cDNA**, estos componentes serán utilizados para asignar la cuantificación de lecturas para cada célula de la siguiente forma:

* Procesamiento de códigos de barra, para identificar células “reales” y artefactos generados por la secuenciación, buscando corregirlos para generar una asignación acertada
* Mapeo de secuencias biológicas (lecturas) al genoma o transcriptoma del organismo estudiado
* De-duplicación de células no-únicas utilizando los UMIs

Cabe mencionar, que este paso debe ser realizado para cada muestra de secuenciación de célula única que analicemos, por lo que, si son 6 muestras, esta parte del tutorial debe repetirse la misma cantidad de veces.
