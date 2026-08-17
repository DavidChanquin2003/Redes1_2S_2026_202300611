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