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

## 6. Justificación de Equipo Activo y Dimensionamiento

Para el correcto flujo de los datos y el soporte de la topología en estrella diseñada, se propone la siguiente infraestructura de equipos activos y su correspondiente dimensionamiento de terminaciones pasivas (Patch Panels).

### 6.1 Switch Principal (Cuarto de Telecomunicaciones / MDF)
*   **Justificación:** Este dispositivo funciona como el núcleo  de toda la red del edificio de QuetzalDev S.A. Su función es concentrar el tráfico que proviene del cableado troncal de los 7 departamentos y proveerles acceso a los servicios centrales.
*   **Dimensionamiento:** Actualmente, el switch principal debe conectar 7 enlaces troncales (uno por cada departamento) más los 3 servidores físicos alojados en el Data Center, requiriendo un mínimo de 10 puertos activos.
*   **Selección:** Se propone un Switch de 24 puertos, emparejado con un Patch Panel principal de 24 puertos. Esto cumple estrictamente con el requerimiento técnico de que el switch debe tener una cantidad de puertos igual o mayor al patch panel, garantizando además un 140% de margen de escalabilidad para futuras conexiones troncales o servidores adicionales.

### 6.2 Switches Departamentales de Acceso
*   **Justificación:** Son los elementos activos encargados de distribuir la conexión hacia el cableado horizontal de cada área de trabajo. Implementar un switch por departamento aísla el tráfico local, optimiza el rendimiento y facilita la administración de los hosts.
*   **Dimensionamiento y Selección:** El dimensionamiento se calculó sumando la cantidad de dispositivos finales (PCs, laptops y servidores) requeridos por área, añadiendo un margen para el crecimiento a corto plazo:
    *   **Áreas de baja densidad (Recepción, Legal, Dirección General):** Requieren 4 puntos de red cada una. Se dimensionaron switches de 8 puertos con Patch Panels de 8 puertos.
    *   **Áreas de alta densidad (Recursos Humanos, Capacitación, Diseño, Backend):** Requieren entre 7 y 10 puntos de red cada una. Se dimensionaron Switches de 16 puertos equiparejados con Patch Panels de 16 puertos. (En el caso de Capacitación, que ocupa 10 puertos, el switch de 16 puertos deja 6 disponibles para rápida expansión).

### 6.3 Servidores
*   **Justificación:** Actúan como los proveedores de recursos, aplicaciones y almacenamiento dentro de la red. 
*   **Distribución:** Se alojan 3 servidores en el Data Center por seguridad y control climático. Los 3 servidores distribuidos (en Recepción, Diseño e Innovación, y Backend) se justifican operativamente para reducir la latencia en tareas específicas de esos equipos (por ejemplo, servidores locales para Laboratorios de Pruebas QA en Diseño, o control de repositorios en Backend).

## 7. Justificación del Medio de Transmisión (Cableado Troncal)

Para la interconexión entre el Cuarto de Telecomunicaciones (MDF) y los switches de distribución ubicados en cada departamento (cableado troncal), se ha seleccionado Fibra Óptica Multimodo.

La elección de este medio de transmisión se fundamenta en los siguientes aspectos técnicos y operativos:

*   **Capacidad y Velocidad de Uplink:** Tal como establecen los fundamentos de cableado estructurado, la fibra óptica es el medio más apropiado para el cableado troncal debido a la altísima capacidad y velocidad de transmisión que ofrece. Al utilizar fibra multimodo, se garantiza un canal de ancho de banda masivo para los enlaces de subida. Esto previene cuellos de botella cuando el tráfico concurrente de los 7 departamentos y los servidores viaje simultáneamente hacia el switch principal en el Data Center.
*   **Inmunidad a Interferencias:** El cableado troncal recorrerá la canalización principal a través del Hall Central, un área donde típicamente coexiste el cableado de iluminación y energía eléctrica del edificio. A diferencia del cobre, la fibra óptica transmite pulsos de luz, haciéndola completamente inmune a la Interferencia Electromagnética y garantizando la pureza de la señal a lo largo de toda la ruta.
*   **Escalabilidad a Futuro y Costo-Beneficio:** Aunque la inversión inicial en fibra y transceptores ópticos es ligeramente superior a la del cable UTP, su vida útil y escalabilidad justifican el costo. Si en un futuro QuetzalDev S.A. demanda velocidades superiores en su columna vertebral (como migrar a 10 Gbps o superior), la infraestructura de fibra multimodo ya estará lista para soportarlo sin necesidad de invertir en recablear el edificio; únicamente se actualizarán los módulos ópticos en los switches.
*   **Distancia:** Aunque las distancias hacia cada departamento son menores a los 30 metros (debido a que el edificio tiene 26 metros de longitud total) y el cobre podría soportar el alcance, la prioridad en el troncal no es solo llegar, sino garantizar la máxima tasa de transferencia y latencia mínima entre el MDF y los Deptos.

