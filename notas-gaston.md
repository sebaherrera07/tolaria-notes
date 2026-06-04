---
type: AirOps
_organized: true
---
# Sebastian / Gaston

#### Introducción a AirOps y contexto del equipo

- El ingreso de Gaston a AirOps se produjo a fines de febrero, con apenas un par de meses en la empresa al momento de la reunión.
- Sebastian había recibido previamente una introducción general a la plataforma por parte de Feli, Vicky, Fede y Rafa, con foco en el equipo de Content Quality.
- El conocimiento acumulado hasta ese momento era superficial, incluyendo una noción básica de los product lines, Brankit y Quality Check.

#### Uso de agentes de IA en el flujo de trabajo diario

- Fantino es el agente de IA integrado en Slack que concentra gran parte del flujo de trabajo interno del equipo.
- Toda interacción con Fantino debe realizarse en canales públicos de Slack, ya que el agente no responde en conversaciones privadas, lo cual genera incomodidad para consultas personales o de depuración.
- Dicho agente se utiliza activamente para diagnosticar problemas en producción, cruzando información de Sentry con logs de Datadog sin necesidad de revisar estas herramientas manualmente.
- Claude es el asistente de IA principal para tareas de desarrollo, incluyendo la generación de especificaciones técnicas, diagramas de secuencia, modelos de datos y tickets de Linear.
- El flujo de trabajo con Claude implica trabajar siempre en plan mode, lo que permite revisar las propuestas antes de ejecutarlas y corregir lo que no resulte adecuado.
- Para las revisiones de código se utiliza una sesión separada de Claude sin contexto previo, cargando el skill de code review para evitar el sesgo del agente que construyó el código.

#### Herramienta GrepTile y su integración en el proceso de revisión de código

- GrepTile es una herramienta de revisión de código que corre automáticamente al crear un pull request y también al hacer push de nuevos cambios.
- Anteriormente era necesario invocar a GrepTile manualmente con una mención en el PR para que corriera nuevamente tras modificaciones; ahora se ejecuta de forma automática con cada push.
- Los comentarios generados por GrepTile se cierran automáticamente como resueltos cuando los cambios sugeridos son aplicados, mejorando significativamente el flujo de revisión.
- El criterio utilizado es iterar hasta obtener un confidence score de 5 sobre 5 antes de solicitar revisión humana, dado que las sugerencias de la herramienta suelen tener sentido técnico.
- En ocasiones GrepTile detecta no solo code smells sino también vulnerabilidades de seguridad, lo que añade valor adicional al proceso.

#### Descripción general de la plataforma AirOps y el concepto de grillas

- Las grillas son la interfaz principal de la plataforma para gestionar contenido a escala, permitiendo configurar múltiples workflows como columnas sobre un conjunto de artículos.
- Cada fila de la grilla representa un artículo con atributos como URL del cliente, keyword y configuración de workflow asociada.
- El modelo de negocio de la plataforma apunta a que empresas con grandes volúmenes de contenido, como blogs con miles de artículos, puedan actualizar, optimizar y publicar contenido de forma automatizada.
- Los workflows pueden incluir pasos de revisión humana, donde el proceso se detiene hasta que un usuario aprueba el contenido antes de continuar hacia la publicación.
- La plataforma utiliza herramientas externas como SEMrush para obtener insights competitivos e incorporarlos al proceso de generación y optimización de contenido.

#### Funcionalidad de Content Review y sus checkers

- Content Review es un proceso de revisión de calidad que corre como postproceso tras la generación de un artículo, evaluando el cumplimiento de una serie de reglas predefinidas.
- Los checkers disponibles incluyen verificación de citas y fuentes, detección de links rotos, presencia del keyword en encabezados H2, densidad del keyword, cantidad de links internos y externos, longitud de párrafos y oraciones, jerarquía de encabezados y consistencia gramatical de bullets.
- Cada checker puede ser activado o desactivado por el usuario según las necesidades de su caso de uso particular.
- La configuración del Content Review se realiza a nivel de columna en la grilla, apuntando a columnas de datos como keyword y URL en lugar de valores fijos, para que aplique genéricamente a todas las filas.
- Algunos checkers son de tipo flag-only, como el de links rotos, donde el sistema solo puede señalar el problema sin poder corregirlo automáticamente.
- Otros checkers permiten corrección automática mediante inteligencia artificial, como el ajuste de la densidad del keyword o la brevedad de los bullets.
- Al aplicar correcciones, el sistema genera un diff que el usuario puede revisar, aceptar o rechazar antes de que los cambios se apliquen al artículo.
- Una vez aceptados los cambios, el sistema permite volver a correr el Content Review para verificar que los checkers ahora pasen correctamente.

