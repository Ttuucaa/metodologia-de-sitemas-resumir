# Resumen Completo – SDLC, Metodologías, Formas Normales (FN) y Patrones

Guía integral para multiple choice. Incluye: SDLC (fases, actividades, entregables y errores típicos), metodologías (tradicionales y ágiles), Formas Normales (1FN–6FN + BCNF), modelado ER esencial, patrones (GRASP, GoF, arquitectónicos), roles/artefactos, V&V, DoR vs DoD, métricas y trampas frecuentes.

--------------------------------------------------------------------------------
0) SDLC – Ciclo de Vida del Desarrollo de Software
--------------------------------------------------------------------------------
Resumen: organiza el trabajo en etapas; en Ágil las mismas actividades ocurren iterativamente en cada sprint.

- Planificación
  - Qué: visión/objetivos, alcance y límites; stakeholders; supuestos, restricciones; riesgos iniciales y plan de mitigación; estimaciones de alto nivel; roadmap.
  - Entregables: Charter/Visión, Business Case, Plan del Proyecto, Registro de Riesgos inicial.
  - Errores: prometer fechas sin capacidad; ignorar riesgos/criterios de éxito.

- Análisis (Requisitos)
  - Qué: elicitar y especificar requisitos funcionales (RF) y no funcionales (RNF); reglas de negocio; casos de uso o historias de usuario (con criterios de aceptación); priorización (MoSCoW).
  - Requisitos RNF (FURPS+): Funcionalidad, Usabilidad, Confiabilidad, Performance, Soportabilidad (+ Seguridad, Compliance).
  - Modelos: Modelo de dominio, casos de uso, prototipos de baja fidelidad (wireframes).
  - Entregables: SRS/Especificación de Requisitos, Backlog inicial, Modelo de Dominio, Glosario.
  - Errores: mezclar solución con necesidad; ambigüedad; RNF vagos (“rápido” sin métrica).

- Diseño (Arquitectura y Diseño Detallado)
  - Arquitectura: estilos (capas, microservicios), decisiones ADR, contratos y APIs.
  - Diseño de datos: Diagramas ER, cardinalidades/opcionalidad, entidades débiles; normalización (1FN–3FN/BCNF), esquema lógico, claves PK/FK, integridad referencial, consideraciones de índices y particionamiento.
  - Diseño OO/UML: clases, componentes, interfaces; secuencia para escenarios clave.
  - Entregables: Documento de Arquitectura, ER + Esquema Lógico, Especificación de APIs, Diagramas UML (clase, componente, secuencia).
  - MC: “Diagramas ER/normalización” → Diseño.
  - Errores: saltar normalización; elegir índices sin workload; sobrediseño.

- Implementación (Construcción)
  - Qué: codificación, migraciones/DDL, seeds; CI; code reviews; TDD donde aplique; IaC si corresponde.
  - Entregables: código, scripts SQL/migraciones, pipelines CI, binarios.
  - Errores: cambios de esquema sin versionado; baja cobertura; feature flags ausentes.

- Pruebas (Verificación y Validación)
  - Verification vs Validation: 
    - Verification = “¿Construimos bien el producto?” (cumple especificación)
    - Validation = “¿Construimos el producto correcto?” (satisface la necesidad)
  - Tipos: unitarias, integración, sistema, aceptación (UAT), regresión, performance/carga, seguridad.
  - Pirámide: más unitarias, menos E2E; automatizar regresión.
  - Entregables: plan de pruebas, casos, datos de prueba, reportes de defectos.
  - Errores: probar tarde; no automatizar; no trazar requisitos ↔ pruebas.

- Despliegue / Transición
  - Qué: release management, CD, migraciones en prod, feature toggles, plan de rollback; documentación operativa; formación a usuarios/soporte.
  - Entregables: versión liberada, runbooks/playbooks, changelog.
  - Errores: no ensayar rollback; cambios “big bang” sin ventanas.

- Operación y Mantenimiento
  - Qué: monitoreo/observabilidad (logs, métricas, trazas), SLI/SLO; gestión de incidentes/problemas; parches, mejoras; backups y pruebas de restore; hardening.
  - Entregables: reportes de disponibilidad, tickets, versiones de mantenimiento.
  - Errores: no medir; deuda técnica acumulada; no probar restores.

