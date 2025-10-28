# ⚡️ GVM/OpenVAS: CheatSheet de Filtros Avanzados (GSA)

Usar la sintaxis de filtro de la interfaz web (GSA) en la sección "Results" para refinar y priorizar el triage de vulnerabilidades.

| Objetivo del Filtro | Sintaxis del Filtro | Comentario Técnico |
| :--- | :--- | :--- |
| **Priorizar Triage** | `severity > 7.0 and qod > 70%` | Muestra solo hallazgos de alta o crítica severidad, con una **Calidad de Detección (QoD)** superior al 70%. **Obligatorio** para reducir falsos positivos. |
| **Vulnerabilidades Activas** | `apply_override=0` | Filtra todos los hallazgos que han sido justificados o desestimados (Override). Muestra solo las vulnerabilidades que **el analista debe abordar**. |
| **Falsos Positivos** | `apply_override=1` | Muestra todas las excepciones y riesgos aceptados. Útil para auditoría y revisión de la política de excepciones. |
| **Por Servidor Web** | `asset: "nombre_del_target"` | Busca resultados para un *Target* específico (ej. `asset: "Web Server Prod"`) para centrar el análisis. |
| **NVT Específico** | `nvt: OID` | Busca la ocurrencia de una prueba de vulnerabilidad concreta. El OID (Object Identifier) se encuentra en los detalles del resultado. |
| **Puertos Abiertos** | `port: 80 or port: 443` | Filtra los resultados para centrarse solo en vulnerabilidades encontradas en puertos específicos (útil para auditorías de aplicaciones web). |
| **Resultados Recientes** | `last_modified > 2024-01-01` | Muestra solo resultados modificados después de una fecha, útil para ver la evolución del riesgo tras un ciclo de parcheo. |
| **Vulnerabilidades Críticas**| `severity = 10.0` | Filtra resultados con la máxima severidad CVSS (Crítica). |
| **Tipo de Familia NVT** | `family: "Web Application"` | Muestra solo los resultados relacionados con una familia específica de pruebas NVT. |