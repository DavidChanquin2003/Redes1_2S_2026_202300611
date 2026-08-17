# Manual Técnico - Red LAN QuetzalDev S.A.

## 1. Inventario de Equipos y Materiales

A continuación se detalla el inventario completo del equipo activo y pasivo propuesto para la infraestructura de Capa 1 del edificio corporativo, diseñado bajo una topología física en estrella por departamento.

### 1.1 Dispositivos Finales (Hosts)
El equipamiento informático a conectar a la red se distribuye de la siguiente manera:

| Tipo de Equipo | Total |
| :--- | :---: |
| Computadoras de Escritorio (PCs) | 30 |
| Laptops | 12 |
| Servidores | 6 |
| **Total de Hosts** | **48** |

### 1.2 Equipo Activo de Red (Switches)
Se contará con un switch principal en el Cuarto de Telecomunicaciones (Data Center) y un switch de distribución por cada departamento. Se ha contemplado margen de crecimiento en los puertos.

| Ubicación | Tipo de Equipo | Puertos Mínimos | Cantidad |
| :--- | :--- | :---: | :---: |
| Data Center (MDF) | Switch Principal (Core/Distribución) | 24 | 1 |
| Sala de Capacitación | Switch de Acceso | 16 o 24 | 1 |
| Recursos Humanos | Switch de Acceso | 16 | 1 |
| Diseño e Innovación | Switch de Acceso | 16 | 1 |
| Departamento de Backend | Switch de Acceso | 16 | 1 |
| Departamento Legal | Switch de Acceso | 8 | 1 |
| Dirección General | Switch de Acceso | 8 | 1 |
| Departamento de Recepción| Switch de Acceso | 8 | 1 |

### 1.3 Equipo Pasivo y Terminaciones (Patch Panels y Racks)
Cumpliendo con la normativa, la cantidad de puertos del Patch Panel es igual o ligeramente inferior a la capacidad del Switch correspondiente.

| Ubicación | Elemento Pasivo | Especificación | Cantidad |
| :--- | :--- | :--- | :---: |
| Data Center (MDF) | Rack de Piso (Gabinete cerrado) | 42U estándar de 19" | 1 |
| Data Center (MDF) | Patch Panel Principal | 24 puertos (Cat 6/6A) | 1 |
| Data Center (MDF) | Bandeja de Fibra Óptica (ODF) | Para terminación de backbone | 1 |
| Departamentos (IDFs) | Gabinete de Pared  | 6U o 9U estándar de 19" | 7 |
| Deptos. (16 puertos)| Patch Panel Departamental | 16 puertos (Cat 6/6A) | 4 |
| Deptos. (8 puertos) | Patch Panel Departamental | 8 puertos (Cat 6/6A) | 3 |

### 1.4 Tomas de Red (Rosetas)
Las salidas de información en las áreas de trabajo se han agrupado estratégicamente según la disposición de los escritorios.

| Departamento | Tomas Unitarias | Tomas Dobles (2 puertos) | Tomas Cuádruples (4 puertos) |
| :--- | :---: | :---: | :---: |
| Recepción | 3 | 0 | 0 |
| Recursos Humanos | 4 | 0 | 1 |
| Legal | 4 | 0 | 0 |
| Sala de Capacitación| 0 | 5 | 0 |
| Diseño e Innovación | 0 | 4 | 0 |
| Dirección General | 4 | 0 | 0 |
| Backend | 1 | 3 | 0 |
| Data Center | 3 | 0 | 0 |
| **Total** | **19** | **12** | **1** |

### 1.5 Respaldo de Energía (UPS)
Para garantizar la continuidad operativa de los equipos de telecomunicaciones.

| Ubicación | Equipo | Capacidad Estimada | Cantidad |
| :--- | :--- | :--- | :---: |
| Data Center (MDF) | UPS Principal (Rackeable) | 3000VA - 5000VA | 1 |
| Departamentos (IDFs)| UPS Secundario (Rackeable)| 1000VA - 1500VA | 7 |

## 2. Justificación de la Ubicación del Cuarto de Telecomunicaciones (MDF)

El Cuarto de Telecomunicaciones Principal (MDF) ha sido ubicado en la sala del Data Center (esquina inferior derecha del plano), fundamentando esta decisión en los siguientes criterios:

*   **Optimización del Espacio Físico:** Con dimensiones de 12m x 5m, el Data Center es el área más amplia del edificio. Esto proporciona el espacio necesario para albergar el rack de piso principal, paneles de parcheo, conmutadores troncales y sistemas de respaldo de energía (UPS) pesados, cumpliendo con los requerimientos de espacios de telecomunicaciones del estándar ANSI/TIA/EIA-569.
*   **Cumplimiento de Distancias Normativas (Estándar 568):** Aunque el MDF no se encuentra en el centro geométrico de la planta, la longitud total del edificio es de 26 metros. Esto garantiza que la ruta del cableado troncal más larga (hacia el Departamento de Recepción en el extremo opuesto) rondará los 30 a 35 metros aproximadamente. Esta medida está holgadamente por debajo del límite de 100 metros establecido por el estándar ANSI/TIA/EIA-568 para atenuación de señal.
*   **Centralización de Recursos Críticos:** La sala del Data Center tiene asignados 3 servidores principales de la empresa. Instalar el MDF en esta misma habitación permite que la conexión entre el Switch Principal y dichos servidores sea directa, confinando el tráfico de datos más pesado dentro del mismo cuarto y reduciendo la latencia.
*   **Aislamiento y Acondicionamiento:** Al integrar el equipo principal de red dentro del Data Center de QuetzalDev S.A, se aprovechan las condiciones de seguridad física, control de acceso y probable climatización dedicada que requiere esta sala por naturaleza, evitando invadir o saturar los espacios de trabajo administrativos.

