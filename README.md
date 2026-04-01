# Cybersecurity-Lab

🛡️ Laboratorio de Monitoreo y Detección de Amenazas (SIEM)

Este repositorio documenta la implementación de un laboratorio de ciberseguridad enfocado en la detección de ataques de fuerza bruta y persistencia en entornos de Windows Server.

🚀 Escenario Técnico

Atacante: Kali Linux (Herramientas: rdesktop, xfreerdp).

Víctima: Windows Server 2022 (Configuración de Hardening y Auditoría).

SIEM: Splunk Enterprise (Ingesta de logs mediante Universal Forwarder).

🛠️ Proyectos y Casos de Uso

1. Detección de Ataque de Fuerza Bruta RDP
Objetivo: Identificar intentos de acceso no autorizados desde una red externa.

Proceso: Configuración de políticas de auditoría local (auditpol) y desactivación controlada de NLA para visibilidad de capas de transporte.

EventCode Detectado: 4625 (Logon Failure).

SPL Query (Splunk):

index=* EventCode=4625 
| table _time, TargetUserName, IpAddress, Logon_Type, Status

2. Monitoreo de Procesos y Detección de Persistencia
Objetivo: Identificar la ejecución de herramientas sospechosas tras una intrusión.

Evento Clave: 4688 (A new process has been created).

<img width="906" height="341" alt="image" src="https://github.com/user-attachments/assets/e1563183-1185-4f85-9f2c-5d460d5dd55b" />


📊 Habilidades Demostradas

Análisis de Logs: Interpretación de Windows Event IDs (4624, 4625, 4688, 4720).

Gestión de SIEM: Creación de dashboards y queries complejas en SPL.

Hardening: Configuración de seguridad y políticas de auditoría en Windows Server.

Troubleshooting: Resolución de conflictos de conectividad entre máquinas virtuales.

📸 Evidencia del Laboratorio 

A continuación se detallan las capturas de pantalla que documentan el ciclo de vida del ataque y su detección:

1) Escaneo de Red: Detección de puertos abiertos desde Kali. Identificación de servicios activos en la IP 192.168.56.10. Se confirma que el puerto 3389 (RDP) se encuentra abierto, permitiendo avanzar a la fase de explotación.

<img width="347" height="142" alt="image" src="https://github.com/user-attachments/assets/8800c672-a608-48cb-9803-94f0cd07c00e" />

2) Intento de Conexión RDP: Ejecución de un intento de inicio de sesión remoto desde el host atacante (Kali Linux). Al intentar la conexión RDP desde Kali, la terminal registra un fallo al inicializar NLA (Network Level Authentication). 

<img width="349" height="107" alt="image" src="https://github.com/user-attachments/assets/cd7e6265-d9fc-4ae0-827b-9a63a6616467" />

3) Hardening Bypass: Desactivación de NLA en Windows Server.

<img width="400" height="149" alt="image" src="https://github.com/user-attachments/assets/11a5f2cd-549a-41bf-b2e1-843af4f57115" />

4) Fuerza Bruta Exitosa: Una vez desactivado el NLA en el servidor, se vuelve a ejecutar el comando de conexión desde Kali Linux. En esta ocasión, se permite al atacante visualizar la interfaz de inicio de sesión de Windows Server, facilitando intentos de fuerza bruta.

<img width="478" height="323" alt="image" src="https://github.com/user-attachments/assets/2d6405db-7b60-4c90-92fd-46477d7efd7e" />

<img width="347" height="142" alt="image" src="https://github.com/user-attachments/assets/0672136a-11f6-4049-8c11-7db25708cf09" />

5) Reconocimiento de Usuario: Ejecución de whoami detectada en el SIEM. Se detectó la ejecución del comando whoami desde una terminal con privilegios. El SIEM registró la creación del proceso, permitiendose identificar que el atacante está intentando verificar su nivel de acceso dentro del sistema comprometido.

<img width="369" height="89" alt="image" src="https://github.com/user-attachments/assets/856f10f5-78c5-45ea-957a-607db052c44e" />

<img width="1210" height="317" alt="image" src="https://github.com/user-attachments/assets/880e7518-9eae-410c-91d5-916144e9276e" />


