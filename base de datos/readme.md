# 🗄️ Sistema de Gestión de Bases de Datos - PMS Hotel

Este repositorio contiene el diseño, implementación y administración de la base de datos centralizada para el Proyecto Intermodular de 1º de ASIR. El sistema ha sido desarrollado bajo el motor **PostgreSQL** y gestionado mediante **pgAdmin4**, diseñado para soportar la operativa completa de un complejo hotelero.

---

## 📋 Análisis del Sistema de Información

La base de datos está diseñada para gestionar el ciclo de vida completo de una reserva y la trazabilidad de los servicios consumidos. Se ha priorizado la integridad referencial y la normalización para evitar redundancias.

### Entidades Gestionadas:
* **Capital Humano:** Control de empleados y su departamento para auditoría de acciones.
* **Inventario:** Gestión de habitaciones, tipologías y estados de disponibilidad.
* **Clientes:** Almacenamiento de datos de huéspedes siguiendo normativas de registro.
* **Finanzas:** Clasificación de flujos de caja mediante métodos de pago definidos.
* **Operativa:** Gestión de reservas y cargos adicionales de servicios extra.

---

## 📐 Diseño Relacional (E-R)

El diseño sigue la **Tercera Forma Normal (3FN)**. A continuación se detalla la lógica de las relaciones:

* **1:N (Uno a Muchos):** Un empleado puede gestionar múltiples reservas; un huésped puede tener varias estancias; una habitación puede figurar en muchas reservas a lo largo del tiempo.
* **Integridad Referencial:** Uso de `FOREIGN KEY` con restricciones `ON UPDATE CASCADE` y `ON DELETE CASCADE` en tablas de cargos para mantener la consistencia contable.



---

## 🛠️ Especificaciones Técnicas (PostgreSQL)

Se han utilizado características avanzadas de PostgreSQL para optimizar el rendimiento y la validación de datos:

* **Tipos de Datos Personalizados (ENUMS):** Para restringir los valores de estados de habitación, departamentos y estados de reserva, evitando errores de entrada de datos.
* **Serialización:** Uso de `SERIAL` para la generación automática de claves primarias.
* **Restricciones (Constraints):** Implementación de `CHECK` para validar que los precios sean positivos y que las fechas de salida sean posteriores a las de entrada.

---

## 🚀 Implementación y Despliegue

Para replicar la base de datos en un entorno local o en una instancia de **AWS RDS**:

1.  **Creación del Esquema:** Ejecutar el script `01_schema.sql`. Este script limpia el entorno y genera los tipos y tablas en el orden correcto de jerarquía.
2.  **Carga de Datos:** Ejecutar `02_data_seed.sql` para insertar los diccionarios de métodos de pago, habitaciones iniciales y personal de administración.
3.  **Verificación:** Ejecutar el set de consultas en `03_queries.sql` para comprobar la correcta vinculación de las tablas.

---

## 🛡️ Administración de la BBDD (Perfil ASIR)

Como parte de las tareas de administración de sistemas, se incluyen procedimientos para:

### 1. Seguridad y Control de Acceso
* Implementación de roles para diferenciar el acceso entre el personal de recepción (DML limitado) y administración (Full access).
* Configuración de acceso mediante la herramienta de gestión **pgAdmin4**.

### 2. Mantenimiento y Rendimiento
* **Optimización:** Ejecución programada de `VACUUM ANALYZE` para la limpieza de tuplas muertas y actualización de estadísticas de consulta.
* **Auditoría:** Consultas SQL diseñadas para detectar quién realizó cada cargo de servicio o modificación de reserva.

### 3. Estrategia de Backup
* Uso de la utilidad `pg_dump` para realizar backups lógicos.
* Procedimiento de restauración para asegurar la continuidad del negocio ante desastres.

---

## 📂 Contenido del Repositorio

* `/sql/01_schema.sql`: Script DDL (Data Definition Language).
* `/sql/02_data_seed.sql`: Script DML (Data Manipulation Language).
* `/sql/03_queries.sql`: Consultas de ejemplo (SELECT, JOIN, AGGREGATIONS).
* `/docs/ER_Diagram.png`: Diagrama Entidad-Relación detallado.

---
**Autor:** [Tu Nombre]  
**Módulo:** Gestión de Bases de Datos - 1º ASIR  
**Año:** 2026
