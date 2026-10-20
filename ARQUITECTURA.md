# Mapa del código y recorrido

## Flujo de un agente móvil

```text
Meta en lenguaje natural
        ↓
Interpretación / planificación
        ↓
Observación del dispositivo ── Accessibility / captura / OCR / datos del sistema
        ↓
Plan validado y acotado
        ↓
Acción local ── nodo semántico / gesto / intent / herramienta Android
        ↓
Nueva observación
        ↓
Verificador → continuar / completar / detenerse / pedir ayuda
```

Las cuatro carpetas muestran aproximaciones distintas a ese recorrido. No comparten módulos compilados entre sí.

## Alpha — `projects/AgenteAlpha`

Aplicación Kotlin con Jetpack Compose.

- `MainActivity.kt`: interfaz conversacional, motor seleccionado y ciclo de tarea cancelable.
- `ui/main/AgentAlphaApp.kt`: presentación de la conversación y progreso.
- `agent/AlphaAgentRuntime.kt`: resolución de apps instaladas, rutas locales simples, prompt de decisión, lectura de JSON, whitelist de acciones y verificación sencilla.
- `engine/InstalledEngines.kt`: catálogo de paquetes instalados de ChatGPT, Gemini y Claude.
- `engine/AlphaAccessibilityService.kt`: captura de nodos semánticos y eventos; interacción con campos/controles; gestos; captura puntual y envío a la app de motor por `ACTION_SEND`.
- `AndroidManifest.xml` y `res/xml`: registro de servicio de accesibilidad y `FileProvider` temporal para la imagen.

**Tarea general en ejecución:** snapshot semántico → captura puntual (cuando se puede) → Intent a la app de modelo → detección de envío y respuesta accesible → decisión JSON → una acción → nueva observación → verificador o siguiente ronda. El paso de modelo depende de la app visible, del estado de cuenta y de que su árbol de accesibilidad deje leer la respuesta.

## Tunnel Turbo — `projects/TunnelTurbo`

Android nativo Kotlin/XML más un controlador Python/MCP.

- `ui/MainActivity.kt` y `coordinator/IntentCoordinator.kt`: chat, progreso y enrutamiento entre conversación, investigación y operación.
- `chat/`: memoria de conversación, cliente Gemini, bóveda de credenciales, motor de chat local y catálogo/benchmark local.
- `agent/GeneralAgentRuntime.kt`: creación y seguimiento de sesiones de agente.
- `agent/AgentSession.kt`, `AdaptivePlanner.kt`, `Goal.kt`, `GoalEvidence.kt`: estado de la tarea, decisión, presupuesto, habilidad y evidencia de meta.
- `model/PlanSchemas.kt` y `PlanValidator.kt`: contrato estructurado para las acciones que propone el modelo.
- `accessibility/`: servicio Android, bridge, representación de nodos y selectores semánticos.
- `perception/`: observación, OCR local, combinación de fuentes, detección de cobertura y filtros de seguridad de OCR.
- `core/`: estado, acciones, secuencias, diario de eventos, arbitraje y verificación.
- `provider/`, `router/`, `local/`: selección de motor, rutas locales y clasificación de planes.
- `shizuku/`, `worker/`, `capability/`: permisos de shell, sesiones de trabajo en otra pantalla y catálogo de capacidades.
- `controller/`: conexión ADB, estado, observador, acciones y servidor MCP desde PC.

**Tarea general en ejecución:** el router decide ruta → el worker configura una sesión → el observer produce evidencia → el planner adapta un plan → el validador comprueba formato y políticas → el executor realiza acciones → el verifier mide el efecto → el diario registra el paso. Puede recurrir a providers diferentes y a PC/controlador según configuración. Cada camino tiene prerrequisitos propios.

## Pulso — `projects/Pulso`

Aplicación Android Java con vistas XML y servicio de accesibilidad.

- `MainActivity.java`, `Ui.java`: chat, controles, panel y progreso.
- `AgentController.java`: ciclo único de tarea, estado, lote, pausas, cancelación y decisión de continuación.
- `PhoneService.java`: snapshot de la ventana, resolución de selectores y acciones de Accessibility.
- `ModelClient.java`, `Provider.java`, `SettingsActivity.java`: llamadas HTTPS de proveedor, elección del motor y configuración de claves.
- `Action.java`, `ActionBatch.java`: lenguaje de operaciones y lotes pequeños.
- `BatchGuard.java`, `SafetyPolicy.java`: validación del lote, revisión y controles por riesgo.
- `LocalRoutes.java`: operaciones deterministas que no necesitan planificación generativa.
- `SecretStore.java`: almacenamiento local protegido de la configuración sensible.
- `tools/`, `tests/`: compilación portable, comprobaciones de host y herramientas de validación de dispositivo.

**Tarea general en ejecución:** observar → enviar contexto a un provider configurado → recibir un lote de acciones → comprobar política/selector → ejecutar de una en una contra observaciones actuales → pedir un nuevo lote cuando cambie el contexto → verificar resultado. La intención de rendimiento es reducir rondas agrupando pasos previsibles.

## Visor semántico — `projects/VisorSemantico`

Prototipo Python GTK 4 para escritorio GNOME.

- `top_level_windows()` descubre las ventanas que AT-SPI publica.
- `inspect_window()` recorre la jerarquía y obtiene roles, nombres, estados, acciones y geometría de componentes.
- `open_capture()` carga una imagen local elegida por la persona.
- `SemanticViewer` presenta la lista de nodos y dibuja límites sobre la imagen.

Su utilidad es enseñar qué puede descubrir el árbol semántico al lado de una captura y hacer visibles los controles personalizados o incompletos. El usuario captura la pantalla por separado y elige la ventana: la demo se mantiene observacional.

