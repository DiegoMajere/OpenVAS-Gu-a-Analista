# GVM/OpenVAS: CheatSheet de Comandos API (gvm-tools CLI)

Comandos esenciales para interactuar con GVM Manager Daemon (GVMd) utilizando el CLI de gvm-tools, útil para scripting y automatización.
(Con'gvm-tools' instalado y configurado para usar un socket local).

## Autenticación y Comprobación

| Comando API (XML) | Propósito |
| :--- | :--- |
| `gvm-cli socket --xml "<get_users/>"` | Lista todos los usuarios configurados en GVM. |
| `gvm-cli socket --xml "<get_version/>"` | Obtiene la versión actual del protocolo GVMd (útil para la compatibilidad del script). |

## Gestión de Targets y Tareas

| Comando API (XML) | Propósito |
| :--- | :--- |
| `gvm-cli socket --xml "<create_target name='Nuevo Target' hosts='192.168.1.5'/>"` | Crea un nuevo Target de forma programática para una única IP. |
| `gvm-cli socket --xml "<get_targets filter='name=Servidores Prod'/>"` | Obtiene el UUID y detalles de un Target específico (necesario para la creación de tareas). |
| `gvm-cli socket --xml "<start_task task_id='UUID_DE_LA_TAREA'/>"` | Inicia una tarea de escaneo usando su UUID (ideal para cronjobs). |
| `gvm-cli socket --xml "<stop_task task_id='UUID_DE_LA_TAREA'/>"` | Detiene una tarea en ejecución (para mantenimiento o emergencia). |

## Gestión de Reportes y Extracción de Datos

| Comando API (XML) | Propósito |
| :--- | :--- |
| `gvm-cli socket --xml "<get_reports max_hosts='1'/>"` | Muestra los reportes más recientes. |
| `gvm-cli socket --xml "<get_report report_id='UUID_DEL_REPORTE' format_id='c87ff54d-1762-4f36-96b5-0c14c53d9e80'/>"` | **EXTRAER XML:** Obtiene el reporte en formato XML. El UUID `c87f...` es el UUID estándar del formato XML. Este XML es la fuente de datos bruta para el análisis. |
| `gvm-cli socket --xml "<get_report report_id='UUID_DEL_REPORTE' format_id='913e390c-7202-4632-84de-c84025a17e13'/>"` | **EXTRAER CSV:** Obtiene el reporte en formato CSV (más simple para hojas de cálculo). El UUID `913e...` es el UUID estándar del formato CSV. |