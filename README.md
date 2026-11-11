# Práctica XAI - PCD

Joaquin Negrete

Práctica de explicabilidad sobre un dataset de reservas de hotel. El objetivo es predecir si un cliente no se presenta a su reserva (`no_presentado = 1`) y explicar las predicciones.

## Contenido del notebook

[`practica.ipynb`](./practica.ipynb) sigue los tres apartados del enunciado:

1. **Selección de variables**: `SelectFromModel` con una regresión logística, usando la mediana de los coeficientes como umbral.
2. **Modelo interpretable**: regresión logística sobre las variables seleccionadas, interpretada con sus coeficientes y odds ratios.
3. **Modelo no interpretable y SHAP**: MLP con todas las variables, explicado de forma global (beeswarm) y local (force plots), con un análisis de la interacción de `llegada_fin_semana`.

El MLP y los valores SHAP se guardan en `artifacts/` para no recalcularlos en cada ejecución.

## Cómo empezar

Hace falta [uv](https://docs.astral.sh/uv/) y Python 3.13 o superior.

```bash
uv sync
uv run jupyter lab practica.ipynb
```

## Dataset

`data.csv` corresponde a `01_reservas_hotel.csv`.

Datos sintéticos para docencia, generados con NumPy y semilla 20261023. No representan un hotel ni clientes reales. CSV UTF-8, con coma como separador y punto decimal.

6.000 reservas, 480 ausencias (8 %). Objetivo: `no_presentado`, 1 = el cliente no se presenta, 0 = se presenta. La etiqueta se conoce después de la fecha de llegada, así que nunca se usa como predictor.

| Variable | Descripción |
|---|---|
| `id_reserva` | Identificador, se excluye de X. |
| `antelacion_dias` | Días entre la reserva y la llegada prevista. |
| `noches` | Noches reservadas. |
| `huespedes` | Número de huéspedes. |
| `precio_noche_eur` | Precio por noche en euros. |
| `reservas_previas` | Reservas anteriores del cliente en el hotel. |
| `ausencias_previas` | Indicador 0/1 de alguna ausencia en reservas anteriores; siempre 0 si no hay reservas anteriores. |
| `cambios_reserva` | Número de cambios realizados hasta el momento de la predicción. |
| `solicitudes_especiales` | Número de solicitudes registradas. |
| `deposito_pagado` | Indicador 0/1 de depósito abonado. |
| `llegada_fin_semana` | Indicador 0/1 de llegada prevista en fin de semana. |

Todos los predictores están disponibles al hacer la predicción, antes de la llegada. El conjunto incluye señales de riesgo solapadas, reservas sin ausencia con comportamiento inusual y atributos sin relación con el objetivo.

Se reserva el 25 % para test (`random_state=42`), estratificando por `y`.
