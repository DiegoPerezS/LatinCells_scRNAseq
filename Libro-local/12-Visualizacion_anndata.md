## Visualización de objeto AnnData

### 9: Inspección – General

Con esta inspección podremos conocer el número de células con las que estamos trabajando (n_obs) y la cantidad de genes encontrados (n_vars), además de la información de cada gen (obs:). Al presionar el botón de visualización en el archivo del historial, deberíamos ver algo similar a este texto:

```
AnnData object with n_obs × n_vars = 331 × 35734
    obs: 'Barcode', 'emptyTotal', 'emptyLogProb', 'emptyPValue', 'emptyLimited', 'emptyFDR', 'batch'
    var: 'ID', 'Symbol', 'NA.'
```

Utilizar herramienta **“Inspect AnnData”** (0.10.3+galaxy0) con los siguientes ajustes:

1. _Annotated data matrix:_ Combined object
2. _What to inspect?:_ General information about the object
3. _Configure Output: ‘general’_
    * _Label:_ Inspección - General
    * _Rename dataset:_ Inspección - General

### 10: Inspección – Anotación de genes

Con esta inspección podremos ver la información de lcada gen, como el identificador, el nombre abreviado, y si pertenece a un gen mitocondrial. Deberíamos visualizarlo de la siguiente forma:

<table>
  <tr>
   <td>Column 1</td>
   <td>Column 2</td>
   <td>Column 3</td>
   <td>Column 3</td>
  </tr>
  <tr>
   <td></td>
   <td>ID</td>
   <td>Symbol</td>
   <td>NA.</td>
  </tr>
  <tr>
   <td>ENSMUSG00000102693</td>
   <td>ENSMUSG00000102693</td>
   <td>4933401J01Rik</td>
   <td>False</td>
  </tr>
  <tr>
   <td>ENSMUSG00000064842</td>
   <td>ENSMUSG00000064842</td>
   <td>Gm26206</td>
   <td>False</td>
  </tr>
  <tr>
   <td>ENSMUSG00000051951</td>
   <td>ENSMUSG00000051951</td>
   <td>Xkr4</td>
   <td>False</td>
  </tr>
  <tr>
   <td>ENSMUSG00000102851</td>
   <td>ENSMUSG00000102851</td>
   <td>Gm18956</td>
   <td>False</td>
  </tr>
  <tr>
   <td>ENSMUSG00000103377</td>
   <td>ENSMUSG00000103377</td>
   <td>Gm37180</td>
   <td>False</td>
  </tr>
  <tr>
   <td>ENSMUSG00000104017</td>
   <td>ENSMUSG00000104017</td>
   <td>Gm37363</td>
   <td>False</td>
  </tr>
</table>

Utilizar herramienta **“Inspect AnnData”** (0.10.3+galaxy0) con los siguientes ajustes:

1. _Annotated data matrix:_ Combined object
2. _What to inspect?:_ Key-indexed annotation of variables/features (var)
3. _Configure Output: ‘general’_
    * _Label:_ Inspección – Anotación de genes
    * _Rename dataset:_ Inspección - Anotación de genes

### 11: Inspección – Información de las células

Con esta inspección podremos conocer la información de las células obtenidas, además podemos agregar metadata para poder diferenciar desde que muestra provienen de una forma legible para nosotros. Cabe destacar que acá podemos ver reflejado el orden en el que las matrices fueron agregadas al archivo concatenado, esto es posible revisarlo en la columna 8, o batch, en la cual el número refleja a qué set de datos pertenece esa célula. Por ejemplo, el número 0 de la columna batch (la cuenta parte desde 0) indica que es la primera matriz del objeto concatenado, que debería ser la matriz perteneciente a la muestra N701. Deberíamos ver esta información de la siguiente forma:

<table>
    <tr>
        <td>Column 1</td>
        <td>Column 2</td>
        <td>Column 3</td>
        <td>Column 4</td>
        <td>Column 5</td>
        <td>Column 6</td>
        <td>Column 7</td>
        <td>Column 8</td>
    </tr>
    <tr>
        <td></td>
        <td>Barcode</td>
        <td>emptyTotal</td>
        <td>emptyLogProb</td>
        <td>emptyPValue</td>
        <td>emptyLimited</td>
        <td>emptyFDR</td>
        <td>batch</td>
    </tr>
    <tr>
        <td>GAGCCCTGTTTA-0</td>
        <td>GAGCCCTGTTTA</td>
        <td>270</td>
        <td>-982.5027417312477</td>
        <td>0.02197802197802198</td>
        <td>False</td>
        <td>0.0</td>
        <td>0</td>
    </tr>
    <tr>
        <td>TTTCCGACCTTC-0</td>
        <td>TTTCCGACCTTC</td>
        <td>230</td>
        <td>-936.1786252396713</td>
        <td>0.000999000999000999</td>
        <td>True</td>
        <td>0.0</td>
        <td>0</td>
    </tr>
    <tr>
        <td>TCGTAAATTGCC-0</td>
        <td>TCGTAAATTGCC</td>
        <td>224</td>
        <td>-888.5342462036547</td>
        <td>0.002997002997002997</td>
        <td>False</td>
        <td>0.0</td>
        <td>0</td>
    </tr>
</table>

Utilizar herramienta **“Inspect AnnData”** (0.10.3+galaxy0) con los siguientes ajustes:

1. _Annotated data matrix:_ Combined object
2. _What to inspect?:_ Key-indexed observations annotation (obs)
3. _Configure Output: ‘general’_
    * _Label:_ Inspección – Información de las células
    * _Rename dataset:_ Inspección - Información de las células
