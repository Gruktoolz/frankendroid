# Frankendroid

**Archivo de experimentos para construir un agente operativo de Android que no dependa de APIs de modelos.**

Frankendroid reúne tres generaciones de código —Agente Alpha, Tunnel Turbo y Pulso— junto con el visor semántico de escritorio de Tunnel Turbo. El nombre es deliberadamente cariñoso: entre Kotlin, Java, Python, Accessibility, Shizuku, OCR, modelos locales, proveedores y pantallas de otras apps, esta criatura terminó siendo un Frankenstein de código espagueti.

## La meta

Escribir una instrucción sencilla en lenguaje natural y dejar que un agente conversacional entienda la intención, use las capacidades del teléfono, complete una secuencia de trabajo y compruebe el resultado. La dirección buscada combina Chat, investigación, operación del dispositivo, trabajo continuado y coordinación de motores, con el teléfono como entorno de ejecución.

El principio que guía el archivo es **independencia de APIs de modelos**. El agente debe poder operar con recursos del dispositivo —Accessibility, acciones nativas, herramientas locales, visión/OCR local cuando aporte valor y, de forma opcional, motores accesibles mediante sus propias apps— sin convertir un endpoint de pago en requisito estructural.

## Estado del proyecto

**ESTANCADO / SIN SALIDA DEMOSTRADA — archivo de investigación, no producto operativo.**

Los prototipos avanzaron en control de pantalla, observación semántica y ejecución por pasos. La prueba documentada de Tunnel Turbo llegó a una selección semántica de 190 capturas, mientras su planificador visible falló tres consultas a ChatGPT consecutivas y no ejecutó acciones automáticas. Pulso mostró una ruta móvil más estructurada y pruebas de host, pero su recorrido automático real no quedó validado de principio a fin. Alpha intentó un ciclo de modelo visible, captura y Accessibility, con un límite deliberado de ocho acciones; su propio README marca la validación de proveedor como pendiente.

El bloqueo es de arquitectura: el objetivo pide independencia de APIs, pero Pulso conecta directamente con proveedores; Tunnel Turbo mezcla integración Gemini, modelos locales y otras rutas de proveedor; Alpha evita un cliente de API propio, pero delega el razonamiento a una app instalada y depende de que su interfaz acepte la petición y exponga la respuesta. Las piezas de operación existen; el motor autónomo que las coordina con rapidez y fiabilidad no quedó demostrado.

## Proyectos incluidos

| Código fuente | Qué es | Recorrido principal |
|---|---|---|
| [Frankendroid-AgenteAlpha-source.zip](Frankendroid-AgenteAlpha-source.zip) | Prototipo Android independiente en Kotlin y Compose. | Conversación local → app visible de modelo → lectura de Accessibility y captura opcional → una acción estructurada → comprobación simple. |
| [Frankendroid-TunnelTurbo-source.zip](Frankendroid-TunnelTurbo-source.zip) | App Android Kotlin/XML, runtime de agente y controlador Python/MCP. | Chat/router → proveedor local o remoto → observación semántica/OCR/Shizuku → plan tipado → ejecutor → verifier y registro de eventos. |
| [Frankendroid-Pulso-source.zip](Frankendroid-Pulso-source.zip) | App Android nativa en Java con servicio de Accessibility y panel flotante. | Meta → controlador → cliente de proveedor → lote de acciones → política de seguridad → observación posterior → resultado. |
| [Frankendroid-VisorSemantico-source.zip](Frankendroid-VisorSemantico-source.zip) | Demo de escritorio GNOME/GTK y AT-SPI. | El usuario elige una captura → la app lee la ventana accesible → superpone roles, nombres y límites → inspección local de nodos. |

El visor permite entender visualmente la relación entre una captura y el árbol de accesibilidad. Es una demostración de inspección semántica en Linux; no es un controlador Android ni un agente autónomo.

## Cómo leer el archivo

1. Empieza por [ARQUITECTURA.md](ARQUITECTURA.md) para seguir el recorrido de cada sistema y ubicar sus componentes.
2. Lee [AUDITORIA.md](AUDITORIA.md) para comparar intención, implementación y evidencia de dispositivo.
3. Consulta [STATUS.md](STATUS.md) para el cierre y el estado observado.
4. Revisa [INVESTIGACION.md](INVESTIGACION.md) para las referencias externas y sus límites probatorios.

Las fuentes se conservan en archivos independientes para mantener el contexto de cada proyecto. Este archivo documenta su trayectoria y sus conexiones.
