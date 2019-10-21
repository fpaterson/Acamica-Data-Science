# Acamica-Data-Science
Archivos de la carrera de data science presencial de Acámica, entre septiembre de 2019 y marzo de 2020.

Devolución del mentor de Acámica a la primera entrega:

Comentario general

¡Felicitaciones Francisco, tu primer entrega está aprobada!
Estuve revisando tu código y esta bien. Se nota que aprendiste todos los conceptos necesarios para hacer la exploración de los datos. Cumpliste con todas las consignas del checklist y tu solución correctamente, incluyendo el desafío. Hay unos detalles a tener en cuenta:

6) Quitá del dataframe las instancias que no tengan ningún valor en los campos nombrados.
Aquí se hace:
df_filtered.dropna(subset=['price_usd_per_m2','price_aprox_usd','rooms'])
Faltaría eliminar los valores nulos en la columna price (El punto dice precio y precio por metro cuadrado).

7) A continuación mostrá cuantas instancias son las que descartaste a partir de quitar las que tenían valores nulos.
Aquí se hace

df.shape[0] - df_filtered.shape[0]  
Sin embargo, df.shape[0] no representa la cantidad de instancias de df_filtered antes de "quitar las que tenían valores nulos", ya que df se filtró para obtener df_filtered
Aquí se debería tomar en cuenta la cantidad de instancias en df_filtered antes y después de quitar los valores faltantes: se podría modificar el código para mostrar el número de filas descartadas contando las filas antes y después de eliminar los nulos
df_filteredA.shape[0]-df_filteredB.shape[0] =>df_filteredA sería justo antes de hacer el dropna, y df_filteredB sería justo después de hacer el dropna

No tengo más comentarios para hacerte, ya que está muy bien

Saludos, Nelson

Sugerencias

Hay unas opciones/mejoras que se podrían tener en cuenta:

En el punto 4) se podría hacer así también:
df.loc[~(df.property_type=='store')]
df.loc[df.property_type!='store']

El punto 8) se podría hacer así también:

df_filtered.columns[df_filtered.isna().any()]  
Index(['floor', 'expenses'], dtype='object')  
df_filtered.isna().sum()[df_filtered.isna().sum()>0]  
floor       10112  
expenses     9703  
dtype: int64  
