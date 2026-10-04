# Código Fuente CLI - Red de la Ciudad Comercial Cayalá
**Curso:** Redes de Computadoras 1 
**Universidad de San Carlos de Guatemala (USAC) - Facultad de Ingeniería**  
**Estudiante:** David Arturo Chanquin Velasquez  
**Carnet:** 202300611  

---

## 1. Mapeo de Dispositivos de Red

| Zona en Ciudad Cayalá | Nombre en Packet Tracer | Hostname CLI | Modelo Cisco | Rol en la Topología |
| :--- | :--- | :--- | :--- | :--- |
| **Zona 3 (Administración)** | `Multilayer Switch0` | `SW-PRIN-Z3` | Catalyst 3560-24PS | Switch Principal / Core L3 (SVIs) / VTP Server / Root VLAN 31 y 99 |
| **Zona 1 (Paseo Cayalá)** | `Multilayer Switch1` | `SW-PRIN-Z1` | Catalyst 3560-24PS | Switch Principal / VTP Server / Root VLAN 11 |
| **Zona 4 (Distrito Empresarial)** | `Multilayer Switch2` | `SW-PRIN-Z4` | Catalyst 3560-24PS | Switch Principal / VTP Server / Root VLAN 41 |
| **Zona 2 (Décimo)** | `Switch0` | `SW-PRIN-Z2` | Catalyst 2960-24TT | Switch Principal / VTP Server / Root VLAN 21 |
| **Zona 5 (Lirios Residencial)** | `Switch1` | `SW-PRIN-Z5` | Catalyst 2960-24TT | Switch Principal / VTP Server / Root VLAN 51 |
| **Zona 1 (Paseo Cayalá)** | `Switch2` | `SW-CLI1-Z1` | Catalyst 2960-24TT | Switch de Acceso / VTP Client (PC9, PC10) |
| **Zona 1 (Paseo Cayalá)** | `Switch3` | `SW-CLI2-Z1` | Catalyst 2960-24TT | Switch de Acceso / VTP Client (PC11, PC12) |
| **Zona 2 (Décimo)** | `Switch7` | `SW-CLI-Z2` | Catalyst 2960-24TT | Switch de Acceso / VTP Client (PC13 a PC16) |
| **Zona 3 (Administración)** | `Switch8` | `SW-CLI-Z3` | Catalyst 2960-24TT | Switch de Acceso / VTP Client (PC1 a PC4) |
| **Zona 4 (Distrito Empresarial)** | `Switch4` | `SW-CLI1-Z4` | Catalyst 2960-24TT | Switch de Acceso / VTP Client (PC5, PC6) |
| **Zona 4 (Distrito Empresarial)** | `Switch5` | `SW-CLI2-Z4` | Catalyst 2960-24TT | Switch de Acceso / VTP Client (PC7, PC8) |
| **Zona 5 (Lirios Residencial)** | `Switch6` | `SW-CLI-Z5` | Catalyst 2960-24TT | Switch de Acceso / VTP Client (PC17 a PC20) |

---

## 2. Scripts de Switches Principales

### 2.1. Zona 3 (Administración Central) - `SW-PRIN-Z3` (`Multilayer Switch0` - Cisco 3560)
Incluye la configuración de capa 2 (Rapid PVST+, EtherChannel PAgP grupos 1 y 2, Trunks, VTP Server, VLANs, Blackhole) y el enrutamiento Inter-VLAN de capa 3 (`ip routing` y SVIs).