- Gestión transversal
  - Riesgos: matriz probabilidad/impacto; dueños y mitigaciones.
  - Triángulo de proyecto: alcance-tiempo-costo (calidad afectada si se tensiona).
  - Trazabilidad: requisitos ↔ diseño ↔ pruebas.

- UML por fase (típico)
  - Análisis: Casos de uso, Actividad (flujos), Modelo de dominio.
  - Diseño: Clase, Secuencia/Colaboración, Componente, Despliegue.

- Pistas MC SDLC
  - “Diagramas de base de datos/ER, normalización” → Diseño.
  - “Casos de uso, reglas de negocio, criterios de aceptación” → Análisis.
  - “CI/TDD, migraciones DDL, code review” → Implementación.
  - “Plan de pruebas / UAT / regresión” → Pruebas.
  - “Rollback/feature flags/runbooks” → Despliegue.
  - “Monitoreo/SLI/SLO/incidentes” → Operación.

--------------------------------------------------------------------------------
1) METODOLOGÍAS DE DESARROLLO
--------------------------------------------------------------------------------
1.1. Tradicionales / Plan-Driven
- Cascada (Waterfall)
  - Secuencial: Requisitos → Análisis → Diseño → Implementación → Pruebas → Mantenimiento.
  - Pros: claridad, útil con requisitos estables y alta regulación.
  - Contras: feedback tardío, cambios costosos.
  - MC: “secuencial”, “requisitos fijos”.

- Modelo en V
  - Cascada + verificación/validación alineadas (Req ↔ Aceptación; Diseño ↔ Integración; Unidad ↔ Implementación).
  - MC: “alineación fases con tipos de prueba”.

- Espiral (Boehm)
  - Iterativo con foco en riesgos: 1) Objetivos 2) Análisis de riesgos (prototipos) 3) Desarrollo/Verificación 4) Plan siguiente ciclo.
  - MC: “análisis explícito de riesgos por iteración”.

- Prototipado
  - Descartable (se tira) vs Evolutivo (se refina hasta el producto).
  - MC: “aclara requisitos mediante prototipos”.

- Incremental vs Iterativo
  - Incremental: añade funcionalidad nueva por bloques.
  - Iterativo: mejora/refina lo existente.

- RUP (Rational Unified Process)
  - Fases: Inicio, Elaboración (riesgos/arquitectura), Construcción (iteraciones), Transición (deploy).
  - Dirigido por casos de uso, centrado en arquitectura, iterativo/incremental.

1.2. Ágiles / Adaptativas
- Manifiesto Ágil (4 valores)
  1) Individuos e interacciones > procesos y herramientas
  2) Software funcionando > documentación exhaustiva
  3) Colaboración con el cliente > negociación contractual
  4) Responder al cambio > seguir un plan
  - Nota: “suficiente documentación”, no “cero”.

- Scrum
  - Roles:
    - Product Owner: maximiza valor; ordena Product Backlog; define Product Goal.
    - Scrum Master: facilita, elimina impedimentos, enseña Scrum; no manda ni prioriza.
    - Developers: construyen el Incremento; autoorganizados y multifuncionales.
  - Artefactos:
    - Product Backlog (lista viva priorizada).
    - Sprint Backlog (selección + plan del sprint).
    - Incremento (potencialmente desplegable) con Definition of Done.
    - Definition of Done (DoD) vs Definition of Ready (DoR): DoD = calidad para “hecho”; DoR = entrada clara antes de empezar.
    - Acceptance Criteria: condiciones de una historia (no confundir con DoD).
  - Eventos:
    - Sprint (time-box), Planning (qué/cómo/objetivo), Daily (sincronización del equipo), Review (feedback), Retrospective (mejora).
    - Backlog Refinement: actividad continua (no evento formal), refina ítems futuros.
  - Métricas: Burn-down (restante), Burn-up (progreso), Velocidad; en flujo: Lead/Cycle Time, Throughput, CFD.
  - Trampas: “SM asigna tareas” (F), “Daily = reporte al SM” (F), “PO define DoD solo” (F).

- XP (Extreme Programming)
  - Prácticas: TDD (Red-Green-Refactor), Pair Programming, Refactoring, CI, Small Releases, Collective Code Ownership, Simple Design, Coding Standards, Sustainable Pace.
  - MC: “test antes del código” = TDD; “dos devs un teclado” = Pair.