## 8. Justificación de la Ruta de Canalización

Para el enrutamiento seguro, ordenado y escalable de los cables a través del edificio de QuetzalDev S.A, se implementará un sistema de canalización estructurado basándose en los lineamientos del estándar **ANSI/TIA/EIA-569** (Estándar para Ductos y Espacios de Telecomunicaciones en Edificios Comerciales).

### 8.1 Canalización Principal (Cableado Troncal)
*   **Selección:** Escalerilla metálica abierta (Bandeja portacables tipo escalera).
*   **Ruta:** Suspendida en el techo a lo largo de los 26 metros del Hall Central, iniciando desde el Data Center y ramificándose hacia cada cuarto.
*   **Justificación:** Se optó por una escalerilla metálica abierta debido a su excelente accesibilidad y capacidad de crecimiento. Este medio permite que el personal técnico pueda agregar, retirar o inspeccionar visualmente los enlaces de fibra óptica del backbone sin necesidad de desarmar tapas o ductos cerrados. Además, su diseño abierto proporciona una ventilación pasiva óptima y soporta estructuralmente el peso de las troncales que alimentan a los 7 departamentos.

### 8.2 Canalización Secundaria (Cableado Horizontal)
*   **Selección:** Canaleta plástica cerrada de superficie (Ducto de pared).
*   **Ruta:** Desde el gabinete de telecomunicaciones interno de cada departamento hacia las salidas de información (rosetas) agrupadas en los escritorios.
*   **Justificación:** Dentro del área de trabajo de oficinas como Legal, Dirección General o Recursos Humanos, la estética y la protección física de los enlaces son prioridades. La canaleta cerrada resguarda los cables de cobre UTP Categoría 6 contra dobleces excesivos, tirones o impactos accidentales causados por el personal. Esto asegura que el tramo final del cableado mantenga su integridad hasta llegar a las tomas de red unitarias o dobles de cada usuario.

## 9. Justificación del Tipo de Rack para el MDF

Para el Cuarto de Telecomunicaciones Principal (MDF), alojado dentro del Data Center, se propone la utilización de un Rack de Piso (Gabinete Cerrado) de 42U estándar de 19 pulgadas. 

Esta decisión se basa en base a los siguientes puntos clave:

*   **Capacidad y Carga Estructural:** El MDF es el núcleo de la red. Un rack de piso ofrece el espacio vertical y la resistencia estructural necesarios para alojar de forma segura el Switch Principal, el Patch Panel central, la bandeja de fibra óptica y los 3 servidores principales asignados a esta área.
*   **Soporte para Respaldo de Energía:** Los equipos de respaldo (UPS) con capacidad suficiente para sostener la infraestructura central (estimados entre 3000VA y 5000VA) son equipos de gran volumen y peso. Un gabinete de piso es indispensable para soportar esta carga de manera segura en su base inferior.
*   **Seguridad Física y Organización:** Al tratarse de un gabinete cerrado, se añade una capa de seguridad física mediante cerraduras (control de acceso físico). Además, su altura permite una gestión óptima mediante ordenadores de cables verticales, manejando adecuadamente el volumen de enlaces troncales que convergen desde todo el edificio corporativo.

## 10. Estimación de Consumo Eléctrico y Capacidad de UPS

Para garantizar la continuidad operativa de la red ante cortes de energia, se dimensionó el respaldo de energía (UPS) basándose en el consumo eléctrico estimado del equipo activo (switches departamentales y central), así como de los servidores locales. 

Para el cálculo de la capacidad en Voltiamperios (VA), se asume un Factor de Potencia (FP) estándar en equipos informáticos de 0.8 utilizando la siguiente ecuación: 

VA = Watts/0.8