#### Arquitectura del pipeline de Content Review

- El pipeline de Content Review está compuesto por cuatro etapas secuenciales que son captura, extracción, enriquecimiento y scoring.
- La etapa de captura toma el artículo y lo envía al backend; la extracción genera estadísticas, claims y ejemplos a partir del contenido.
- El enriquecimiento valida en dos pasadas que los claims, ejemplos y estadísticas extraídos sean correctos, utilizando un LLM.
- El scoring ejecuta los checkers en paralelo, donde la mayoría son determinísticos y solo algunos requieren llamadas a LLMs, como el checker de encabezados descriptivos.
- La optimización del pipeline consistió en identificar qué checkers podían ejecutarse en paralelo sin depender del enriquecimiento, reduciendo el tiempo total de ejecución de aproximadamente 10 minutos a cerca de 1 o 2 minutos.
- Un objeto score centralizado recibe escrituras concurrentes de todos los checkers, gestionado mediante un sistema de semáforos para evitar condiciones de carrera.
- Agregar un nuevo checker es relativamente sencillo si es determinístico, ya que basta con crear una clase que extienda la clase padre; si requiere preprocesamiento con LLM, es necesario modificar también la etapa de enriquecimiento.
- El Remediator es el componente encargado de generar los diffs de corrección, y trabaja de forma coordinada cuando múltiples checkers afectan la misma sección del artículo para evitar que un diff pise las correcciones del otro.
- Todo el pipeline cuenta con trazabilidad en Langsmith, lo cual es obligatorio para poder medir el consumo de tokens por etapa y habilitar el cobro futuro del feature.

#### Content Review en Playbooks

- Los Playbooks son artefactos individuales donde también se puede configurar y ejecutar el Content Review, con una interfaz similar a la de las grillas aunque visualmente más básica.
- La configuración en Playbooks requiere definir el keyword y el site domain como inputs del playbook, los cuales se mapean a campos de texto configurables.
- El Content Review en Playbooks se dispara automáticamente cuando la sesión queda idle, es decir, cuando el agente terminó de generar el contenido y no hay revisiones humanas pendientes.

#### Modelo de cobro del Content Review

- Actualmente el Content Review no se cobra a los usuarios, ya que al momento de su desarrollo no estaba definida la estrategia de pricing.
- El modelo de cobro en desarrollo para otros features como los Power Steps consiste en convertir el consumo de tokens en unidades llamadas tareas, que se descuentan del plan del usuario.
- Se realizó un análisis preliminar sobre cómo aplicar este modelo al Content Review, pero se concluyó que los cambios necesarios corresponden al equipo de Pricing y no al equipo de Content Quality.

#### Problemas actuales y trabajo en progreso

- Existe un problema de desconexión entre el Content Review y el agente, ya que el proceso corre fuera del contexto del agente y este no tiene visibilidad sobre los resultados ni puede responder preguntas al respecto.
- Desde la perspectiva del usuario, resulta contradictorio que la plataforma genere contenido y luego señale que ese mismo contenido no cumple con los estándares de calidad, generando una percepción negativa del producto.
- La solución en progreso consiste en incorporar el Content Review dentro del contexto del agente, de modo que este reciba el feedback de los checkers fallidos y se autocorrija antes de finalizar la sesión.
- Aunque la autocorrección ocurrirá dentro de la sesión, se mantendrá la ejecución final del Content Review como checklist visible para el usuario, alineándose con lo que ofrece la competencia.
- Mantener el proceso final también tiene valor práctico porque algunos checkers como el de links rotos no pueden corregirse automáticamente y requieren intervención manual del usuario.
- Otro problema identificado es que el Remediator no tiene visibilidad de las writing rules definidas en el Brankit, lo que puede generar correcciones que violan las preferencias del usuario, como el caso del uso de em dashes.
- Pat está trabajando en integrar las writing rules del Brankit tanto en la generación del artículo como en el proceso de corrección del Content Review para garantizar consistencia.