```ios
enable
configure terminal

hostname SW-PRIN-Z3
spanning-tree mode rapid-pvst

! Grupo 1 hacia SW-PRIN-Z1 (Paseo Cayalá)
interface range fa0/1 - 2
 channel-group 1 mode desirable
 exit

! Grupo 2 hacia SW-PRIN-Z4 (Distrito Empresarial)
interface range fa0/3 - 4
 channel-group 2 mode desirable
 exit

! --- 3. Configuración de Enlaces Troncales, VLAN Nativa 99 y Restricción de VLANs ---
interface port-channel 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

interface port-channel 2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

interface range fa0/1 - 4, fa0/24, gi0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

! --- 4. Configuración de Servidor VTP ---
vtp mode server
vtp domain 202300611
vtp password 123

! --- 5. Creación de Estructura de VLANs ---
vlan 11
 name Z1_Paseo_Cayala
vlan 21
 name Z2_Decimo
vlan 31
 name Z3_Administracion
vlan 41
 name Z4_Distrito_Empresarial
vlan 51
 name Z5_Lirios_Residencial
vlan 99
 name Nativa_Gestion
vlan 999
 name Blackhole_No_Usados
 exit

! --- 6. Prioridad STP (Root Bridge para VLAN 31 y VLAN 99) ---
spanning-tree vlan 31,99 priority 4096

! --- 7. Seguridad Blackhole (Aislamiento de puertos inactivos) ---
interface range fa0/5 - 23, gi0/2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

! --- 8. Enrutamiento Inter-VLAN (Capa 3) y Default Gateways (SVI) ---
ip routing

interface vlan 11
 description Gateway_Z1_Paseo_Cayala
 ip address 192.168.10.1 255.255.255.192
 no shutdown
 exit

interface vlan 21
 description Gateway_Z2_Decimo
 ip address 192.168.10.129 255.255.255.224
 no shutdown
 exit

interface vlan 31
 description Gateway_Z3_Administracion
 ip address 192.168.10.161 255.255.255.240
 no shutdown
 exit

interface vlan 41
 description Gateway_Z4_Distrito_Empresarial
 ip address 192.168.10.65 255.255.255.192
 no shutdown
 exit

interface vlan 51
 description Gateway_Z5_Lirios_Residencial
 ip address 192.168.10.177 255.255.255.240
 no shutdown
 exit

interface vlan 99
 description Gateway_Nativa_Gestion
 ip address 192.168.10.193 255.255.255.240
 no shutdown
 exit

end
write memory
```

---

### 2.2. Zona 1 (Paseo Cayalá) - `SW-PRIN-Z1` (`Multilayer Switch1` - Cisco 3560)

```ios
enable
configure terminal

! --- 1. Nombre de dispositivo y Spanning Tree ---
hostname SW-PRIN-Z1
spanning-tree mode rapid-pvst

! --- 2. EtherChannel PAgP Grupo 1 (Conecta con SW-PRIN-Z3) ---
interface range fa0/1 - 2
 channel-group 1 mode desirable
 exit

! --- 3. Configuración del Port-channel 1 como Trunk ---
interface port-channel 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

! --- 4. Configuración de puertos físicos troncales ---
interface range fa0/1 - 2, fa0/23 - 24, gi0/1 - 2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

! --- 5. Configuración de Servidor VTP ---
vtp mode server
vtp domain 202300611
vtp password 123

! --- 6. Declaración de VLANs ---
vlan 11
 name Z1_Paseo_Cayala
vlan 21
 name Z2_Decimo
vlan 31
 name Z3_Administracion
vlan 41
 name Z4_Distrito_Empresarial
vlan 51
 name Z5_Lirios_Residencial
vlan 99
 name Nativa_Gestion
vlan 999
 name Blackhole_No_Usados
 exit

! --- 7. Prioridad STP (Root Bridge para VLAN 11) ---
spanning-tree vlan 11 priority 4096

! --- 8. Seguridad Blackhole (Puertos no utilizados Fa0/3 al 22) ---
interface range fa0/3 - 22
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

end
write memory
```

---

### 2.3. Zona 4 (Distrito Empresarial) - `SW-PRIN-Z4` (`Multilayer Switch2` - Cisco 3560)

