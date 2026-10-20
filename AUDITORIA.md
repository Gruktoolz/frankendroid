# Auditoría de cierre

## Intención común

Los proyectos intentan acercarse a un operador conversacional del teléfono: recibir una meta humana, interpretar el estado del dispositivo, usar aplicaciones y capacidades disponibles, actuar, observar el resultado y continuar hasta completar el trabajo. La dirección de producto incluye conversación, investigación, operación, tareas largas y coordinación de motores. El principio de diseño repetido en esta línea de trabajo es reducir la dependencia de APIs de modelos y mantener el trabajo operativo cerca del teléfono.

## Hallazgo central: las tres rutas no resuelven el motor de la misma manera

### Agente Alpha

Alpha fue creado como prototipo Android independiente. `MainActivity` mantiene la conversación y lanza la tarea; `AlphaAgentRuntime` resuelve órdenes sencillas como abrir una app y dirige las tareas generales; `InstalledEngines` enumera ChatGPT, Gemini y Claude por paquete; `AlphaAccessibilityService` captura el árbol, actúa sobre nodos, puede ejecutar gestos y comparte texto/capturas con la app visible del modelo.

En una tarea general, Alpha toma una observación, consulta al motor visible, interpreta JSON, ejecuta una sola acción y vuelve a observar. El tope actual es de ocho acciones para cerrar ciclos sin progreso. El lector de respuestas busca texto estable en nodos de la app proveedora, lo que acopla el runtime a la estructura de esa interfaz. El propio código marca `isSensitiveTarget`, pero no lo invoca desde la ruta de ejecución que se auditó. Las acciones de borrar, enviar o comprar necesitan una política real en el ejecutor antes de describirlas como protegidas.

**Lectura:** separa el producto y evita un cliente de API propio. El planificador sigue siendo el motor remoto de una app tercera; pasarle una imagen a esa app no convierte la inferencia en local ni da a Alpha un contrato estable de respuesta.

### Tunnel Turbo

La app Android reúne conversación, selector de rutas, memoria, providers, capacidades, observación, agente y ejecución. `IntentCoordinator` une la UI con los servicios; `GeneralAgentRuntime` crea una sesión; `AgentSession` coordina meta, presupuesto, observaciones, acciones, rondas de razonamiento y verificación. `AdaptivePlanner`, `WorldModel`, `SkillMemory`, `PlanValidator` y `ObservationReducer` estructuran la planificación. `TunnelTurboAccessibilityService` y `AccessibilityBridge` operan sobre nodos; `PerceptionRouter`, `PerceptionFusionEngine`, `LocalOcrScanner` y `OCRSafetyGate` agregan evidencia visual y textual. `ShizukuBridge` conecta capacidades con permisos shell. El controlador Python/MCP expone otra ruta desde la PC.

El árbol incluye una API Gemini, un cliente de conversación local LiteRT-LM y providers de apps IA. Por eso describe una arquitectura de experimentación multi-provider, pero no una ruta API-free verificada. La documentación de estado ya separa lo implementado en código, lo dependiente del entorno y lo no demostrado; esa distinción debe conservarse.

**Lectura:** es la fuente más amplia y también la más compleja. La documentación y los tipos de código muestran intención y piezas; el recorrido de prueba registrado muestra que la selección semántica manual funcionó, mientras las consultas automatizadas al modelo visible fallaron.

### Pulso

`MainActivity` y `Ui` llevan la conversación; `AgentController` posee el ciclo de tarea; `PhoneService` observa y actúa mediante Accessibility; `ModelClient` encapsula peticiones a proveedores; `Provider` configura los motores; `Action` y `ActionBatch` definen operaciones y lotes; `BatchGuard` y `SafetyPolicy` gobiernan la ejecución; `LocalRoutes` resuelve algunos casos directos; `SecretStore` protege la configuración del proveedor.

Pulso se concentró en enviar secuencias breves de acciones y resolver selectores semánticos en observaciones nuevas. La fuente y README describen proveedores API configurables —Gemini, Groq, OpenRouter, DeepSeek y OpenAI— junto a capturas puntuales, controles de presupuesto y políticas. Por lo tanto, la ejecución local de acciones no equivale a razonamiento local: el modelo planificador todavía se conecta mediante API en los recorridos descritos.

**Lectura:** de los tres, es el intento más explícito de reducir llamadas agrupando acciones. La validación documentada reporta el objetivo de Android 16 y una prueba real que no terminó con un resultado verificado. El mismo historial registra una lectura accidental de texto de WhatsApp durante una inspección de desarrollo; el código luego describe una barrera de paquete/ventana, que debe probarse en condiciones controladas.

### Visor semántico de Tunnel Turbo

`main.py` implementa una app GTK 4 que inicializa AT-SPI, enumera ventanas, lee rol/nombre/estado/acciones y extensiones de componentes, abre una imagen elegida por la persona y dibuja los límites estimados de nodos accesibles sobre esa imagen. La selección de la lista presenta las propiedades del elemento.

**Lectura:** es una demo de observabilidad: hace más tangible lo que la semántica de accesibilidad ofrece y dónde termina. Trabaja sobre escritorio Linux/AT-SPI; no automatiza el teléfono ni integra el visor dentro de Tunnel Turbo Android.

## ¿Qué se logró que sea diferente?

La inspección no aporta evidencia de una invención fundamental inédita: la automatización mediante Accessibility, la fusión de árbol visual y captura, los planes estructurados y los ciclos observar/actuar/verificar tienen precedentes publicados y repositorios comparables. El trabajo sí produjo una combinación de prototipos con énfasis en semántica, OCR, Shizuku, límites de presupuesto, verificación independiente y un visor gráfico que expone nodos AT-SPI sobre capturas.

La contribución más defendible es **la exploración concreta del espacio de diseño y su registro de fallos reales**, incluida la diferencia entre “la acción llegó a Android” y “la meta quedó verificada”, el problema de latencia/lectura de respuestas en apps de modelos y los riesgos de leer una ventana equivocada. El visor semántico es una pieza demostrativa útil, no prueba de novedad académica.

## Cierre

Frankendroid deja juntas las tres líneas para que se entienda su recorrido sin disfrazarlas de una única app. Su meta sigue siendo valiosa; el estado de la ingeniería reunida es experimental y estancado. La siguiente generación requeriría un motor y una fuente de planificación compatibles con la meta API-free, además de verificación móvil controlada que mida finalización, latencia, recuperación y seguridad sobre tareas variadas.

