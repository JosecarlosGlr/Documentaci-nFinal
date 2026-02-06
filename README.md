# Documentación final

# Memoria Final: Administración de FileZilla Server

---

### 1. Instalación del Servidor
Para la puesta en marcha, se descargó el instalador de **FileZilla Server para Linux (64bit x86)** desde la web oficial. Una vez instalado, se procedió a registrar el servicio en el sistema operativo para asegurar su disponibilidad.

* **Comando de habilitación:** `sudo systemctl enable filezilla-server`
* **Comando de verificación:** `systemctl status filezilla-server` (confirmando el estado *active/running*)

---

### 2. Configuración Básica
La configuración inicial se realizó a través de la interfaz de administración, estableciendo los parámetros de escucha necesarios para permitir conexiones externas.

* **Server Listeners:** Se configuró la escucha en la dirección IP `0.0.0.0` (todas las interfaces) a través del **puerto 21**.
* **Protocolo:** Se estableció como requisito el uso de **Explicit FTP over TLS** para proteger las credenciales desde el primer contacto.

---

### 3. Usuarios y Permisos
Se implementó una estructura basada en grupos para simplificar la administración y asegurar la jerarquía de accesos.

* **Grupos:** Se creó el **"Grupo1"** con un límite de velocidad de **1000 KiB/s** y acceso de lectura/escritura sobre el directorio `/home`.
* **Usuarios:** Se crearon las cuentas **"Usuario1"**, **"Usuario2"** y **"tester"**, asociándolas al grupo para heredar automáticamente sus restricciones.
* **Acceso Anónimo:** Se habilitó un usuario `anonymous` sin contraseña, limitado estrictamente a **Modo Lectura** para descargas públicas.

---

### 4. Seguridad (FTPS)
Para garantizar la privacidad de los datos, se configuró una capa de seguridad mediante **FTPS Explícito**.

* **Certificado:** Se generó un certificado X.509 autofirmado con algoritmo **EC/ECDSA de 256 bits**.
* **Política:** Se configuró el servidor para rechazar cualquier conexión que no utilice cifrado **TLS 1.3**, protegiendo así el canal de datos con **AES-256-GCM**.

---

### 5. Modos Activo y Pasivo
Se analizaron ambos métodos de transferencia para asegurar la compatibilidad con firewalls y redes NAT.

* **Modo Activo:** El servidor inicia la conexión de datos (puerto 20). Suele fallar si el cliente tiene un firewall restrictivo.
* **Modo Pasivo:** El cliente inicia la conexión. Se configuró en el servidor el rango de puertos **50000-50100** para facilitar este modo, que es el estándar recomendado en redes modernas.



---

### 6. Clientes Utilizados
La funcionalidad del servidor se validó mediante tres tipos de clientes:

* **CLI (lftp):** Ideal para automatización y entornos sin interfaz gráfica. Permite gestionar el cifrado TLS mediante comandos manuales.
* **Gráfico (FileZilla Client):** Utilizado para transferencias masivas y gestión visual de archivos mediante el "Gestor de Sitios".
* **Navegador Web:** Se comprobó que los navegadores modernos (como Firefox) ya no soportan FTP nativo, actuando únicamente como lanzadores de aplicaciones externas.

---

### 7. Integración Web
Se demostró la capacidad de FileZilla como herramienta de despliegue de contenidos.

* **Flujo:** Se vinculó el directorio raíz del usuario FTP con el **DocumentRoot** de Apache (`/var/www/html`).
* **Resultado:** Al subir un archivo `prueba.html` vía FTP, este quedó disponible inmediatamente para el público a través del protocolo HTTP.

---

### 8. Recomendaciones de Administración
Tras las pruebas realizadas, se sugieren las siguientes pautas para la gestión del servidor:

* **Monitorización:** Revisar los logs de FileZilla para identificar intentos de acceso fallidos o errores de conexión TLS.
* **Mantenimiento de Permisos:** Utilizar siempre permisos de grupo para evitar inconsistencias entre usuarios que desempeñan la misma función.
* **Seguridad:** Mantener el rango de puertos pasivos lo más estrecho posible y renovar periódicamente los certificados TLS.
* **Optimización:** Establecer límites de ancho de banda (KiB/s) para evitar que un solo usuario sature la conexión de red del servidor.