## 3. Justificación de la Topología Física por Departamento

Para la distribución del cableado horizontal dentro de los 7 departamentos y el Data Center, se ha seleccionado y diseñado una Topología Física en Estrella. En este diseño, cada nodo (PC, laptop o servidor) se conecta de forma directa e independiente al switch de acceso instalado en su respectiva área.

La elección de esta topología se fundamenta en los siguientes criterios:

*   **Tolerancia a fallos y criticidad:** Al conectar cada dispositivo de manera individual hacia el switch local, se aísla el dominio de fallos. Si un cable de red (horizontal) se daña, sufre interferencias o se desconecta, únicamente ese equipo específico perderá conectividad. El resto del departamento podrá seguir operando con normalidad, lo cual asegura la continuidad del negocio.
*   **Escalabilidad controlada:** La topología en estrella facilita enormemente el crecimiento de la red. Si QuetzalDev S.A. quiere incorporar nuevo personal en la Sala de Capacitación o añadir estaciones en Diseño e Innovación, bastará con habilitar un puerto libre en el patch panel/switch departamental y tender un nuevo enlace hacia la toma de red, sin necesidad de apagar o reestructurar el segmento existente.
*   **Facilidad de Mantenimiento y Diagnóstico**: Esta topología centraliza las conexiones físicas en un único punto por departamento (el gabinete de pared). Esto agiliza la labor del personal de soporte, permitiendo identificar rápidamente problemas de capa 1 al revisar el estado del puerto específico en el switch o guiándose por las etiquetas (ej. `RRHH-PR01`).

## 4. Tipo y Categoría de Cable por Segmento

La selección de los medios de transmisión físicos se ha realizado tomando en cuenta las dimensiones del edificio de QuetzalDev S.A. (26 metros de largo y 18 de ancho en su totalidad) y las necesidades de ancho de banda de una empresa asi.

| Segmento de Red | Tipo y Categoría de Cable | Justificación Técnica |
| :--- | :--- | :--- |
| **Cableado Troncal** (Backbone / Enlaces MDF a Switches de Depto.) | **Fibra Óptica Multimodo** | Según los principios de cableado estructurado, la fibra óptica es el medio más apropiado para el backbone debido a su alta capacidad y velocidad de uplink. Garantiza que el tráfico sumado de las 48 computadoras/servidores no genere un cuello de botella al comunicarse con el Data Center principal. |
| **Cableado Horizontal** (Switches de Depto. a Dispositivos Finales) | **Cobre UTP - Categoría 6** | Las distancias desde los switches departamentales hasta las áreas de trabajo son considerablemente cortas (menores a 30 metros), cumpliendo holgadamente el estándar de 100 metros máximos. El cable UTP Cat 6 es ideal ya que soporta velocidades de 1 Gbps (1000BASE-T) a 250 MHz, ofreciendo el ancho de banda necesario para las tareas de desarrollo, QA y diseño de la empresa a un costo equilibrado. |

## 5. Estimación de Distancias y Cálculo de Bobinas (Cableado Horizontal)

Dado que la topología física establece un switch de acceso dentro de cada departamento, el cableado horizontal se confina al perímetro interno de cada oficina, reduciendo drásticamente las distancias. Se han tomado en cuenta las medidas arquitectónicas internas de los cuartos (ej. 10m x 5m, 8m x 6m), añadiendo longitud para los descensos verticales desde el techo o canaleta hacia las rosetas y el margen de ordenamiento en los gabinetes.

### 5.1 Distancias Estimadas por Segmento

| Departamento | Puntos de Red (Hosts) | Distancia Promedio Estimada (por enlace) | Subtotal de Cable |
| :--- | :---: | :---: | :---: |
| Departamento de Recepción | 4 | 10 metros | 40 metros |
| Recursos Humanos | 8 | 8 metros | 64 metros |
| Departamento Legal | 4 | 8 metros | 32 metros |
| Sala de Capacitación | 10 | 9 metros | 90 metros |
| Diseño e Innovación | 8 | 9 metros | 72 metros |
| Dirección General | 4 | 8 metros | 32 metros |
| Departamento de Backend | 7 | 9 metros | 63 metros |
| Data Center | 3 | 5 metros | 15 metros |
| **Total Neto Estimado** | **48 enlaces** | | **408 metros** |

Nota: La estimación incluye tanto computadoras de escritorio y laptops, como los servidores ubicados en los distintos departamentos.

### 5.2 Cálculo de Bobinas (Cable UTP Cat 6)

Para determinar la adquisición de materiales, se añade un 15% adicional al metraje neto en concepto de holgura (curvas de canalización, terminación en patch panels, ponchado de rosetas y perdidas).

*   **Cableado Neto Estimado:** 408 metros.
*   **Margen de Holgura (15%):** 61.2 metros.
*   **Total de Cable Horizontal Requerido:** 469.2 metros.

Con base en la normativa comercial y los requerimientos del proyecto, se utilizan bobinas estándar de cable UTP que contienen 305 metros.
*   Cálculo: 469.2 m / 305 m = 1.53 bobinas.

**Conclusión de Material:** Se requiere presupuestar y adquirir 2 bobinas de cable UTP Categoría 6 (305 metros c/u) para satisfacer holgadamente la demanda de cableado horizontal del edificio.