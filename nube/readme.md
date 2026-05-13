#  Proyecto Hotel - Despliegue de Infraestructura en la Nube (AWS)

---

##  Justificación del Proveedor (AWS)
Se ha seleccionado **Amazon Web Services** por los siguientes motivos:
* [cite_start]**Modelo de Pago por Uso:** Permite ajustar recursos según la ocupación del hotel (temporada alta/baja)[cite: 10].
* [cite_start]**Capa Gratuita (Free Tier):** Posibilidad de probar servicios durante 12 meses con coste \$0[cite: 11].
* [cite_start]**Seguridad y Segmentación:** Herramientas avanzadas para replicar el diseño de VLANs físicas de forma virtual[cite: 12].

---

##  Arquitectura Propuesta

[cite_start]La arquitectura se divide en tres zonas principales dentro de una **VPC (10.0.0.0/16)**[cite: 46]:

### 1. Subred Pública (DMZ)
* [cite_start]**Bastion Host:** Única puerta de entrada para administración mediante SSH/RDP[cite: 38, 41].
* [cite_start]**NAT Gateway:** Permite que las instancias en subredes privadas descarguen actualizaciones sin ser expuestas a Internet[cite: 51].

### 2. Subred Privada - Huéspedes (VLAN 20)
* [cite_start]**Portal Cautivo:** Instancias **EC2** que alojan la web de bienvenida[cite: 15].
* [cite_start]**Acceso:** El tráfico llega a través de un **Internet Gateway (IGW)** y un **Application Load Balancer (ALB)**[cite: 34].

### 3. Subred Privada - Admin & Servidores (VLAN 10)
* [cite_start]**Servidor PMS y NAS:** Instancias EC2 aisladas que ejecutan la lógica de gestión y almacenamiento[cite: 18, 19].
* [cite_start]**Base de Datos (Amazon RDS):** Servicio gestionado para la base de datos del PMS, facilitando copias de seguridad y mantenimiento[cite: 24, 26].
* [cite_start]**Backups S3:** Almacenamiento duradero para copias de seguridad y archivos del hotel[cite: 21, 75].

---

##  Servicios Cloud Utilizados

| Servicio AWS | Equivalente Físico / Función |
| :--- | :--- |
| **Amazon EC2** | [cite_start]Sustituye a los servidores físicos (PMS, NAS y Web)[cite: 68]. |
| **Amazon RDS** | [cite_start]Base de datos gestionada para reservas y huéspedes[cite: 70]. |
| **Amazon VPC** | [cite_start]Sustituye al Router y Multilayer Switch; gestiona el direccionamiento[cite: 72, 73]. |
| **Security Groups** | [cite_start]Firewall virtual para segmentar el tráfico entre zonas[cite: 74]. |
| **Amazon S3** | [cite_start]Almacenamiento de archivos y facturas de forma económica[cite: 75]. |

---

##  Estimación de Costes (Mensual)

| Servicio | Concepto | Coste Estimado |
| :--- | :--- | :--- |
| **Computación (EC2)** | 1 Instancia t3.micro | [cite_start]\$9.00 (o \$0.00 Free Tier) [cite: 78] |
| **Base de Datos (RDS)** | 1 Instancia db.t3.micro | [cite_start]\$15.00 (o \$0.00 Free Tier) [cite: 78] |
| **Almacenamiento (S3)** | 20 GB + Transferencia | [cite_start]\$1.50 [cite: 78] |
| **Transferencia Datos** | Tráfico saliente | [cite_start]\$5.00 [cite: 78] |
| **Networking** | Reglas de red y Gateway | [cite_start]\$0.00 [cite: 78] |
| **TOTAL** | | [cite_start]**~\$21.50/mes** [cite: 78] |

---

##  Seguridad
* [cite_start]**Aislamiento:** Las instancias críticas no tienen IP pública directa[cite: 35].
* [cite_start]**Acceso Seguro:** El personal técnico solo puede acceder a través del Bastion Host autenticado[cite: 41, 42].
* [cite_start]**Control de Tráfico:** Los Security Groups restringen el acceso a la base de datos únicamente desde las instancias autorizadas[cite: 27].
