#  Proyecto Hotel - Despliegue de Infraestructura en la Nube (AWS)

---

##  Justificación del Proveedor (AWS)
Se ha seleccionado **Amazon Web Services** por los siguientes motivos:
* **Modelo de Pago por Uso:** Permite ajustar recursos según la ocupación del hotel (temporada alta/baja).
* **Capa Gratuita (Free Tier):** Posibilidad de probar servicios durante 12 meses con coste \$0.
* **Seguridad y Segmentación:** Herramientas avanzadas para replicar el diseño de VLANs físicas de forma virtual.

---

##  Arquitectura Propuesta

La arquitectura se divide en tres zonas principales dentro de una **VPC (10.0.0.0/16)**:

### 1. Subred Pública (DMZ)
* **Bastion Host:** Única puerta de entrada para administración mediante SSH/RDP.
* [**NAT Gateway:** Permite que las instancias en subredes privadas descarguen actualizaciones sin ser expuestas a Internet.

### 2. Subred Privada - Huéspedes (VLAN 20)
* **Portal Cautivo:** Instancias **EC2** que alojan la web de bienvenida.
* **Acceso:** El tráfico llega a través de un **Internet Gateway (IGW)** y un **Application Load Balancer (ALB)**.

### 3. Subred Privada - Admin & Servidores (VLAN 10)
* **Servidor PMS y NAS:** Instancias EC2 aisladas que ejecutan la lógica de gestión y almacenamiento.
* **Base de Datos (Amazon RDS):** Servicio gestionado para la base de datos del PMS, facilitando copias de seguridad y mantenimiento.
* **Backups S3:** Almacenamiento duradero para copias de seguridad y archivos del hotel.

---

##  Servicios Cloud Utilizados

| Servicio AWS | Equivalente Físico / Función |
| :--- | :--- |
| **Amazon EC2** | Sustituye a los servidores físicos (PMS, NAS y Web). |
| **Amazon RDS** | Base de datos gestionada para reservas y huéspedes. |
| **Amazon VPC** | Sustituye al Router y Multilayer Switch; gestiona el direccionamiento. |
| **Security Groups** | Firewall virtual para segmentar el tráfico entre zonas. |
| **Amazon S3** | Almacenamiento de archivos y facturas de forma económica. |

---

##  Estimación de Costes (Mensual)

| Servicio | Concepto | Coste Estimado |
| :--- | :--- | :--- |
| **Computación (EC2)** | 1 Instancia t3.micro | \$9.00 (o \$0.00 Free Tier)  |
| **Base de Datos (RDS)** | 1 Instancia db.t3.micro | \$15.00 (o \$0.00 Free Tier)  |
| **Almacenamiento (S3)** | 20 GB + Transferencia | \$1.50  |
| **Transferencia Datos** | Tráfico saliente | \$5.00  |
| **Networking** | Reglas de red y Gateway | \$0.00  |
| **TOTAL** | | **~\$21.50/mes**  |

---

##  Seguridad
* **Aislamiento:** Las instancias críticas no tienen IP pública directa.
* **Acceso Seguro:** El personal técnico solo puede acceder a través del Bastion Host autenticado.
* **Control de Tráfico:** Los Security Groups restringen el acceso a la base de datos únicamente desde las instancias autorizadas.
