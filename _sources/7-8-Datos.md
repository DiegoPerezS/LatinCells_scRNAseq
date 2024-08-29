Adaptado desde la página de entrenamiento en Galaxy:

[https://training.galaxyproject.org/training-material/topics/single-cell/tutorials/scrna-case_alevin/tutorial.html](https://training.galaxyproject.org/training-material/topics/single-cell/tutorials/scrna-case_alevin/tutorial.html)

En esta primera parte se tomarán los datos crudos en formato FASTQ y serán llevados a un formato de matriz de células y genes en formato AnnData, el cual será usado en el siguiente paso. Este análisis se enfoca en identificar y asignar los genes expresados en cada célula secuenciada en un formato de matriz en donde la primera columna corresponde a las células, la primera fila a los genes, y el contenido las lecturas asignadas para X gen en Y célula.

Para esto, se usará la aproximación basada en gotas (en lugar de la metodología basada en placa), ya que esta aproximación es la que más se diferencia de otras desarrolladas para secuenciación de RNA completo. La aproximación basada en gotas tiene tres componentes: **códigos de barra para cada célula, identificadores moleculares únicos (UMIs), y** **lecturas de cDNA**, estos componentes serán utilizados para asignar la cuantificación de lecturas para cada célula de la siguiente forma:

* Procesamiento de códigos de barra, para identificar células “reales” y artefactos generados por la secuenciación, buscando corregirlos para generar una asignación acertada
* Mapeo de secuencias biológicas (lecturas) al genoma o transcriptoma del organismo estudiado
* De-duplicación de células no-únicas utilizando los UMIs

Cabe mencionar, que este paso debe ser realizado para cada muestra de secuenciación de célula única que analicemos, por lo que, si son 6 muestras, esta parte del tutorial debe repetirse la misma cantidad de veces.

## Datos 

    * (Puede encontrar el archivo GTF y el transcriptoma en formato FASTA de su especie de interés en ENSEMBL, dentro del siguiente enlace: http://www.ensembl.org/info/data/ftp/index.html/)

    ```
    https://zenodo.org/record/4574153/files/Experimental_Design.tabular
    https://zenodo.org/record/4574153/files/Mus_musculus.GRCm38.100.gtf.gff
    https://zenodo.org/record/4574153/files/Mus_musculus.GRCm38.cdna.all.fa.fasta
    https://zenodo.org/record/4574153/files/SLX-7632.TAAGGCGA.N701.s_1.r_1.fq-400k.fastq
    https://zenodo.org/record/4574153/files/SLX-7632.TAAGGCGA.N701.s_1.r_2.fq-400k.fastq
    ```

    * Una vez cargados los archivos, se debe cambiar el nombre de:
        * SLX-7632.TAAGGCGA.N701.s_1.r_1.fq-400k:  N701-Read1
        * SLX-7632.TAAGGCGA.N701.s_1.r_2.fq-400k: N701-Read2

el archivo de **GTF, transcriptoma de referencia, Read 1 **(contiene código de barras de células, y UMI), y **Read 2** (contiene los transcritos).

Este tutorial está diseñado para ser usado con secuenciaciones obtenidas utilizando química tanto de Drop-seq como 10x. En este tutorial se usará como base una secuenciación obtenida con Drop-seq, en el caso de que desee realizar un análisis con archivos de secuenciación obtenidos empleando el protocolo de 10x, debe hacer los ajustes correspondientes indicados en el **Paso 11**. Como punto de partida se usarán los datos crudos de secuenciación en formato FASTQ, el transcriptoma de referencia del organismo que se está estudiando, la información de los transcritos en formato GTF. Además importamos el diseño experimental en formato tabular como se indica en el siguiente ejemplo, el cual no será usado en esta parte del tutorial, pero es bueno tener en cuenta la estructura de nuestras muestras desde un principio.

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
   <td>male
   </td>
   <td>wt
   </td>
   <td>N701
   </td>
  </tr>
  <tr>
   <td>male
   </td>
   <td>wt
   </td>
   <td>N704
   </td>
  </tr>
  <tr>
   <td>male
   </td>
   <td>wt
   </td>
   <td>N705
   </td>
  </tr>
  <tr>
   <td>male
   </td>
   <td>wt
   </td>
   <td>N706
   </td>
  </tr>
  <tr>
   <td>male
   </td>
   <td>ko
   </td>
   <td>N702
   </td>
  </tr>
  <tr>
   <td>female
   </td>
   <td>ko
   </td>
   <td>N703
   </td>
  </tr>
  <tr>
   <td>male
   </td>
   <td>ko
   </td>
   <td>N707
   </td>
  </tr>
</table>

En este tutorial se utilizará únicamente el set de datos de la primera fila, haciendo que este primer acercamiento sea más rápido.