#### Herramientas de monitoreo y observabilidad

- Datadog se utiliza para monitorear la salud general de la plataforma, con el dashboard WebMonitor que muestra latencia y requests por segundo.
- Sentry se usa para monitorear errores en producción, filtrando por entorno productivo y observando spikes en Trends y la antigüedad de los issues para detectar anomalías post-deployment.
- Langsmith permite visualizar el rendimiento de todas las llamadas a LLMs, incluyendo tiempo de ejecución, tokens consumidos y costo por etapa, tanto para Scoring como para Remediation.
- Los modelos de Anthropic reportan automáticamente el costo por token en Langsmith, mientras que los modelos de OpenAI no incluyen esa información en la integración actual.
- El tracing correcto en Langsmith es un requisito obligatorio para cualquier checker nuevo que se agregue, ya que es la base para el modelo de cobro futuro.

#### Configuración de modelos LLM en el panel de administración

- En Platform Settings existe una sección de Content Quality donde se pueden configurar los modelos LLM utilizados para los checkers, el Remediator y el Fast Remediation LLM.
- El Fast Remediation LLM existe como optimización para correcciones estructurales simples que no requieren modelos de alto razonamiento.
- Los checkers utilizan modelos de OpenAI en modo asíncrono, ya que Gemini y Anthropic tienen latencias de entre 12 y 24 horas en modo asíncrono, lo cual es incompatible con los tiempos esperados del proceso.
- El Remediator opera siempre en modo síncrono y soporta los tres providers, permitiendo elegir el modelo y el nivel de razonamiento deseado.

#### Proceso de deployment a producción

- El proceso de deployment comienza marcando el inicio en el canal de deployments de Slack con un indicador amarillo para avisar al equipo.
- El comando de deployment se ejecuta desde la raíz del proyecto y toma como base el código que está en la rama main.
- Antes de ejecutar el deployment es recomendable verificar en GitHub Actions que no haya un staging deployment en curso ni que el último haya fallado.
- Durante el deployment se debe monitorear simultáneamente la terminal, el dashboard de Datadog y el feed de issues de Sentry para detectar anomalías.
- La mayoría de los deployments no requieren reload, y el proceso completo demora aproximadamente entre 15 y 20 minutos.
- Ante un incidente post-deployment, la práctica habitual es escalar en el canal interno de Slack taggeando a Fantino para que realice el triage, identifique los últimos cambios relevantes y coordine la respuesta del equipo.

#### Próximos pasos

- Se planea agregar un nuevo checker al pipeline de Content Review, cuya complejidad dependerá de si es determinístico o requiere procesamiento previo con LLM.
- Gaston y Sebastian trabajarán juntos en la incorporación del Content Review dentro del contexto del agente en Playbooks, para que la generación y corrección ocurran en una sola pasada.
- Se realizará una sesión de pairing para que Sebastian vea el flujo de trabajo completo de punta a punta, desde la creación del story con Claude hasta la implementación y el PR.
- Sebastian deberá solicitar acceso a Sentry, Datadog y al panel de administración de producción para contar con todas las herramientas necesarias.
- Se recomienda que Sebastian explore el código base con ayuda de Claude, utilizando los diagramas de Notion como punto de partida para entender la arquitectura del sistema.
- El documento de Notion sobre Content Review Feedback in Playbooks fue identificado como la referencia prioritaria de lectura para el trabajo inmediato del equipo.

***

## Tarea

#### Estrategia de onboarding con Claude para entender el repositorio

