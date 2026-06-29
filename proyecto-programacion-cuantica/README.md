# proyecto final QMexico
### Dataset

Nombre del dataset:
Asignacion optima de estrategias de movilidad en vialidades principales de la CDMX

Fuente oficial o confiable:
Diversas fuentes oficiales y literatura científica sobre movilidad urbana, incluyendo portales de datos abiertos del Gobierno de la Ciudad de México, MexicoCity,  tomtom, SEMOVI, INFOVIAL, INEGI, secretaria de obras y servicios, y artículos científicos especializados Gaceta UNAM, secretaria de servicios CDMX, Tesis UNAM, Guia ofical de usuarios de las calles, Posgrado de ingenieria civil UNAM, IIMAS, Instituto de Investigaciones Parlamentarias, LaSalle.

Institución responsable:
Secretaría de Movilidad de la Ciudad de México (SEMOVI); Gobierno de la Ciudad de México; Instituto Nacional de Estadística y Geografía (INEGI); Secretaría de Infraestructura, Comunicaciones y Transportes (SICT), UNAM gaceta, CDMX secretarias, UNAM, INFOVIAL, tomtom, Asamblea Legislativa del Distrito Federal, Universidad de la Salle.

URL de la fuente:
https://ciencia.unam.mx/leer/88/Desarrollan_sistema_de_semaforos_auto-organizantes_para_enfrentar_trafico_en_las_ciudades

https://www.ssc.cdmx.gob.mx/storage/app/media/Avisos/CIRCUITO%20INTERIOR%20-%20REVERSIBLE%20.pdf

https://revistasinvestigacion.lasalle.mx/index.php/mclidi/article/view/4151

http://aldf.gob.mx/archivo-9f6f5328e0f0853d4453d481cbffa2b6.pdf

https://www.gaceta.unam.mx/crean-modelo-para-agilizar-flujo-vehicular/

https://obras.cdmx.gob.mx/comunicacion/nota/desnivel-mixcoac-insurgentes-mejora-movilidad-en-circuito-interior

https://mexico.itdp.org/wp-content/uploads/2023/08/FICHAS-INSURGENTES.pdf

http://www.ptolomeo.unam.mx:8080/xmlui/bitstream/handle/132.248.52.100/417/A4.pdf

https://www.semovi.cdmx.gob.mx/storage/app/media/GuiaDeUsuarioDeLaCalleCDMX_V1_estilo_091216.pdf

https://datos.cdmx.gob.mx/group/movilidad?organization=secretaria-de-movilidad&groups=movilidad

https://datos.cdmx.gob.mx/dataset/d7d82ecf-1a5a-41eb-855e-a3fd39a942b6/resource/6a731080-1055-4fbd-b721-2e66e98d6b31/download/infovial-semovi_ver03-corregida-1.pdf?utm_source=chatgpt.com

https://www.inegi.org.mx/programas/transporteurbano/?utm_source=chatgpt.com

https://www.tomtom.com/traffic-index/?utm_source=chatgpt.com

https://mexicocity.cdmx.gob.mx/?utm_source=chatgpt.com

http://www.ptolomeo.unam.mx:8080/xmlui/bitstream/handle/132.248.52.100/2547/osunaruiz.pdf?sequence=1

URL raw del CSV usado en data/:
https://raw.githubusercontent.com/RCanthony845/proyecto-programacion-cuantica/refs/heads/main/proyecto-programacion-cuantica/data/dataset_real_4x4.csv

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
Estrategias de mejora de la movilidad urbana. (Carril reversible, Semaforización inteligente, Gestión dinámica del tránsito, Mantenimiento prioritario de la vialidad)

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
Nota importante, no se utilizo una formula matematica exacta para obtener la matriz S, esta se construyo atraves de ver los beneficios que logran las estrategias a las diferentes vialidas, es decir fue una construccion manual, dado de las investigaciones observadas, sobre el tema, la cual se uso como matriz S final (S_obs) en vez de utilizar un modelo semi-teorico para obtener el valor de S. Pero si se calculo la matriz S, con la siguiente ecuacion aunque al ser muy simplista no se tomo en cuenta para mostrar un buen factor de impacto.
S_ij =F_i^{rela}/ I_j .
donde F_i^{rela}=\frac{F_i-F_{i,min}}{F_{i,max}-F_{i,min}} 
e I_j corresponde al índice promedio de efectividad de la estrategia j obtenido a partir de la matriz de beneficios observada S_obs.

Normalización aplicada:
Normalización Min-Max individual para cada vialidad utilizando el rango del numero de vehiculos que pasan por la vialidad al dia promedio o representativo de flujo vehicular relativo: F_i^{rela}=\frac{F_i-F_{i,min}}{F_{i,max}-F_{i,min}}
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
El problema consiste en asignar exactamente una estrategia de movilidad a cada una de las cuatro vialidades consideradas, maximizando un puntaje de compatibilidad representado por la matriz (S). Cada posible asignación se modela mediante una variable binaria (x_{ij}), donde (x_{ij}=1) indica que la estrategia (j) se asigna a la vialidad (i), y (x_{ij}=0) en caso contrario.

