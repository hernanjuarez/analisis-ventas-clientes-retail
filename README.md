# Análisis de ventas y comportamiento de clientes de una tienda online

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/hernanjuarez/analisis-ventas-clientes-retail/blob/main/notebooks/analisis-ventas-clientes-retail.ipynb)

Limpieza, análisis exploratorio y segmentación de clientes (RFM) con Python sobre más de 1 millón de transacciones reales de una tienda online británica, entre diciembre de 2009 y diciembre de 2011.

📓 **Notebook completo:** [analisis-ventas-clientes-retail.ipynb](notebooks/analisis-ventas-clientes-retail.ipynb)

## Preguntas de negocio

1. ¿Cómo evolucionó la facturación mes a mes y hay estacionalidad?
2. ¿Qué productos y países generan más ingresos?
3. ¿Quiénes son los mejores clientes y cuánto concentran?
4. ¿Cuántos clientes vuelven a comprar?

## Resultados principales

- **Estacionalidad marcada:** el pico va de septiembre a noviembre. Noviembre factura unos £1,4 millones, más del doble de un mes típico (£600.000-£700.000). Enero y febrero son los meses más bajos.
- **Crecimiento modesto:** noviembre de 2011 fue solo un 2% mayor que noviembre de 2010.
- **Ingresos repartidos entre productos y concentrados en un país:** el top 10 de productos suma cerca del 7% de los ingresos netos (£18,9 millones), y Reino Unido aporta cerca del 85%.
- **Pocos clientes sostienen el negocio:** el 20% de los clientes concentra el 76,9% de los ingresos, aunque ningún cliente individual domina (el mejor aporta cerca del 3,5%).
- **Alta recompra:** el 72,4% de los clientes compró más de una vez.
- **Mercados que dependen de pocos compradores:** Países Bajos tiene 22 clientes con unos £24.800 por cliente (unas 10 veces más que un cliente británico) e Irlanda factura £567.000 con solo 3 clientes. Los datos sugieren un perfil mayorista, pero es una hipótesis que no se puede confirmar con este dataset.

![Ingresos netos por mes](imagenes/ventas-mes.png)

*Diciembre de 2011 está incompleto (los datos llegan hasta el 9/12), por lo que la caída final no es real.*

## Segmentación de clientes (RFM)

Cada cliente se evaluó según hace cuánto compró (Recency), cuántas veces compró (Frequency) y cuánto gastó (Monetary). Se asignaron puntajes de 1 a 4 por cuartiles y se aplicaron reglas simples para armar los segmentos.

| Segmento | Clientes | % clientes | % ingresos |
|---|---|---|---|
| Campeones | 1.803 | 30,9% | 77,1% |
| En riesgo | 881 | 15,1% | 12,3% |
| Dormidos / perdidos | 2.033 | 34,8% | 6,3% |
| Clientes leales | 787 | 13,5% | 3,5% |
| Recientes (poca frecuencia) | 334 | 5,7% | 0,8% |

![Participación de cada segmento](imagenes/RFM.png)

**Acciones sugeridas:**

- Proteger a los Campeones con atención personalizada: un grupo del 31% de los clientes genera el 77% de los ingresos.
- Lanzar campañas de reactivación para el segmento En riesgo antes del pico de temporada (por ejemplo, en agosto), con contacto directo y descuentos por volumen si se confirma un perfil mayorista.
- Destinar solo campañas de bajo costo a los Dormidos, que son el 35% de los clientes pero aportan el 6% de los ingresos.

## Limpieza de datos

El dataset original tiene 1.067.371 filas. Estas son las decisiones tomadas y su justificación:

| Decisión | Filas afectadas | Justificación |
|---|---|---|
| Eliminar duplicados exactos | 34.335 | Inflarían ventas y cantidades |
| Separar códigos que no son productos (envíos, descuentos, ajustes, pruebas) | 5.980 | Son cargos administrativos, no ventas de productos |
| Separar filas con precio <= 0 | 5.928 | Ajustes de inventario sin cliente asociado |
| Conservar y marcar cancelaciones | 17.914 | Permiten medir ingresos netos |
| Conservar filas sin Customer ID | 226.965 | Sirven para el análisis de ventas; se excluyen solo del análisis de clientes |
| Marcar pedidos de volumen extremo | 13 | Pueden ser compras mayoristas reales; el análisis de sensibilidad mostró un impacto de solo 0,09% en los ingresos |

Quedaron **1.021.128 filas** para el análisis. Las filas separadas se guardan aparte, no se descartan.

## Limitaciones

- No hay datos de márgenes ni de costos, por lo que no se puede estimar la rentabilidad de las acciones propuestas.
- Cerca del 22% de las filas no tienen Customer ID y quedan fuera del análisis de clientes.
- Diciembre de 2011 está incompleto, por lo que no es comparable con otros meses.
- El perfil mayorista de algunos clientes es una hipótesis sugerida por los datos, no confirmada.
- Los segmentos RFM se definieron con reglas simples sobre cuartiles y pueden ajustarse.

## Estructura del repositorio

```
analisis-ventas-clientes-retail/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── muestra_retail.csv                      # muestra de 5.000 filas del dataset original
├── notebooks/
│   └── analisis-ventas-clientes-retail.ipynb   # análisis completo
└── imagenes/                                   # gráficos usados en este README
```

## Cómo reproducir el análisis

1. Descargá el dataset completo desde Kaggle: [LINK AL DATASET EN KAGGLE] (archivo `merged_dataset_retail_2009_2011.csv`). No se incluye en el repositorio por su tamaño (88 MB). Revisá la licencia en la página de origen.
2. Abrí el notebook en Google Colab con el botón de arriba, o en Jupyter.
3. Cambiá la variable `ruta` de la segunda celda por la ubicación del CSV en tu Drive o en tu equipo. El notebook original monta Google Drive; si lo corrés localmente, podés quitar esa parte.
4. Si lo corrés en tu equipo, instalá las dependencias con `pip install -r requirements.txt`.

## Tecnologías

Python, pandas, NumPy, Matplotlib, Seaborn y Google Colab.

## Próximos pasos posibles

- Segmentar clientes con KMeans y comparar con los segmentos por reglas.
- Pronosticar ventas mensuales considerando la estacionalidad.
- Armar un dashboard interactivo.

## Autor

[@hernanjuarez](https://github.com/hernanjuarez) · [(https://www.linkedin.com/in/hernanjuarez99/)]
