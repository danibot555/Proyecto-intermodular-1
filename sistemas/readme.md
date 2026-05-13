# Implantación de Sistemas Operativos - Infraestructura Hotel San Tech 🏨

Este repositorio contiene la documentación técnica, scripts de automatización y evidencias de la implantación de los sistemas operativos para el proyecto intermodular del hotel. El enfoque principal es la integración de un entorno de dominio bajo **Windows Server 2019** y terminales de recepción con **Windows 11 Pro**.

## 📑 Índice
1. [Análisis del Sistema](#-análisis-del-sistema)
2. [Entorno de Virtualización](#-entorno-de-virtualización)
3. [Plan de Implantación](#-plan-de-implantación)
4. [Configuración del Servidor (Windows Server 2019)](#-configuración-del-servidor)
5. [Configuración del Cliente (Windows 11 Pro)](#-configuración-del-cliente)
6. [Gestión de Usuarios y Servicios](#-gestión-de-usuarios-y-servicios)

---

## 🧐 Análisis del Sistema

Para cumplir con los requisitos de seguridad y gestión del hotel, se han seleccionado los siguientes sistemas:

| Equipo | Sistema Operativo | Rol / Función | Justificación |
| :--- | :--- | :--- | :--- |
| **Servidor Central** | Windows Server 2019 | DC, DNS, DHCP, File Server | Estabilidad, gestión de Active Directory y soporte para software PMS. |
| **Recepción** | Windows 11 Pro | Cliente de Dominio | Interfaz moderna, seguridad TPM y compatibilidad con dominios locales. |
| **Huéspedes** | Varios (BYOD) | Cliente DHCP | Aislamiento mediante VLAN 20 y direccionamiento dinámico. |

---

## 💻 Entorno de Virtualización

La infraestructura se ha desplegado utilizando **Oracle VM VirtualBox**. 

### Configuración de Red Virtual:
Para emular la topología de red (VLANs), se ha configurado un adaptador de **Red Interna** llamado `VLAN_HOTEL` en ambas máquinas virtuales. Esto garantiza:
* Aislamiento total de la red física.
* Simulación de comunicación de capa 2 entre servidor y cliente.

---

## 🚀 Plan de Implantación

1. **Fase 1:** Instalación de Windows Server 2019 e implementación de AD DS.
2. **Fase 2:** Instalación de Windows 11 Pro con bypass de requisitos (TPM/SecureBoot) en VirtualBox.
3. **Fase 3:** Configuración de red estática en el servidor y resolución DNS en el cliente.
4. **Fase 4:** Unión al dominio `hotel.local` y aplicación de políticas de grupo (GPOs).

---

## 🛠 Configuración del Servidor

### Automatización con PowerShell
Se han utilizado los siguientes comandos para la puesta en marcha:

```powershell
# 1. Configuración de Red
New-NetIPAddress -IPAddress 192.168.30.10 -PrefixLength 24 -DefaultGateway 192.168.30.1 -InterfaceAlias "Ethernet"

# 2. Instalación de Active Directory
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
Install-ADDSForest -DomainName "hotel.local" -InstallDns:$true -Force
