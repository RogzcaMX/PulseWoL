<div align="center">

# Pulse WoL
### By: Rogzca

[![Read in English](https://img.shields.io/badge/Read%20in-English-blue?style=for-the-badge)](EN/README_EN.md)

**Control de energía ligero, seguro y directo para tus ordenadores en red local (LAN).**

[Descargar última versión](https://github.com/RogzcaMX/PulseWoL/releases/latest) • [Instalación](#instalación-y-uso) • [Configurar Wake-on-LAN](#configuración-de-wake-on-lan-wol) • [Características](#características-principales)

</div>

---

## Descripción general

**Pulse WoL** es un ecosistema pensado para gestionar el encendido (**Wake-on-LAN**) y el **apagado remoto** de tus equipos Windows desde tu dispositivo Android sin depender de servicios externos en la nube ni configuraciones complejas.

La solución se divide en dos componentes independientes que se comunican exclusivamente dentro de tu red local:

1. **Pulse WoL Android (`.apk`)**: Aplicación nativa en Jetpack Compose con soporte multidispositivo, ordenamiento desacoplado y widgets interactivos.
2. **Pulse WoL Server (`.exe`)**: Servicio en segundo plano para Windows, de bajo consumo de recursos (0% CPU en reposo), minimizable al área de notificación (*System Tray*).

---

## Características principales

### App Android
* **Autodescubrimiento LAN**: Encuentra y vincula tus PCs en segundos mediante difusión UDP sin escribir manualmente direcciones MAC o IPs.
* **Gestión multidispositivo desacoplada**: Organiza tus equipos con prioridades distintas para la pestaña de encendido y la de apagado.
* **Widgets de escritorio con Jetpack Glance**:
  * **Modo Extendido**: Muestra el nombre asignado a cada equipo sobre sus respectivos botones de acción.
  * **Modo Compacto (Vertical)**: Formato minimalista tipo interruptor (1×2) con iconos vectoriales dedicados.
* **Control flexible de widgets**: Elige si el widget opera de forma automática por orden de lista, mediante un único equipo universal o con destinos individuales para cada acción.
* **Temas visuales**: Compatibilidad completa con Modo Claro, Modo Oscuro y colores dinámicos del sistema (Material You).

### Servidor Windows
* **Operación silenciosa**: Se minimiza en la bandeja del sistema (*System Tray*) para no saturar tu barra de tareas.
* **Apagado quirúrgico**: Atiende únicamente peticiones de apagado dirigidas de forma explícita a la dirección física (MAC) del equipo.
* **Arranque automático opcional**: Opción para iniciar junto con el sistema operativo sin interacción manual.
* **Instalador guiado**: Empaquetado completo para instalación y desinstalación estándar de Windows.

---

## Instalación y uso

### 1. Servidor (Windows)
1. Ve a la sección de **[Releases](https://github.com/RogzcaMX/PulseWoL/releases/latest)** y descarga el instalador del servidor (`PulseWoLServer_Setup.exe` o equivalente).
2. Ejecuta el asistente de instalación y completa los pasos.
3. Al iniciar, el icono de Pulse WoL se situará en la bandeja del sistema junto al reloj. Si Windows Defender Firewall solicita permisos para sockets UDP en redes privadas, concédelos para permitir la escucha de órdenes.

### 2. Aplicación móvil (Android)
1. Descarga el archivo `.apk` de la última versión desde **[Releases](https://github.com/RogzcaMX/PulseWoL/releases/latest)**.
2. Instálalo en tu dispositivo Android autorizando la instalación desde fuentes externas si el sistema lo requiere.
3. Asegúrate de que el móvil esté conectado a la **misma red Wi-Fi** que la PC.
4. Pulsa en **Buscar PC automáticamente** en cualquiera de las secciones para enlazar tu ordenador con un solo toque.

---

## Configuración de Wake-on-LAN (WoL)

Para que tu ordenador pueda encenderse a través de la red, es fundamental habilitar la función tanto en el hardware (BIOS/UEFI) como en el sistema operativo (Windows).

---

### 1. En la BIOS / UEFI de la placa base

> **Nota importante:** Cada fabricante organiza y nombra sus menús de forma diferente. Si las opciones descritas a continuación no coinciden exactamente con tu pantalla, consulta el manual específico de la marca y modelo de tu placa base.

#### Procedimiento genérico:
1. Apaga el equipo y enciéndelo presionando repetidamente la tecla de acceso a la BIOS (habitualmente `Supr`, `Del`, `F2` o `F12` según el fabricante).
2. Cambia al modo avanzado (**Advanced Mode** o tecla `F7`).
3. Dirígete a la pestaña de energía o periféricos. Suele encontrarse bajo nombres como:
   * **Advanced** > **APM Configuration** (ASUS / ASRock).
   * **Settings** > **Advanced** > **Wake Up Event Setup** (MSI).
   * **Power** o **Power Management Setup** (Gigabyte).
4. Localiza y activa (**Enabled**) la opción correspondiente a WoL, que suele llamarse:
   * *Power On By PCIE/PCI* o *Power On By PCI-E Devices*.
   * *Resume By Onboard Intel LAN* / *Realtek LAN*.
   * *Wake on LAN* / *PME Event Wake Up*.
5. Si existe la opción **ErP Ready** o **Deep Sleep**, asegúrate de dejarla en **Disabled** (desactivada), ya que cortar por completo la energía de la placa al apagar impide que la tarjeta de red escuche el paquete mágico.
6. Guarda los cambios y sal (habitualmente con `F10`).

---

### 2. En Windows 10 y Windows 11

Una vez configurada la BIOS, debes autorizar a Windows y a tu adaptador de red a recibir y despertar el equipo mediante el paquete mágico:

#### A. Propiedades del adaptador de red:
1. Haz clic derecho sobre el menú Inicio de Windows y selecciona **Administrador de dispositivos**.
2. Despliega la categoría **Adaptadores de red**.
3. Haz doble clic sobre tu tarjeta de red Ethernet principal (por ejemplo, *Realtek PCIe GbE Family Controller* o *Intel Ethernet Connection*).
4. Ve a la pestaña **Administración de energía** y marca las tres casillas:
   * *Permitir que este dispositivo reactive el equipo*.
   * *Permitir solo que un Magic Packet reactive el equipo*.
5. Ve a la pestaña **Opciones avanzadas**, busca y ajusta los siguientes valores en la lista:
   * **Activar en Magic Packet** (o *Wake on Magic Packet*): `Habilitado` (`Enabled`).
   * **Apagado reactivar LAN** (o *Shutdown Wake-On-LAN*): `Habilitado` (`Enabled`).
   * **Eficiencia energética Ethernet / Green Ethernet**: `Deshabilitado` (`Disabled`) *(recomendado)*.
6. Pulsa **Aceptar**.

#### B. Desactivar el Inicio Rápido de Windows *(Recomendado)*:
El inicio rápido de Windows a veces pone la tarjeta de red en un estado de hibernación profunda (S4/S5 híbrido) que bloquea el paquete de encendido:
1. Abre el **Panel de control** clásico y ve a **Opciones de energía**.
2. En la barra lateral izquierda, haz clic en **Elegir el comportamiento de los botones de inicio/apagado**.
3. Pulsa sobre **Cambiar la configuración actualmente no disponible**.
4. Desmarca la casilla **Activar inicio rápido (recomendado)**.
5. Guarda los cambios.

---

## Requisitos del sistema

* **Android**: Android 6.0 (API 23) o superior.
* **Windows**: Windows 10 / Windows 11.
* **Hardware de red**: Placa base conectada mediante cable de red Ethernet (LAN) compatible con Wake-on-LAN.

---

## Licencia

Distribuido bajo la licencia especificada en el repositorio. Consulta el archivo de licencia correspondiente para más detalles.
