# Acamica-Data-Science
Archivos de la carrera de data science presencial de Acámica, entre septiembre de 2019 y marzo de 2020. A continuación presento los comentarios a las entregas que hice durante la carrera (disponibles en la carpeta "Entregas").

------------------------------------------------------------------------------------------------------------------

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

-----------------------------------------------------------------------------------------------------------

Devolución del mentor de Acámica a la segunda entrega:

Comentario general

¡Felicitaciones Francisco, tu segunda entrega está aprobada!
Estuve revisando tu código y realmente está todo muy bien. Se nota que aprendiste todos los conceptos necesarios para hacer la transformación de los datos. Cumpliste con todas las consignas del checklist y tu solución funcionó correctamente. Hay sólo unos detalles a tener en cuenta:

12) Aplicar OneHotEncoder sobre las variables categóricas para crear un dataframe categoricals_df
*Si bien no está mal que el dataframe categorical_df quedé como float, esta parte podría mejorarse al crear el dataframe como Int64 y 2- definiendo los nombres de las columnas:
categoricals_df = pd.DataFrame(X, columns=le.classes_, dtype='int64')

No tengo más comentarios para hacerte, ya que está muy bien.

Saludos, Nelson

Sugerencias

Con respecto a tu pregunta del punto 7), NA groups in GroupBy are automatically excluded en Python, por lo cual, no parece ser alcanzable así. Sin embargo, hay una serie de opciones en este link:
https://stackoverflow.com/questions/18429491/groupby-columns-with-nan-missing-values

Hay unas mejoras/opciones que podrían tenerse en cuenta:
El punto 4) se podría resolver así también:
iqr = df.price_usd_per_m2.quantile(0.75) - df.price_usd_per_m2.quantile(0.25)
min = q1_price_usd_per_m2 - (iqr*1.5)
max = q3_price_usd_per_m2 + (iqr*1.5)
df_filtered = df[df.price_usd_per_m2.between(min,max)]

El Punto 7) se podría resolver así también:
df.isna().mean().sort_values(ascending=False)*100

floor                      84.77  
expenses                   73.66  
rooms                      17.56  
surface_covered_in_m2       3.78  
barrio                      0.00  
                           ...    
lat                         0.00  
place_with_parent_names     0.00  
place_name                  0.00  
property_type               0.00  
created_on                  0.00  
Length: 15, dtype: float64  
El Punto 8) se podría resolver así también:
imp_mean = SimpleImputer(strategy='mean', missing_values=np.nan)
df[['surface_total_in_m2','surface_covered_in_m2']] = imp.fit_transform(df[['surface_total_in_m2','surface_covered_in_m2']])

---------------------------------------------------------------------------------------------------------------------
Devolución de la mentora de Acámica a la tercera entrega:

¡Felicitaciones tu entrega está aprobada!
Entendiste como usar estos primeros algoritmos de regresión. Está todo muy prolijo y se entiende perfectamente el código de cada sección.

Cumpliste con todas las consignas del checklist y la solución propuesta funcionó correctamente.

Al hacer split podes agregar random_state. Esto es importante porque hace que tu experimentación sea reproducible, es decir que no cambie cada vez que lo ejecutás. Permite comparar distintas pruebas de forma más fácil.

Sugerencias

Te sugiero que sigas investigando sobre otros algoritmos de regresión para compararlos con los que ya conocés. Podés empezar estudiando los otros que existen en scikit-learn.

Acá te dejo el link a la documentación donde figuran otros modelos que podés probar: link

También podes leer el libro Python Machine Learning. Es un excelente libro para complementar el contenido de las clases.

La entrega está muy bien. Cualquier duda podes consultar por el canal de slack.

Saludos

Daniela

-----------------------------------------------------------------------------
Comentarios del mentor de Acámica a la cuarta entrega

¡Felicitaciones Francisco, tu entrega está aprobada!
Estuve revisando tu código y esta bien. Se nota que aprendiste todos los conceptos necesarios para hacer la Optimización de Parámetros. Cumpliste con todas las consignas del checklist y tu solución funciona correctamente en casi todos los puntos. Hay unos detalles a tener en cuenta:

Checklist
Item	Cumple
Separación del dataset en train y test : El dataset se separa correctamente dejando un 80% para train y otro 20% para test.	Si
param_grid: Se crea variable param_grid para los atributos max_depth y max_features	Si
GridSearch: Se crea una variable GridSearch y se hace el fit recorriendo el param_grid con el algoritmo DecisionTreeRegresor .	Si
Scores y mejores parámetros: Se muestran los scores y los mejores parámetros obtenidos con GridSearch.	Si (Ver notas)
Mejor modelo: Se busca el mejor modelo para la grilla de parámetros dada	Si
Evaluación: Se evalúa en el conjunto de test al mejor modelos	Si (Ver notas)
Comentarios Generales
Convertimos a RMSE.

Aquí faltó aplicar la función nmsq2rmse al mejor score obtenido en el primer GridSearchCV (gridsearch.best_score ) . Esto te servirá para luego comparar el valor de estos parámetros con el GridSearchCV con un espacio de búsqueda mayor del siguiente
De haber obtenido el RMSE del primer GridSearchCV, se hubiese comparado con el RMSE del segundo GridSearchCV el cual tuvo un error menor: es por ello que es importante ir probando distintas configuraciones de parámetros para encontrar la que mejor se adapta a nuestros datos.
Evaluemos en testing el desempeño de este modelo.
Aquí se hace:
optimised_decision_tree = grid_search.best_estimator_
y_opt_pred = optimised_decision_tree.predict(X_test)
Y se debería haber usado el mejor estimador de grid_search_r2, del segundo modelo, no del primero, es decir:
optimised_decision_tree = grid_search_r2.best_estimator_

Con respecto a tu comentario sobre los parámetros obtenidos, no hay problema, simplemente se usó una semilla distinta np.random.seed(xxx) y por lo tanto se obtuvieron resultados distintos, ;)
No tengo más comentarios para hacerte, ya que está bien
