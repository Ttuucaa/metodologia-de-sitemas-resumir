# Resumen Completo – Metodología de Sistemas, Formas Normales (FN) y Patrones

Guía integral para multiple choice. Incluye: metodologías (tradicionales y ágiles), TODAS las Formas Normales (1FN–6FN + BCNF), patrones (GRASP, GoF, arquitectónicos), roles/artefactos y trampas típicas.

--------------------------------------------------------------------------------
1) METODOLOGÍAS DE DESARROLLO
--------------------------------------------------------------------------------
1.1. Tradicionales / Plan-Driven
- Cascada (Waterfall)
  - Secuencial rígido: Requisitos → Análisis → Diseño → Implementación → Pruebas → Mantenimiento.
  - Pros: claridad, útil con requisitos estables y alta regulación.
  - Contras: feedback tardío, cambios costosos.
  - MC: “secuencial”, “requisitos fijos”.

- Modelo en V
  - Cascada + verificación/validación alineadas (Requisitos ↔ Aceptación; Diseño ↔ Integración; Unidad ↔ Implementación).
  - MC: “alineación fases con tipos de prueba”.

- Espiral (Boehm)
  - Iterativa con foco en RIESGOS: 1) Objetivos 2) Análisis de riesgos (prototipos) 3) Desarrollo/Verificación 4) Plan del ciclo siguiente.
  - MC: “análisis explícito de riesgos por iteración”.

- Prototipado
  - Descartable: se tira tras aclarar requisitos.
  - Evolutivo: se refina hasta ser el producto.
  - MC: “aclara requisitos mediante prototipos”.

- Incremental vs Iterativo
  - Incremental: agrega funcionalidad nueva por bloques.
  - Iterativo: refina/mejora lo existente.

- RUP (Rational Unified Process)
  - Fases: Inicio (visión), Elaboración (riesgos/arquitectura), Construcción (iteraciones), Transición (deploy).
  - Dirigido por casos de uso, centrado en arquitectura, iterativo/incremental.
  - MC: “iterativo por fases”, “arquitectura”, “riesgos tempranos”.

1.2. Ágiles / Adaptativas
- Manifiesto Ágil (4 valores)
  1) Individuos e interacciones > procesos y herramientas
  2) Software funcionando > documentación exhaustiva
  3) Colaboración con el cliente > negociación contractual
  4) Responder al cambio > seguir un plan
  - Trampa: no significa “cero documentación”, sino la necesaria.

- Scrum
  - Roles:
    - Product Owner (PO): define y prioriza Product Backlog, maximiza valor. NO asigna tareas.
    - Scrum Master (SM): facilita, elimina impedimentos, cuida Scrum. NO manda ni prioriza.
    - Developers: construyen el Incremento; autoorganizados y multifuncionales.
  - Artefactos:
    - Product Backlog: lista priorizada viva.
    - Sprint Backlog: selección + plan del sprint.
    - Incremento: potencialmente desplegable; cumple Definition of Done (DoD).
    - DoD vs Acceptance Criteria: DoD = lista general de calidad; AC = condiciones de una historia.
  - Eventos:
    - Sprint (time-box), Planning, Daily (sincronización del equipo), Review (feedback), Retrospective (mejora).
  - Métricas: Burn-down (restante), Burn-up (progreso), Velocidad (estimación).
  - Trampas: “SM asigna tareas” (F), “Daily es reporte al SM” (F), “PO define DoD solo” (F).

- XP (Extreme Programming)
  - Prácticas: TDD (Red-Green-Refactor), Pair Programming, Refactoring, CI, Small Releases, Collective Code Ownership, Simple Design, Coding Standards, Sustainable Pace.
  - MC: “test antes del código” = TDD; “dos devs un teclado” = Pair.

- Kanban
  - Flujo continuo, tablero visual, límites WIP, Lead/Cycle Time, Throughput, CFD.
  - No impone sprints; cambios en cualquier momento.
  - MC: “limitar WIP”, “flujo continuo”.

- Lean Software Development
  - 7 principios: eliminar desperdicio; construir calidad; crear conocimiento; postergar decisiones comprometedoras; entregar rápido; respetar personas; optimizar el todo.
  - Desperdicios típicos: trabajo parcialmente hecho, esperas, sobreproducción, multitarea, transferencias, inventario (features no usadas), defectos.

