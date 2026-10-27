# Fionn

### Trading algorítmico, programación y decisiones bajo incertidumbre

Fionn es un Expert Advisor desarrollado en **MQL4 para MetaTrader 4**. Este repositorio comparte el código fuente de la **versión 5.30** para facilitar su lectura, análisis y experimentación.

El proyecto forma parte de mi recorrido como desarrollador y neuropsicólogo: dos áreas desde las que me interesa explorar cómo observamos información, formulamos reglas y respondemos cuando los resultados contradicen nuestras expectativas.

## El proyecto

Programar una estrategia obliga a expresar cada decisión de forma concreta:

- ¿Qué información se considera relevante?
- ¿Qué condición activa una entrada?
- ¿Cómo cambia la exposición cuando el precio se mueve en contra?
- ¿Cuándo se intenta cerrar una operación?

Fionn permite examinar esas decisiones directamente en el código.

Es un sistema basado en reglas. No incorpora aprendizaje automático ni un modelo neuropsicológico. La conexión con la neuropsicología está en las preguntas sobre conducta, incertidumbre y toma de decisiones que acompañan el desarrollo.

## Funcionamiento

### Señales de entrada

La lógica compara la ubicación temporal de máximos y mínimos dentro de ventanas recientes de velas.

A partir de esas posiciones calcula una diferencia y busca cruces de umbral para generar señales de compra o venta.

En esta versión, la longitud de esas ventanas está vinculada a la cantidad de decimales del instrumento (`DigitosPrecio`). Es una particularidad de la implementación que debe tenerse en cuenta al analizarla.

### Entradas adicionales

Cuando el precio se desplaza en contra de una posición, el sistema puede agregar operaciones del mismo lado a una distancia configurable.

El lotaje de cada entrada adicional se calcula a partir del lote anterior y de un multiplicador. Su valor predeterminado es **1.05**.

El redondeo aplicado y las restricciones del bróker pueden hacer que ese incremento no se traduzca en un aumento efectivo en cada entrada.

### Gestión de operaciones

El código incluye:

- Stop loss y take profit configurables.
- Trailing stop bajo determinadas condiciones.
- Una rutina para calcular un objetivo de salida conjunto.
- Comprobaciones de margen disponible antes de enviar órdenes.
- Parámetros de horario, cantidad de órdenes e instrumentos simultáneos.

Estos mecanismos no constituyen una garantía de control de pérdidas.

### Visualización

Incluye un panel operativo y elementos gráficos:

- Niveles de soporte y resistencia.
- Canal de máximos y mínimos.
- Referencia visual de una media móvil.
- Marcas de señales.
- Información sobre órdenes y resultados flotantes.

Los soportes, resistencias, canales y la media móvil no intervienen en las condiciones de entrada de esta versión.

## Parámetros principales

Valores incluidos en el código:

| Parámetro | Valor predeterminado | Función |
|---|---:|---|
| `LoteFijo` | `0.01` | Lote inicial |
| `MultiplicadorLote` | `1.05` | Factor aplicado al lote anterior |
| `DistanciaPips` | `15.0` | Distancia configurada entre entradas adicionales |
| `TakeProfitPips` | `30.0` | Distancia configurada del objetivo inicial |
| `StopLossPips` | `400` | Distancia configurada del stop inicial |
| `MaxOrdenesPorLado` | `50` | Máximo configurado de órdenes por dirección y símbolo |
| `MaxParesSimultaneos` | `2` | Límite previsto de instrumentos simultáneos |
| `ActivarTrailing` | `true` | Activa la lógica de trailing |
| `TrailingStopPips` | `12.0` | Distancia configurada del trailing |
| `PasoTrailingPips` | `1` | Paso configurado del trailing |
| `NumeroMagico` | `9999` | Identificador de órdenes del EA |

Estos valores describen la configuración del archivo. No son recomendaciones de operación.

La conversión de las distancias a precios debe revisarse para cada instrumento y entorno de ejecución.

## Instalación para pruebas

1. Abrir MetaTrader 4.
2. Seleccionar **Archivo → Abrir carpeta de datos**.
3. Copiar `Fionn.mq4` dentro de `MQL4/Experts`.
4. Abrir el archivo en MetaEditor.
5. Compilar y revisar los mensajes del compilador.
6. Actualizar la lista de Expert Advisors en MetaTrader 4.
7. Cargar el EA en un gráfico de una cuenta demo.
8. Revisar los parámetros, los nombres de los símbolos y los permisos de trading automatizado.

El código contempla sufijos de símbolos tomando como referencia el nombre del instrumento del gráfico. Esa lógica debe comprobarse si el bróker utiliza nombres diferentes.

No ejecutar varias instancias con el mismo número mágico sin revisar cómo compartirían la gestión de órdenes.

## Riesgo de la estrategia

Fionn incorpora entradas tipo grid con progresión configurable de lotaje.

Agregar operaciones cuando el mercado se mueve en contra aumenta la exposición. Una tendencia persistente puede acumular pérdidas importantes, consumir el margen disponible y provocar la pérdida del capital destinado a la cuenta.

Un multiplicador pequeño no elimina ese riesgo. Operar varios pares tampoco garantiza diversificación: los instrumentos pueden estar correlacionados.

## Estado y limitaciones conocidas

Esta publicación comparte el código para revisión. No incluye una validación independiente de rendimiento ni certifica su funcionamiento en todos los brókeres.

Entre los puntos pendientes de revisión se encuentran:

- El cálculo del objetivo de salida conjunto y su conversión entre dinero y precio.
- La actualización del conteo de símbolos ocupados: se calcula al inicio del tick y puede quedar desactualizado si se abren operaciones durante el recorrido.
- El uso de la precisión decimal del instrumento como longitud de las ventanas de señal.
- La consistencia de las conversiones entre puntos, ticks y pips.
- El tratamiento de errores en algunas llamadas de envío y modificación de órdenes.
- La selección de la última operación, que depende del orden de recorrido de las órdenes.

El filtro horario se aplica a las entradas iniciales. Las entradas adicionales del grid no están condicionadas por ese filtro.

La versión compartida requiere compilación, pruebas históricas y observación en demo antes de considerar cualquier otro uso.

## Resultados

Este repositorio no incluye un historial auditado ni presenta cifras de rentabilidad.

El código fuente permite estudiar la implementación; por sí solo no demuestra una ventaja estadística ni resultados futuros.

## Autor

Desarrollado por **Emanuel**, neuropsicólogo y desarrollador del proyecto **WineBOX**.

- https://winebox.cloud
- https://wineboxtool.cloud

## Intercambio técnico

Son bienvenidos los comentarios, los reportes de errores y las propuestas de mejora mediante Issues y Pull Requests.

Para reportar un problema, incluir la versión de MetaTrader 4, el instrumento, los parámetros utilizados y los mensajes relevantes del registro, sin datos personales ni credenciales.

## Aviso

Material técnico y educativo. No constituye asesoramiento financiero ni una promesa de resultados.
