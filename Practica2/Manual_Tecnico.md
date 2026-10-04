# Manual Técnico - Red de la Ciudad Comercial Cayalá
**Curso:** Redes de Computadoras 1 
**Universidad de San Carlos de Guatemala (USAC) - Facultad de Ingeniería**  
**Escuela de Ciencias y Sistemas**  
**Estudiante:** David Arturo Chanquin Velasquez  
**Carnet:** 202300611  

---

## 1. Selección y Análisis Operativo de las 5 Zonas de Ciudad Cayalá

Con base en la investigación de los sectores reales que integran el complejo urbano Ciudad Cayalá (Zona 16, Ciudad de Guatemala), se identificaron 5 zonas estratégicas cuyas características operativas, densidad de usuarios y criticidad determinan los requerimientos de la infraestructura de red:

| Zona | Establecimiento Real en Cayalá | VLAN ID | Hosts Req. | Rol Operativo | Volumen de Tráfico | Nivel de Criticidad |
| :--- | :--- | :---: | :---: | :--- | :---: | :---: |
| **Zona 1** | **Paseo Cayalá** (Centro Urbano Comercial y Gastronómico) | `11` | 60 | Operación comercial masiva: terminales de punto de venta (POS), facturación electrónica de restaurantes, locales retail y puntos de acceso Wi-Fi. | **Muy Alto** | **Alta** |
| **Zona 2** | **Décimo de Cayalá** (Centro Financiero, Clínicas y Supermercado) | `21` | 28 | Servicios bancarios, cajeros automáticos, consultorios médicos, laboratorios y cajas de supermercado. | **Medio-Alto** | **Media-Alta** |
| **Zona 3** | **Administración Grupo Cayalá** (Edificio I, Nivel 2, Distrito Moda) | `31` | 12 | Gestión administrativa central del complejo, servidores de control de acceso, monitoreo de seguridad (CCTV) y administración TI. | **Moderado / Constante** | **Crítica (Muy Alta)** |
| **Zona 4** | **Distrito Empresarial y Cayibel** (6 Torres de Oficinas Corporativas) | `41` | 50 | Entorno empresarial: estaciones de trabajo corporativas, telefonía IP (VoIP), impresoras de red y salas de videoconferencias. | **Alto** | **Alta** |
| **Zona 5** | **Condominio Lirios de Cayalá** (Garita de Control y Casa Club) | `51` | 7 | Servicios secundarios residenciales: control de acceso vehicular/peatonal en garita y recepción de la Casa Club. | **Bajo** | **Baja** |

### Detalle de Criticidad por Zona
* **Zona 3 (Administración Central y Seguridad):** Es el núcleo de control del complejo. Una interrupción del servicio afectaría la vigilancia, el control de garitas y el enrutamiento general entre zonas, por lo que requiere la máxima disponibilidad y actúa como punto central de conmutación Multicapa (Capa 3).
* **Zona 1 (Paseo Cayalá) y Zona 4 (Distrito Empresarial):** Concentran la mayor densidad de hosts (60 y 50 dispositivos respectivamente). Una caída detendría las transacciones comerciales de decenas de locales o las operaciones laborales de las 6 torres corporativas, exigiendo tanto redundancia de caminos como mayor ancho de banda agregado.
* **Zona 2 (Décimo de Cayalá):** Presenta una carga media (28 hosts), pero al albergar servicios financieros y de salud, requiere un enlace de respaldo hacia el núcleo para garantizar continuidad operativa ante el fallo de un cable troncal.
* **Zona 5 (Condominio Lirios Residencial):** Posee una carga mínima de tráfico (7 hosts) dedicada a tareas administrativas locales del condominio. Su impacto ante un corte temporal es bajo en comparación con las zonas comerciales o de administración central, por lo que no requiere enlaces troncales redundantes.

---

## 2. Diseño y Justificación de la Topología Híbrida

Para responder a las condiciones operativas de forma diferenciada (evitando un esquema uniforme para todas las áreas), se diseñó una Topología Híbrida que combina **Malla Parcial** en el núcleo de alta disponibilidad y **Estrella Extendida (Jerárquica)** hacia las zonas de bacia afuera y la capa de acceso.