- Otras ágiles
  - Crystal: “peso” según tamaño/criticidad.
  - FDD: orientado a features pequeñas planificadas.
  - DSDM: time-boxing + MoSCoW (Must/Should/Could/Won’t).

--------------------------------------------------------------------------------
2) FORMAS NORMALES (FN) – NORMALIZACIÓN DE BASES DE DATOS
--------------------------------------------------------------------------------
2.1. Conceptos base
- Relación, atributo, dominio.
- Clave candidata: conjunto mínimo que determina tupla.
- Clave primaria: una clave candidata elegida.
- Superclave: conjunto que determina tupla (no necesariamente mínimo).
- Atributo primo: pertenece a alguna clave candidata. No primo: no pertenece.
- Dependencia funcional (DF): X → Y si el valor de X determina Y.
  - Parcial: si clave compuesta K = (A,B) y A→Y (o B→Y) con Y no primo.
  - Completa: Y depende de toda la clave (A,B).
  - Transitiva: A→B y B→C, entonces A→C (con C no primo y B no clave).
- Dependencia multivaluada (DMV): X →→ Y (listas independientes).
- Dependencia de unión (Join Dependency): tabla se descompone en varias y la unión sin pérdida reconstruye.

2.2. Anomalías (motiva normalizar)
- Inserción: no se puede insertar un dato sin otros no relacionados.
- Actualización: hay que actualizarlo en múltiples filas (riesgo de inconsistencia).
- Borrado: se pierde información relacionada al borrar otra.

2.3. Primera Forma Normal (1FN)
- Atributos atómicos (sin campos repetidos ni listas dentro de celdas).
- No grupos repetitivos; una columna, un valor por celda.
- MC: “dominios atómicos”, “sin multivaluados”.

2.4. Segunda Forma Normal (2FN)
- Requisitos: estar en 1FN y que no existan dependencias parciales de atributos no-primos respecto de clave compuesta.
- Todos los atributos no-primos dependen de TODA la clave primaria (si es compuesta).
- Violación típica: tabla con clave (Curso, Estudiante) y columna “NombreCurso” que depende solo de Curso.
- MC: “eliminar dependencias parciales”.

2.5. Tercera Forma Normal (3FN)
- Requisitos: estar en 2FN y que no existan dependencias TRANSITIVAS de atributos no-primos respecto de alguna clave.
- Atributos no-primos dependen solo de claves, no de otros no-primos.
- Violación: Clave → A; A → B (B no primo) ⇒ transitiva.
- MC: “eliminar transitivas”.

2.6. Forma Normal de Boyce–Codd (BCNF)
- Regla: para TODA DF X → Y, X debe ser SUPERCLAVE.
- Más estricta que 3FN. Rompe casos donde 3FN deja anomalías si hay múltiples claves candidatas y determinantes no superclave.
- Ejemplo típico: (Curso, Aula, Docente) con DF Aula → Docente y (Curso, Aula) clave; “Aula” no es superclave ⇒ viola BCNF.
- MC: “X→Y con X superclave”.

2.7. Cuarta Forma Normal (4FN)
- Estar en BCNF y no tener dependencias multivaluadas no triviales.
- Si X →→ Y (y Y independiente de otros atributos), separar en dos tablas X–Y y X–Z.
- Ejemplo: Estudiante →→ Idioma, Estudiante →→ Deporte (idiomas y deportes independientes).
- MC: “listas independientes” = 4FN.

2.8. Quinta Forma Normal (5FN) o PJ/NF
- Elimina dependencias de unión no triviales: toda descomposición en proyecciones debe poder recomponerse por JOIN sin pérdida y sin tuplas espurias.
- Caso típico: Proveedor–Parte–Proyecto donde combinaciones válidas emergen solo al recomponer tres relaciones binarias.
- MC: “dependencias de unión”, “reconstrucción por join”.

2.9. Sexta Forma Normal (6FN)
- Descomposición máxima (a nivel de atributos), usada en bases temporales y almacenes altamente desnormalizados lógicamente pero atomizados físicamente.
- Poca en currículas intro; conocer que existe.
- MC: si aparece, “temporalidad/variantes por tiempo”.

2.10. ¿Hasta dónde normalizar?
- Práctico: 3FN o BCNF suelen ser suficiente. 4FN/5FN para multivaluadas/joins complejos.
- Denormalización consciente para performance (con índices/materializaciones).