6) Enumeración de Red: Tras obtener acceso, el atacante ejecutó ipconfig /all para mapear la infraestructura de red del servidor. Esta actividad de reconocimiento fue detectada por Splunk mediante el monitoreo de creación de procesos, permitiendo identificar el intento de descubrimiento de otros activos en el segmento de red.

<img width="372" height="256" alt="image" src="https://github.com/user-attachments/assets/1a56aada-dc62-4259-b85c-98242864cc1d" />

<img width="1310" height="311" alt="image" src="https://github.com/user-attachments/assets/49711163-d36a-4359-b461-1726d566bab8" />

7) Creación de Backdoor: EventCode 4720 detallando el nuevo usuario "hacker". 

<img width="377" height="101" alt="image" src="https://github.com/user-attachments/assets/b780a650-756b-42c2-ad62-9511a3e5aa6a" />

<img width="1203" height="307" alt="image" src="https://github.com/user-attachments/assets/72e3b93e-979a-47f3-aeb9-d1fbf3a218c6" />

8) Escalada de Privilegios: EventCode 4732 (Adición al grupo Administradores). Para obtener control total sobre el servidor, el atacante elevó los privilegios de la cuenta creada integrándola al grupo de Administradores. El SIEM registró esta acción crítica bajo el EventCode 4732 (A member was added to a security-enabled local group), permitiendo al equipo de seguridad detectar el intento de toma de control total del activo.

<img width="275" height="73" alt="image" src="https://github.com/user-attachments/assets/6955eaa7-456a-4201-b029-2ce1f95cdc18" />

<img width="1227" height="264" alt="image" src="https://github.com/user-attachments/assets/3db8d8d9-4e81-49bc-bb45-08b70f7b13fd" />

9) Persistencia mediante PowerShell: Script de ejecución detectado vía 4688. Se detectó un intento de ejecución de scripts de PowerShell con políticas de ejecución omitidas. Mediante el monitoreo del EventCode 4688, se logró identificar el comando exacto, permitiendo diferenciar entre tareas administrativas legítimas y actividad sospechosa de post-explotación.

<img width="472" height="88" alt="image" src="https://github.com/user-attachments/assets/ae5f2d2e-099e-46cd-abd0-15200ac15bd7" />

<img width="1183" height="283" alt="image" src="https://github.com/user-attachments/assets/78fdf2e4-01a1-4287-83f8-56e918e4da65" />

10) Dashboard Final: Vista general de alertas críticas en el panel de Splunk. Vista consolidada de la seguridad del servidor. El pico en el código 4688 indica una actividad inusual de ejecución de procesos, lo que dispararía una alerta inmediata para un analista de seguridad.

<img width="1867" height="489" alt="image" src="https://github.com/user-attachments/assets/75136c81-3adf-4839-bc3d-9929f253cc9f" />

4688: Creación de Procesos. Es el que registra cada vez que el atacante ejecuta un comando (ipconfig, whoami, net user, powershell). Al ser tan alto, indica una actividad intensa de ejecución de comandos en el servidor.

7036: Cambio de estado en un servicio. Este registra cuando servicios de Windows se detienen o inician 

4624: Inicio de sesión exitoso (Logon). Confirma que el atacante entró al sistema.

4625: Falla de inicio de sesión. Es el rastro de los intentos de contraseña incorrecta previos a la entrada.

4719: Cambio en la política de auditoría. Este es muy importante en seguridad; salta cuando alguien intenta modificar qué cosas se graban en los logs (a veces los atacantes intentan apagar la auditoría para no ser vistos).

1014: Advertencia de DNS. Indica que el servidor intentó resolver un nombre de red y no pudo (común cuando el atacante intenta conectar el servidor a internet o a otra red).

6, 55, 98, 34: Son eventos de sistema relacionados con drivers, kernel y el sistema de archivos (NTFS). No son necesariamente de "ataque", pero Splunk los captura porque son cambios en el estado del servidor.

💡 Perfil del Desarrollador

Este laboratorio integra conocimientos de Backend Development con Ciberseguridad, enfocándose en la creación de aplicaciones e infraestructuras resilientes y monitoreables.