### 10.1 Consumo en el Data Center (MDF)
El Cuarto de Telecomunicaciones (MDF) centraliza el equipo de mayor carga de la red corporativa.
*   **Consumo estimado:** 1 Switch Principal de 24 puertos (aprox 50W) + 3 Servidores Principales (aprox 400W c/u) = 1250 Watts.
*   Cálculo: VA = 1250/0.8 = 1562.5 VA
*   **Capacidad Recomendada:** Se propone instalar 1 UPS Rackeable de 3000VA. Esta capacidad teorica cubre la carga actual del switch central y los servidores, dejando un amplio margen de seguridad (superior al 50%) para conectar futuras expansiones, sistemas de enfriamiento o monitores del rack.

### 10.2 Consumo en los Departamentos
Cada departamento cuenta con un switch de acceso de 8 o 16 puertos, y tres áreas específicas (Recepción, Diseño e Innovación, y Backend) incluyen un servidor local.
*   **Consumo máximo estimado por IDF:** 1 Switch de acceso (aprox 30W) + 1 Servidor (aprox 400W) = 430 Watts.
*   **Cálculo:** VA = 430/0.8 = 537.5 VA
*   **Capacidad Recomendada:** Se propone instalar 1 UPS de 1000VA a 1500VA para cada gabinete departamental. Esta capacidad soporta de forma sobrada el consumo eléctrico del equipo activo de cada departamento y provee la autonomía necesaria para mantener la conexión de red y resguardar la información en caso de emergencia eléctrica.

## 11. Especificación de Conexiones (Straight-Through y Crossover)

A continuación defino el tipo de armado para los cables de par trenzado (cobre) en la infraestructura de la red, de acuerdo con la naturaleza de los dispositivos en cada extremo de la conexión.

| Segmento / Enlace | Dispositivos a Conectar | Formato de Cable | Justificación Técnica |
| :--- | :--- | :--- | :--- |
| **Cableado Horizontal** | PC / Laptop / Servidor, Switch de Acceso | **Directo (Straight-Through)** | Se utiliza específicamente para conectar dispositivos diferentes, como una compu de escritorio a un switch. En este cable, la secuencia de los pines se mantiene igual en ambos extremos (el pin 1 va con el pin 1, el 2 con el 2, etc.), permitiendo que la transmisión de un equipo llegue correctamente a la recepción del otro. |
| **Cableado Troncal** *(Si se utilizara UTP en lugar de Fibra)* | Switch Departamental, Switch Principal (MDF) | **Cruzado (Crossover)** | Se utiliza de manera estricta para conectar dispositivos iguales, como dos switches entre sí, sin intermediarios. Los pines se cruzan intencionalmente (el pin 1 en un extremo se conecta al pin 3 del otro, y viceversa), lo que permite que los pares de transmisión y recepción de equipos idénticos puedan "hablarse" directamente. 

## 12. Documentación de Disposición de Pines

Como evidencia de la aplicación de estándares internacionales para la infraestructura de Capa 1, se detalla el orden de colores (pines 1 al 8) a utilizar en el ponchado de los conectores RJ45, basándose en la norma ANSI/TIA/EIA-568-A y TIA/EIA-568-B.

### 12.1 Cable Directo (Straight-Through)
Para la conexión entre dispositivos diferentes (Cableado Horizontal: PC a Switch), se aplica el mismo estándar en ambos extremos del cable. En este proyecto se estandarizará el uso de T568B en ambos lados.

| Pin | Extremo 1 (Estándar T568B) | Extremo 2 (Estándar T568B) |
| :---: | :--- | :--- |
| **1** | Blanco / Naranja | Blanco / Naranja |
| **2** | Naranja | Naranja |
| **3** | Blanco / Verde | Blanco / Verde |
| **4** | Azul | Azul |
| **5** | Blanco / Azul | Blanco / Azul |
| **6** | Verde | Verde |
| **7** | Blanco / Café | Blanco / Café |
| **8** | Café | Café |

### 12.2 Cable Cruzado (Crossover)
Para la conexión de dispositivos iguales (de Switch a Switch), se utiliza un estándar diferente en cada extremo para cruzar los pares de transmisión y recepción. Se utilizará T568A en un extremo y T568B en el otro.

| Pin | Extremo 1 (Estándar T568A) | Extremo 2 (Estándar T568B) |
| :---: | :--- | :--- |
| **1** | Blanco / Verde | Blanco / Naranja |
| **2** | Verde | Naranja |
| **3** | Blanco / Naranja | Blanco / Verde |
| **4** | Azul | Azul |
| **5** | Blanco / Azul | Blanco / Azul |
| **6** | Naranja | Verde |
| **7** | Blanco / Café | Blanco / Café |
| **8** | Café | Café |