```ios
enable
configure terminal

! --- 1. Nombre de dispositivo y Spanning Tree ---
hostname SW-PRIN-Z4
spanning-tree mode rapid-pvst

! --- 2. EtherChannel PAgP Grupo 2 (Conecta con SW-PRIN-Z3) ---
interface range fa0/3 - 4
 channel-group 2 mode desirable
 exit

! --- 3. Configuración del Port-channel 2 como Trunk ---
interface port-channel 2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

! --- 4. Configuración de puertos físicos troncales ---
interface range fa0/3 - 4, fa0/23 - 24, gi0/1 - 2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

! --- 5. Configuración de Servidor VTP ---
vtp mode server
vtp domain 202300611
vtp password 123

! --- 6. Declaración de VLANs ---
vlan 11
 name Z1_Paseo_Cayala
vlan 21
 name Z2_Decimo
vlan 31
 name Z3_Administracion
vlan 41
 name Z4_Distrito_Empresarial
vlan 51
 name Z5_Lirios_Residencial
vlan 99
 name Nativa_Gestion
vlan 999
 name Blackhole_No_Usados
 exit

! --- 7. Prioridad STP (Root Bridge para VLAN 41) ---
spanning-tree vlan 41 priority 4096

! --- 8. Seguridad Blackhole (Puertos no utilizados Fa0/1-2 y Fa0/5-22) ---
interface range fa0/1 - 2, fa0/5 - 22
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

end
write memory
```

---

### 2.4. Zona 2 (Décimo de Cayalá) - `SW-PRIN-Z2` (`Switch0` - Cisco 2960)

```ios
enable
configure terminal

! --- 1. Nombre de dispositivo y Spanning Tree ---
hostname SW-PRIN-Z2
spanning-tree mode rapid-pvst

! --- 2. Configuración de Enlaces Troncales ---
interface range fa0/1 - 2, gi0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

! --- 3. Configuración de Servidor VTP ---
vtp mode server
vtp domain 202300611
vtp password 123

! --- 4. Declaración de VLANs ---
vlan 11
 name Z1_Paseo_Cayala
vlan 21
 name Z2_Decimo
vlan 31
 name Z3_Administracion
vlan 41
 name Z4_Distrito_Empresarial
vlan 51
 name Z5_Lirios_Residencial
vlan 99
 name Nativa_Gestion
vlan 999
 name Blackhole_No_Usados
 exit

! --- 5. Prioridad STP (Root Bridge para VLAN 21) ---
spanning-tree vlan 21 priority 4096

! --- 6. Seguridad Blackhole (Puertos libres Fa0/3 al 24 y Gi0/2) ---
interface range fa0/3 - 24, gi0/2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

end
write memory
```

---

### 2.5. Zona 5 (Lirios Residencial) - `SW-PRIN-Z5` (`Switch1` - Cisco 2960)

```ios
enable
configure terminal

! --- 1. Nombre de dispositivo y Spanning Tree ---
hostname SW-PRIN-Z5
spanning-tree mode rapid-pvst

! --- 2. Configuración de Enlaces Troncales ---
interface range gi0/1 - 2
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

! --- 3. Configuración de Servidor VTP ---
vtp mode server
vtp domain 202300611
vtp password 123

! --- 4. Declaración de VLANs ---
vlan 11
 name Z1_Paseo_Cayala
vlan 21
 name Z2_Decimo
vlan 31
 name Z3_Administracion
vlan 41
 name Z4_Distrito_Empresarial
vlan 51
 name Z5_Lirios_Residencial
vlan 99
 name Nativa_Gestion
vlan 999
 name Blackhole_No_Usados
 exit

! --- 5. Prioridad STP (Root Bridge para VLAN 51) ---
spanning-tree vlan 51 priority 4096

! --- 6. Seguridad Blackhole (Puertos libres Fa0/1 al 24) ---
interface range fa0/1 - 24
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

end
write memory
```

---

## 3. Scripts de Switches Clientes (Capa de Acceso y Clientes VTP)

### 3.1. Zona 1 (Paseo Cayalá) - `SW-CLI1-Z1` (`Switch2` - Cisco 2960)

```ios
enable
configure terminal

hostname SW-CLI1-Z1
spanning-tree mode rapid-pvst

interface gi0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

vtp domain 202300611
vtp password 123

! Puertos de acceso para PC9 y PC10 (VLAN 11)
interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 11
 spanning-tree portfast
 exit

! Puertos inactivos a Blackhole 999
interface range fa0/3 - 24, gi0/2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

vtp mode client

end
write memory
```

### 3.2. Zona 1 (Paseo Cayalá) - `SW-CLI2-Z1` (`Switch3` - Cisco 2960)

