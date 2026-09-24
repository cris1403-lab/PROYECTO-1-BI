# Proyecto 1 — Business Intelligence

Análisis del dataset "The Complete Journey" (dunnhumby) para responder dónde debería
ir el presupuesto de marketing del próximo año, y por qué.

## Qué hicimos

Trabajamos con las 7 tablas del dataset (transacciones, productos, demografía,
campañas, cupones) para construir una tabla analítica reconciliada, y con eso
respondimos los 5 análisis obligatorios: concentración del gasto, mix de categorías,
costo del descuento, efecto de campañas, y cobertura de demografía.

El hallazgo principal está en el análisis de campañas: los hogares que reciben
campaña ya gastaban en promedio 5 veces más que el resto *antes* del corte usado
(día 398, mediana de inicio de campaña por hogar), y solo el 27.4% de los cupones
enviados se redime. También encontramos que el 98% de los hogares del top 20% de
gasto ya recibe campaña, y que los hogares con datos demográficos gastan 168% más
que el resto, por lo que ese 32% de cobertura no es una muestra representativa.
Con esa evidencia, elegimos la palanca de **Mecánica** (C) como recomendación:
congelar la expansión de campañas y cupones dirigidos, y usar los $50K hoy
destinados a cupones en un piloto con grupo de control en hogares de gasto medio,
en vez de concentrarnos en un tipo de hogar (A) o una categoría de producto (B).

La recomendación completa, con la evidencia detallada, está en la última sección
del notebook.

## Cómo correrlo

1. Cloná el repositorio.
2. Descargá los CSV del dataset desde el link de Google Drive que da el enunciado
   (o desde `dunnhumby.com/source-files/`), y colocalos dentro de una carpeta
   `data/` en la raíz del proyecto. Esa carpeta no viaja en el repo (ver
   `.gitignore`) porque los datos no son nuestros para redistribuir.
3. Instalá el entorno con `uv`:
   ```
   uv sync
   ```
4. Abrí el notebook:
   ```
   uv run jupyter notebook
   ```
5. Abrí `proyecto1_Pineda_Menegazzo.ipynb` y corré **Kernel → Restart Kernel and
   Run All Cells**. Corre completo de principio a fin sin errores, en orden.

## Quién hizo qué

- **Pineda:** setup del proyecto (uv, `.gitignore`), Etapa 1 completa (perfilado
  de las 7 tablas, los tres hallazgos de calidad de datos), y el arranque de la
  Etapa 2 (merge de `transaction_data` con `product` y `hh_demographic`,
  reconciliación).
- **Menegazzo:** resto de la Etapa 2 (limpieza y merge de campañas, columnas de
  descuento), Etapa 3 completa (los 5 gráficos con pregunta/respuesta), y Etapa 4
  (análisis de campañas, comparación de mecánicas, recomendación final).
- **Ambas:** armado y revisión de la presentación final (8 diapositivas), incluida
  la corrección de la diapositiva 1, del Análisis 5, y de las afirmaciones que no
  se sostenían con los datos (ver "Uso de LLMs" abajo).

## Uso de LLMs

Usamos Claude (Anthropic) durante todo el proyecto, principalmente en modo guía:
le pedíamos ayuda para entender qué hacía cada bloque de código de pandas (merges,
`groupby`, `validate='m:1'`, la función `reconciliar()`) y por qué convenía hacerlo
de esa forma, en vez de que nos generara el análisis completo de una sola vez.

También lo usamos para pensar en voz alta sobre las decisiones del proyecto: nos
hizo repreguntas sobre por qué un crecimiento de categoría podía ser ruido de una
base muy chica, lo que nos llevó a agregar el filtro de volumen mínimo en el
Análisis 2, y sobre qué evidencia respaldaba mejor cada una de las tres palancas
antes de que eligiéramos la Mecánica como recomendación final.

El código que terminó en el notebook lo escribimos y decidimos nosotras en cada
paso. Claude explicaba y sugería, pero revisamos cada resultado antes de darlo por
bueno, como cuando detectamos y corregimos a mano un departamento con nombre en
blanco que estaba inflando un gráfico a 100% de descuento.

Para la presentación final, además del apoyo en el notebook, usamos Claude para
armar las 8 diapositivas a partir de los resultados ya calculados. Los gráficos de
la presentación se generaron con el mismo código de `matplotlib` que usamos en el
notebook, con `plt.savefig()`, no con capturas de pantalla. Le pedimos que
evaluara la presentación contra la rúbrica del proyecto y contra la de otra pareja
de compañeros, a modo de control de calidad, lo cual nos hizo corregir varias
cosas antes de la entrega: la diapositiva 1 pasó de una acción vaga a una decisión
de presupuesto concreta y cuantificada, corregimos una etiqueta mal puesta (el
27.4% de redención de cupones), completamos el Análisis 5, que nos había quedado
incompleto, y quitamos una afirmación que no se sostenía con los datos. Decíamos
que los hogares gastaban más "antes de que la campaña existiera", pero el corte
usado (día 398) es una mediana, así que para la mitad de los hogares con campaña
esa frase era técnicamente falsa, y lo corregimos a "antes del día 398".
