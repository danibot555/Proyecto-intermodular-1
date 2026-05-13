#  Proyecto Intermodular ASIR: Infraestructura de Hardware - Hotel Grand Horizon

Este repositorio contiene el análisis detallado y el diseño de la infraestructura física del **Proyecto Intermodular** de 1º de **Administración de Sistemas Informáticos en Red (ASIR)**. 

El proyecto plantea una solución de **nube híbrida** para un complejo hotelero, combinando la fiabilidad del hardware local (*on-premise*) con la escalabilidad de **Amazon Web Services (AWS)**.

---

## Autor
* **Alumno:** Daniel Madrigal Sánchez
* **Curso:** 1º de Administración de Sistemas Informáticos en Red (ASIR)
* **Fecha:** Mayo 2026
---

## Índice
1. [Análisis de Necesidades](#-análisis-de-necesidades)
2. [Infraestructura Local (On-Premise)](#-infraestructura-local-on-premise)
3. [Infraestructura Cloud (AWS)](#-infraestructura-cloud-aws)
4. [Componentes Técnicos y Utilidades](#-componentes-técnicos-y-utilidades)
5. [Presupuesto e Inventario](#-presupuesto-e-inventario)
6. [Mejoras y Escalabilidad](#-mejoras-y-escalabilidad)

---

##  Análisis de Necesidades
Tras estudiar la topología de red (diseñada en Cisco Packet Tracer), se han identificado las siguientes necesidades de hardware:

* **Disponibilidad:** El servidor PMS (Property Management System) debe funcionar 24/7.
* **Seguridad de Datos:** Implementación de sistemas RAID para evitar la pérdida de información de huéspedes.
* **Segmentación:** Separación física/lógica de la red en **VLAN 10 (Administración)** y **VLAN 20 (Huéspedes)**.
* **Presencia Web:** Delegación del motor de reservas a la nube para garantizar acceso global.

---

##  Infraestructura Local (On-Premise)

### Servidores Críticos
* **Servidor PMS/DB:** Nodo central con procesador Intel Xeon y memoria ECC.
* **Servidor NAS:** Unidad de almacenamiento masivo para backups y logs de seguridad.

### Red y Conectividad
* **Router Cisco 2911:** Puerta de enlace y gestión de VPN con la nube.
* **Switch Multicapa 3650:** Núcleo de la red que gestiona el enrutamiento inter-VLAN.
* **Switches de Acceso 2960:** Distribución de red hacia los puestos finales.

---

## Infraestructura Cloud (AWS)
Se han desplegado servicios en la región de **AWS Europe (Spain)** para optimizar la latencia:

| Recurso | Tipo de Instancia | Función |
| :--- | :--- | :--- |
| **Computación (EC2)** | `t3.large` | Servidor Web (Apache/Nginx) y Reservas Online. |
| **Almacenamiento (EBS)** | 100GB SSD `gp3` | Almacenamiento persistente de alto rendimiento. |
| **Backup (S3)** | Object Storage | Almacenamiento de copias de seguridad de larga duración. |

---

## Componentes Técnicos y Utilidades

* **CPU (Procesamiento):** Uso de **Intel Xeon Silver** en servidores para manejar múltiples hilos de ejecución simultáneos.
* **Memoria RAM:** Módulos con tecnología **ECC (Error Correcting Code)** para detectar y corregir errores de bits en la base de datos.
* **Almacenamiento Mixto:** * **SSD Enterprise:** Para el Sistema Operativo y la DB (baja latencia).
  * **HDD de Helio:** En el NAS para almacenamiento masivo de backups (coste eficiente).
* **Fuentes Redundantes:** Configuración **N+1** en el rack para evitar caídas ante fallos eléctricos de un módulo.

---

##  Presupuesto e Inventario
*Precios de mercado profesional (Abril 2026).*

| Componente | Modelo | Características | Cantidad | PVP Est. (Sin IVA) |
| :--- | :--- | :--- | :--- | :--- |
| **Servidor** | Intel Xeon Silver 4314 | 16C/32T - 2.4GHz | 1 | 695,00 € |
| **RAM Server** | 16GB DDR4 ECC RDIMM | 3200 MHz | 4 | 480,00 € |
| **Discos NAS** | WD Red Pro 12TB | 7200 RPM / SATA | 4 | 1.840,00 € |
| **PC Admin** | Dell OptiPlex SFF | i5 / 16GB / 512GB | 3 | 2.160,00 € |
| **Switch Core** | Cisco 3650-24PS | PoE / L3 | 1 | 550,00 € |
| **Instancia AWS** | EC2 `t3.large` | 2 vCPU / 8GB RAM | 1 | ~55,00 €/mes |

---

##  Mejoras y Escalabilidad
1. **Virtualización:** Implementación de un hipervisor (Proxmox o VMware) para consolidar servidores físicos.
2. **Alta Disponibilidad:** Configurar un segundo servidor local en modo *failover*.
3. **Migración a NVMe:** Sustituir discos SATA por NVMe en toda la red de administración para mejorar los tiempos de respuesta.

---
> *Nota: Este documento forma parte de la entrega del Proyecto Intermodular de Fundamentos de Hardware.*
