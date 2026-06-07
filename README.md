# 🏢 Arquitectura de Red Empresarial: Región Norte-Centro

![Cisco](https://img.shields.io/badge/Cisco-Packet_Tracer-blue?style=for-the-badge&logo=cisco)
![Routing](https://img.shields.io/badge/Routing-EIGRP-lightgrey?style=for-the-badge)
![Switching](https://img.shields.io/badge/Switching-VLANs%20%7C%20HSRP%20%7C%20STP-lightgrey?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completado-success?style=for-the-badge)

##  Resumen Ejecutivo
Diseño, simulación e implementación de una infraestructura de red WAN/LAN convergente y de alta disponibilidad para "La Vaca Lola S.A. de C.V."[cite: 1]. El proyecto interconecta un Headquarters (Durango) con dos plantas satélites de producción (Gómez Palacio y Santiago Papasquiaro) y una red aislada para Puntos de Venta (POS)[cite: 1]. 

El enfoque principal del diseño es la **continuidad del negocio (Business Continuity)**: garantizar que las plantas de producción mantengan su autonomía operativa (servicios de red, bases de datos y gestión documental) incluso ante la pérdida total de los enlaces WAN con la matriz[cite: 1].

---

##  Topología y Diseño de Red

### Estrategia de Direccionamiento (VLSM & IPv6)
Se implementó un diseño de subredes escalable (Dual-Stack IPv4/IPv6)[cite: 1]:
*   **Bloque Principal:** `10.5.0.0/16`[cite: 1]
*   **HQ Durango:** `10.5.0.0/18`[cite: 1]
*   **Planta Gómez Palacio:** `10.5.64.0/18`[cite: 1]
*   **Planta Santiago Papasquiaro:** `10.5.128.0/18`[cite: 1]
*   **Enlaces WAN:** `10.5.255.0/24` (Subdividido en `/30` para eficiencia punto a punto)[cite: 1].
*   **IPv6:** Implementación de SLAAC con prefijos `/64` para asignación dinámica en VLANs operativas[cite: 1].

### Segmentación Lógica (VLANs)
El tráfico fue segmentado para seguridad y optimización de broadcast en 10 redes virtuales por sitio[cite: 1]:
`MGMT (10)`, `TI (20)`, `ADMIN (30)`, `CORRALES (40)`, `RASTRO (50)`, `ALMACÉN (60)`, `VENTAS (70)`, `SERVIDORES (80)`, `WIFI (90)` y `CÁMARAS (100)`[cite: 1].

---

##  Tecnologías y Protocolos Implementados

*   **Enrutamiento Dinámico (EIGRP AS 100):** Elegido por su rápida convergencia y capacidad de sumarización de rutas en infraestructuras Cisco[cite: 1].
*   **Rutas Estáticas Flotantes:** Configuración con distancia administrativa de 200 como mecanismo de *failover* ante caídas de la vecindad EIGRP[cite: 1].
*   **Alta Disponibilidad (HSRP):** Gateways redundantes (Active/Standby) compartiendo IPs virtuales en los switches de distribución de la sede principal[cite: 1].
*   **Agregación de Enlaces (EtherChannel - LACP):** Maximización del ancho de banda y prevención de bucles lógicos en enlaces troncales entre switches multicapa[cite: 1].
*   **Spanning Tree (Rapid-PVST+):** Control determinista de la topología con puentes raíz (Root Bridge) primarios y secundarios configurados manualmente (Prioridades 4096 y 8192)[cite: 1].
*   **Gestión de VLANs (VTP v1):** Sincronización centralizada bajo el dominio `LVL-DGO` (Modos Server/Client)[cite: 1].
*   **Seguridad Inalámbrica:** Redes WiFi 5GHz autenticadas mediante WPA2-PSK para puntos de venta remotos[cite: 1].

---

##  Casos de Uso y Soluciones Técnicas

1.  **Aislamiento Comercial (POS):** Las tiendas operan de manera independiente a la infraestructura de las plantas, accediendo a los servidores centrales vía Web a través de túneles ISP, mitigando el impacto de caídas WAN en el flujo de ventas[cite: 1].
2.  **Operación Offline:** Despliegue de Servidores Web, FTP y Bases de Datos en la VLAN 80 local de cada planta satélite. Esto asegura que la recolección de datos por sensores (RFID, Temperatura) no se interrumpa[cite: 1].

---

##  Despliegue y Pruebas Locales

Este proyecto fue desarrollado utilizando **Cisco Packet Tracer**. 

### Prerrequisitos
*   Cisco Packet Tracer 8.x o superior.

### Instrucciones de Ejecución
1.  Clonar este repositorio.
2.  Abrir el archivo `Red-Planta-Durango.pkt`[cite: 1] en el simulador.
3.  Esperar la convergencia de los protocolos (Spanning Tree y adyacencias EIGRP)[cite: 1].
4.  **Verificación de Enrutamiento:** En cualquier router, acceder a la CLI y ejecutar `show ip route` y `show ip eigrp neighbors`[cite: 1].
5.  **Verificación de Servicios Web:** Desde el host `PC-GOP-ADMIN`, abrir el navegador web e ingresar a `http://10.5.71.2` para visualizar el *Dashboard* de gestión o `http://10.5.71.3` para el Repositorio Documental FTP/HTTP[cite: 1].

> **🔐 Nota de Seguridad:** Por políticas de cumplimiento, las contraseñas de acceso privilegiado (`enable secret`, `vty`, `console`) no se exponen en este documento. El archivo de topología está configurado para propósitos de demostración.