- Abrirse una sesión de Claude es la forma recomendada de ganar contexto sobre el proyecto desde cero.
- Claude tiene la capacidad de analizar un repositorio completo si se le pasa la ruta absoluta de la carpeta del proyecto.
- El enfoque sugerido consiste en pasarle a Claude el proyecto de Python que contiene los checkers del Content Quality Pipeline y pedirle que explique cómo está implementado el primero de la lista.
- A partir de esa explicación, se puede iterar preguntándole qué hace falta para migrar un checker implementado en Python hacia Rails dentro de la plataforma existente.

#### Flujo de trabajo recomendado con Claude antes de comenzar a desarrollar

- Antes de pedirle a Claude que cree tickets en Linear, es importante quedar conforme con el diseño propuesto.
- Solicitar la creación de tickets de forma prematura hace que Claude se vuelva muy lento, ya que cada cambio posterior obliga al modelo a validar su estado mental y actualizar cada ticket en Linear.
- El proceso recomendado es primero iterar sobre el diseño hasta estar satisfecho y recién después pedirle que genere los elementos de planificación.

#### Uso de Mermaid y Notion para visualizar el diseño

- Mermaid es una herramienta que Claude maneja muy bien para generar diagramas, y es similar a Plant UML que Sebastian ya conoce.
- Para visualizar los diagramas sin necesidad de una cuenta de Mermaid, se puede crear una página en Notion, ya sea pública o privada.
- Claude puede volcar todo su razonamiento en Notion, incluyendo propuestas de cambios al modelo de datos, diagramas de entidades y diagramas de secuencia, para facilitar la comprensión del diseño.

#### Consideraciones de observabilidad y patrones de logging en los checkers

- Al pedirle a Claude que proponga una implementación, es importante indicarle explícitamente que tenga en cuenta la observabilidad.
- Claude debe revisar los patrones de logging existentes en los demás checkers y respetarlos en cualquier nueva implementación que proponga.

#### Arquitectura de trazabilidad en Langsmith con llamadas asincrónicas

- En Langsmith, los registros están organizados en niveles jerárquicos donde se abre una chain base y luego se van abriendo chains internas que se agregan como hijas de la cadena padre.
- Las llamadas a modelos de lenguaje dentro de los checkers o enriquecedores se ejecutan de forma asincrónica para no bloquear el worker ni afectar la performance general de la plataforma.
- El flujo asincrónico consiste en encolar la llamada al LLM, liberar el job y luego encolar un segundo worker que se ejecuta aproximadamente 15 segundos después para verificar el estado del proceso anterior.
- Esta arquitectura implica una complejidad adicional en Langsmith, ya que la chain debe abrirse sin cerrarse y guardarse en un modelo para poder recuperarla y cerrarla una vez que el proceso asincrónico finaliza.
- Este patrón ya está resuelto en otros checkers existentes y puede consultarse directamente con Claude para obtener los fragmentos de código relevantes.

#### Entorno local de desarrollo y uso de la clave de producción de Langsmith

- Todo el desarrollo puede realizarse en local sin necesidad de un ambiente especial, ya que el proyecto está completamente configurado para ejecutarse localmente, incluyendo la integración con Langsmith.
- La clave de Langsmith utilizada en local es la misma que la de producción, por lo que los registros generados durante el desarrollo local aparecen en el mismo entorno que los de producción.
- Los registros locales se distinguen fácilmente de los de producción porque tienen IDs de score significativamente más bajos, dado que producción ya acumula miles de registros mientras que los locales comienzan desde números muy bajos.
- Generalmente el último registro que aparece al refrescar Langsmith corresponde a la ejecución local más reciente, lo que facilita identificar los propios registros.

#### Próximos pasos

- Sebastian explorará el repositorio de Python de forma autónoma utilizando Claude como herramienta principal de comprensión y diseño, iterando sobre la arquitectura antes de avanzar al desarrollo.
- Se acordó que Sebastian intentará avanzar de forma independiente, chocándose con el código, y que cuando encuentre bloqueos o necesite revisión del diseño se coordinarán para revisarlo juntos.
- Gaston estará disponible para resolver dudas y acompañar el proceso sin restricciones, con énfasis en que no hay preguntas tontas y en que Sebastian arranque bien.
