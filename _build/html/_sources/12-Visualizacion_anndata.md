## Visualización de objeto AnnData

### 9: Inspección – General

Con esta inspección podremos conocer el número de células con las que estamos trabajando (n_obs) y la cantidad de genes encontrados (n_vars), además de la información de cada gen (obs:). Al presionar el botón de visualización en el archivo del historial, deberíamos ver algo similar a este texto:

AnnData object with n_obs × n_vars = 331 × 35734

    obs: 'Barcode', 'emptyTotal', 'emptyLogProb', 'emptyPValue', 'emptyLimited', 'emptyFDR', 'batch'

    var: 'ID', 'Symbol', 'NA.'

Utilizar herramienta **“Inspect AnnData”** (0.10.3+galaxy0) con los siguientes ajustes:

1. _Annotated data matrix:_ Combined object
2. _What to inspect?:_ General information about the object
3. _Configure Output: ‘general’_
    * _Label: _Inspección - General
    * _Rename dataset: _Inspección - General

### 10: Inspección – Anotación de genes

Con esta inspección podremos ver la información de lcada gen, como el identificador, el nombre abreviado, y si pertenece a un gen mitocondrial. Deberíamos visualizarlo de la siguiente forma:

Column 1	Column 2	Column 3	Column 4

ID	Symbol	NA.

ENSMUSG00000051951	ENSMUSG00000051951	Xkr4	False

ENSMUSG00000102851	ENSMUSG00000102851	Gm18956	False

ENSMUSG00000103147	ENSMUSG00000103147	Gm7341	False

ENSMUSG00000025900	ENSMUSG00000025900	Rp1	False

ENSMUSG00000102948	ENSMUSG00000102948	Gm6101	False

Utilizar herramienta **“Inspect AnnData”** (0.10.3+galaxy0) con los siguientes ajustes:

1. _Annotated data matrix:_ Combined object
2. _What to inspect?:_ Key-indexed annotation of variables/features (var)
3. _Configure Output: ‘general’_
    * _Label: _Inspección – Anotación de genes
    * _Rename dataset: _Inspección - Anotación de genes

### 11: Inspección – Información de las células

Con esta inspección podremos conocer la información de las células obtenidas, además podemos agregar metadata para poder diferenciar desde que muestra provienen de una forma legible para nosotros. Cabe destacar que acá podemos ver reflejado el orden en el que las matrices fueron agregadas al archivo concatenado, esto es posible revisarlo en la columna 8, o batch, en la cual el número refleja a qué set de datos pertenece esa célula. Por ejemplo, el número 0 de la columna batch (la cuenta parte desde 0) indica que es la primera matriz del objeto concatenado, que debería ser la matriz perteneciente a la muestra N701. Deberíamos ver esta información de la siguiente forma:

Column 1	Column 2	Column 3	Column 4	Column 5	Column 6	Column 7	Column 8

Barcode	emptyTotal	emptyLogProb	emptyPValue	emptyLimited	emptyFDR	batch

GAGCCCTGTTTA-0	GAGCCCTGTTTA	270	-982.5027417312477	0.02197802197802198	False	0.0	0

TTTCCGACCTTC-0	TTTCCGACCTTC	230	-936.1786252396713	0.000999000999000999	True	0.0	0

TCGTAAATTGCC-0	TCGTAAATTGCC	224	-888.5342462036547	0.002997002997002997	False	0.0	0

Utilizar herramienta **“Inspect AnnData”** (0.10.3+galaxy0) con los siguientes ajustes:

1. _Annotated data matrix:_ Combined object
2. _What to inspect?:_ Key-indexed observations annotation (obs)
3. _Configure Output: ‘general’_
    * _Label: _Inspección – Información de las células
    * _Rename dataset: _Inspección - Información de las células
