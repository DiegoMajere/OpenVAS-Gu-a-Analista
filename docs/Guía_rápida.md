# ⚡ GVM (Greenbone OpenVAS): Guía Rápida para Analistas Operacionales

Esta guía es un recurso de *onboarding* centrado en las decisiones de **criterio técnico** y **eficiencia operativa** que un analista debe tomar al usar Greenbone Vulnerability Management (GVM) en el día a día.

---

## 1. ⚙️ Triage Rápido: ¿Cuándo Usar GVM?

GVM no es para todas las tareas. Su fortaleza es la **auditoría amplia, histórica y continua** (Vulnerability Management), no el testing puntual.

| Escenario Operacional | Decisión | Criterio Rápido |
| :--- | :--- | :--- |
| **Auditoría periódica** (semanal/mensual) de la infraestructura. | **✅ Usar GVM** | Necesidad de **reportes de *compliance***, **visión histórica** y **NVT feeds** actualizados. |
| **Escaneo de un *host*** específico después de un parche. | **✅ Usar GVM (con Perfil Mínimo)** | Uso eficiente para confirmar mitigación con un perfil de escaneo específico (parches y OS). |
| **Verificación de un *exploit*** manual (PoC). | **❌ No Usar GVM** | Usar **Metasploit** o herramientas manuales (ej: Burp Suite, Nmap Scripts) para explotación activa. |
| **Mapeo rápido de puertos** y versión en un entorno nuevo. | **❌ No Usar GVM** | Usar **Nmap** (`-sS`, `-sV`). Es más rápido, silencioso y consume menos recursos. |

---

## 2. 🛡️ La Regla de Oro: Escaneo Autenticado

Un escaneo **no autenticado** es una auditoría **incompleta**.

| Tipo de Escaneo | Lo que Detecta | Limitación |
| :--- | :--- | :--- |
| **No Autenticado** (Caja Negra) | Vulnerabilidades de **red** (servicios expuestos, puertos, errores de banner). | No ve fallos internos (parches, configuraciones de registro, privilegios). |
| **Autenticado** (SSH/SMB) | Fallos de **configuración interna**, parches faltantes de OS/aplicaciones. | Requiere credenciales de **mínimo privilegio** para no comprometer el activo. |

* **Instrucción Clave:** Asegúrate siempre de que el *Target* tenga asignada una Credencial con el **Principio del Menor Privilegio** (solo lectura de auditoría).

---

## 3. 🎯 Perfiles de Escaneo: Eficiencia vs. Ruido

La elección del *Scan Config* es crítica para la eficiencia y para reducir los falsos positivos.

| Perfil | Riesgo Operacional | Recomendación (Criterio del Analista) |
| :--- | :--- | :--- |
| **Full and Fast** | **Alto Ruido.** Genera muchos falsos positivos y es lento. | **EVITAR** en entornos de producción. Solo para pruebas de laboratorio o PoCs. |
| **System Discovery** | Bajo. | Usar **ANTES** de un escaneo profundo para confirmar *hosts* vivos y no perder tiempo escaneando *targets* caídos. |
| **Perfil Personalizado** | Mínimo. | **ESTÁNDAR PROFESIONAL.** Crea un perfil que **excluya** explícitamente familias intrusivas (Denegación de Servicio, *Bruteforce*). |

---

## 4. 🗂️ Priorización (Triage): Más Allá del CVSS

El valor real del analista reside en aplicar **Criterio de Negocio**, no solo el puntaje técnico.

### Filtros Esenciales para el Triage Rápido

Utiliza los filtros avanzados en la interfaz de GVM para aislar las vulnerabilidades críticas accionables.

| Propósito del Filtro | Comando de Filtro (CheatSheet) |
| :--- | :--- |
| **Hallazgos Críticos y Fiables** | `severity > 7.0 and qod > 70%` |
| **Vulnerabilidades Sin Justificar** | `severity > 7.0 and apply_override=0` |
| **Buscar por NVT Específico** | `nvt:"[OID del NVT]" and asset:"[Nombre del Target]"` |

### Proceso de Validación

1.  **Aislar:** Filtrar resultados con alta severidad y alta Calidad de Detección (QoD).
2.  **Validar:** Abrir un hallazgo y verificar la vulnerabilidad manualmente usando herramientas de caja de herramientas (Nmap scripts, cURL, etc.).
3.  **Decisión:**
    * **REAL:** Escalar al equipo de mitigación.
    * **FALSO POSITIVO:** Aplicar un **Override (Excepción)** y documentar la justificación técnica (`Confirmado falso positivo por Nmap`).

---

## 5. 🤖 Automatización y API (`gvm-tools`)

Para integración con CMDB, ticketing y análisis de datos.

| Tarea Rápida | Comando `gvm-cli` (Python/Bash) |
| :--- | :--- |
| **Exportar un Reporte XML** | `gvm-cli socket --xml "<get_reports report_id='UUID'/>"` |
| **Listar Tareas Activas** | `gvm-cli socket --xml "<get_tasks filter='rows=-1'/>"` |
| **Iniciar una Tarea Específica** | `gvm-cli socket --xml "<start_task task_id='UUID'/>"` |