### 2.1. Esquemas Combinados en la Topología
1. **Malla Parcial (Núcleo Crítico - Zonas 1, 2, 3 y 4):**
   * Los switches principales de **Zona 3 (`SW-PRIN-Z3`)**, **Zona 1 (`SW-PRIN-Z1`)** y **Zona 4 (`SW-PRIN-Z4`)** están interconectados formando un triángulo cerrado de alta disponibilidad.
   * El switch principal de **Zona 2 (`SW-PRIN-Z2`)** se integra a esta malla parcial mediante dos enlaces físicos independientes: uno hacia `SW-PRIN-Z1` y otro hacia `SW-PRIN-Z3`.
   * **Justificación:** Cumple con la condición operativa de Alta Disponibilidad, garantizando que las zonas críticas y de alto tráfico cuenten con al menos 2 rutas físicas hacia el resto de la red. Si cualquier enlace troncal sufre un corte, el protocolo **Rapid PVST+** converge de inmediato activando el enlace de respaldo sin interrumpir la operación.
2. **Estrella Extendida (Zona 5 y Capa de Acceso):**
   * El switch principal de **Zona 5 (`SW-PRIN-Z5`)** se conecta mediante un único enlace troncal simple hacia `SW-PRIN-Z4`.
   * A su vez, dentro de cada una de las 5 zonas, los switches clientes (`SW-CLI...`) se conectan en estrella hacia su respectivo switch principal, y los hosts finales (PCs) se conectan en estrella hacia los switches clientes.
   * **Justificación:** En la Zona 5 (baja criticidad y solo 7 hosts) se optimizan costos de cableado y puertos al prescindir de redundancia innecesaria. Por su parte, en las Zonas 1 y 4 se implementaron 2 switches de acceso (Clientes VTP) por zona para distribuir físicamente la carga de 60 y 50 hosts, facilitando múltiples puntos de acceso a la red.

### 2.2. Justificación Técnica de Enlaces Físicos y Estándar de Cableado
La red aplica el estándar **TIA/EIA-568B** diferenciando el tipo de cable e interfaz según la función y carga de cada conexión:

| Origen | Puerto Origen | Destino | Puerto Destino | Tipo de Cable / Medio | Rol y Justificación Técnica del Enlace |
| :--- | :---: | :--- | :---: | :--- | :--- |
| `SW-PRIN-Z3` | `Fa0/1, Fa0/2` | `SW-PRIN-Z1` | `Fa0/1, Fa0/2` | Cobre Crossover (x2) | **Trunk Po1 (EtherChannel PAgP - 200 Mbps):** Soporta el alto tráfico de los 60 hosts de Paseo Cayalá hacia el núcleo. |
| `SW-PRIN-Z3` | `Fa0/3, Fa0/4` | `SW-PRIN-Z4` | `Fa0/3, Fa0/4` | Cobre Crossover (x2) | **Trunk Po2 (EtherChannel PAgP - 200 Mbps):** Soporta el alto tráfico de los 50 hosts de Distrito Empresarial hacia el núcleo. |
| `SW-PRIN-Z1` | `Gi0/1` | `SW-PRIN-Z4` | `Gi0/1` | Cobre Crossover | **Trunk Gigabit (1 Gbps):** Cierra el anillo de redundancia (Alta Disponibilidad) entre Paseo Cayalá y Distrito Empresarial. |
| `SW-PRIN-Z2` | `Fa0/1` | `SW-PRIN-Z1` | `Gi0/2` | Cobre Crossover | **Trunk Primario Zona 2:** Enlace troncal principal desde Décimo hacia Paseo Cayalá. |
| `SW-PRIN-Z2` | `Fa0/2` | `SW-PRIN-Z3` | `Gi0/1` | Cobre Crossover | **Trunk Redundante Zona 2:** Segundo enlace físico hacia Administración Central para garantizar Alta Disponibilidad en Décimo. |
| `SW-PRIN-Z5` | `Gi0/2` | `SW-PRIN-Z4` | `Gi0/2` | Cobre Crossover | **Trunk Único Zona 5 (Estrella Extendida):** Enlace simple suficiente para los 7 hosts de Lirios Residencial (baja criticidad). |
| `SW-PRIN-Z1` | `Fa0/23, Fa0/24` | `SW-CLI1/2-Z1` | `Gi0/1` | Cobre Crossover | **Trunks Internos Zona 1:** Conectan los 2 switches clientes para distribuir los 60 hosts de la zona comercial. |
| `SW-PRIN-Z4` | `Fa0/23, Fa0/24` | `SW-CLI1/2-Z4` | `Gi0/1` | Cobre Crossover | **Trunks Internos Zona 4:** Conectan los 2 switches clientes para distribuir los 50 hosts corporativos. |
| `SW-PRIN-Z2/3/5`| `Gi0/1` o `Fa0/24`| `SW-CLI-Z2/3/5`| `Gi0/1` | Cobre Crossover | **Trunks Internos Zonas 2, 3 y 5:** Sincronizan VTP y transportan el tráfico de los switches de acceso locales. |
| `Switches Clientes`| `Fa0/1 - Fa0/4` | `PC1 - PC20` | `Fa0` | Cobre Straight-Through | **Enlaces de Acceso (100 Mbps):** Conexión directa entre dispositivos de distinta capa (Switch a Host). |