- Kanban
  - Flujo continuo, limitar WIP, políticas explícitas, gestión del flujo, mejora evolutiva.
  - Métricas: Lead/Cycle Time, Throughput, CFD.
  - No impone sprints.

- Lean Software Development
  - 7 principios: eliminar desperdicio; construir calidad; crear conocimiento; postergar decisiones; entregar rápido; respetar personas; optimizar el todo.
  - Desperdicios: trabajo parcialmente hecho, esperas, sobreproducción, multitarea, transferencias, inventario, defectos.

- Otras
  - Crystal (peso según contexto), FDD (features pequeñas), DSDM (time-boxing + MoSCoW).

--------------------------------------------------------------------------------
2) FORMAS NORMALES (FN) y MODELADO DE DATOS
--------------------------------------------------------------------------------
2.1. Conceptos base (BD relacional)
- Clave candidata, primaria (PK), superclave; atributo primo vs no primo.
- Dependencia funcional (DF) X → Y: parcial, completa, transitiva.
- Dependencia multivaluada (DMV) X →→ Y; Join Dependency (JD).
- Integridad: entidad (PK no nula, única), referencial (FK válidas).

2.2. Modelado ER esencial
- Niveles: Conceptual (ER: entidades/relaciones), Lógico (relacional), Físico (implementación: tipos, índices, particiones).
- Cardinalidades y opcionalidad: 1:1, 1:N, M:N; total vs parcial (participación).
- Entidades débiles: dependen de otra (identificación por PK compuesta).
- Relaciones M:N: se resuelven con tablas intermedias (con FKs).
- Claves: Natural (derivada del negocio) vs Sustituta (surrogate, p.ej. ID autoincremental).
- Índices: en PK y FKs suelen ser útiles; considerar columnas de búsqueda/orden; trade-offs en escritura.
- ACID: Atomicidad, Consistencia, Aislamiento, Durabilidad (concepto general).

2.3. 1FN – Primera Forma Normal
- Atributos atómicos; sin listas ni grupos repetitivos.
- MC: “dominios atómicos”, “sin multivaluados”.

2.4. 2FN – Segunda Forma Normal
- 1FN + sin dependencias parciales de no-primos respecto de clave compuesta.
- Violación típica: (Curso, Estudiante) → NombreCurso depende solo de Curso.
- MC: “eliminar dependencias parciales”.

2.5. 3FN – Tercera Forma Normal
- 2FN + sin transitivas de no-primos respecto de una clave.
- MC: “eliminar transitivas”.

2.6. BCNF – Boyce–Codd
- Para toda DF X → Y, X debe ser superclave.
- Más estricta que 3FN; elimina determinantes no-superclave.
- MC: “X→Y con X superclave”.

2.7. 4FN – Cuarta Forma Normal
- BCNF + sin DMV no triviales (listas independientes).
- MC: “idiomas/deportes independientes” → separar.

2.8. 5FN – Quinta Forma Normal (PJ/NF)
- Elimina dependencias de unión no triviales; recomposición por joins sin pérdida y sin tuplas espurias.
- MC: “dependencias de unión”, “join sin pérdida”.

2.9. 6FN – Sexta Forma Normal
- Descomposición máxima (útil en temporalidad).

2.10. ¿Hasta dónde normalizar?
- Práctica: 3FN/BCNF suele bastar; 4FN/5FN si hay DMV/JD.
- Denormalizar conscientemente para performance (índices/materializaciones).

2.11. Pasos prácticos (algoritmo de examen)
1) Identificar claves candidatas.
2) Listar DF (y DMV si hay).
3) Asegurar 1FN.
4) Quitar parciales ⇒ 2FN.
5) Quitar transitivas ⇒ 3FN.
6) Verificar BCNF.
7) Si hay DMV ⇒ 4FN.
8) Si hay JD ⇒ 5FN.

2.12. Trampas MC (FN/ER)
- Confundir parcial con transitiva.
- 3FN permite X→Y si Y es primo; BCNF no si X no es superclave.
- 1FN no “se logra” separando por comas en una celda.
- Clave sustituta no elimina la necesidad de validar unicidad de la natural si el negocio lo exige.
- Relaciones M:N requieren tabla puente.

