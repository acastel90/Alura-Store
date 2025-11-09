# Alura-Store
Análisis a Tiendas Store
Proyecto de Análisis de Ventas — Tiendas LATAM

Descripción general

Este proyecto tiene como propósito analizar y comparar el desempeño de cuatro tiendas online en Latinoamérica, a partir de bases de datos públicas alojadas en GitHub.
El objetivo principal es identificar la tienda más rentable y eficiente, considerando factores como:

1. Ingresos totales

2. Categorías de productos más vendidos

3. Calificaciones promedio de los clientes

4. Costos de envío promedio

5. Distribución geográfica de las ventas

6. El análisis integra herramientas de manipulación y visualización de datos en Python, utilizando bibliotecas como pandas, matplotlib y folium, y está diseñado para ejecutarse en Google Colab.

Objetivos específicos

1. Unificar los datos de las cuatro tiendas en un solo DataFrame.

2. Analizar los ingresos totales por tienda.

3. Calcular las calificaciones promedio de los clientes para cada tienda.

4. Identificar los productos más y menos vendidos.

5. Analizar los costos de envío promedio.

6. Evaluar la distribución geográfica de las ventas mediante un mapa de calor.

7. Recomendar una decisión estratégica sobre qué tienda mantener, vender o cerrar.

Estructura del proyecto

├── data/
│   ├── tienda_1.csv
│   ├── tienda_2.csv
│   ├── tienda_3.csv
│   └── tienda_4.csv
│
├── notebooks/
│   └── analisis_tiendas.ipynb
│
├── outputs/
│   ├── graficos/
│   ├── mapa_calor.html
│   └── informe_final.pdf
│
├── README.md
└── requirements.txt

Librerías Utilizadas

pandas
matplotlib
folium
numpy
plotly (opcional)

Uso del código

1. Importar las bibliotecas y cargar los datos

import pandas as pd

url1 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science-latam/main/base-de-datos-challenge1-latam/tienda_1%20.csv"
url2 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science-latam/main/base-de-datos-challenge1-latam/tienda_2.csv"
url3 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science-latam/main/base-de-datos-challenge1-latam/tienda_3.csv"
url4 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science-latam/main/base-de-datos-challenge1-latam/tienda_4.csv"

tienda1 = pd.read_csv(url1)
tienda2 = pd.read_csv(url2)
tienda3 = pd.read_csv(url3)
tienda4 = pd.read_csv(url4)

2. Unir todos los DataFrames

tiendas = pd.concat([tienda1, tienda2, tienda3, tienda4], ignore_index=True)


3. Análisis principales

a) Ingresos totales por tienda
tiendas['Ingreso'] = tiendas['Precio'] * tiendas['Cantidad']
ingresos = tiendas.groupby('Tienda')['Ingreso'].sum().reset_index()

b) Calificación promedio
calificaciones = tiendas.groupby('Tienda')['Calificación'].mean().reset_index()

c) Costo de envío promedio
envio = tiendas.groupby('Tienda')['Costo_envío'].mean().reset_index()

d) Categorías más vendidas
ventas_cat = tiendas.groupby(['Tienda','Categoría']).size().reset_index(name='Cantidad_Vendida')

e) Productos más y menos vendidos
ventas_prod = tiendas.groupby(['Tienda','Producto'])['Cantidad'].sum().reset_index()

4. Visualizaciones

Gráfico de ingresos por tienda

import matplotlib.pyplot as plt

plt.bar(ingresos['Tienda'], ingresos['Ingreso'], color='skyblue')
plt.title('Ingresos Totales por Tienda')
plt.xlabel('Tienda')
plt.ylabel('Ingreso Total ($)')
plt.show()

Mapa de calor de ventas

from folium.plugins import HeatMap
import folium

mapa = folium.Map(location=[tiendas['lat'].mean(), tiendas['lon'].mean()], zoom_start=5)
HeatMap(tiendas[['lat', 'lon', 'Precio']].values.tolist(), radius=8).add_to(mapa)
mapa.save('outputs/mapa_calor.html')

Resultados clave

Tienda 1: Mayor nivel de ingresos y cobertura geográfica amplia.

Tienda 2: Mercado concentrado, buena satisfacción del cliente, pero menor volumen.

Tienda 3: Buen equilibrio entre ventas y calificaciones.

Tienda 4: Menor número de ventas, bajo ingreso y alta dispersión geográfica.


Conclusión del análisis

Tras integrar los indicadores de desempeño —ingresos, calificaciones, costos de envío, y distribución de ventas— se concluye que:

- La Tienda 1 es la más rentable y con mejor potencial de crecimiento, por su combinación de ingresos altos, amplia cobertura y niveles aceptables de satisfacción del cliente.
- La Tienda 4 presenta los peores resultados y se recomienda su venta o cierre, debido a sus bajos ingresos, calificaciones promedio reducidas y limitada concentración geográfica.
   