---

## 3. Estructura de VLANs y Plan de Direccionamiento IP (VLSM)

El identificador de cada VLAN de usuario utiliza como segundo dígito el último número de mi carnet (`202300611` $\rightarrow$ `X = 1`). Adicionalmente, se implementó la VLAN 99 como VLAN Nativa para gestión de enlaces troncales y la VLAN 999 (Blackhole) para aislar todos los puertos inactivos.

A partir del bloque de red `192.168.10.0/24`, se aplicó subnetting con máscara de longitud variable (**VLSM**) ordenando las zonas de mayor a menor demanda de hosts:

| VLAN ID | Nombre de VLAN | Zona Asignada | Hosts Req. | Hosts Disp. | Subred Asignada | Máscara de Subred | Default Gateway (SVI) | Rango de IPs Útiles | Broadcast |
| :---: | :--- | :--- | :---: | :---: | :--- | :--- | :--- | :--- | :--- |
| **11** | `Z1_Paseo_Cayala` | Zona 1 | 60 | 62 | `192.168.10.0/26` | `255.255.255.192` | `192.168.10.1` | `192.168.10.2` - `.62` | `192.168.10.63` |
| **41** | `Z4_Distrito_Empresarial`| Zona 4 | 50 | 62 | `192.168.10.64/26` | `255.255.255.192` | `192.168.10.65` | `192.168.10.66` - `.126` | `192.168.10.127` |
| **21** | `Z2_Decimo` | Zona 2 | 28 | 30 | `192.168.10.128/27`| `255.255.255.224` | `192.168.10.129` | `192.168.10.130` - `.158`| `192.168.10.159` |
| **31** | `Z3_Administracion` | Zona 3 | 12 | 14 | `192.168.10.160/28`| `255.255.255.240` | `192.168.10.161` | `192.168.10.162` - `.174`| `192.168.10.175` |
| **51** | `Z5_Lirios_Residencial` | Zona 5 | 7 | 14 | `192.168.10.176/28`| `255.255.255.240` | `192.168.10.177` | `192.168.10.178` - `.190`| `192.168.10.191` |
| **99** | `Nativa_Gestion` | Trunks | N/A | 14 | `192.168.10.192/28`| `255.255.255.240` | `192.168.10.193` | `192.168.10.194` - `.206`| `192.168.10.207` |
| **999** | `Blackhole_No_Usados` | Inactivos| 0 | N/A | Sin direccionamiento | N/A | N/A | Puertos en `shutdown` | N/A |

### 3.1. Tabla de Asignación de Direcciones IP en Hosts Simulados

