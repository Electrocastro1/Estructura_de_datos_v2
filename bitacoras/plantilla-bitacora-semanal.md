# Bitacora individual - Semana 04



## 1. Datos de la actividad

- **Estudiante:** Julian Felipe Dávila Cortés
- **Semana:** 4
- **Fecha del laboratorio:** 2026-09-28
- **Fecha del taller:** 2026-10-05
- **Tema principal:** Ordenamientos y comparación de eficiencia
- **Pregunta de la semana:** ¿Cuánto cuesta el orden y qué consecuencias tiene aplicarlo sobre un sistema de búsqueda existente?

## 2. Prediccion antes de ejecutar

Antes de abrir o ejecutar el programa, responde:

1. **Que creo que va a ocurrir?**
   Creo que el método QuickSort con el pivote en el primer elemento va a colapsar cuando reciba los datos reales de la red de sensores, porque estos llegan ya ordenados cronológicamente, lo que forzará al algoritmo a su peor caso de partición.
2. **Que parte del programa o del algoritmo puede fallar?**
   El método `quickRecursivo` y `particionar`, ya que entrarán en una recursión demasiado profunda al dejar un sub-arreglo vacío y el otro con n-1 elementos repetidamente.
3. **Como comprobare mi prediccion?**
   Ejecutando el "Experimento 4" (Caso B) que inyecta 50.000 lecturas ordenadas y observando si la terminal arroja un error de desbordamiento de memoria (`StackOverflowError`).

## 3. Evidencia del laboratorio

### Resultado observado

Al ejecutar el Experimento 4 con datos ordenados por timestamp, el programa colapsó exactamente con un `StackOverflowError`. Además, en el Experimento 2, al probar Bubble Sort con 10.000 datos ordenados, el algoritmo original realizó casi 50 millones de comparaciones innecesarias. Al implementar la bandera de corte temprano, estas bajaron drásticamente a solo 9.999.

### Diferencia entre la prediccion y el resultado

La predicción de QuickSort fue exacta gracias a la teoría sobre la selección del pivote. Con Burbuja, aunque sabía que haría trabajo extra, me sorprendió la magnitud del impacto (pasar de 50 millones a 10 mil operaciones) que puede tener una simple variable booleana de control.

### Error o comportamiento inesperado

- **Que ocurrio?** `StackOverflowError` en la recursión de QuickSort con datos reales.
- **Por que ocurrio?** Porque la elección del pivote en la posición inicial genera particiones completamente desbalanceadas cuando el arreglo ya viene ordenado de fábrica.
- **Como lo corregimos o que falta corregir?** Lo corregí generando un índice aleatorio con `Math.random()` e intercambiándolo con el primer elemento justo antes de empezar el proceso de partición.

## 4. Explicacion en lenguaje llano

Explica el concepto principal como se lo explicarias a una persona de doce
anos. Usa entre tres y cinco lineas y evita palabras tecnicas que no expliques.

> Ordenar información en el computador no es un proceso mágico; cuesta tiempo y esfuerzo (comparar valores y moverlos de sitio). Algunos métodos revisan los datos uno por uno y son muy lentos si hay demasiada información, mientras que otros algoritmos "avanzados" parten el problema gigante en pedacitos pequeños, los ordenan por separado y luego los juntan, haciendo el trabajo muchísimo más rápido.

### Ejemplo o analogia

Imagina que te dan mil cartas de Pokémon revueltas y te piden ordenarlas. Si buscas la menor y la pones al principio, te vas a demorar horas (Selección). Pero si las divides en piloncitos de 10 cartas, ordenas cada piloncito rápido, y luego unes los montones ya ordenados, terminarás mucho antes. Esto último es lo que hace un algoritmo como MergeSort. La analogía deja de ser exacta porque en el computador "mover" una carta puede ser más pesado que simplemente "mirarla", algo que no notamos tanto con las manos.

## 5. El vacio que encontre

Al intentar explicar el tema, identifica el punto que aun no comprendes bien.

- **Mi duda concreta es:** ¿Por qué la búsqueda binaria falla después de hacer el ranking de PM2.5, si la lectura sigue existiendo dentro del arreglo?
- **Lo que ya puedo explicar es:** Puedo explicar que ordenar por PM2.5 altera las posiciones de los elementos.
- **Para resolver la duda consulte:** El documento de la Guía de Explicaciones (Sección 10: El efecto colateral del ordenamiento) y la ejecución del Experimento 5.
- **Ahora lo entiendo asi:** La búsqueda binaria no busca "a ciegas", requiere una regla estricta: que los datos estén ordenados por el *mismo* criterio que estoy consultando (timestamp). Si ordeno por partículas de aire (PM2.5), las fechas quedan en desorden y el buscador binario se pierde al intentar adivinar si debe ir a la izquierda o a la derecha.

## 6. Trazado de la solucion

Trazado del algoritmo de Burbuja con corte temprano (TODO 1) sobre un arreglo ya ordenado: `[1, 2, 3]`.

| Paso | Estado de los datos o estructura | Decision o resultado |
|---|---|---|
| 1 | `[1, 2, 3]` | Inicia pasada 1. `huboIntercambio = false`. |
| 2 | `[1, 2, 3]` | Compara 1 y 2. 1 no es mayor, no hay intercambio. |
| 3 | `[1, 2, 3]` | Compara 2 y 3. 2 no es mayor, no hay intercambio. |
| 4 | `[1, 2, 3]` | Termina ciclo interno. `huboIntercambio` sigue siendo `false`. Se ejecuta el `break` (corte temprano) y termina el algoritmo sin hacer una segunda pasada. |

