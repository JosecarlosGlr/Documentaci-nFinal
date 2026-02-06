# Documentación final

---

### 1. Instalación del Servidor
Para poner en marcha el servicio, descargué el instalador oficial de **FileZilla Server para Linux (64bit x86)**. Una vez finalizada la instalación, procedí a registrar el demonio en mi sistema operativo para asegurar que estuviera siempre disponible.

* **Habilitación del servicio:** Ejecuté `sudo systemctl enable filezilla-server` para que arranque automáticamente con el sistema.
* **Verificación:** Comprobé con el comando `systemctl status filezilla-server` que el estado aparecía correctamente como *active (running)*.

---

### 2. Configuración Básica
Realicé la configuración inicial a través de la interfaz de administración, donde establecí los parámetros necesarios para que el servidor aceptara conexiones externas.

* **Server Listeners:** Configuré la escucha en la dirección IP `0.0.0.0` para aceptar peticiones desde cualquier interfaz a través del **puerto 21**.
* **Protocolo:** Establecí como requisito obligatorio el uso de **Explicit FTP over TLS**, garantizando así la protección de mis credenciales desde el primer contacto.

---

### 3. Usuarios y Permisos
Diseñé una estructura basada en grupos para gestionar los accesos de forma más eficiente y organizada.

* **Gestión de Grupos:** Creé el **"Grupo1"**, al cual asigné un límite de velocidad de **1000 KiB/s** y permisos de lectura/escritura sobre mi directorio `/home`.
* **Cuentas de Usuario:** Creé los usuarios **"Usuario1"**, **"Usuario2"** y **"tester"**, integrándolos en el grupo anterior para que heredaran todas sus restricciones automáticamente.
* **Acceso Anónimo:** Habilité la cuenta `anonymous` sin requerir contraseña, pero limitando su acceso estrictamente a **Modo Lectura** para permitir descargas públicas seguras.

---

### 4. Seguridad (FTPS)
Para proteger la privacidad de mis archivos y datos, configuré una capa de seguridad mediante **FTPS Explícito**.

* **Certificado:** Generé mi propio certificado X.509 autofirmado utilizando un algoritmo moderno **EC/ECDSA de 256 bits**.
* **Directivas:** Configuré el servidor para rechazar conexiones inseguras, obligando al uso de **TLS 1.3** y cifrado **AES-256-GCM** para el canal de datos.

---

### 5. Modos Activo y Pasivo
Analicé ambos métodos de transferencia para entender cómo se comportan frente a firewalls y redes con NAT.

* **Modo Activo:** Comprobé que el servidor inicia la conexión de datos (puerto 20), lo cual suele dar problemas si el firewall de mi cliente es muy restrictivo.
* **Modo Pasivo:** Es el que he dejado configurado como estándar. Definí un rango de puertos entre el **50000 y el 50100** en el servidor para facilitar que el cliente inicie la conexión, que es lo ideal en redes modernas.



---

### 6. Clientes Utilizados
Validé el funcionamiento de mi servidor probando tres tipos de clientes diferentes:

* **CLI (lftp):** Lo utilicé para realizar pruebas desde la terminal. Me resultó muy útil para gestionar el cifrado TLS mediante comandos manuales.
* **Interfaz Gráfica (FileZilla Client):** Fue mi herramienta principal para transferencias masivas, aprovechando el "Gestor de Sitios" para guardar mi conexión.
* **Navegador Web:** Pude verificar que navegadores como Firefox ya no soportan FTP de forma nativa, funcionando ahora solo como un enlace para abrir aplicaciones externas.

---

### 7. Integración Web
Logré integrar el servidor FTP con mi servidor web para facilitar el despliegue de contenidos.

* **Flujo de trabajo:** Vinculé el directorio raíz de mi usuario FTP directamente con el **DocumentRoot** de Apache (`/var/www/html`).
* **Resultado:** Pude comprobar que, al subir mi archivo `prueba.html` vía FTP, este se publicaba instantáneamente y era accesible a través de HTTP en el navegador.

---

### 8. Mis Recomendaciones de Administración
Tras completar todas las pruebas, he extraído estas pautas clave para gestionar el servidor:

* **Monitorización constante:** Recomiendo revisar los logs de FileZilla con frecuencia para detectar intentos de acceso no autorizados.
* **Uso de grupos:** Es fundamental trabajar con permisos de grupo para mantener la consistencia y evitar errores al crear nuevos usuarios.
* **Seguridad activa:** Conviene mantener el rango de puertos pasivos ajustado y renovar los certificados TLS periódicamente.
* **Control de tráfico:** Establecer límites de ancho de banda es vital para evitar que un solo usuario consuma todos los recursos de red de mi servidor.