| Zona | Host (PC) | Switch de Acceso Conectado | Puerto | VLAN | Dirección IPv4 | Máscara de Subred | Default Gateway |
| :--- | :---: | :--- | :---: | :---: | :--- | :--- | :--- |
| **Zona 1** | `PC9` | `SW-CLI1-Z1` (`Switch2`) | `Fa0/1` | 11 | `192.168.10.2` | `255.255.255.192` | `192.168.10.1` |
| **Zona 1** | `PC10` | `SW-CLI1-Z1` (`Switch2`) | `Fa0/2` | 11 | `192.168.10.3` | `255.255.255.192` | `192.168.10.1` |
| **Zona 1** | `PC11` | `SW-CLI2-Z1` (`Switch3`) | `Fa0/1` | 11 | `192.168.10.4` | `255.255.255.192` | `192.168.10.1` |
| **Zona 1** | `PC12` | `SW-CLI2-Z1` (`Switch3`) | `Fa0/2` | 11 | `192.168.10.5` | `255.255.255.192` | `192.168.10.1` |
| **Zona 2** | `PC13` | `SW-CLI-Z2` (`Switch7`) | `Fa0/1` | 21 | `192.168.10.130` | `255.255.255.224` | `192.168.10.129` |
| **Zona 2** | `PC14` | `SW-CLI-Z2` (`Switch7`) | `Fa0/2` | 21 | `192.168.10.131` | `255.255.255.224` | `192.168.10.129` |
| **Zona 2** | `PC15` | `SW-CLI-Z2` (`Switch7`) | `Fa0/3` | 21 | `192.168.10.132` | `255.255.255.224` | `192.168.10.129` |
| **Zona 2** | `PC16` | `SW-CLI-Z2` (`Switch7`) | `Fa0/4` | 21 | `192.168.10.133` | `255.255.255.224` | `192.168.10.129` |
| **Zona 3** | `PC4` | `SW-CLI-Z3` (`Switch8`) | `Fa0/1` | 31 | `192.168.10.165` | `255.255.255.240` | `192.168.10.161` |
| **Zona 3** | `PC2` | `SW-CLI-Z3` (`Switch8`) | `Fa0/2` | 31 | `192.168.10.163` | `255.255.255.240` | `192.168.10.161` |
| **Zona 3** | `PC1` | `SW-CLI-Z3` (`Switch8`) | `Fa0/3` | 31 | `192.168.10.162` | `255.255.255.240` | `192.168.10.161` |
| **Zona 3** | `PC3` | `SW-CLI-Z3` (`Switch8`) | `Fa0/4` | 31 | `192.168.10.164` | `255.255.255.240` | `192.168.10.161` |
| **Zona 4** | `PC5` | `SW-CLI1-Z4` (`Switch4`) | `Fa0/1` | 41 | `192.168.10.66` | `255.255.255.192` | `192.168.10.65` |
| **Zona 4** | `PC6` | `SW-CLI1-Z4` (`Switch4`) | `Fa0/2` | 41 | `192.168.10.67` | `255.255.255.192` | `192.168.10.65` |
| **Zona 4** | `PC7` | `SW-CLI2-Z4` (`Switch5`) | `Fa0/1` | 41 | `192.168.10.68` | `255.255.255.192` | `192.168.10.65` |
| **Zona 4** | `PC8` | `SW-CLI2-Z4` (`Switch5`) | `Fa0/2` | 41 | `192.168.10.69` | `255.255.255.192` | `192.168.10.65` |
| **Zona 5** | `PC17` | `SW-CLI-Z5` (`Switch6`) | `Fa0/1` | 51 | `192.168.10.178` | `255.255.255.240` | `192.168.10.177` |
| **Zona 5** | `PC18` | `SW-CLI-Z5` (`Switch6`) | `Fa0/2` | 51 | `192.168.10.179` | `255.255.255.240` | `192.168.10.177` |
| **Zona 5** | `PC19` | `SW-CLI-Z5` (`Switch6`) | `Fa0/3` | 51 | `192.168.10.180` | `255.255.255.240` | `192.168.10.177` |
| **Zona 5** | `PC20` | `SW-CLI-Z5` (`Switch6`) | `Fa0/4` | 51 | `192.168.10.181` | `255.255.255.240` | `192.168.10.177` |

---

## 4. Configuración de Protocolos de Capa 2

### 4.1. Seguridad en Enlaces Troncales y Puertos de Acceso
* **Encapsulamiento y Modo Troncal:** Todos los enlaces entre switches se configuraron explícitamente en modo `trunk` utilizando encapsulamiento IEEE `802.1Q` (`dot1q`).
* **VLAN Nativa (ID 99):** Se modificó la VLAN nativa por defecto (VLAN 1) a la `VLAN 99` en ambos extremos de todos los enlaces troncales (`switchport trunk native vlan 99`) para aislar el tráfico de control no etiquetado.
* **Restricción de VLANs Permitidas:** Se evitó el uso de `allow all` aplicando el comando `switchport trunk allowed vlan 11,21,31,41,51,99`, impidiendo que el tráfico de la VLAN 1 por defecto o de la VLAN Blackhole (999) circule por los enlaces troncales.
* **VLAN Blackhole (ID 999):** Todos los puertos físicos que no tienen dispositivos conectados fueron forzados a modo acceso (`switchport mode access`), asignados a la `VLAN 999` (`switchport access vlan 999`) e inhabilitados administrativamente (`shutdown`), mitigando accesos físicos no autorizados.

