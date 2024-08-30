# Datos

En esta parte del tutorial combinaremos los diferentes archivos de secuenciación que preprocesamos en el paso anterior. Esto quiere decir que el paso anterior será ejecutado para cada una de las muestras, y cada objeto AnnData que obtenemos al final será el archivo de entrada para esta parte. Estos los podemos obtener desde el historial, y para poder identificar con mayor facilidad, el usuario puede cambiar el nombre del objeto AnnData de cada muestra para poder identificarlos fácilmente. Además, también es posible importar los objetos AnnData, y existen 3 formas de hacerlo:

Debido a que este tutorial utiliza set de datos de entrenamiento, en la **Parte 1** no analizamos más de una muestra, por lo que, para continuar con este tutorial, debe **importar** los sets de datos con los siguientes enlaces:

<https://zenodo.org/records/10852529/files/Experimental_Design.tabular.tabular>
<https://zenodo.org/records/10852529/files/N701-400k-AnnData.h5ad>
<https://zenodo.org/records/10852529/files/N702-400k-AnnData.h5ad>
<https://zenodo.org/records/10852529/files/N703-400k-AnnData.h5ad>
<https://zenodo.org/records/10852529/files/N704-400k-AnnData.h5ad>
<https://zenodo.org/records/10852529/files/N705-400k-AnnData.h5ad>
<https://zenodo.org/records/10852529/files/N706-400k-AnnData.h5ad>
<https://zenodo.org/records/10852529/files/N707-400k-AnnData.h5ad>

Al importar los archivos, es importante que sean leídos como h5ad, de lo contrario, se debe cambiar el tipo de archivo a h5ad. 

Para esto, debe seleccionar el ícono en forma de pincel para el archivo, y seleccionar la pestaña “Datatypes”, y luego en “Assign Datatype”, seleccionar el formato h5ad, y guardar presionando el botón “Save”.

Entre los archivos importados, podemos encontrar una tabla llamada “Experimental_Design”, en esta se cuenta la información para que las herramientas que usaremos entiendan que los set de datos son diferentes muestras y en qué consisten. Particularmente en este set de datos de entrenamiento encontramos la información del nombre de cada muestra y si proviene de un macho o una hembra. Esta metadata es relevante para los siguientes pasos del tutorial, así como la metadata que agregamos en la **Parte 1**, el nombre de los genes (gene_name) y si estos pertenecen a la mitocondria (mito).

**Importante:** Cabe destacar, que para que los datos sean bien leídos en los futuros pasos, debemos asignar correctamente el archivo descargado al input al momento de correr el Flujo de Trabajo, o sea, que la muestra N701 sea asignada como la muestra N701 efectivamente, y no la N704 como podría pasar si dejamos que Galaxy las asigne por orden de importación de archivos.