2.11. Pasos prácticos (algoritmo de examen)
1) Identificar claves candidatas (y clave primaria).
2) Listar DF (y DMV si hay listas).
3) Chequear 1FN (campos atómicos).
4) Eliminar parciales ⇒ 2FN (separar entidad dependiente de parte de la clave).
5) Eliminar transitivas ⇒ 3FN (crear tablas para atributos que determinan otros).
6) Chequear BCNF (determinantes no-superclave ⇒ descomponer).
7) Si hay DMV ⇒ 4FN (separar listas independientes).
8) Si hay join dependencies ⇒ 5FN.

2.12. Trampas MC (FN)
- Confundir parcial con transitiva.
- Atributo primo puede depender parcialmente sin violar 2FN (lo que importa son no-primos).
- 3FN permite X→Y si Y es primo (cierta sutileza teórica); BCNF no lo permite si X no es superclave.
- 4FN siempre implica antes BCNF.
- Decir “1FN permite listas si están separadas por comas” → Falso.

--------------------------------------------------------------------------------
3) PATRONES – GRASP, GoF y ARQUITECTÓNICOS
--------------------------------------------------------------------------------
3.1. GRASP (Principios de Asignación de Responsabilidades)
- Information Expert: asignar responsabilidad al que tiene la info.
- Creator: quien agrega/contiene/usa intensamente crea el objeto.
- Controller: objeto que recibe eventos del sistema y coordina casos de uso.
- Low Coupling: minimizar dependencias entre clases.
- High Cohesion: responsabilidades relacionadas agrupadas.
- Polymorphism: manejar variantes por tipo usando polimorfismo.
- Pure Fabrication: clase artificial para lograr bajo acoplamiento/alta cohesión.
- Indirection: intermediario para desacoplar (p.ej. patrón Mediator).
- Protected Variations: encapsular puntos de cambio (interfaces/abstracciones).
- MC: palabras ancla “experto de información”, “bajo acoplamiento”, “alta cohesión”.

3.2. GoF – Creacionales
- Singleton: 1 única instancia con acceso global controlado. MC: “una sola instancia”.
- Factory Method: subclases deciden qué clase concreta crear. MC: “delegar creación a subclase”.
- Abstract Factory: crear familias de productos relacionados. MC: “familias coherentes”.
- Builder: construir objeto complejo paso a paso. MC: “construcción por pasos”.
- Prototype: clonar instancias existentes. MC: “clonación”.

3.3. GoF – Estructurales
- Adapter: interfaz compatible sin cambiar código cliente. MC: “adaptar interfaz”.
- Bridge: separar abstracción de su implementación. MC: “desacoplar jerarquías”.
- Composite: parte–todo, tratar compuesto como hoja. MC: “árbol uniforme”.
- Decorator: añadir responsabilidades dinámicamente. MC: “envolver para agregar”.
- Facade: interfaz simple a subsistema complejo. MC: “fachada simplificadora”.
- Flyweight: compartir estado para ahorrar memoria. MC: “muchos objetos livianos”.
- Proxy: representante que controla acceso. MC: “sustituto con control”.

3.4. GoF – Comportamiento
- Chain of Responsibility: pasar petición por cadena. MC: “cadena de manejadores”.
- Command: encapsular petición como objeto. MC: “deshacer/cola de comandos”.
- Interpreter: gramática y evaluador. MC: “mini lenguaje”.
- Iterator: recorrer colección secuencialmente. MC: “recorrido uniforme”.
- Mediator: centralizar comunicación entre objetos. MC: “coordinador”.
- Memento: capturar/ restaurar estado. MC: “deshacer snapshots”.
- Observer: notificación a suscriptores. MC: “publish/subscribe”.
- State: cambiar comportamiento según estado interno. MC: “estado como objeto”.
- Strategy: familia de algoritmos intercambiables. MC: “política elegible”.
- Template Method: esqueleto de algoritmo; subclases rellenan pasos. MC: “ganchos”.
- Visitor: agregar operaciones sin cambiar clases visitadas. MC: “doble despacho”.

