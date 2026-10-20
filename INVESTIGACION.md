# Referencias y contraste externo

Consulta: 20 de octubre de 2026. Estas fuentes orientan la auditoría; no son pruebas de que Frankendroid, Alpha, Tunnel Turbo, Pulso o el visor funcionen.

| Fuente | URL directa | Qué respalda | Qué no demuestra |
|---|---|---|---|
| **Create an accessibility service** — Android Developers | https://developer.android.com/guide/topics/ui/accessibility/service | Android permite leer jerarquías de interfaz y ejecutar acciones de accesibilidad cuando el servicio y la app exponen esos datos. | No demuestra árboles completos, estables ni semánticamente buenos en todas las apps. |
| **Behavior changes: Apps targeting Android 14 or higher** — Android Developers | https://developer.android.com/about/versions/14/behavior-changes-14 | Las reglas de inicio de Activities desde segundo plano agregan condiciones y afectan handoffs de app. | No determina por sí sola el resultado en el Android/OEM concreto de cada corrida. |
| **AccessibilityService** — Android API Reference | https://developer.android.com/reference/android/accessibilityservice/AccessibilityService | Describe acciones globales, lectura de ventanas y captura desde un servicio con las capacidades declaradas. | No convierte gestos de pantalla en automatización fiable ni garantiza captura de ventanas seguras. |
| **Agent Phone / adev0x/android-agent** — GitHub | https://github.com/adev0x/android-agent | Documenta un loop de agente Android que combina captura visual, accesibilidad y acciones, además de detenerse ante algunas acciones destructivas. | Su cliente de planificación usa la API de Claude; no demuestra un puente estable sin API. |
| **AndroidWorld: A Dynamic Benchmarking Environment for Autonomous Agents** — Google Research / arXiv | https://arxiv.org/abs/2405.14573 | Apoya escenarios variados, inicialización reproducible y verificación de éxito independiente; reporta retos de generalización y variaciones de tareas. | Sus cifras experimentales no predicen el rendimiento de estos APK en un teléfono real. |
| **PalmClaw: A Native On-Device Agent Framework for Mobile Phones** — arXiv / GitHub | https://arxiv.org/abs/2607.13027 · https://github.com/ModalityDance/PalmClaw | Es un marco móvil que organiza sesiones, memoria, habilidades, herramientas y el loop en el dispositivo. | Su README dice que la inferencia usa un LLM API configurado por el usuario; la estructura local no elimina dependencia del modelo remoto ni valida nuestro código. |
| **Shizuku API README** — RikkaApps | https://github.com/RikkaApps/Shizuku-API | Permite a apps autorizadas ejecutar código con identidad shell/ADB a través de Shizuku. | No concede acceso universal a datos privados de otras apps ni resuelve razonamiento o calidad de UI. |
| **Get started with Prompt API** — Google ML Kit | https://developers.google.com/ml-kit/genai/prompt/android/get-started | Describe inferencia local Gemini Nano mediante AICore y generación de texto/imagen en dispositivos compatibles. | La disponibilidad depende del dispositivo/estado del modelo; no demuestra adecuación a planes largos ni latencia en el teléfono del proyecto. |
| **Recognize text in images with ML Kit on Android** — Google Developers | https://developers.google.com/ml-kit/vision/text-recognition/v2/android | Ofrece OCR local como fuente estructurada de texto de imagen. | OCR no es razonamiento visual ni generalización de tareas móviles. |
| **PhonePilot** — GitHub | https://github.com/gotogrub/PhonePilot | Muestra una ruta VLM local desde una máquina que controla Android por ADB. | El README plantea Docker, Ollama y GPU con VRAM de escritorio; no demuestra viabilidad rápida en el teléfono sin PC. |
| **Tunnel Turbo — Visor semántico de escritorio** — fuente de este archivo | `projects/VisorSemantico` | Implementa inspección AT-SPI y overlay de límites sobre una captura elegida localmente. | No es una publicación académica ni demuestra automatización móvil. |

## Ruta comparada

- **Control semántico primero:** reduce la ambigüedad cuando el árbol está bien expuesto y mantiene acciones ligadas a elementos etiquetados. El proyecto ya experimentó con esta ruta; la evidencia real del teléfono muestra una navegación manual útil.
- **Visión/OCR como apoyo:** aporta texto o referencias visuales cuando falta semántica. OCR local facilita leer caracteres, mientras un modelo visual pesado incrementa cómputo, memoria y latencia.
- **Modelo local como único cerebro:** mantiene la inferencia en el dispositivo, pero las opciones dependen de capacidad de hardware, AICore y calidad/latencia del modelo. No es una garantía de rapidez en el POCO ni en otros teléfonos.
- **App proveedora como motor visible:** evita implementar un cliente HTTP/API dentro de Alpha, y mantiene la consulta visible durante el uso. El protocolo depende de una interfaz externa, su accesibilidad, su estado y su política de uso.
- **Shizuku y herramientas Android:** amplían operaciones estructuradas que el sistema autoriza. Son una capa de ejecución; no reemplazan al planificador ni permiten saltarse los límites de Android.

El contraste externo contradice dos atajos: que un loop largo convierta por sí solo un prototipo en agente general, y que “sin cliente API” signifique razonamiento sin servicio remoto. La implementación propia debe diferenciar la independencia de API del transporte de modelo y medir ambas cosas.

