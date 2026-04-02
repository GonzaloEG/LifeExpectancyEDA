# Análisis de Esperanza de Vida (2000-2015)
**Fuente:** Organización Mundial de la Salud (OMS) | **Dataset:** Kaggle - Life Expectancy WHO
**Autor:** Gonzalo Emmanuel Garcia (https://www.linkedin.com/in/gonzalo-garcia-b019a1397/)

## Descripción

Proyecto de análisis exploratorio y modelado predictivo sobre los factores que 
influyen en la esperanza de vida a nivel mundial, utilizando datos de 193 países 
entre 2000 y 2015.

El objetivo fue identificar qué variables sociosanitarias y económicas tienen 
mayor impacto en cuánto vive una población, y construir un modelo de regresión 
lineal capaz de predecirla.

## Preguntas que guían el análisis

- ¿Qué variables tienen mayor correlación con la esperanza de vida?
- ¿Existe una diferencia significativa entre países desarrollados y en desarrollo?
- ¿El gasto en salud realmente impacta en cuánto vive la gente?
- ¿Podemos predecir la esperanza de vida con un modelo lineal?

## Herramientas utilizadas

`Python` | `Pandas` | `NumPy` | `Matplotlib` | `Seaborn` | `Scikit-learn`

## Estructura del proyecto
```
├── Life Expectancy Data.csv   # Dataset original (Kaggle - WHO)
├── LifeExp.ipynb              # Notebook principal
└── README.md
```

## Proceso

### Limpieza de datos
- Eliminación de espacios en nombres de columnas
- Conversión de ceros a NaN en variables donde cero es un valor imposible (GDP, 
Population, Alcohol, BMI, entre otras)
- Imputación de valores faltantes con mediana para preservar los datos sin 
distorsionar la distribución

### EDA
Se analizaron las variables agrupadas por categorías: salud, mortalidad, 
enfermedades/vacunación y economía. Se construyeron rankings por país para 
identificar patrones globales y un heatmap de correlaciones filtrado a las 
variables del modelo.

### Modelo
Regresión lineal múltiple con las siguientes features:
- GDP, Schooling, BMI, HIV/AIDS, Adult Mortality, Thinness 1-19 years, 
Thinness 5-9 years

## Iteración del modelo — una decisión importante

La primera versión del modelo incluía *Income composition of resources*, 
una variable que resultó problemática: al ser un índice compuesto que incorpora 
métricas de salud directamente relacionadas con la esperanza de vida, estaba 
generando sesgo en los coeficientes e inflando el R² artificialmente.

Al detectarlo, decidí eliminarlo y rehacer el modelo con variables más atómicas 
e independientes. El R² bajó levemente (de 0.80 a 0.79), pero el modelo ganó 
en honestidad e interpretabilidad. El coeficiente de Schooling, por ejemplo, 
pasó de 0.39 a 1.38 — su peso real, que antes estaba siendo absorbido por 
el IDH.

## Resultados

| Métrica | Valor |
|--------|-------|
| MAE | 3.18 años |
| RMSE | 4.37 años |
| R² | 0.791 |
| R² promedio (CV 5-fold) | 0.769 ± 0.033 |

Un MAE de 3.18 años representa aproximadamente un 7% de error relativo sobre 
un rango de ~45 años — resultado sólido para un modelo lineal con 7 variables.

La validación cruzada de 5 folds confirmó que el modelo es estable y no depende 
del split particular de datos.

## Conclusiones principales

- El predictor de mayor peso fue **Schooling** (+1.38): cada año adicional de 
educación promedio se asocia a 1.38 años más de esperanza de vida.
- El **GDP** resultó casi irrelevante de forma directa (coef. +0.000054), 
sugiriendo que el dinero impacta a través de lo que financia — educación y salud — 
y no por sí solo.
- El **HIV/AIDS** fue el factor negativo más determinante (coef. -0.50), explicando 
en parte por qué países del África subsahariana concentran las esperanzas de vida 
más bajas del dataset.

## Limitaciones y próximos pasos

El modelo asume relaciones lineales y no captura interacciones complejas entre 
variables. Como próximo paso, explorar modelos como Random Forest o Gradient 
Boosting permitiría identificar relaciones no lineales y potencialmente mejorar 
la capacidad predictiva.