La función objetivo busca maximizar el beneficio total de las asignaciones seleccionadas, mientras que las restricciones de que cada vialidad reciba una única estrategia y que cada estrategia se asigne a una única vialidad se incorporan como términos de penalización cuadráticos. De esta manera, el problema se transforma en una función cuadrática sin restricciones explícitas, que corresponde exactamente a una formulación Quadratic Unconstrained Binary Optimization (QUBO).

Esta formulación es apropiada porque permite resolver el problema tanto mediante algoritmos clásicos de optimización como mediante algoritmos cuánticos variacionales, en particular el Quantum Approximate Optimization Algorithm.

### Resultados
Solución clásica exacta:
La asignación óptima maximiza el puntaje total de la matriz de compatibilidad S:
Score óptimo: 3.0
Energía QUBO: −3.0
Solución factible: Sí.

Resultado QAOA local:

Profundidad: p=1
Optimizador clásico: COBYLA
Reinicios aleatorios: 10
Iteraciones máximas por reinicio: 59
Muestreo final: 5000 shots
Los mejores parámetros encontrados fueron

γ=1.860668,β=2.691575.

Durante el muestreo, la mejor solución obtenida por QAOA presentó:

Score: 3.0
Energía QUBO: −3.0
Solución factible: Sí.

Además, el estado óptimo apareció con una probabilidad aproximada de 0.001129, mientras que la probabilidad total de obtener una solución factible fue aproximadamente 0.026578.

Comparación clásico vs QAOA local:
Clásico exacto : energia QUBO de −3.0, score de 3.0, es factible Sí
QAOA local: energia QUBO de -3.0, score de 3.0, es factible si.

### Ética y limitaciones
Riesgos éticos:
Los resultados no pueden interpretarse como recomendaciones definitivas para la planeación urbana, únicamente representan un ejercicio académico de optimización.

Medidas de mitigación:
Se utilizan únicamente datos públicos y de acceso abierto provenientes de fuentes oficiales y literatura científica.
No se emplean datos personales ni información que permita identificar individuos.

Limitaciones del modelo:
La instancia estudiada considera únicamente cuatro vialidades y cuatro estrategias de movilidad.
La matriz de beneficios (S_obs) corresponde a un dataset semirreal construido mediante integración de diversas fuentes documentales y estimaciones fundamentadas, debido a la inexistencia de una base de datos pública que relacione directamente todas las combinaciones vialidad–estrategia.
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
Esperar a que finalice la instalación de las dependencias requeridas.

###preguntas finales
¿Cuál fue la mejor asignación encontrada?
La mejor asignación encontrada, tanto por el método clásico como por QAOA local, fue la asignación uno a uno que maximiza la suma de los puntajes de la matriz de compatibilidad S. Esta solución respeta todas las restricciones del problema, asignando exactamente una estrategia de movilidad a cada vialidad y utilizando cada estrategia una única vez.

¿Cuál fue su score en el dominio?
El score total obtenido fue: 3.0

¿La asignación cumple todas las restricciones?
Sí. La solución satisface completamente las restricciones del modelo:
cada vialidad recibe exactamente una estrategia; cada estrategia se asigna exactamente a una vialidad; no existen asignaciones duplicadas ni estrategias sin utilizar.

¿QAOA local observó el óptimo clásico?
Sí. Durante el proceso de muestreo, QAOA local encontró una solución con el mismo score y la misma energía QUBO que la solución clásica exacta.
Esto indica que, para esta instancia 4×4, QAOA logró recuperar el óptimo global del problema.

¿Qué tan frecuente fue observar soluciones factibles?
Durante el muestreo de 5000 shots, aproximadamente el 2.66 % de los estados medidos correspondieron a soluciones factibles.
Dentro de ese conjunto, el estado óptimo apareció con una probabilidad aproximada del 0.11 %.

¿Qué limitaciones tiene el modelo 4 × 4?
Sus principales limitaciones son:
únicamente considera cuatro vialidades y cuatro estrategias;
la matriz de compatibilidad S fue construida a partir de información pública y criterios de evaluación definidos por los autores, por lo que representa una aproximación semirreal;
no incorpora restricciones adicionales presentes en un sistema de movilidad real, como costos económicos, tiempos de implementación, presupuesto disponible, capacidad operativa o interacción entre estrategias.

¿Qué cambiaría si el dataset creciera?
crecería el número de variables binarias; la matriz QUBO aumentaría de tamaño; el espacio de búsqueda crecería factorialmente para el método clásico; el problema se volvería computacionalmente más complejo.

¿Qué riesgos éticos existen y cómo se mitigaron?
El proyecto no utiliza información personal ni datos identificables de ciudadanos.
Los datos empleados provienen de fuentes públicas y describen únicamente características generales de vialidades y estrategias de movilidad.