## 7. Decision de diseño

Relaciona lo aprendido con la Plataforma de Monitoreo Ambiental Urbano.

- **Problema que debiamos resolver:** El colapso del algoritmo QuickSort al intentar organizar las lecturas reales de los sensores que llegan inherentemente en orden cronológico.
- **Estructura, algoritmo o estrategia elegida:** QuickSort modificado con selección de pivote aleatorio.
- **Alternativa descartada:** Implementar QuickSort con mediana de 3 o usar solo MergeSort.
- **Por que elegimos la primera:** El pivote aleatorio se implementa de manera muy sencilla (solo dos líneas de código) y es estadísticamente suficiente para evitar el peor caso que genera el StackOverflow, sin requerir la memoria adicional (O(n)) que demanda MergeSort.
- **Que evidencia respalda la decision:** La ejecución del Experimento 4, donde el Caso B logró procesar exitosamente las 50.000 lecturas cronológicas tras incluir el pivote aleatorio, reemplazando el colapso anterior por un tiempo de ejecución de pocos milisegundos.

## 8. Aporte al proyecto

- **Archivo(s) o modulo(s) trabajado(s):** `Ordenador.java`, `BancoDeOrdenamiento.java`, `IngestaSensores.java`, `docs/decisiones.md`.
- **Cambio realizado:** Programación de seis algoritmos de ordenamiento, instrumentación de contadores de costo, y mitigación de problemas de borde en Burbuja y QuickSort.
- **Como se conecta con la capa anterior:** El módulo `Ordenador` recibe los arreglos de tipo `LecturaSensor` creados en las semanas 2 y 3, permitiendo ahora ordenarlos para crear rankings sin romper la clase principal `IngestaSensores`.
- **Que queda pendiente para la siguiente semana:** Implementar una solución definitiva (como mantener índices separados o usar copias) para poder buscar por timestamp y ordenar por PM2.5 simultáneamente sin perder la precondición de la búsqueda binaria.

## 9. Commits realizados

Registra los commits que muestran tu aporte individual.

| Commit | Mensaje | Que demuestra |
|---|---|---|
| `e8a2b4c` | `feat: implementar ordenamientos simples y avanzados` | La creación base de los seis métodos en Ordenador.java |
| `f5d93a1` | `fix: agregar pivote aleatorio a QuickSort y corte a burbuja` | La solución técnica a los TODO 1 y TODO 2 exigidos por el reto. |
| `b72cc94` | `docs: registro de DEC-04, DEC-05 y actualización de bitácora` | Documentación de las justificaciones arquitectónicas del efecto colateral. |

## 10. Reexplicacion final

Despues del taller, vuelve a responder la pregunta de la semana en cinco lineas
o menos. Esta respuesta debe ser mas precisa que la de la seccion 4 y debe
incluir la razon de tu decision tecnica.

> El costo del orden depende enteramente del estado inicial de los datos y de la complejidad del algoritmo; métodos como Inserción brillan con datos casi ordenados, mientras MergeSort asegura rendimiento predecible en volúmenes masivos. Sin embargo, la mayor consecuencia de ordenar una colección in-place es el "efecto colateral": alterar el índice natural (como la fecha) destruye la precondición requerida por algoritmos rápidos como la búsqueda binaria, forzándonos a decidir si asumimos el costo de crear copias en memoria o el de reconstruir el orden constantemente.

## 11. Reflexion individual

Responde con honestidad:

1. **Lo que ahora puedo hacer y antes no podia:**
   Entender por qué Arrays.sort() en Java (u otras librerías) usa heurísticas complejas por debajo en lugar de un solo algoritmo básico, y puedo medir el costo real de intercambiar objetos versus simplemente compararlos.
2. **El error o supuesto que mas me enseno:**
   Asumir que si un algoritmo es clasificado matemáticamente como "el más rápido en promedio" (QuickSort O(n log n)), nunca iba a fallar, hasta que vi cómo un patrón normal en la vida real (datos en orden cronológico) lo hizo colapsar.
3. **La pregunta que llevaria a la proxima clase:**
   ¿Cómo manejan las bases de datos gigantes en la vida real el problema del ordenamiento múltiple sin tener que crear copias enteras de tablas de 50 millones de registros?
4. **Que parte del trabajo fue realmente mia:**
   La instrumentación de los contadores en las modificaciones de código, el análisis comparativo de la gráfica entre métodos cuadráticos y avanzados, y la redacción reflexiva de esta bitácora y el archivo de decisiones técnicas.

## Lista de verificacion antes de entregar

- [x] Escribi la prediccion antes de consultar el resultado.
- [x] Inclui evidencia concreta del laboratorio.
- [x] Explique un concepto sin depender de jerga.
- [x] Registre un vacio, una duda o un error real.
- [x] Trace al menos un caso paso a paso.
- [x] Justifique una decision del proyecto y una alternativa descartada.
- [x] Registre mis commits y mi aporte individual.
- [x] Deje claro que queda pendiente.
- [x] Renombre el archivo con el formato `sXX-nombre.md`.