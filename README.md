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

📸 Evidencia del Laboratorio (Investigación de 10 Puntos)
A continuación se detallan las capturas de pantalla que documentan el ciclo de vida del ataque y su detección:

Escaneo de Red: Detección de puertos abiertos desde Kali. ![Evidencia 1](img/nmap_scan.png)

Intento de Conexión RDP: Error de CredSSP/NLA inicial en la terminal. ![Evidencia 2](img/rdp_error.png)

Hardening Bypass: Desactivación de NLA en Windows Server. ![Evidencia 3](img/nla_disable.png)

Fuerza Bruta Exitosa: Log de Splunk con EventCode 4625 seguido de 4624. ![Evidencia 4](img/splunk_login.png)

Reconocimiento de Usuario: Ejecución de whoami detectada en el SIEM. ![Evidencia 5](img/splunk_whoami.png)

Enumeración de Red: Registro del comando ipconfig desde la IP atacante. ![Evidencia 6](img/splunk_ipconfig.png)

Creación de Backdoor: EventCode 4720 detallando el nuevo usuario "hacker". ![Evidencia 7](img/splunk_4720.png)

Escalada de Privilegios: EventCode 4732 (Adición al grupo Administradores). ![Evidencia 8](img/splunk_4732.png)

Persistencia mediante PowerShell: Script de ejecución detectado vía 4688. ![Evidencia 9](img/splunk_ps_exec.png)

Dashboard Final: Vista general de alertas críticas en el panel de Splunk. ![Evidencia 10](img/splunk_dashboard.png)

💡 Perfil del Desarrollador
Este laboratorio integra conocimientos de Backend Development con Ciberseguridad, enfocándose en la creación de aplicaciones e infraestructuras resilientes y monitoreables.
