# Estado — Frankendroid

**Clasificación:** estancado; la salida técnica no está demostrada todavía.

**Propósito:** investigar un agente conversacional y operativo para Android que complete tareas simples y complejas con una instrucción, use capacidades del teléfono y reduzca la dependencia estructural de APIs de modelos.

**Lectura del estado:**

- Alpha es el intento más directo de separar la app nueva de Tunnel Turbo y Pulso. Su ciclo de operación conecta una app visible de modelo con Accessibility, pero el traspaso de petición/respuesta y la continuación de tareas no quedaron validados en todos los motores.
- Tunnel Turbo contiene la arquitectura más amplia: observación, acciones, verificación, memoria, providers, OCR, Shizuku y controlador externo. La presencia de esas capas en código no prueba que todas funcionen juntas en un teléfono ni que el worker termine tareas de forma sostenida.
- Pulso aporta un controlador móvil más acotado, lotes de acciones y políticas de ejecución. Su conexión al modelo depende de proveedores por API en el flujo descrito en su fuente; la validación de la tarea automática en dispositivo quedó inconclusa.
- El visor semántico muestra una herramienta de inspección útil: alinea una captura local con nombres, roles, estados y límites de los nodos que una app expone a AT-SPI.

## Resultado demostrable

El recorrido de Tunnel Turbo en el POCO llegó manualmente a la carpeta semántica `DCIM/Screenshots`, donde se seleccionaron 190 elementos para inspección. La consulta automática a ChatGPT falló tres veces y no generó acciones. La interfaz de limpieza mostró categorías separadas, pero no se ejecutó el borrado.

La validación registrada de Pulso incluye compilación y comprobaciones locales anteriores, además de intentos reales en Android 16. La prueba del modo automático no alcanzó una respuesta final verificada. Su registro también documenta una extracción de accesibilidad que leyó contenido de WhatsApp durante una comprobación de desarrollo; ese episodio forma parte del historial técnico del proyecto y motiva el control de paquete/ventana antes de cada lectura.

Alpha declara una compilación observada en su registro de trabajo y una estrategia de parada por presupuesto. Su recorrido de modelo visible no está validado de extremo a extremo contra todas las apps proveedoras.

## Conclusión de cierre

La colección aporta implementaciones y evidencia de ingeniería alrededor de Accessibility, acciones semánticas, visión/OCR, ejecución Android, providers locales/remotos y verificación. El resultado novedoso es la integración experimental y el aprendizaje documentado; la inspección no encontró evidencia para afirmar que se inventó una técnica de agente móvil inédita o que ya exista un agente general fiable.

La razón para clasificarlo como estancado es concreta: las tres rutas tienen dependencias incompatibles con la aspiración de autonomía API-free o dependen de interfaces externas frágiles, y ninguna evidencia reunida confirma un ciclo general, rápido, persistente y autónomo hasta verificar la meta. El repositorio conserva el trabajo como archivo para reutilizar piezas y evidencia si se retoma con un motor y una arquitectura mejor delimitados.