```ios
enable
configure terminal

hostname SW-CLI2-Z1
spanning-tree mode rapid-pvst

interface gi0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

vtp domain 202300611
vtp password 123

! Puertos de acceso para PC11 y PC12 (VLAN 11)
interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 11
 spanning-tree portfast
 exit

! Puertos inactivos a Blackhole 999
interface range fa0/3 - 24, gi0/2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

vtp mode client

end
write memory
```

### 3.3. Zona 2 (Décimo de Cayalá) - `SW-CLI-Z2` (`Switch7` - Cisco 2960)

```ios
enable
configure terminal

hostname SW-CLI-Z2
spanning-tree mode rapid-pvst

interface gi0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

vtp domain 202300611
vtp password 123

! Puertos de acceso para PC13, PC14, PC15 y PC16 (VLAN 21)
interface range fa0/1 - 4
 switchport mode access
 switchport access vlan 21
 spanning-tree portfast
 exit

! Puertos inactivos a Blackhole 999
interface range fa0/5 - 24, gi0/2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

vtp mode client

end
write memory
```

### 3.4. Zona 3 (Administración Central) - `SW-CLI-Z3` (`Switch8` - Cisco 2960)

```ios
enable
configure terminal

hostname SW-CLI-Z3
spanning-tree mode rapid-pvst

interface gi0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

vtp domain 202300611
vtp password 123

! Puertos de acceso para PC1, PC2, PC3 y PC4 (VLAN 31)
interface range fa0/1 - 4
 switchport mode access
 switchport access vlan 31
 spanning-tree portfast
 exit

! Puertos inactivos a Blackhole 999
interface range fa0/5 - 24, gi0/2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

vtp mode client

end
write memory
```

### 3.5. Zona 4 (Distrito Empresarial) - `SW-CLI1-Z4` (`Switch4` - Cisco 2960)

```ios
enable
configure terminal

hostname SW-CLI1-Z4
spanning-tree mode rapid-pvst

interface gi0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

vtp domain 202300611
vtp password 123

! Puertos de acceso para PC5 y PC6 (VLAN 41)
interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 41
 spanning-tree portfast
 exit

! Puertos inactivos a Blackhole 999
interface range fa0/3 - 24, gi0/2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

vtp mode client

end
write memory
```

### 3.6. Zona 4 (Distrito Empresarial) - `SW-CLI2-Z4` (`Switch5` - Cisco 2960)

```ios
enable
configure terminal

hostname SW-CLI2-Z4
spanning-tree mode rapid-pvst

interface gi0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

vtp domain 202300611
vtp password 123

! Puertos de acceso para PC7 y PC8 (VLAN 41)
interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 41
 spanning-tree portfast
 exit

! Puertos inactivos a Blackhole 999
interface range fa0/3 - 24, gi0/2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

vtp mode client

end
write memory
```

### 3.7. Zona 5 (Lirios Residencial) - `SW-CLI-Z5` (`Switch6` - Cisco 2960)

```ios
enable
configure terminal

hostname SW-CLI-Z5
spanning-tree mode rapid-pvst

interface gi0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,21,31,41,51,99
 exit

vtp domain 202300611
vtp password 123

! Puertos de acceso para PC17, PC18, PC19 y PC20 (VLAN 51)
interface range fa0/1 - 4
 switchport mode access
 switchport access vlan 51
 spanning-tree portfast
 exit

! Puertos inactivos a Blackhole 999
interface range fa0/5 - 24, gi0/2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit

vtp mode client

end
write memory
```

---

## 4. Comandos de Verificación y Diagnóstico Utilizados

```ios
! Verificación de canales agregados PAgP
show etherchannel summary

! Verificación de estado y sincronización del dominio VTP
show vtp status

! Verificación de asignación de VLANs y aislamiento Blackhole (VLAN 999)
show vlan brief

! Verificación de enlaces troncales, VLAN nativa 99 y VLANs permitidas
show interfaces trunk

! Verificación de Root Bridge y prioridades por VLAN en Rapid PVST+
show spanning-tree vlan 11
show spanning-tree vlan 21
show spanning-tree vlan 31
show spanning-tree vlan 41
show spanning-tree vlan 51

! Verificación de tabla de enrutamiento Inter-VLAN en SW-PRIN-Z3
show ip route
```