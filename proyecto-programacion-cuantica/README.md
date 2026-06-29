# proyecto final QMexico
### Dataset

Nombre del dataset:
Asignacion optima de estrategias de movilidad en vialidades principales de la CDMX

Fuente oficial o confiable:
Diversas fuentes oficiales y literatura científica sobre movilidad urbana, incluyendo portales de datos abiertos del Gobierno de la Ciudad de México, SEMOVI, INEGI y artículos científicos especializados.

Institución responsable:
Secretaría de Movilidad de la Ciudad de México (SEMOVI); Gobierno de la Ciudad de México; Instituto Nacional de Estadística y Geografía (INEGI); Secretaría de Infraestructura, Comunicaciones y Transportes (SICT)


URL de la fuente: 
modo lista
XXXXX

URL raw del CSV usado en data/:
Dataset construido por los autores del proyecto.

Licencia o condiciones de uso: 
El dataset fue construido a partir de fuentes públicas y de acceso abierto. Cada conjunto de datos conserva la licencia establecida por su institución de origen y se utiliza exclusivamente con fines académicos, citando las fuentes correspondientes.

Fecha de consulta:
28/06/26

Dominio del problema:
Optimización de la movilidad urbana mediante asignación de estrategias de gestión del tránsito utilizando QUBO y QAOA.

### Modelado
Conjunto A:
Vialidades principales de la CDMX (Insurgentes Sur, Anillo Periférico Sur,  Calzada de Tlalpan, Circuito Interior)

Criterio para elegir exactamente 4 elementos de A: 
Se seleccionaron 4 corredores viales de alta importancia con respecto a la movilidad de la ciudad, que tiene informacion publica y fiable sobre aforos vehiculares y donde resulta tecnicamente viable implementar estrategias de gestion de transito

Conjunto B: 
Estrategias de mejora de la movilidad urbana. (Carril reversible, Semaforización inteligente, Gestión dinámica del tránsito, Mantenimiento prioritario de la vialidad )

Criterio para elegir exactamente 4 elementos de B: 
Se seleccionaron cuatro estrategias ampliamente utilizadas en ingeniería de tránsito, implementables en vialidades urbanas.

Definición de x_ij = 1:
La estrategia denotada por j se asigna a la vialidad denotada por i, es un emparejamiento.

Interpretación de x_ij = 0: 
La estrategia de movilidad j no es asignada a la vialidad i, no se emparejan.

### Matriz de score
Columnas usadas: 
Para el conjunto A se coloco nombre, flujo vehicular/dia minimo, flujo vehicular/dia  maximo y flujo relativo.

Fórmula exacta de S_ij :  
S_ij =F_i^{rela}/ I_j .
donde F_i^{norm}=\frac{F_i-F_{i,min}}{F_{i,max}-F_{i,min}} 
e I_j corresponde al índice promedio de efectividad de la estrategia j obtenido a partir de la matriz de beneficios observada S^{obs}.

Normalización aplicada:
Normalización Min-Max individual para cada vialidad utilizando el rango del numero de vehiculos que pasan por la vialidad al dia promedio o representativo de flujo vehicular: F_i^{norm}=\frac{F_i-F_{i,min}}{F_{i,max}-F_{i,min}}

Esta normalización genera un índice entre 0 y 1 que representa la demanda relativa de cada vialidad respecto a su propio comportamiento histórico.

Matriz S 4x4:
El dataset fue construido por integración de información proveniente de diversas fuentes oficiales y literatura técnica. Los valores correspondientes a la matriz de beneficios S_obs constituyen una estimación semirreal obtenida mediante el análisis conjunto de dichas fuentes, ya que no existe una base de datos pública que relacione directamente cada estrategia de movilidad con cada vialidad seleccionada.

### Restricciones
Restricción por filas:
Cada vialidad puede recibir como máximo una estrategia de movilidad.
\sum_{j=1}^{|B|} x_{ij}\leq 1,\qquad \forall i\in A

Esta restricción evita asignar simultáneamente varias estrategias a una misma vialidad, garantizando que cada corredor urbano reciba una única intervención durante el proceso de optimización.

Restricción por columnas:
Cada estrategia puede asignarse como máximo a una vialidad.
\sum_{i=1}^{|A|} x_{ij}\leq 1,\qquad \forall j\in B

Esta restricción asegura que una misma estrategia no sea reutilizada en varias vialidades dentro de la misma instancia del problema, manteniendo la estructura de un matching bipartito uno a uno.

Otras restricciones, si existen:
En esta primera aproximación no se consideran restricciones adicionales relacionadas con presupuesto, tiempo de implementación, capacidad operativa o compatibilidad técnica. Dichas restricciones podrían incorporarse en futuras extensiones del modelo mediante términos adicionales en la formulación QUBO.

Justificación de por qué el problema es matching bipartito:
El problema se modela como un matching bipartito porque existen dos conjuntos disjuntos de elementos:
- Conjunto A: vialidades principales de la Ciudad de México.
- Conjunto B: estrategias de movilidad urbana.

Las asignaciones únicamente pueden realizarse entre elementos de conjuntos diferentes (vialidad–estrategia), sin existir relaciones entre elementos del mismo conjunto. Además, cada asignación es representada mediante una variable binaria (x_{ij}), donde cada vialidad puede asociarse con una sola estrategia y cada estrategia puede aplicarse a una sola vialidad, cumpliendo la definición de un matching bipartito.

Justificación de por qué es razonable modelarlo como QUBO:


### Resultados
Solución clásica exacta:
Resultado QAOA local:
Comparación clásico vs QAOA local:
Si se usó hardware real o pipeline híbrido, comparación adicional:

### Ética y limitaciones
Riesgos éticos:
Los resultados podrían interpretarse como recomendaciones definitivas para la planeación urbana cuando únicamente representan un ejercicio académico de optimización.

Medidas de mitigación:
Se utilizan únicamente datos públicos y de acceso abierto provenientes de fuentes oficiales y literatura científica.
No se emplean datos personales ni información que permita identificar individuos.

Limitaciones del modelo:
La instancia estudiada considera únicamente cuatro vialidades y cuatro estrategias de movilidad.
La matriz de beneficios (S^{obs}) corresponde a un dataset semirreal construido mediante integración de diversas fuentes documentales y estimaciones fundamentadas, debido a la inexistencia de una base de datos pública que relacione directamente todas las combinaciones vialidad–estrategia.
El modelo no incorpora restricciones presupuestales, tiempos de implementación, capacidad institucional ni efectos dinámicos del tránsito.


### Ejecución
Instrucciones para abrir el archivo .ipynb en Google Colab:
Acceder al repositorio de GitHub del proyecto.
Abrir el archivo proyecto_qubo_qaoa.ipynb.
Seleccionar Open in Colab o abrir el archivo mediante Google Colab utilizando la URL del repositorio.

Instrucciones para ejecutar todas las celdas sin errores:
Abrir el notebook en Google Colab.
Ejecutar las celdas en orden mediante Runtime → Run all.
El notebook descarga automáticamente el archivo dataset_real_4x4.csv desde la carpeta data/ del repositorio utilizando la URL raw de GitHub, por lo que no es necesario realizar cargas manuales de archivos.
Esperar a que finalice la instalación de las dependencias requeridas (si aplica).
Al finalizar la ejecución se generan:
la construcción de la matriz de beneficios,
la formulación QUBO,
la solución clásica,
la solución obtenida mediante QAOA,
y la comparación entre ambos métodos.


