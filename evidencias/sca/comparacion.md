# Comparación del análisis SCA

## Identificación

- Grupo: 3
- Repositorio: spring-boot-webapi-secure (rama feature/devsecops-lab)
- Commit anterior: vulnerable (commons-text 1.9)
- Commit posterior: pendiente (fix a 1.10.0)
- Dependency-Check (plugin Maven): 11.1.1
- SBOM: CycloneDX 1.6, 53 componentes (`evidencias/local/04-bom.json`)
- Dependency-Check local: BUILD FAILURE con 73 hallazgos CVSS>=7 (commons-text CVE-2022-42889 9.8 incluido; resto CVE-2026 en Spring/Tomcat por verificar)

## Hallazgo seleccionado

| Campo | Antes | Después |
|---|---|---|
| Componente | commons-text | (pendiente re-escaneo) |
| Versión resuelta | 1.9 | 1.10.0 (propuesto) |
| CVE seleccionado | CVE-2022-42889 | debe desaparecer |
| Severidad reportada | CRITICAL (CVSS 9.8) | debe desaparecer del diff |
| Presencia del hallazgo | sí (`bom.json` + dependency-check) | pendiente |
| Estado del quality gate | rojo (falla CVSS>=7) | verde esperado para este CVE |

## Análisis

1. ¿La dependencia era directa o transitiva? Directa (declarada en `pom.xml` solo para el laboratorio).
2. ¿Qué cambio se realizó y por qué? Bump 1.9 → 1.10.0, mínimo histórico que corrige CVE-2022-42889. Alternativa válida: eliminar la dependencia al cerrar.
3. ¿Qué pruebas se ejecutaron para verificar compatibilidad? `mvn -B clean verify` (JaCoCo) antes y después.
4. ¿Qué evidencia muestra que desapareció el hallazgo seleccionado? Diff de `dependency-check-report.json` antes/después + gate en verde.
5. ¿Qué otros hallazgos o limitaciones quedan pendientes? Resto de HIGH/CRITICAL del reporte; SpotBugs fijado a JDK 21 por incompatibilidad ASM con JDK 25/26 (ver informe PDF §7).