--------------------------------------------------------------------------------
3) PATRONES – GRASP, GoF y ARQUITECTURA
--------------------------------------------------------------------------------
3.1. GRASP (Principios)
- Information Expert, Creator, Controller, Low Coupling, High Cohesion, Polymorphism, Pure Fabrication, Indirection, Protected Variations.

3.2. GoF – Creacionales
- Singleton (una instancia), Factory Method (subclases deciden), Abstract Factory (familias), Builder (por pasos), Prototype (clonado).

3.3. GoF – Estructurales
- Adapter (compatibiliza), Bridge (separa abstracción/implementación), Composite (parte–todo), Decorator (agrega dinámico), Facade (simplifica), Flyweight (comparte), Proxy (control de acceso).

3.4. GoF – Comportamiento
- Chain of Responsibility, Command, Interpreter, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor.

3.5. Arquitectura y Datos
- MVC, Capas, Microservicios vs Monolito, Repository, Unit of Work, DI/IoC, Event-Driven / Pub-Sub, CQRS.

3.6. Trampas MC (Patrones)
- Strategy vs State; Adapter vs Facade; Decorator ≠ Subclase; Observer vs Mediator; Singleton ≠ Global sin control.

--------------------------------------------------------------------------------
4) ROLES, ARTEFACTOS, PRÁCTICAS Y MÉTRICAS (Scrum/Kanban)
--------------------------------------------------------------------------------
- Roles Scrum: PO (valor/prioridad), SM (proceso/impedimentos), Developers (incremento).
- Artefactos: Product Backlog, Sprint Backlog, Incremento, DoD, DoR, Acceptance Criteria.
- Eventos: Sprint, Planning, Daily, Review, Retrospective; Backlog Refinement (actividad continua).
- Metas: Product Goal, Sprint Goal.
- Métricas: Velocidad, Burn-down/Burn-up; en flujo: Lead/Cycle Time, Throughput, CFD.
- Kanban: límites WIP, políticas explícitas, clases de servicio (expedite/standard), mejora continua.

--------------------------------------------------------------------------------
5) PREGUNTAS TIPO MC – PISTAS RÁPIDAS
--------------------------------------------------------------------------------
- SDLC: ER/normalización → Diseño; Casos de uso → Análisis; UAT → Pruebas; Rollback → Despliegue; Monitoreo/SLI → Operación.
- 1FN/2FN/3FN/BCNF/4FN/5FN/6FN: ver definiciones arriba.
- “Modelo con análisis explícito de riesgos”: Espiral.
- “Una instancia global controlada”: Singleton.
- “Agregar responsabilidades dinámicamente”: Decorator.
- “Elección de algoritmo intercambiable”: Strategy.
- “Notificación a suscriptores”: Observer.
- “Límite que reduce multitarea”: WIP Limit (Kanban).
- “Rol que maximiza valor del producto”: Product Owner.
- “Gráfico de trabajo restante / progreso”: Burn-down / Burn-up.

--------------------------------------------------------------------------------
6) MINI HOJA DE ÚLTIMO REPASO (30 LÍNEAS)
--------------------------------------------------------------------------------
- SDLC: Planificación; Análisis (RF/RNF FURPS+); Diseño (ER/normalización, arquitectura); Implementación (CI/TDD/migraciones); Pruebas (V&V, pirámide); Despliegue (rollback); Operación (SLI/SLO).
- Cascada secuencial; V alinea desarrollo/pruebas; Espiral gestiona riesgos; RUP iterativo por fases.
- Ágil: 4 valores; Scrum roles/artefactos/eventos; DoD vs DoR vs Acceptance.
- XP: TDD, Pair, Refactor, CI; Kanban: flujo y WIP; Lean: 7 principios.
- ER: cardinalidades/opcionalidad; M:N con tabla puente; PK/FK; natural vs sustituta.
- FN: 1FN atómico; 2FN sin parciales; 3FN sin transitivas; BCNF determinante=superclave; 4FN DMV; 5FN JD; 6FN granular.
- Patrones GRASP/GoF; Arquitectura: MVC/Capas/Microservicios, Repository/UoW, DI, Event-Driven, CQRS.
- Trampas: “Ágil = cero docs” (F); “SM asigna tareas” (F); Strategy≠State; Adapter≠Facade; Decorator≠Subclase.