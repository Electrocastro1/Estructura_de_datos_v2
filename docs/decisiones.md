# Decisiones de diseño — Semana 3

## 1. Punto de entrada

El proyecto mantiene un único punto de entrada:

```text
IngestaSensores.main()
```

No se crean aplicaciones independientes por semana.

`BancoDePruebas` es una clase auxiliar y no contiene `main`.

## 2. Búsqueda por timestamp

Se utilizan dos estrategias:

- Búsqueda lineal: no requiere ordenamiento.
- Búsqueda binaria: requiere que el arreglo esté ordenado por timestamp.

Los datos sintéticos de `GeneradorDatos` se generan en orden cronológico, por lo que la búsqueda binaria por timestamp cumple su precondición.

## 3. Búsqueda por PM2.5

No se asume que los datos estén ordenados por PM2.5.

Por tanto, la búsqueda binaria por PM2.5 se conserva como experimento para demostrar el efecto de una precondición incumplida.

## 4. Comparación de String

Los identificadores de estación se comparan mediante:

```java
equals();
```

y no mediante:

```java
==
```

porque se necesita comparar contenido.

## 5. Medición

La comparación principal entre algoritmos utiliza el número de comparaciones.

El tiempo en milisegundos se conserva como evidencia experimental, pero no es la única medida utilizada.

## 6. Evolución del proyecto

La Semana 3 agrega una nueva capacidad a la misma plataforma:

```text
Sensores
   ↓
Ingesta
   ↓
Repositorio
   ↓
Búsqueda
   ↓
Medición de eficiencia
```

La Semana 4 podrá extender esta misma arquitectura para estudiar ordenamiento.

## DEC-04 — Pivote de QuickSort

**Semana:** 4

**Problema:** QuickSort con pivote fijo en el primer elemento produce particiones desbalanceadas cuando los datos ya están ordenados cronológicamente, causando un desbordamiento de memoria (StackOverflowError)[cite: 1, 2, 3].

**Alternativas:**
- Pivote aleatorio.
- Mediana de tres.

**Decisión:** Implementar pivote aleatorio.

**Justificación:** Al elegir una posición al azar antes de particionar, se evita caer en el peor caso sistemático que ocurre cuando las lecturas de los sensores llegan en orden de fecha[cite: 2, 3].

**Consecuencia:** El algoritmo ahora procesa eficientemente 50.000 lecturas cronológicas sin romper la ejecución[cite: 3].

## DEC-05 — Ordenamiento y búsqueda

**Semana:** 4

**Problema:** Al ordenar los datos por niveles de PM2.5 para crear el ranking, el arreglo pierde su ordenamiento cronológico (timestamp), lo que destruye la precondición necesaria para que la búsqueda binaria funcione[cite: 1, 2, 3].

**Alternativas:**
- Trabajar sobre una copia del arreglo[cite: 2].
- Restaurar el orden original después de generar el ranking[cite: 2].
- Mantener índices separados[cite: 2].

**Decisión:** [Elige una opción, por ejemplo: Trabajar sobre una copia].

**Justificación:** [Escribe por qué, por ejemplo: Consumir un poco de memoria extra copiando el arreglo es preferible a perder el orden original que requieren las demás operaciones de consulta].