### 4.2. Protocolo de Enlace Troncal de VLAN (VTP)
* **Dominio VTP:** `202300611` (Número de carnet).
* **Contraseña VTP:** `123`
* **Distribución de Roles:**
  * **Servidores VTP (`vtp mode server`):** Configurado en los 5 switches principales de cada zona (`SW-PRIN-Z1`, `SW-PRIN-Z2`, `SW-PRIN-Z3`, `SW-PRIN-Z4` y `SW-PRIN-Z5`).
  * **Clientes VTP (`vtp mode client`):** Configurado en los 7 switches secundarios de capa de acceso (`SW-CLI1-Z1`, `SW-CLI2-Z1`, `SW-CLI-Z2`, `SW-CLI-Z3`, `SW-CLI1-Z4`, `SW-CLI2-Z4` y `SW-CLI-Z5`), los cuales sincronizan dinámicamente la base de datos de VLANs a través de los enlaces troncales.

### 4.3. Spanning Tree Protocol (Rapid PVST+) y Selección de Root Bridge
Se habilitó **Rapid PVST+** (`spanning-tree mode rapid-pvst`) en los 12 switches de la red para prevenir bucles de Capa 2 en la malla parcial y lograr tiempos de convergencia inmediatos. Asimismo, en los puertos de acceso conectados a las PCs se habilitó `spanning-tree portfast`.

Cada switch principal fue configurado explícitamente como Root Bridge de la VLAN correspondiente a su zona asignando una prioridad base de **`4096`** (inferior al valor por defecto de `32768`):

| VLAN ID | Nombre de Zona | Switch Designado como Root Bridge | Prioridad Base Configurada | Prioridad Resultante (Base + System ID Ext) | Justificación Técnica de la Selección |
| :---: | :--- | :--- | :---: | :---: | :--- |
| **11** | Zona 1 - Paseo Cayalá | `SW-PRIN-Z1` (`Multilayer Switch1`) | `4096` | `4107` (`4096 + 11`) | Centraliza el árbol de expansión de la VLAN 11 en el switch de distribución de Paseo Cayalá, haciendo que el tráfico intra-VLAN entre `SW-CLI1-Z1` y `SW-CLI2-Z1` se conmute localmente sin dar rodeos por otras zonas. |
| **21** | Zona 2 - Décimo | `SW-PRIN-Z2` (`Switch0`) | `4096` | `4117` (`4096 + 21`) | Garantiza que ambos enlaces ascendentes de Zona 2 (hacia Zona 1 y Zona 3) permanezcan en estado Designated Forwarding para la VLAN 21, optimizando la salida de tráfico de Décimo. |
| **31** | Zona 3 - Administración | `SW-PRIN-Z3` (`Multilayer Switch0`) | `4096` | `4127` (`4096 + 31`) | Ubica la raíz de la VLAN administrativa directamente en el switch núcleo donde residen las interfaces SVI de Capa 3, minimizando la latencia para los servidores y estaciones de gestión. |
| **41** | Zona 4 - Distrito Empresarial| `SW-PRIN-Z4` (`Multilayer Switch2`) | `4096` | `4137` (`4096 + 41`) | Permite que el tráfico local entre los switches de acceso de las torres corporativas (`SW-CLI1-Z4` y `SW-CLI2-Z4`) converja directamente en su switch principal sin atravesar enlaces externos. |
| **51** | Zona 5 - Lirios Residencial | `SW-PRIN-Z5` (`Switch1`) | `4096` | `4147` (`4096 + 51`) | Cumple con la distribución por zona asegurando que `SW-PRIN-Z5` lidere la topología lógica de la VLAN 51 hacia su switch cliente local (`SW-CLI-Z5`). |
| **99** | VLAN Nativa / Gestión | `SW-PRIN-Z3` (`Multilayer Switch0`) | `4096` | `4195` (`4096 + 99`) | Alinea el tráfico de gestión de enlaces troncales hacia el nodo central de Administración. |

(Con esta distribución de prioridades por instancia de VLAN se logra además balanceo de carga en Capa 2, ya que diferentes enlaces de la malla parcial bloquean o reenvían tramas según la VLAN que transportan).

### 4.4. Agrupación de Enlaces con EtherChannel (PAgP)
Dado que el último dígito del carnet `202300611` es **`1` (impar)**, se implementó el protocolo de agregación propietario de Cisco **PAgP (Port Aggregation Protocol)** configurando los puertos en modo activo **`desirable`** (`channel-group X mode desirable`):