3.5. Patrones Arquitectónicos y de Acceso a Datos
- MVC: separar Modelo–Vista–Controlador.
- Arquitectura en Capas (Layers): presentación, lógica, datos.
- Microservicios vs Monolito: servicios independientes vs unidad.
- Repository: abstraer acceso a datos.
- Unit of Work: agrupar cambios como transacción.
- DI/IoC: inyección de dependencias, inversión de control.
- Event-Driven / Pub-Sub: eventos y suscriptores.
- CQRS: separar comandos y consultas.
- MC: “separación de preocupaciones”, “bajo acoplamiento”.

3.6. Trampas MC (Patrones)
- Confundir Strategy vs State (Strategy elige algoritmo; State modela fases/estados).
- Decorator ≠ Subclase: agrega dinámicamente, no en compilación.
- Adapter ≠ Facade: adapter compatibiliza interfaces; facade simplifica acceso.
- Observer ≠ Mediator: observer notifica; mediator coordina.
- Singleton ≠ Global sin control: debe controlar creación/hilo-seguridad.

--------------------------------------------------------------------------------
4) ROLES Y ARTEFACTOS (Scrum y Ágil)
--------------------------------------------------------------------------------
- Roles Scrum: Product Owner (valor/prioridad), Scrum Master (proceso/impedimentos), Developers (incremento).
- Artefactos: Product Backlog (lista viva priorizada), Sprint Backlog, Incremento, Definition of Done, Acceptance Criteria (por historia).
- Métricas: Velocidad (estimación), Burn-down (restante), Burn-up (progreso), Lead/Cycle Time (flujo).
- Kanban: Límites WIP, CFD, throughput.
- DSDM: MoSCoW (Must, Should, Could, Won’t).

--------------------------------------------------------------------------------
5) PREGUNTAS TIPO MC – PISTAS RÁPIDAS
--------------------------------------------------------------------------------
- “Modelo con análisis explícito de riesgos”: Espiral.
- “Eliminar dependencias parciales”: 2FN.
- “Eliminar dependencias transitivas”: 3FN.
- “Para toda DF X→Y, X es superclave”: BCNF.
- “Listas independientes (idiomas/deportes)”: 4FN.
- “Dependencias de unión (recomposición por JOIN)”: 5FN.
- “Una instancia global controlada”: Singleton.
- “Agregar responsabilidades dinámicamente”: Decorator.
- “Elección de algoritmo intercambiable”: Strategy.
- “Notificación de cambios a suscriptores”: Observer.
- “Control de acceso mediante representante”: Proxy.
- “Límite que reduce multitarea”: WIP Limit (Kanban).
- “Rol que maximiza valor del producto”: Product Owner.
- “Gráfico de trabajo restante”: Burn-down. Progreso acumulado: Burn-up.

--------------------------------------------------------------------------------
6) MINI HOJA DE ÚLTIMO REPASO (30 LÍNEAS)
--------------------------------------------------------------------------------
- Cascada: secuencial rígido. V: pruebas alineadas. Espiral: riesgo iterativo. RUP: iterativo por fases, arquitectura.
- Ágil: 4 valores (individuos, software funcionando, colaboración cliente, cambio).
- Scrum: PO/SM/Devs; Backlogs, Incremento, DoD; Sprints, Daily/Review/Retro.
- XP: TDD, Pair, Refactor, CI, Small Releases.
- Kanban: flujo continuo, WIP, Lead/Cycle Time, CFD.
- Lean: 7 principios; desperdicios comunes.
- Crystal/FDD/DSDM: adaptabilidad, features, MoSCoW/time-box.
- 1FN: atómico sin listas. 2FN: sin parciales (no-primos dependen de toda la clave).
- 3FN: sin transitivas (no-primos no dependen de no-primos). BCNF: X→Y ⇒ X superclave.
- 4FN: sin DMV no triviales; 5FN: sin dependencias de unión; 6FN: granular/temporal.
- GRASP: experto, creador, controller, bajo acoplamiento, alta cohesión, polimorfismo, fabricación pura, indirección, variaciones protegidas.
- GoF Creacionales: Singleton, Factory Method, Abstract Factory, Builder, Prototype.
- GoF Estructurales: Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy.
- GoF Comportamiento: Chain, Command, Interpreter, Iterator, Mediator, Memento, Observer, State, Strategy, Template, Visitor.
- Arquitectónicos: MVC, Capas, Microservicios, Repository, Unit of Work, DI/IoC, Event-Driven, CQRS.
- Trampas: “Ágil = cero docs” (F), “SM asigna tareas” (F), Strategy≠State, Adapter≠Facade, Decorator≠Subclase.