1. **Port-channel 1 (`Po1`):** Agrupa los puertos `Fa0/1` y `Fa0/2` entre **`SW-PRIN-Z3` (Administración)** y **`SW-PRIN-Z1` (Paseo Cayalá)**.
   * **Justificación:** La Zona 1 alberga 60 hosts con tráfico comercial muy alto. Un solo puerto FastEthernet (100 Mbps) representaría un cuello de botella al comunicarse con el Default Gateway en Zona 3; al agrupar 2 enlaces físicos con PAgP se duplica el ancho de banda a 200 Mbps y se obtiene tolerancia a fallos dentro del mismo canal lógico.
2. **Port-channel 2 (`Po2`):** Agrupa los puertos `Fa0/3` y `Fa0/4` entre **`SW-PRIN-Z3` (Administración)** y **`SW-PRIN-Z4` (Distrito Empresarial)**.
   * **Justificación:** La Zona 4 concentra 50 hosts corporativos y además sirve como puente de tránsito para el tráfico proveniente de la Zona 5 (7 hosts adicionales). El canal agregado `Po2` garantiza capacidad de sobra y redundancia inmediata hacia el switch de enrutamiento central.

---

## 5. Análisis de Dominios de Colisión y Dominios de Broadcast

### 5.1. Dominios de Colisión
* **Dispositivos que los delimitan:** Los Switches de Capa 2 y Capa 3 (Cisco 2960 y 3560). A diferencia de los hubs antiguos (Capa 1), cada puerto activo de un switch opera en modo Full-Duplex y microsegmenta la red, constituyendo un dominio de colisión independiente por cada enlace punto a punto.
* **Conteo exacto en la topología implementada:**
  1. **Enlaces Host a Switch de Acceso:** 20 PCs conectadas = **20 dominios de colisión**.
  2. **Enlaces Switch Cliente a Switch Principal (Intra-zona):** 7 enlaces troncales internos (2 en Zona 1, 1 en Zona 2, 1 en Zona 3, 2 en Zona 4 y 1 en Zona 5) = **7 dominios de colisión**.
  3. **Enlaces entre Switches Principales (Inter-zona):**
     * A nivel de interfaces físicas activas: 2 cables en `Po1` + 2 cables en `Po2` + 1 cable (`Z1-Z4`) + 2 cables (`Z2-Z1` y `Z2-Z3`) + 1 cable (`Z5-Z4`) = **8 enlaces físicos**.
     * (Si se contabilizan los 2 EtherChannels `Po1` y `Po2` como enlaces lógicos unificados, equivalen a **6 enlaces troncales inter-zona**).
  * **Total de Dominios de Colisión:** Existen **35 dominios de colisión físicos** (o **33 dominios lógicos** considerando la agregación de `Po1` y `Po2`), todos de tamaño mínimo (punto a punto entre 2 interfaces) y libres de colisiones gracias a la operación Full-Duplex.
* **Por qué esta segmentación es adecuada:** Al dedicar un puerto de switch exclusivo para cada host y enlace troncal, se elimina por completo la contienda por el medio físico (CSMA/CD), permitiendo que las transacciones comerciales de Paseo Cayalá o las videollamadas del Distrito Empresarial se transmitan simultáneamente a máxima velocidad sin colisiones de tramas.

### 5.2. Dominios de Broadcast
* **Dispositivos y mecanismos que los delimitan:** Las **VLANs** en Capa 2 y el **Switch Multicapa (`SW-PRIN-Z3`)** con sus interfaces virtuales **SVI** en Capa 3.
* **Conteo exacto en la topología:** Existen **6 dominios de broadcast** activos (`VLAN 11`, `VLAN 21`, `VLAN 31`, `VLAN 41`, `VLAN 51` y `VLAN 99`), más 1 dominio aislado inactivo (`VLAN 999`).
* **Justificación:** El tráfico de difusión (como solicitudes ARP) generado por una computadora en Paseo Cayalá (`VLAN 11`) queda estrictamente confinado a los puertos que pertenecen a la `VLAN 11` y no consume ancho de banda ni procesamiento en los hosts de Administración (`VLAN 31`) o del área Residencial (`VLAN 51`). La comunicación entre dominios distintos ocurre únicamente mediante enrutamiento IP (`ip routing`) a través de las puertas de enlace predeterminadas (SVIs) configuradas en `SW-PRIN-Z3`.